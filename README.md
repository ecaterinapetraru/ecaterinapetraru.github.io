## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/ecaterinapetraru/ecaterinapetraru.github.io/edit/main/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

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

=====================================================================================================================================================
<!DOCTYPE html>
<html lang="ro">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>AGA (Actors Guild Awards Visualizer) Scholarly HTML Report</title>

    <link rel="stylesheet" href="https://w3c.github.io/scholarly-html/css/scholarly.min.css">
    <script src="https://w3c.github.io/scholarly-html/js/scholarly.min.js"></script>
</head>
<body>
<header>
    <div class="banner">
        <img style="width: 100%" src="/static/assets/scholarly-html.svg" width="227" height="50" alt="Scholarly HTML">
    </div>
    <h1><project>AGA (Actors Guild Awards Visualizer)</project> Raport - Scholarly HTML</h1>
</header>

<div role="contentinfo">
    <dl>
        <dt>Authors</dt>
        <dd>
            <a href="https://github.com/ecaterinapetraru" target="_blank">Petraru Ecaterina</a>,
            <a href="https://github.com/andradalascar" target="_blank">Andrada Georgiana Lascar</a>
        </dd>
    </dl>
</div>
<section typeof="sa:Abstract" id="abstract" role="doc-abstract">
    <h2>Abstract</h2>
    <p>
        Acest document cuprinde aspectele tehnice si interactiunea cu utilizatorul pentru proiectul AGA (Actors Guild Awards Visualizer), din cadrul materiei <web>Tehnologii Web</web>, in semestrul al II-lea al anului II de studiu (2022), realizat de <a href="https://github.com/ecaterinapetraru">Petraru Ecaterina</a> si <a href="https://github.com/andradalascar">Andrada Georgiana Lascar</a>.
        <br>
        <br>
        <infoiasi><i>Universitatea "Alexandru Ioan Cuza" Iasi, Facultatea de Informatica</i></infoiasi>
    </p>
</section>
<!--1.Introduction-->
<section id="introduction" role="doc-introduction">
    <h2>Introducere</h2>
    <section id="purpose">
        <!-- review? -->
        <h3>Scop</h3>
        <p>
		Aplicatia Web AGA, reprezinta un instrument Web de vizualizare flexibila a datelor referitoare la nominalizarile actorilor pentru ultimii ani la Screen Actors Guild (SAG) Awards. Astfel utilizatorul poate aceesa facil informatii referitoare la fiecare actor si productie cinematografica din cadrul acestei decernari de premii.
        </p>
    </section>

    <section id="intended-audience">
        <h3>Public ținta</h3>
        <p>
	In continuare vom prezenta o descriere generala a aplicatiei, urmand ca apoi sa fie detaliata interactiunea utlizatorului cu aceasta. Acest document se adreseaza in principal utilizatorului obisnuit, si vizeaza in special principalele functionalitati ale aplicatiei si modul de interactiune cu utlizatorul, pentru a facilita utilizarea aplicatiei.
        </p>
    </section>
    <section id="scope">
        <h3>Viziune</h3>
        <p>
        Aplicatia doreste sa usureze efortul depus de utilizator pentru a accesa vizualizare flexibila a datelor referitoare la nominalizarile actorilor pentru ultimii ani la Screen Actors Guild (SAG) Awards, redarea de stiri privitoare la fiecare nominalizat in parte.
        </p>
    </section>
</section>

<!-- 2. Description -->
<section id="description" role="doc-introduction">
    <h2>Descriere</h2>
    <section id="perspective">
        <h3>Perspectiva</h3>
        <p>
        Aplicatia vine ca o extensie pentru celelalte site-uri de acest tip, oferind utilizatorului o viziune mai larga asupra informatiilor suplimentare referitoare la fiecare actor si productie cinematografica din cadrul SAG Awards.
        </p>
    </section>

    <section id="functions">
        <h3>Functiile Produsului</h3>
        <p>
        Mai jos sunt functionalitatile pe care aplicatia AGA le ofera:
        <ul>
        <li>Crearea unui cont si inregistrarea unui utilizator.</li>
        <li>Modificarea datelor asociate utilizatorului.</li>
        <li>Alegerea modului de afisare a informatiilor (3 modalitati).</li>
        <li>Vizualizarea detaliilor despre actori (scurta descriere despre film, box office-ul filmului, nota IMDb, un citat al actorului si alte nominalizari/premii ale actorului).</li>
	<li>Vizualizarea detaliilor despre productia cinematografica (preluate din baza de date sursa).</li>
        <li>Export in format csv, WebP si afisarea unor grafice in format svg.</li>
	<li>Suport pentru redarea de stiri privitoare la fiecare actor nominalizat.</li>
	<li>Functie de search.</li>
	<li>Functie de filtrare a informatiilor.</li>
        <li>Actualizarea in timp real a informatiilor.</li>
        </ul>
        </p>
    </section>
    <section id="operating-environment">
        <h3>Mediul de operare</h3>
        <p>
        Fiind o aplicatie web, ea va rula pe orice browser modern. Astfel aplcatia va putea rula pe toate sistemele de operare ce suporta un browser cum ar fi: Linux, Windows, Android, IOS etc.
        </p>
    </section>
	<section id="user-documentation">
        <h3>Documentatia utilizatorului</h3>
        <p>
        Solutia finala va fi insotita de un user documentatios/menu.
        </p>
    </section>
	<section id="assumptions-and-dependencies">
        <h3>Dependente</h3>
        <p>
        Va fi folosita baza The Movie Database (TMDb) pentru preluarea informatiilor suplimentare referitoare la fiecare actor si productie cinematografica. Va fi folosita o biblioteca pentru export de tip csv si svg. Va fi folosita biblioteca jQuery, pentru o manipulare facila si compactare a codului JavaScript.
        </p>
    </section>
