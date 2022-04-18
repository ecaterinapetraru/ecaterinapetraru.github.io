# 
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

###Triggere de tip DML (cu BEFORE / AFTER)


Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).


### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

