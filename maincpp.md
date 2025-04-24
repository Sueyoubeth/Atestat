<h2 style="text-align:center;">Lucrare de Atestat</h2>
<h3 style="text-align:right;"> Ionescu Matei </h3>

# Introducere

  Buna ziua! Mă numesc Ionescu Matei, și în ceea ce urmează a fi lucrarea mea de atestat, vă voi prezenta **top 10 probleme** pe care le-am rezolvat pe parcursul evoluției mele la informatică.

## #1 Prisonbreak (ONI $2025$ clasele $11-12$)

### Enunț
Se dă un graf neorientat cu $N$ noduri, fiecarei muchii fiind asociat un cost. Fiecare nod are o culoare, reprezentată printr-un număr natural $\leq N$. $\newline$

### Cerință
Aflați lungimea minimă a unui drum de la nodul $1$ la nodul $N$ care conțime maxim două culori distincte, câte asemenea drumuri există și un drum posibil.

### Rezolvare

Trebuie doar să găsim acele perechi de culori pentru care știm sigur că avem drum minim între cele două noduri. Cum nu prea avem o idee validă prin care să facem asta, vom lua toate perechile legite de culori. Astfel, vrem să știm pentru care perechi $(a,b)$ există un drum de la $1$ la $N$ parcurgând doar culorile $a$ sau $b$. $\newline$

Fie $u$ un vecin al nodului $1$, și $v$ un vecin al nodului $u$ ($v \neq 1$). Atunci $(color_u, color_v)$ este o pereche legită. Ne mai trebuie doar perechile de noduri $(x,y)$ cu proprietatea că există un drum de la $1$ la $x$, unde nodurile au aceeași culoare cu $x$,  și $color_x \neq color_y$. Dacă sortam perechile deja selectate și eliminăm dublicatele (perechea $(a,b)$ = $(b,a)$ ), atunci o soluție care va lua punctaj maxim este: iterăm prin fiecare pereche; facem un algoritm de tip **dijkstra** din nodul $1$, calculând în același timp și câte drumuri avem. $\newline$

Algoritmul rulează în $O(N log(N) \cdot \text{nr perechi})$. După timpul de execuție al programului, deducem că numarul de perechi este maxim undeva la $100$ (poate sunt testele proaste).
<br>
<br>
## #2 Festival (ONI $2025$ clasele $11-12$)

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
O solutie care calculează dp-ul în $N^2$ ia undeva la $34$ puncte, iar pentru alte $34$ puncte vom expluata subtaskul unde $D = 10^9$ , de unde rezultă că restricția $|X_i - X_j| \leq D$ devine absolut irelevantă (diferența lor e oricum mai mica ca $10^9$). Și atunci ne rămâne doar ultima restricție. $\newline$.
* Cazul când $X_i > X_j$

$|X_i - X_j| \leq T_j - T_i \rightarrow X_i - X_j \leq T_i - T_j \rightarrow X_i - T_i \leq X_j - T_j$.

* Cazul când $X_i \leq  X_j$

$|X_i - X_j| \leq T_j - T_i \rightarrow X_j - X_i \leq T_i - T_j \rightarrow X_j +  T_j \leq X_i + T_i$.

În ambele cazuri, ne vom forma puncte de genul $(X_i, X_i - T_i)$, respectiv $(X_i, X_i + T_i)$. De unde deducem că punctul $j$ influențează punctul $i$ doar dacă este strict mai mic sau strict mai mare ca punctul $i$. (adică $X_j < X_i$ și $X_i - T_i \leq X_j - T_j$, sau invers) $\newline$
Atunci noi trebuie să suportăm update-uri de genul : "adaugă punctul $i$ cu un cost $x$", și queriuri de tipul: "află costul maxim dintr-o submatrice", lucruri care pot fi implementate folosind Arbori indexati binar 2D (sau **aib2D**). Pentru că memoria e prea mare, trebuie sa facem niște trucuri pentru a o reduce la $N \cdot log^2 N$.
<br>
<br>
## #3 F. Maximize Nor (Codeforces 1019 DIV2)

### Enunț

Se definește operația **nor** între două numere ca fiind operația **or** dar negată. Exemplu $2$ **nor** $6$ = $9$, dacă luăm în calcul doar primii $4$ biți.

### Cerință

Se dă un vector $v$ cu $N$ elemente și un număr $k$. Să se afle pentru fiecare poziție $i$ ( $1 \leq i \leq N$ ) care este suma maximă **nor** al unui inteval care acoperă complet poziția $i$, dacâ luăm în calcul doar primii $k$ biți din fiecare număr.

### Soluție

Pentru un bit anume, fie el $b$, contribuția acestuia asupra intervalului $[l,r]$ este dată de ultima poziție din intervalul ales care are bitul $b$ setat. 

