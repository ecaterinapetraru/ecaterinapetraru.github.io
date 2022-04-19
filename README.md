
## PLSQL 7

### Triggers (declansatoare)

**Triggerele** sunt blocuri de cod care se pot executa in mod repetat in functie de cand se doreste (activare/dezacivare), insa acestea nu pot fi invocate in mod express, ci se executa automat. Acestea au un nume (identificator) si pot fi activate sau dezactivate utilizand acest nume.

Aceasta executie este asociata cu o anumita operatie care se efectueaza asupra:
* unei tabele sau a unui view: triggere de tip _DML_
* unei scheme de baze de date: triggere de tip _DDL_
* intregii bazei de date: triggere de tip _system_. 

Tipuri de evenimente care pot determina execuția unui trigger sunt:
* Comenzi INSERT, UPDATE, DELETE pe o tabelă
* Comenzi INSERT, UPDATE, DELETE pe un view cu opțiunea INSTEAD OF
* Comenzi CREATE, ALTER, DROP la nivel de schemă sau bază de date
* Comenzi SHUTDOWN, LOGON, LOGOFF la nivel de schemă sau bază de date

> De exemplu, daca vrem sa facem o operatie de stergere dintr-o tabela si dorim ca valoarea ce este stearsa sa fie copiata intr-o alta tabela de bkup, este firesc ca sa dorim executarea triggerului inainte ca stergerea efectiva sa fie efectuata - in acest fel aveam acces la valoarea ce va fi stearsa si putem sa o copiem in tabela de bkup.

Acestea sunt motivele pentru care am dori sa utilizam un trigger:

- declanșarea automată, la apariția evenimentului monitorizat
- generarea automata de valori intr-o coloana
- realizarea de LOG-uri
- prelucrarea de informații statistice în legătură cu accesul tabelelor
- modificarea datelor dintr-o tabela atunci cand este executata o operatie intr-un view
- asigurarea integritatii dintre chei primare/straine atunci cand tabelele nu sunt in acelasi tablespace (de exemplu tabela studenti este pe un calculator si tabela note este pe alt calculator si vrem ca atunci cand inseram o nota sa verificam daca exista cheia in tabela stocata pe celalalt calculator)
- publicarea de evenimente cand sunt facute anumite opreatii in baza de date (de exemplu afisarea automata in consola ca cineva incearca sa stearga anumite date dintr-o tabela).
- interzicearea operatiilor de tip DML intr-un anumit interval orar (de exemplu pentru a nu se putea pune note decat in ziua examenului)
- interzicerea tranzactiilor incorecte sau restrictionarea pe baza unor reguli complexe ce nu pot fi obtinute doar prin chei primare/straine, unicitate sau alte constrangeri la nivel de tabela (de exemplu am putea sa permitem sa avem mai multe inregistrari cu un acelasi identificator dar care sa aiba suma unui anumit camp mai mica decat o anumita valoare).

Sintaxa unui trigger este: 
```
CREATE [OR REPLACE] TRIGGER [schema.]trigger_name
	{BEFORE | AFTER | INSTEAD OF}
	{DELETE | INSERT | UPDATE [OR {DELETE | INSERT | UPDATE }…] 
		[OF COLUMN[, COLUMN …] ]} 
	ON [schema.]tabel _name
	[referencing_clauses] 
	[FOR EACH ROW] 
	[WHEN (condition) ] 
	DECLARE
		trigger_variables
	BEGIN
		trigger_body
	END
```

### Triggere de tip DML (cu BEFORE / AFTER)

Cand triggerul este executat asupra unei atbele sau a unui view, acesta este de tip `DML`. Tinand cont de momentul in care este executat triggerul relativ la momentul in care se face operatia asupra tabelei, triggerele pot fi `BEFORE` sau `AFTER`. De asemenea, triggerul este compus din `DELETE`, `INSERT`, si `UPDATE`. Cand zicem BEFORE ne putem referi la momentul executiei comenzii de tip DML sau putem sa specificam o granularitate mai mare si sa executam triggereul inainte de modificarea fiecarui rand ce va fi transformat de operatia DML.

Sa vedem un trigger care se executa inainte ca o operatie de tip insert, delete sau update sa fie executata:
```
set serveroutput on;

CREATE OR REPLACE TRIGGER dml_stud
   BEFORE INSERT OR UPDATE OR DELETE ON studenti
BEGIN
  dbms_output.put_line('Operatie DML in tabela studenti !');
  -- puteti sa vedeti cine a declansat triggerul:
  CASE
     WHEN INSERTING THEN DBMS_OUTPUT.PUT_LINE('INSERT');
     WHEN DELETING THEN DBMS_OUTPUT.PUT_LINE('DELETE');
     WHEN UPDATING THEN DBMS_OUTPUT.PUT_LINE('UPDATE');
  END CASE;
END;
/

delete from studenti where id=10000;
```
Puteti sa vedeti efectul BEFORE/AFTER compiland urmatoarele doua triggere:
```
set serveroutput on;

CREATE OR REPLACE TRIGGER dml_stud1
   BEFORE INSERT OR UPDATE OR DELETE ON studenti
declare   
   v_nume studenti.nume%type;
BEGIN  
  select nume into v_nume from studenti where id=200;
  dbms_output.put_line('Before DML TRIGGER: ' || v_nume);
END;
/
```
```
CREATE OR REPLACE TRIGGER dml_stud2
   AFTER INSERT OR UPDATE OR DELETE ON studenti
declare   
   v_nume studenti.nume%type;
BEGIN  
  select nume into v_nume from studenti where id=200;
  dbms_output.put_line('After DML TRIGGER: ' || v_nume);
END;
/
```
si executand apoi:
```
update studenti set nume='NumeNou' where id=200;
```

=================================================================================

