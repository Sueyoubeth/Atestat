<h2 style="text-align:center;">Lucrare de Atestat</h2>
<h3 style="text-align:right;"> Ionescu Matei </h3>

# Introducere

Buna ziua! Mă numesc Ionescu Matei, și în ceea ce urmează a fi lucrarea mea de atestat, vă voi prezenta **top 10 probleme** pe care le-am rezolvat pe parcursul evoluției mele la informatică.

# Prisonbreak (ONI $2025$ clasele $11-12$)

### Enunț
Se dă un graf neorientat cu $N$ noduri, fiecarei muchii fiind asociat un cost. Fiecare nod are o culoare, reprezentată printr-un număr natural $\leq N$. $\newline$

### Cerință
Aflați lungimea minimă a unui drum de la nodul $1$ la nodul $N$ care conțime maxim două culori distincte, cate asemenea drumuri exista și un drum posibil.

### Rezolvare

Trebuie doar să găsim acele perechi de culori pentru care știm sigur că avem drum minim între cele două noduri. Cum nu prea avem o idee validă prin care să facem asta, vom lua toate perechile legite de culori. Astfel, vrem să știm pentru care perechi $(a,b)$ există un drum de la $1$ la $N$ parcurgând doar culorile $a$ sau $b$. $\newline$

Fie $u$ un vecin al nodului $1$, și $v$ un vecin al nodului $u$ ($v \neq 1$). Atunci $(color_u, color_v)$ este o pereche legită. Ne mai trebuie doar perechile de noduri $(x,y)$ cu proprietatea că există un drum de la $1$ la $x$, unde nodurile au aceeași culoare cu $x$,  și $color_x \neq color_y$. Dacă sortam perechile deja selectate și eliminăm dublicatele (perechea $(a,b)$ = $(b,a)$ ), atunci o soluție care va lua punctaj maxim este: iterăm prin fiecare pereche; facem un algoritm de tip **dijkstra** din nodul $1$, calculând în același timp și câte drumuri avem. $\newline$

Algoritmul rulează în $O(N log(N) \cdot \text{nr perechi})$. După timpul de execuție al programului, deducem că numarul de perechi este maxim undeva la $100$ (poate sunt testele proaste).

# Festival (ONI $2025$ clasele $11-12$)

### Enunț

Se dau $N$ festivale. Al $i$ -lea începe de la momentul $T_i$, de pe poziția $X_i$ și îți aduce o satisfacție de $S_i$. 

### Cerință

Știind că poți începe de la oricare festival, să se calculeze satisfacția totală maximă dacă putem să ne mutăm de la festivalul $i$ la festivalul $j$, cu condiția că: $\newline$
* $T_i \leq T_j$
* $|X_i - X_j| \leq D$
* $|X_i - X_j| \leq T_j - T_i$

unde $D$ face parte din input.

### Rezolvare ($71$ puncte)

Vom sorta festivalele după timp și vom face **dp** (programare dinamică). Avem: $dp_i = \displaystyle MAX_{j = 0}^{i-1} dp_j + S_i$ , asta dacă cele două festivale îndeplinesc condițiile date mai sus. $\newline$
O solutie care calculează dp-ul în $N^2$ ia undeva la $34$ puncte, iar pentru alte $34$ puncte vom expluata subtaskul unde $D = 10^9$.$\newline$
$D = 10^9$, de unde rezultă că restricția $|X_i - X_j| \leq D$ devine absolut irelevantă (diferența lor e oricum mai mica ca $10^9$). Și atunci ne rămâne doar ultima restricție. $\newline$.
* $X_i > X_j$

$|X_i - X_j| \leq T_j - T_i \rightarrow X_i - X_j \leq T_i - T_j \rightarrow X_i - T_i \leq X_j - T_j$.

*  $X_i \leq  X_j$

$|X_i - X_j| \leq T_j - T_i \rightarrow X_j - X_i \leq T_i - T_j \rightarrow X_j +  T_j \leq X_i + T_i$.

În ambele cazuri, ne vom forma puncte de genul $(X_i, X_i - T_i)$, respectiv $(X_i, X_i + T_i)$. De unde deducem că punctul $j$ influențează punctul $i$ doar dacă este strict mai mic sau strict mai mare ca punctul $i$. (adică $X_j < X_i$ și $X_i - T_i \leq X_j - T_j$, sau invers) $\newline$
Atunci noi trebuie să suportăm update-uri de genul : "adaugă punctul $i$ cu un cost $x$", și queriuri de tipul: "află costul maxim dintr-o submatrice", lucruri care pot fi implementate folosind Arbori indexati binar 2D (sau **aib2D**). Pentru că memoria e prea mare, trebuie sa facem niște trucuri pentru a o reduce la $N \cdot log^2 N$.