*Exemplu operații **nor***
| **nor**  |  $0$  | $1$ | 
|--------|---------|-----|
| $0$ |  $1$ | $0$| 
| $1$ |  $0$ | $0$| 

Observăm că, indiferent de ce valoare are celălalt bit, dacă facem operația **nor** cu un bit setat, rezultatul este mereu 0. De aici rezultă faptul că, bitul $b$ va aparea în suma finală pentru intervalul $[l,r]$ dacă:
* bitul $b$ nu apare deloc : $l$ și $r$ au parități distincte
* ultima apariție a lui $b$ este difertă de $l$ : este un număr impar de $0$ - uri după ultimul $1$
* ultima apariție a lui $b$ este egală cu $l$ : $l$ și $r$ au aceeași paritate.

Observația crucială este că, dacă ne fixăm capătul drept al intervalulul, atunci suma **nor** pe toate intervalele cu capătul fixat în $r$ se schimbă de maxim $O(k)$ ori, fapt ce reiese din condițiile de existență de mai sus. Atunci, o soluție care va lua verdictul de **accepted** implică reținerea propriu-zisă a sumelor **nor** distincte asupra tuturor intervalelor care se termină pe poziția $i$, permițându-ne să actualizăm maximele cu ajutorul unui **arbore de intevale**. Soluția finală are o complexitate de aproximativ $O(N \cdot k log)$.
<br>
<br>
## #4 Nogcd (LOT Seniori 2017)

### Enunț

Se dă un graf conex cu $N$ noduri și $M$ muchii.

### Cerință

Trebuie să etichetăm fiecare muchie cu un număr natural de la $1$ la $M$, astfel încât muchiile să aibă etichete diferite, iar pentru fiecare nod cu grad mai mare ca $1$, **c.m.m.d.c**-ul etichetelor muchiilor incidente trebuie să fie $1$.

### Soluție

Voi nota  **c.m.m.d.c** cu **gcd**. Știm că $gcd(a,b)  \ \ = \ \  gcd(a,a-b)$, pentru oricare $a > b$, de unde deducem că $gcd(a + 1, a) \ \ = \ \  gcd(a + 1, 1)  \ \ =  \ \ 1$. Acest lucru ne motivează să punem cât mai multe etichete consecutive cu putință. Putem să realizăm asta cu un simplu **dfs**.

### Code c++
```cpp
void solve(){
    int n, m;
    cin >> n >> m;
    vector<vector<int>> liste(n + 1);
    map<pair<int,int>, int>mp;

    for(int i = 1; i <= m; i++){
        int u,v;
        cin >> u >> v;
        liste[u].push_back(v);
        liste[v].push_back(u);
    }
    
    vector<int> ans;
    int cnt = 0;
    vector<int> viz(n + 1);
    function<void(int)> dfs = [&](int nod){
        viz[nod] = 1;
        for(auto i : liste[nod]){
            if(mp[{min(i,nod), max(i,nod)}] == 0){
                mp[{min(i,nod),max(i,nod)}] = ++cnt;
            }
            if(viz[i] == 0){
                dfs(i);;
            }
        }
    };

    dfs(1);
    for(int i = 1; i <= n; i++){
        for(auto j : liste[i]){
            if(i < j) cout << i << " " << j << " " << mp[{min(i,j),max(i,j)}] << '\n';
        }
    }
}
```
<br>
<br>

## #5 Sonde (LOT Seniori 2023)

### Enunț
Se dă un vector $v$ cu $N$ elemente și un număr $K$. Toate elementele sunt **distincte**

### Cerință

Aflați mulțimea de elemente care sunt median pentru minim un interval de lungime **impară** și mai mare sau egală cu $K$.

### Soluție

Dacă vom nota cu $-1$ elementele care sunt mai mici decât $v_i$, iar cu $1$ elementele care sunt mai mari decât $v_i$, atunci problema se reduce la: „află dacă există un interval care acoperă poziția $i$ și are suma $0$”. În general, problema astfel obținută este destul de dificil de rezolvat pe restricțiile impuse, însă avantajul nostru constă în faptul că valorile sunt fie $-1$, fie $1$.

Dacă notăm $sp_i$ ca fiind suma primelor $i$ elemente, atunci $|sp_i - sp_{i-1}| \leq 1$, iar asta implică faptul că, între două poziții $i$ și $j$, pentru orice $c \in [sp_i, sp_j]$, există o poziție $p$ pentru care $sp_p = c$. Pentru a verifica dacă există $l < i \leq r$ cu $sp_l = sp_r$, vom lua intervalele $[min(sp_0, \dots , sp_{i-1}), max(sp_0, \dots, sp_{i-1})]$ și $[min(sp_i, \dots , sp_N), max(sp_i, \dots, sp_N)]$ și vom verifica dacă se intersectează. Iar dacă nu uităm să tratăm și restricția referitoare la lungimea intervalului, obținem o soluție în $O(N \cdot k \log)$ cu ajutorul unor arbori de intervale, care se poate reduce la $O(N \cdot k + N \cdot \log)$.
<br>
<br>
## #6 Almost Video Game III (IIOT 2024-2025 Round II)
### Enunț