</section>

<!--3.External Interface Requirements-->
<section id="interfaces">
    <h2>Interfete externe</h2>
    <section id="user-interface">
        <h3>Interfata pentru utlizator</h3>
        <p>
        Aplicatia ofera o interfata moderna, usor de folosit si atractiva vizual pentru a maximiza experienta utilizatorului. AGA poate fi folosita atat pe ecrane mari cat si pe browser-ul implicit al telefonului cu ecran de dimensiuni mai reduse, datorita design-ului responsive.
        </p>
        <p>
        Utilizatorul are posibilitatea sa modifice modul de afisare al informatiilor in 3 feluri in pagina de listare a informatiilor, pentru o experienta completa.
        </p>
    </section>

    <section id="software-interfaces">
        <h3>Interfete software</h3>
        <p>
        Baza de date folosita este MySQL pentru ca este flexibila si rapida, avand modul pentru intergrare usoara pentru limbajul de programare din partea de backend.
        </p>
        <p>
        Design-ul arhitectural al aplicatiei este MVC(Model - View - Controller), lucru ce va modulariza aplicatia si va facilita scrierea codului, fiind usor de intretinut, cu noi module care pot fi adaugate fara probleme. Pe server vor fi module separate de PHP (cod, backend) si HTML (frontend). Partea de Model este asociata partii de server a aplicatiei care preia date din baza de date. Partea de View este asociata tot server-ului, afisand HTML-ul si fisiere statice de JS, CSS, JPEG, sau altele. Partea de Controller este asociata partii proxy care face un request la server, preia datele necesare (oferite de microserviciul auth), le prelucreaza si returneaza pagina utilizatorului. Footer-ul si topbar-ul sunt separate de paginile care le vor folosi, iar modificare lor presupune o modificare overall a site-ului intrucat la incarcare sunt preluate si imbinate pentru a face o pagina intreaga.
        </p>
        <p>
        Frontend-ul este scris in HTML5, CSS3 si Javascript, folosind si libraria jQuery.
        </p>
        <p>
        Pentru backend se va folosi PHP. Aplicatia ruleaza pe un server web (http).
        </p>
    </section>

<!--4.System Features-->
<section id="system-features">
    <!-- review? -->
    <h2>Functionalitati</h2>
    <section id="login">
        <h3>Autentificarea</h3>
        <p>
        Autentificarea utilizatorului se realizeaza ca optiune in prima pagina si presupune completarea campurilor necesare (nume de utilizator sau email si parola), iar la apasarea de buton, in cazul in care datele sunt corecte, acesta sa fie redirectionat pe pagina principala. Autentificarea presupune existenta in prealabil a unui cont de utilizator.
        </p>
    </section>
    <section id="register">
        <h3>Inregistrarea</h3>
        <p>
        Creare unui nou cont de utlizator. Utilizatorul completeaza campurile necesare (nume de utlizator, email, parola, confirmare parola), iar la apasarea pe buton, in cazul in care informatiile sunt valide (nume de utlizator este nou, email-ul respecta formatul standard, parola corespunde), utilizatorul este redirectionat pe pagina de confirmare a adresei de email, dupa confirmare, un nou cont de utilizator este creat.
        </p>
    </section>

    <section id="top-subbreddits">
        <h3>Vizualizarea actorilor</h3>
        <p>
        Pe pagina principala, user-ul va avea access la o lista (un grid/ o lista cu detalii) cu Premiile SAGA, ordonate pe ani (in prealabil userul selecteaza anul), unde vor aparea actorii si filmele nominalizate in acel an. Apasand click pe actor sau film, va aparea o pagina de detalii (pentru fiecare).
        </p>
    </section>

<!--5.Other Nonfunctional Requirements-->
<section id="other">
    <h2>Security Requirements</h2>
    <p>
	Implementarea va recurge la tehnici de prevenire a atacurilor (precum Cross Site Scripting sau SQL injection).
    </p>
</section>

</body>
</html>
