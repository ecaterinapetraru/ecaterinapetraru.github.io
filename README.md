
## PLSQL 7

### Triggers (declansatoare)

**Triggerele** sunt blocuri de cod care se pot executa in mod repetat in functie de cand se doreste (activare/dezacivare), insa acestea nu pot fi invocate in mod express, ci se executa automat. Acestea au un nume (identificator) si pot fi activate sau dezactivate utilizand acest nume.

Aceasta executie este asociata cu o anumita operatie care se efectueaza asupra:
* unei tabele sau a unui view: triggere de tip _DML_
* unei scheme de baze de date: triggere de tip _DDL_
* intregii bazei de date: triggere de tip _system_. 

> De exemplu, daca vrem sa facem o operatie de stergere dintr-o tabela si dorim ca valoarea ce este stearsa sa fie copiata intr-o alta tabela de bkup, este firesc ca sa dorim executarea triggerului inainte ca stergerea efectiva sa fie efectuata - in acest fel aveam acces la valoarea ce va fi stearsa si putem sa o copiem in tabela de bkup.

Acestea sunt motivele pentru care am dori sa utilizam un trigger:

- generarea automata de valori intr-o coloana
- realizarea de LOG-uri
- realizarea de statistici
- modificarea datelor dintr-o tabela atunci cand este executata o operatie intr-un view
- asigurarea integritatii dintre chei primare/straine atunci cand tabelele nu sunt in acelasi tablespace (de exemplu tabela studenti este pe un calculator si tabela note este pe alt calculator si vrem ca atunci cand inseram o nota sa verificam daca exista cheia in tabela stocata pe celalalt calculator)
- publicarea de evenimente cand sunt facute anumite opreatii in baza de date (de exemplu afisarea automata in consola ca cineva incearca sa stearga anumite date dintr-o tabela).
- interzicearea operatiilor de tip DML intr-un anumit interval orar (de exemplu pentru a nu se putea pune note decat in ziua examenului)
- interzicerea tranzactiilor incorecte sau restrictionarea pe baza unor reguli complexe ce nu pot fi obtinute doar prin chei primare/straine, unicitate sau alte constrangeri la nivel de tabela (de exemplu am putea sa permitem sa avem mai multe inregistrari cu un acelasi identificator dar care sa aiba suma unui anumit camp mai mica decat o anumita valoare).

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
==========================================================================================

### Aparitia erorilor de tip Mutating Table