Într-un joc video sunt $N$ monștri care trebuie eliminați folosind $K$ abilități. Runda $i$ constă în selectarea unei mulțimi de abilități neactivate și activarea lor, urmând să eliminăm monstrul $i$ cu abilitățile selectate. La final, costul jocului este $b^2$, unde $b$ este numărul de monștri eliminați folosind toate cele $K$ abilități.

### Cerință

Să se calculeze suma costurilor pe toate jocurile posibile distincte.

### Soluție

Dacă costul unui joc era doar $b$, unde $b$ este numărul de monștri eliminați cu toate cele $K$ abilități, atunci putem să vedem contribuția fiecărui monstru în parte și să le adunăm. În acest caz, contribuția monstrului $i$ este $i^k$, rezultatul fiind $\displaystyle \\sum_{i = 1}^{N} i^k$.

Vom defini $dp_{i,j}$ ca fiind egal cu suma jocurilor dacă luăm în calcul doar primii $i$ monștri, iar răspunsul este calculat la puterea $j$. Evident, $dp_{i,1} = dp_{i-1,1} + i^k$. Observăm însă că $dp_{i,2}$ se poate scrie în felul următor:

$$ 1^2 \\cdot (i^k - (i - 1)^k) + 2 \\cdot ((i-1)^k - (i-2)^k) + \\dots + i^2 \\cdot 1^k $$

iar când facem tranziția la $i + 1$:

$$ (1 + 1)^2 \\cdot (i^k - (i - 1)^k) + (2 + 1)  \\cdot ((i-1)^k - (i-2)^k) + \\dots + (j + 1)^2 \\cdot ((i - j + 1)^k - (i - j)^k) + \\dots + (i + 1) \\cdot 1^k $$

Desfacem parantezele și obținem:

$$ \\displaystyle \\sum_{j = 1}^{i} (i - j + 1)^2 \\cdot (j^k - (j-1)^k) + 2 \\cdot (i - j + 1) \\cdot (j^k - (j-1)^k) + (j^k - (j-1)^k) $$

care este efectiv $dp_{i-1,2} + 2 \\cdot dp_{i-1,1} + (i - 1)^k$, iar când adăugăm monstrul $i$, obținem $dp_{i-1,2} + 2 \\cdot dp_{i-1,1} + i^k$. Deci $dp_{i,2} = dp_{i-1,2} + 2 \\cdot dp_{i-1,1} + i^k$.
Dacă desfacem $dp_{n,2}$ vedem că este egal cu suma: $2n \\cdot \\displaystyle \\sum_{i=1}^{N} i^k - \\displaystyle \\sum_{i = 1}^{N} i^{k+1}$, care este de fapt și rezultatul nostru.

Pentru a calcula sumele, datorită faptului că $N$ este prea mare, ne vom folosi de polinomul de interpolare al lui **Lagrange**, care poate să afle valoarea unui polinom în oricare punct (dacă știm valorile polinomului evaluate în $k + 1$ puncte, unde $k$ este gradul polinomului).

### Cum interpolăm $i^k$ în $O(K \\cdot \\log)$

```cpp
Mint interpolate(int k, ll n){
    vector<Mint> f1(k + 3,0);

    for(int i = 1; i <= k + 2; i++){
        f1[i] = f1[i-1] + fp(i,k);
    }
    if(n <= k + 2) return f1[n];

    Mint ans = 0;
    
    vector<Mint> p1(k + 3), p2(k + 3), f(k + 3), rf(k + 3);
    p1[0] = n-0;
    f[0] = 1;
    rf[0] = 1;
    for(int i = 1; i <= k + 2; i++){
        p1[i] = p1[i-1] * (n - i);
        f[i] = f[i-1] * i;
        rf[i] = rf[i-1] * (mod - i);
    } 
    p2[k + 2] = (n - k - 2);
    for(int i = k + 1; i >= 0; i--){
        p2[i] = p2[i + 1] * (n - i);
    }
    for(int i = 0; i <= k + 2; i++){
        Mint sus = (i == 0 ? 1 : p1[i-1]) * (i == k + 2 ? 1 : p2[i + 1]);
        Mint jos = f[i] * rf[k + 2 - i];
        ans = ans + f1[i] * sus  / jos;
    }
    return ans;
}
```

Soluția finală:

```cpp
void solve(){
    ll n, k;
    cin >> n >> k >> mod;

    Mint S1 = interpolate(k, n);
    Mint S2 = interpolate(k + 1, n);

    cout << S1 + S1 * n * 2 - S2 * 2;
}
```