### Aparitia erorilor de tip Mutating Table

În momentul în care se dorește executarea unei operații DML care implicit execută un trigger, acest trigger nu are permisiunea să citească date din tabela în curs de modificare. Încercarea de a realiza un select asupra tabelei va genera o eroare de tipul Mutating table (ORA-04091).

Există două metode pentru a evita aceste erori:
- Utilizarea triggerilor compusi (compunerea celor 4 tipuri de trigere disponibile: `BEFORE`, `BEFORE EACH ROW`, `AFTER EACH ROW` si `AFTER`).
- Utilizarea unei tabele temporare

_exemplu_:
```
create or replace trigger mutate_example
after delete on note for each row
declare 
   v_ramase int;
begin
   dbms_output.put_line('Stergere nota cu ID: '|| :OLD.id);
   select count(*) into v_ramase from note;
   dbms_output.put_line('Au ramas '|| v_ramase || ' note.');
end;
/
delete from note where id between 101 and 110;
/
```
In momentul in care o exceptie este aruncata, nici modificarea din DML nu va avea loc. 

Solutia pentru problema anterioara este sa construim un trigger care sa afiseze doar la sfarsit numarul de linii ramase (numai triggerele de tipul "for each row" vor crea mutating table).

```
set serveroutput on;

CREATE OR REPLACE TRIGGER stergere_note 
FOR DELETE ON NOTE
COMPOUND TRIGGER
  v_ramase INT;
  
  AFTER EACH ROW IS 
  BEGIN
     dbms_output.put_line('Stergere nota cu ID: '|| :OLD.id);
  END AFTER EACH ROW;
  
  AFTER STATEMENT IS BEGIN
     select count(*) into v_ramase from note;
     dbms_output.put_line('Au ramas '|| v_ramase || ' note.');  
  END AFTER STATEMENT ;
END stergere_note;
/
delete from note where id between 241 and 250;
/
```

### Triggere de tipul instead of

Acest trigger se defineste numai pe view-uri, nu si pe tabele. Unele view-uri nu pot fi modificate prin comenzi DML, dar folosind un trigger cu opțiunea `INSTEAD OF` acest lucru este realizabil.

View-urile care nu pot fi modificate prin comenzile `UPDATE`, `INSERT` sau `DELETE` sunt cele create printr-o interogare care conține în construcție:
- Un operator `SET` sau `DISTINCT`
- Clauzele `GROUP BY`, `ORDER BY`, `CONNECT BY` sau `START WITH`
- O expresie tip colectie într-o clauza `SELECT`
- O subcerere într-o clauza `SELECT`

Datorita existentei triggerelor acum putem sterge cu usurinta un obiect, triggerul avand rolul de a sterge toate informatiile din tabelele aditionale.
_exemplu:_
```
create view std as select * from studenti;

CREATE OR REPLACE TRIGGER delete_student
  INSTEAD OF delete ON std
BEGIN
  dbms_output.put_line('Stergem pe:' || :OLD.nume);
  delete from note where id_student=:OLD.id;
  delete from prieteni where id_student1=:OLD.id;
  delete from prieteni where id_student2=:OLD.id;
  delete from studenti where id=:OLD.id;
END;

delete from std where id=75;
```

=================================================================================

### Triggere DDL

Din punctul de vedere al momentului cand sunt executate, triggerele de sistem pot fi de tipul `BEFORE`, `AFTER` sau `INSTEAD`.

Din punctul de vedere al evenimentului ce poate fi tratat, pot fi triggere ce au loc cand este modificata schema de baze de date sau de tipul `INSTEAD OF CREATE`.

Sintaxa:
```
CREATE [OR REPLACE] TRIGGER nume_trigger
[ddl_event1 [OR ddl_event2 OR ddl_event3...]] 
ON {DATABASE|SCHEMA} -- se aplica la nivel de baza de date sau la nivel de user [DECLARE ...] 
BEGIN 
...
END 
```

_exemplu:_
```
CREATE OR REPLACE TRIGGER drop_trigger
  BEFORE DROP ON student.SCHEMA
  BEGIN
    RAISE_APPLICATION_ERROR (
      num => -20000,
      msg => 'can''t touch this');
  END;
/

DROP TABLE NOTE;
```

Alte operatii `DDL` pe care se pot realiza triggere sunt:
- BEFORE OR AFTER:
- ALTER
- DROP (!!) OR ANALYZE
- ASSOCIATE OR STATISTICS OR AUDIT/NOAUDIT OR COMMENT
- CREATE OR DDL OR DISASSOCIATE STATISTICS
- GRANT OR RENAME OR REVOKE OR TRUNCATE
- AFTER SUSPEND OR LOGOFF 
- BEFORE FORELOGON

Lista tuturor trigerelor de tip `DDL` poate fi consultata <a href="https://docs.oracle.com/cd/E11882_01/appdev.112/e25519/triggers.htm#CHDGIJDB">aici</a>.

### Triggere Sistem

Pe langa triggerele pentru operatii de tip DDL mai exista 2 tipuri de triggere prin intermediul carora puteti obtine informatii despre evenimentele intamplate in sistem.

Un exemplu ar putea fi, stocarea intr-un tabel a utilizatorilor si orele la care acestia se autentifica.
```
create table autentificari(nume varchar2(30), ora timestamp);
/
CREATE OR REPLACE TRIGGER check_user
  AFTER LOGON ON DATABASE
DECLARE
  v_nume VARCHAR2(30);
BEGIN
  v_nume := ora_login_user;
  INSERT INTO autentificari VALUES(v_nume, CURRENT_TIMESTAMP);
END;
/
```
Atunci cand nu mai aveti nevoie de triggere, puteti sa le eliminati cu drop trigger urmata de numele triggerului pe care doriti sa il eliminati.









