# Solucions Final OICat 2023

## Taula de continguts
* [Problema C1. Comandament de televisió](#C1)
* [Problema Q1. Nombres molt divisibles](#Q1)
* [Problema G1. Silueta urbana](#G1)
* [Problema C2. Dues paraules](#C2)
* [Problema G2. Pedra, paper, tisores](#G2)
* [Problema Q2. Moda del triangle de Pascal](#Q2)
* [Problema C3. Nombres de Hamming](#C3)
* [Problema Q3. Conjectura d'Euler](#Q3)
* [Problema G3. Mastermind (2)](#G3)
* [Problema C4. Selectivitat](#C4)
* [Problema Q4. Amazones pacífiques](#Q4)
* [Problema C5. Fent els nombres iguals](#C5)
* [Problema C6. Interval cíclic](#C6)



## [Problema C1. Comandament de televisió](https://jutge.org/problems/P12029_ca)

Observeu que en una solució òptima només s'utilitza un dels dos botons (perquè prémer una vegada cada botó és equivalent a no prémer res).

Si escollim el botó que baixa el volum, l'haurem de prémer tantes vegades com el residu de dividir $x$ per $k$ (`x % k` en la majoria de llenguatges de programació). Si escollim el botó que puja el volum, l'haurem de prémer `k - (x % k)` vegades. Per tant, la solució serà el mínim d'aquests dos valors.

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    int x, k;
    while (cin >> x >> k) {
        int residu = x % k;
        cout << min(residu, k - residu) << endl;
    }
}
```

</details>

<details>
    <summary><b>Codi (Python3)</b></summary>

```python
from easyinput import read
x, k = read(int, int)
while x is not None:
    residu = x % k
    print(min(residu, k - residu))
    x, k = read(int,int) # llegim l'entrada per a la propera iteració del bucle.
```
</details>

## [Problema Q1. Nombres molt divisibles](https://jutge.org/problems/P89047_ca) <a name="Q1"/>

Per resoldre el problema, iterem per tots els nombres de 1 fins a 2023, calculant per cadascun la suma dels dígits i la suma dels quadrats dels dígits, i comprovant si divideixen al nombre original. Podem declarar un comptador a fora del bucle i sumar-li 1 cada cop que trobem una solució vàlida. Al final del bucle, el valor del comptador es correspondrà al nombre de solucions.

Per calcular la suma dels dígits es pot convertir l'enter a una 'string' (a C++ es faria amb `string s = to_string(x);`) o alternativament podem anar dividint el nombre per 10 fins que arribi a 0, tal i com es fa al següent codi:

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int solucio = 0;
    for (int x = 1; x <= 2023; ++x) {
        int suma = 0;
        int suma_quadrats = 0;
        int y = x; // copiem x en una altra variable per poder-lo modificar sense afectar al bucle 'for'.
        while (y != 0) {
            int digit = y % 10; // dígit de les unitats.
            y /= 10; // eliminem l'últim dígit.
            suma += digit;
            suma_quadrats += digit*digit;
        }
        if (x % suma == 0 and x % suma_quadrats == 0) {
            solucio++;  // hem trobat una solució vàlida.
        }
    }
    cout << "La solució és: " << solucio << endl;
}
```
</details>

## [Problema G1. Silueta urbana](https://jutge.org/problems/P90013_ca) <a name="G1"></a>

Per resoldre aquest problema ens és suficient amb trobar, depenent de per quin costat estem mirant, el màxim element de cada fila o columna, i a continuació dibuixar les columnes corresponents en l'ordre adequat, com sempre anant amb compte de no posar una columna o fila de píxels de més.

Per llegir una llista `v` en ordre invers, ens podem ajudar de l'objecte `range()`, com podreu veure al codi solució, però també podem formar la llista revertida fent `reversed(v)` o, equivalentment, `v[::-1]`.

Al codi solució també us ensenyem una manera de llegir una matriu en una sola línia de codi, amb una explicació una mica més detallada del que està fent.

<details>
    <summary><b>Codi</b></summary>

```python
from PIL import Image, ImageDraw

# Llegim per quin costat mirem, i les dimensions del quadrat
costat = input()
n = int(input())

"""
mapa = []
for i in range(n):
    # Llegim la fila, com a string
    fila = input()

    # Obtenim la llista d'elements que la composen (separant-la per espais)            
    fila = fila.split()

    # map(int, fila) converteix cada element de la llista (encara strings),
    # en el valor enter corresponent, i amb list() creem la llista
    # formada per aquests
    fila = list(map(int, fila))

    # Afegim la fila a la llista
    mapa.append(fila)
"""

# Llegim els valors. Aquesta línia és equivalent al comentari anterior
mapa = [list(map(int, input().split())) for i in range(n)]

# Retorna l'element més gran de la i-èssima fila
def max_fila(i):
    return max(mapa[i][j] for j in range(n))

# Retorna l'element més gran de la j-èssima columna
def max_columna(j):
    return max(mapa[i][j] for i in range(n))

# silueta[i] serà l'altura de la i-èssima columna de la silueta
silueta = None
if costat == 'S':
    silueta = [max_columna(j) for j in range(n)]
if costat == 'N':
    silueta = [max_columna(j) for j in range(n - 1, -1, -1)]
if costat == 'O':
    silueta = [max_fila(i) for i in range(n)]
if costat == 'E':
    silueta = [max_fila(i) for i in range(n - 1, -1, -1)]

# Creem la imatge
max_altura = max(silueta)
altura_imatge = 100*max_altura + 50
img = Image.new('RGB', [100*n, altura_imatge], 'LightBlue')
dib = ImageDraw.Draw(img)

# Dibuixem les columnes
for i, x in enumerate(silueta):
    dib.polygon(
        [
            (100*i, altura_imatge - 1),
            (100*i, altura_imatge - 100*silueta[i]),
            (100*i + 99, altura_imatge - 100*silueta[i]),
            (100*i + 99, altura_imatge - 1),
        ],
        'DimGray'
    )

# Guardem la imatge
img.save('output.png')
```
</details>

## [Problema C2. Dues paraules](https://jutge.org/problems/P71106_ca) <a name="C2"></a>

Donades dues strings $s$ i $t$, volem veure si podem transformar $s$ en $t$ eliminant algunes de les lletres de $s$. Observeu que amb les eliminacions no podem canviar l'ordre relatiu de les lletres, així que la resposta serà `SI` si tots els caràcters de $t$ apareixen en el mateix ordre a $s$, possiblement amb alguns caràcters extra pel mig.

Per resoldre el problema, iterem pels caràcters de $s$, guardant-nos l'índex $j$ del primer caràcter de $t$ que encara no hem trobat en el tros de $s$ que portem vist (a l'inici, $j = 0$). En el pas $i$-èssim, quan considerem el caràcter $s_i$ de $s$, tenim que si $s_i \neq t_j$ llavors l'haurem d'eliminar per força (ja que no el podem emparellar amb cap caràcter posterior de $t$, perquè hem de conservar l'ordre relatiu). Per contra, si $s_i = t_j$, podem emparellar els dos caràcters i augmentar en 1 l'índex $j$.

Al final, obtindrem una solució vàlida si $j$ és igual a la mida de $t$ (ja que això indica que hem emparellat tots els caràcters de $t$).

<details>
    <summary><b>Codi (C++)</b></summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    string s, t;
    while (cin >> s >> t) {
        //j es l'iterador de t (paraula "curta")
        //i es l'iterador de s (paraula "llarga")
        int j = 0;
        for (int i = 0; i < s.size(); ++i) {
            //quan la lletra s[i] coincideix amb t[j], suposem que hem eliminat
            //les lletres des de l'ultima que coincidia fins a l'actual
            if (t[j] == s[i]) {
                //indiquem que ja hem trobat la lletra t[j]
                ++j;
                //si hem recorregut la paraula t sencera ja hem acabat
                if (j == t.size()) break;
            }
        }
        //hem aconseguit transformar s en t si i nomes si hem aconseguit trobar totes les lletres
        //de t en ordre dintre s (i per tant l'iterador j ha arribat al final de la paraula)
        if (j == t.size()) cout << "SI" << endl;
        else cout << "NO" << endl;
    }
}
```
</details>

## [Problema G2. Pedra, paper, tisores](https://jutge.org/problems/P80812_ca) <a name="G2"></a>

x

<details>
    <summary><b>Codi</b></summary>

```python
```
</details>

## [Problema Q2. Moda del triangle de Pascal](https://jutge.org/problems/P57403_ca) <a name="Q2"></a>

x

<details>
    <summary><b>Codi (C++)</b></summary>

```cpp
#include<iostream>
#include<vector>
using namespace std;

using ll = long long;

int main() {
    int N = 10000;
    ll LIMIT_OVERFLOW = 1e14; // si cap dels resultats intermitjos no es passen d'aquest nombre, sabem que podem multiplicar per qualsevol nombre de 1 fins a 10^4 sense passar-nos del valor màxim d'un long long (al voltant de 4*10^18).
    vector<int> ocurrencies(N+1, 0);

    for(int n = 0; n <= N; ++n) {
        for(int k = 0; k <= n/2; ++k) {
            ll coef = 1;
            for(int i = 0; i < k; ++i) {
                coef *= (n-i);
                if(coef > LIMIT_OVERFLOW) 
                    cerr << "ATENCIÓ: pot haver-hi overflow!" << endl;
            }
            for(int i = 1; i <= k; ++i) {
                coef /= i;
            }

            if(coef > N) break;
            if(2*k == n)
                ocurrencies[coef] += 1;
            else
                ocurrencies[coef] += 2;
        }
    }

    int solucio = 0;
    for(int i = 2; i <= N; ++i) {
        if(ocurrencies[i] > ocurrencies[solucio])
            solucio = i;
    }
    cout << "El nombre que apareix més vegades és el " << solucio << ", que apareix " << ocurrencies[solucio] << " vegades.\n";
}
```
</details>

## [Problema C3. Nombres de Hamming](https://jutge.org/problems/P24627_ca) <a name="C3"></a>

Una possible manera de calcular els nombres de Hamming és utilitzant una *cua de prioritats*.

Una cua de prioritats és una estructura de dades que permet emmagatzemar una col·lecció d'elements amb les operacions d'afegir un nou element, consultar l'element més petit, i esborrar l'element més petit. Usualment les cues de prioritats s'implementen amb *heaps* i totes aquestes operacions tenen cost logarítmic respecte el nombre d'elements a la cua. 

Per calcular els nombres de Hamming, es comença amb una cua de prioritats `pq` que només conté l'1, el primer nombre de Hamming. Després, es fa el següent de forma iterativa: s'extreu l'element mínim `x` de `pq` i, si aquest no era el mateix que el darrer que s'ha extret, `x` és el següent nombre de Hamming en ordre creixent i, llavors, s'afegeixen els elements `2*x`, `3*x` i `5*x` a `pq`, ja que tots ells també han de ser nombres de Hamming. Aquest procés es repeteix fins trobar l'`n`-èsim nombre de Hamming.

En Python, les cues de prioritat es troben al mòdul `heapq`, documentat a https://docs.python.org/3/library/heapq.html. EL programa següent implementa l'algorisme descrit:

<details>
    <summary><b>Codi (Python3)</b></summary>

```python
from yogi import *
from heapq import *

n = read(int)   # entrada
h = 0           # últim nombre de Hamming (valor especial per començar)
pq = [1]        # cua de prioritats que només conté un 1
c = 0           # comptador de nombres de Hamming descoberts
while c < n:
    x = heappop(pq)
    if x > h:
        h = x
        c += 1
        heappush(pq, 2 * x)
        heappush(pq, 3 * x)
        heappush(pq, 5 * x)
print(x)
```
</details>

## [Problema Q3. Conjectura d'Euler](https://jutge.org/problems/P80756_ca) <a name="Q3"></a>

x

<details>
    <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ll = long long;

// Calcula b^e.
ll potencia(ll b, ll e) {
    ll ans = 1;
    for(int i = 0; i < e; ++i) {
        ans *= b;
    }
    return ans;
}

// Troba solucions enteres positives a:
// a_1^k + ... + a_n^k = b^k,   on 3 <= k <= 5,  2 <= n < k,  1 <= a_i, b <= N.
int main() {
    int N = 250; // Màxim valor d'un dels a_i.
    int max_pow = 5; // Màxim valor de l'exponent.
    for(int k = 3; k <= max_pow; ++k) {
        for(int n = 2; n < k; ++n) {
            // Busquem solucions enteres positives a:
            // a_1^k + ... + a_n^k = b^k
            vector<int> a(n);

            // Comprova si el vector 'a' actual es una soluciós correcta. 
            // En cas que sí, l'escriu per la terminal.
            function<void()> comprova = [&](){
                ll sum = 0;
                for(int i = 0; i < n; ++i) {
                    sum += potencia(a[i], k);
                }
                if(potencia(N, k) < sum) return;

                // Fem cerca binària per trobar l'arrel k-èssima de la suma de potències.
                int esq = 0; // Invariant: esq < sum^(1/k)
                int dre = N; // Invariant: dre >= sum^(1/k)
                while(dre - esq > 1) {
                    int mid = (esq + dre) / 2;
                    if(potencia(mid, k) < sum) esq = mid;
                    else dre = mid;
                }

                // Si és una solució vàlida, l'escrivim per pantalla.
                if(potencia(dre, k) == sum) {
                    cout << "Solució: ";
                    for(int i = 0; i < n; ++i) {
                        cout << a[i] << "^" << k;
                        if(i != n-1) cout << " + ";
                        else cout << " = " << dre << "^" << k << " = " << sum << endl;
                    }
                }
            };

            // Omple el vector 'a' a partir de la posició 'pos', de manera no decreixent (és a dir, a[i] <= a[i+1] per tot i).
            function<void(int)> genera_totes_possibilitats = [&](int pos) {
                if(pos == n) return comprova();
                int last = pos == 0? 1 : a[pos-1];
                for(int i = last; i <= N; ++i) {
                    a[pos] = i;
                    genera_totes_possibilitats(pos + 1);
                }
            };

            genera_totes_possibilitats(0);
        }
    }
}
```
</details>

## [Problema G3. Mastermind (2)](https://jutge.org/problems/P36841_ca) <a name="G3"></a>

Amb les restriccions donades, n'hi ha prou de fer un programa de força bruta que consideri totes les possibles configuracions, i comprovar per a cadascuna si és consistent amb les respostes a les conjectures. Fins
i tot, es poden generar les $4^6$ configuracions amb quatre bucles anidats.

Per a cada configuració, el codi primer comprova les pistes negres i després les pistes blanques, ignorant les posicions que s'hagin fet servir prèviament per a les pistes negres. Si les $2n$ respostes són totes coherents, la variable `ok` queda a `True` i hem trobat la solució. El programa ara podria parar de buscar solucions perquè l'enunciat ens garanteix que és única, però no cal perquè el Jutge doni un AC.

<details>
    <summary><b>Codi</b></summary>

```python
from PIL import Image, ImageDraw

M = 200
cols = [ 'B', 'G', 'O', 'P', 'R', 'Y' ]


def col(c):
     if c == 'B': return 'Blue'
     if c == 'G': return 'Green'
     if c == 'O': return 'Orange'
     if c == 'P': return 'Pink'
     if c == 'R': return 'Red'
     return 'Yellow'


n = int(input())
mat = []
negre = []
blanc = []
for i in range(n):
     s = input()
     mat += [s]
     neg = int(input())
     bla = int(input())
     negre += [neg]
     blanc += [bla]

solu = []
for a in cols:
     for b in cols:
         for c in cols:
             for d in cols:
                 ok = True
                 V = [ a, b, c, d ]
                 for i in range(n):
                     q = 0
                     usat = [False]*4
                     for j in range(4):
                         if mat[i][j] == V[j]:
                             q += 1
                             usat[j] = True
                     if q != negre[i]:
                         ok = False

                     q = 0
                     for j in range(4):
                         if mat[i][j] != V[j]:
                             k = 0
                             while k < 4 and (usat[k] or mat[i][j] != V[k]):
                                 k += 1
                             if k < 4:
                                 q += 1
                                 usat[k] = True
                     if q != blanc[i]:
                         ok = False

                 if ok:
                     solu = V

img = Image.new('RGB', (4*M, M), 'Sienna')
dib = ImageDraw.Draw(img)

for j in range(4):
     dib.ellipse([M*j, 0, M*(j + 1) - 1, M - 1], col(solu[j]))

img.save("output.png")
```
</details>

## [Problema C4. Selectivitat](https://jutge.org/problems/P50335_ca) <a name="C4"></a>

Per resoldre aquest problema cal descompondre adientment les dades i les operacions que cal realitzar sobre elles. A continuació es descriu una solució al problema utilitzant Python amb tipus.

El propi enunciat ja presenta les entitats més importants del domini: els estudiants i les titulacions. Per tant, podríem definir estructures per elles:

```python
@dataclass
class Estudiant:
    nom: str
    dni: int
    nota: float
    preferencies: list[str]
    assignacio: Optional[str]

@dataclass
class Titulacio:
    nom: str
    places: int
    nota_de_tall: float
```

Fixeu-vos que el camp `assignacio`  d'`Estudiant` contindrà el valor especial `None` quan no tingui titulació assignada i valdrà el nom de la titulació quan ja la tingui assignada.

Amb aquestes dades definides, podem descompondre el programa principal seguint les accions que determina l'enunciat, usant llistes d'estudiants i llistes de titulacions per emmagatzemar les dades necessàries:

```python
def main() -> None:
    estudiants = llegir_estudiants()
    titulacions = llegir_titulacions()
    assignar(estudiants, titulacions)
    escriure_assignacio(estudiants)
    escriure_notes_de_tall(titulacions)
```

Les operacions de lectura són senzilles:

```python
def llegir_estudiant() -> Estudiant:
    return Estudiant(read(str), read(int), read(float), read(str).split(','), None)

def llegir_estudiants() -> list[Estudiant]:
    return [llegir_estudiant() for _ in range(read(int))]

def llegir_titulacio() -> Titulacio:
    return Titulacio(read(str), read(int), 0.0)

def llegir_titulacions() -> list[Titulacio]:
    return [llegir_titulacio() for _ in range(read(int))]
```

La feina grossa recau en la funció `assignar`, que modifica la llista d'estudiants que rep per assignar-los una titulació i que modifica la llista de titulacions que rep per mantenir el seu nombre de places i nota de tall . Tot això, evidentment, d'acord amb les regles d'assignació descrites a l'enunciat.

```python
def assignar(estudiants: list[Estudiant], titulacions: list[Titulacio]) -> None:
    estudiants.sort(key=lambda e: (-e.nota, e.dni))
    dic = {titulacio.nom: titulacio for titulacio in titulacions}
    for estudiant in estudiants:
        for preferencia in estudiant.preferencies:
            titulacio = dic[preferencia]
            if titulacio.places > 0:
                estudiant.assignacio = preferencia
                titulacio.places -= 1
                titulacio.nota_de_tall = estudiant.nota
                break  
```

Fixeu-vos que a la funció anterior, la llista d'estudiants s'ordena per notes decreixents (i DNI en cas d'empat) per tal de tractar els estudiants en l'ordre prescrit. Això s'aconsegueix especificant aquests dos camps en una tupla al paràmetre `key` del mètode `sort`. A la nota se li canvia el signe, per ordenar de gran a petit.

Fixeu-vos també que, per tal de poder trobar ràpidament una titulació a través del seu nom, es crea un diccionari `dic` que vincula noms i dades. D'aquesta forma, la cerca de cada titulació a la llista de titulacions triga temps constant en mitjana, enlloc de temps lineal respecte el nombre de titulacions.

La resta de detalls d'aquesta funció són senzills.

Ara ja només queda fer les funcions que escriuen la sortida, i ambdues són fàcils de fer, utilitzant de nou les funcions d'ordenació de llistes tot especificant els camps a usar per ordenar:

```python
def escriure_assignacio(estudiants: list[Estudiant]) -> None:
    print('---')
    estudiants.sort(key=lambda e: e.dni)
    for estudiant in estudiants:
        print(estudiant.dni, estudiant.nom, estudiant.assignacio)


def escriure_notes_de_tall(titulacions: list[Titulacio]) -> None:
    print('---')
    titulacions.sort(key=lambda e: (-e.nota_de_tall, e.nom))
    for titulacio in titulacions:
        nota_arrodonida = "{:.5f}".format(titulacio.nota_de_tall)
        print(titulacio.nom, nota_arrodonida, titulacio.places)
```


El programa complet és el següent:

<details>
    <summary><b>Codi (Python3)</b></summary>

```python
from yogi import read
from dataclasses import dataclass
from typing import Optional


@dataclass
class Estudiant:
    nom: str
    dni: int
    nota: float
    preferencies: list[str]
    assignacio: Optional[str]


def llegir_estudiant() -> Estudiant:
    return Estudiant(read(str), read(int), read(float), read(str).split(','), None)


def llegir_estudiants() -> list[Estudiant]:
    return [llegir_estudiant() for _ in range(read(int))]


@dataclass
class Titulacio:
    nom: str
    places: int
    nota_de_tall: float


def llegir_titulacio() -> Titulacio:
    return Titulacio(read(str), read(int), 0.0)


def llegir_titulacions() -> list[Titulacio]:
    return [llegir_titulacio() for _ in range(read(int))]


def assignar(estudiants: list[Estudiant], titulacions: list[Titulacio]) -> None:
    dic = {titulacio.nom: titulacio for titulacio in titulacions}
    estudiants.sort(key=lambda e: (-e.nota, e.dni))
    for estudiant in estudiants:
        for preferencia in estudiant.preferencies:
            titulacio = dic[preferencia]
            if titulacio.places > 0:
                estudiant.assignacio = preferencia
                titulacio.places -= 1
                titulacio.nota_de_tall = estudiant.nota
                break   


def escriure_assignacio(estudiants: list[Estudiant]) -> None:
    print('---')
    estudiants.sort(key=lambda e: e.dni)
    for estudiant in estudiants:
        print(estudiant.dni, estudiant.nom, estudiant.assignacio)


def escriure_notes_de_tall(titulacions: list[Titulacio]) -> None:
    print('---')
    titulacions.sort(key=lambda e: (-e.nota_de_tall, e.nom))
    for titulacio in titulacions:
        nota_arrodonida = "{:.5f}".format(titulacio.nota_de_tall)
        print(titulacio.nom, nota_arrodonida, titulacio.places)


def main() -> None:
    estudiants = llegir_estudiants()
    titulacions = llegir_titulacions()
    assignar(estudiants, titulacions)
    escriure_assignacio(estudiants)
    escriure_notes_de_tall(titulacions)


if __name__ == '__main__':
    main()
```
</details>

## [Problema Q4. Amazones pacífiques](https://jutge.org/problems/P61195_ca) <a name="Q4"></a>

La solució esperada és un backtracking que calculi el nombre demanat. No cal fer cap optimització especialment astuta, com ara aprofitar simetries, perquè el programa només triga uns pocs segons.

<details>
    <summary><b>Codi (C++)</b></summary>

```cpp
 #include <iostream>
#include <vector>
using namespace std;

using VB = vector<bool>;
using VVB = vector<VB>;
using VI = vector<int>;

const VI inc_x = { -1, -1, -2, -2 };
const VI inc_y = {  2, -2,  1, -1 };

const int n = 16;
VB colu(n, false); // columnes ocupades
VB dia1(2*n - 1, false); // diagonals ocupades
VB dia2(2*n - 1, false); // idem, en l'altra direcció
VVB M(n, VB(n, false)); // posicions ocupades del tauler

// Comprova que les quatre caselles a sobre de l'actual
// que estan a distància de cavall no continguin cap amazona.
bool ok(int i, int j) {
   for (int d = 0; d < 4; ++d) {
     int x = i + inc_x[d];
     int y = j + inc_y[d];
     if (x >= 0 and y >= 0 and y < n and M[x][y]) return false;
   }
   return true;
}

int f(int i) {
   if (i == n) return 1;

   int res = 0;
   // Intentem posar l'amazona de la fila i a totes les columnes j.
   for (int j = 0; j < n; ++j) {
     int d1 = i + j;
     int d2 = i - j + n - 1;
     if (not colu[j] and not dia1[d1] and not dia2[d2] and ok(i, j)) {
       colu[j] = dia1[d1] = dia2[d2] = M[i][j] = true;
       res += f(i + 1);
       colu[j] = dia1[d1] = dia2[d2] = M[i][j] = false;
     }
   }
   return res;
}

int main() {
   cout << f(0) << endl;
}
```
</details>

## [Problema C5. Fent els nombres iguals](https://jutge.org/problems/P54072_ca) <a name="C5"></a>

La clau d'aquest problema és veure que la suma i el màxim comú divisor de tots els nombres no canvia quan fem un moviment. En el cas de la suma, és fàcil veure-ho, ja que esborrem un element amb valor $y$ i afegim dos elements amb valors $x$ i $y-x$.



<details>
    <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ll = long long;

int main() {
    int n;
    while(cin >> n) {
        vector<int> a(n);
        for(int& x : a) 
            cin >> x;
        int g = a[0]; // Màxim comú divisor dels a[i].
        ll suma = a[0]; // Suma dels a[i].
        for(int i = 1; i < n; ++i) {
            suma += a[i];
            g = __gcd(g, a[i]);
        }
        cout << suma/g - n << endl;
    }
}
```
</details>

<details>
    <summary><b>Codi (Python3)</b></summary>

```python
from easyinput import read

def gcd(a, b):
    if b == 0:
        return a
    return gcd(b, a%b)


n = read(int)
while n is not None:
    a = [read(int) for _ in range(n)]
    g = a[0]
    for i in range(1, n):
        g = gcd(g, a[i])
    ans = (sum(a) // g) - n
    print(ans)
    n = read(int)
```
</details>


## [Problema C6. Interval cíclic](https://jutge.org/problems/P72067_ca) <a name="C6"></a>

La primera observació que s'ha de fer és que el fet que el interval sigui cíclic no complica gaire el problema, ja que un interval que "dona la volta" és simplement la unió d'un prefix i un sufix (que siguin disjunts). Alternativament, podem duplicar la seqüència de nombres que ens donen, de manera que tot interval cíclic de la seqüència original es correspon amb un interval "normal" de la seqüència duplicada. Si fem això hem d'anar amb compte que la longitud de l'interval que donem com a solució mai sigui més gran que $n$, ja que llavors estaríem sumant algun dels elements més d'una vegada.

En resum, si dupliquem la seqüència donada, el problema es redueix a trobar $l$ i $r$ tals que $a_l + \dots + a_r = s$ i $r-l < n$. Guardant-nos les sumes parcials $p_i := a_0 + \dots + a_i$, tenim que $a_l + \dots + a_r = p_r - p_{l-1}$ (on definim $p_{-1} = 0$). Per tant, podríem iterar per tots els valors possibles de $l$ i $r$, calculant per cadascun si $p_r - p_{l-1} = s$, resolent el problema en temps $\mathcal{O}(n^2)$. Com ho podem optimitzar?

Pel subcas on tots els elements són positius, hi ha dues maneres de fer-ho:

<details>
    <summary><b>Mètode 1</b></summary>

Utilitzem cerca binària. Per una $r$ fixada, observem que la suma $a_l + \dots + a_r$ és una funció decreixent en $l$ (és a dir, decreix o es queda igual si el valor de $l$ augmenta). Per tant, per cada $r$ podem fer cerca binària per trobar si hi ha un valor de $l$ pel qual la suma dona exactament $s$.

Complexitat total: $\mathcal{O}(n \log n)$.
</details>

<details>
    <summary><b>Mètode 2</b></summary>

Utilitzem la tècnica dels 2 punters. Iterem pels valors possibles de $r$ (de $0$ a $2n-1$). Per cada valor de $r$ anem augmentant la $l$ fins que la suma $a_l + \dots + a_r \leq s$. Si és exactament igual a $s$, ja tenim una solució, mentre que si és més petita passem a la següent $r$. La clau és que el valor de $l$ en el qual ens aturem només augmenta o es queda igual si augmentem la $r$, però mai disminueix. Per tant, si ens guardem el valor de $l$ en el que ens hem parat a la última iteració, podem començar des d'allà en la següent iteració. Així doncs, només haurem de provar com a molt $2n$ parelles de valors de $l$ i $r$.

Complexitat total: $\mathcal{O}(n)$.
</details>
 
<details>
  <summary><b>Codi puntuació parcial (C++)</b></summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;

int main() {
    ll s;
    int n;
    while (cin >> s >> n) {
        vector<int> a(n);
        for (int& x : a) 
            cin >> x;

        // Dupliquem l'array:
        for (int i = 0; i < n; ++i)
            a.push_back(a[i]);
        
        // Calculem sumes parcials:
        vector<ll> suma(2*n);
        suma[0] = a[0];
        for (int i = 1; i < n; ++i)
            suma[i] = suma[i-1] + a[i];

        // Busquem si hi ha dos indexos l, r tals que suma[r] - suma[l] = s
        // (i r - l < n, perquè si no estem sumant el mateix element dos cops)
        int l = 0;
        ll suma_actual = 0;
        bool trobat = false;
        for (int r = 0; r < 2*n; ++r) {
            suma_actual += a[r];
            while (l != r and (suma_actual > s or r-l >= n) ) {
                suma_actual -= a[l];
                ++l;
            }
            if (suma_actual == s) {
                cout << "SI " << l%n + 1 << " " << r%n + 1 << endl;
                trobat = true;
                break;
            } 
        }
        if (not trobat) {
            cout << "NO" << endl;
        }
    }
}
```
</details>


Si tenim nombres negatius, no podem utilitzar cap dels dos mètodes del cas anterior, ja que ara el valor de $l$ òptim pot disminuir a mesura que augmentem la $r$. Ho podem resoldre utilitzant l'estructura `map` de la llibreria estàndard de C++. Iterem per tots els valors possibles de $r$ (de $0$ a $2n-1$), i ens guardem les sumes parcials $p_i := a_0 + \dots + a_i$ per les $i$ que ja hem passat en un mapa, de manera que $\text{mapa}[x]$ contingui el màxim índex $i < r$ tal que $p_i = x$. Llavors, a cada $r$ hem de comprovar si existeix un $l$ tal que $a_l + \dots + a_r = s$, és a dir, si tenim el valor $x = p_r - s$ guardat al mapa. 

Observem que la solució només serà vàlida si $r - \text{mapa}[x] \leq n$, ja que la suma $a_l + \dots + a_r$ no pot contenir més de $n$ termes. És per això que és important que $\text{mapa}[x]$ sigui el màxim índex $i$ amb $p_i = x$, i no un de qualsevol.

Tant afegir o modificar un element com buscar si una clau està present té cost $\mathcal{O}(\log n)$ en un `map`, on $n$ és el nombre d'elements que hi ha al mapa, així que la complexitat total de l'algorisme és $\mathcal{O}(n \log n)$.

<details>
  <summary><b>Codi puntuació total (C++)</b></summary>

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = long long;

int main() {
    ll s;
    int n;
    while (cin >> s >> n) {
        vector<int> a(n);
        for (int& x : a) 
            cin >> x;

        // Dupliquem l'array:
        for (int i = 0; i < n; ++i)
            a.push_back(a[i]);

        map<ll,int> sumes_anteriors; // sumes_anteriors[x] := màxim index i (i < r) 
                                     //   tal que a_0 + ... + a_i = x
        sumes_anteriors[0] = -1;
        ll suma_actual = 0; // suma_actual := a_0 + ... + a_r
        bool trobat = false;
        for (int r = 0; r < 2*n; ++r) {
            suma_actual += a[r];
            ll x = suma_actual - s; // volem que suma_actual - x = s
            if (sumes_anteriors.count(x) and r - sumes_anteriors[x] <= n) {
                int l = sumes_anteriors[x] + 1;
                // Hem trobat una solució a_l + ... + a_r = s.
                cout << "SI " << l%n + 1 << " " << r%n + 1 << endl;
                trobat = true;
                break;
            } 
            sumes_anteriors[suma_actual] = r;
        }
        if (not trobat) {
            cout << "NO" << endl;
        }
    }
}
```
</details>

## [Problema G4. Importància de les caselles](https://jutge.org/problems/P24979_ca) <a name="G4"></a>

La solució esperada fa dos recorreguts en amplada (BFS), un des de I i l'altre des de F. Sigui $d$ la distància mínima entre I i F. Per a cada posició $(x, y)$, sigui `D1[x][y]` la mínima distància des de I a $(x, y)$, sigui `R1[x][y]` el nombre de camins mínims que arriben a $(x, y)$ des de I, i siguin `D2[x][y]` i `R2[x][y]` les mateixes quantitats però des de F.

Llavors, per $(x, y)$ passa algun camí mínim si i només si `D1[x][y] + D2[x][y] = d`. A més, el nombre de camins mínims que passen per $(x, y)$ és `R1[x][y]*R2[x][y]`.

<details>
    <summary><b>Codi</b></summary>

```python
from easyinput import read
from PIL import Image, ImageDraw


B = 10
INF = 10000


inc_x = [ 1, -1, 0,  0 ]
inc_y = [ 0,  0, 1, -1 ]


def quadrat(x, y, color):
     x *= B
     y *= B
     dib.polygon([(x, y), (x, y + B - 1), (x + B - 1, y + B - 1), (x + B
- 1, y)], color)


# llegim l'entrada
n = read(int)
m = read(int)
M = [[read(chr) for _ in range(m)] for _ in range(n)]

# busquem l'inici i el final
for i in range(n):
     for j in range(m):
         if M[i][j] == 'I':
             xi = i
             yi = j
         elif M[i][j] == 'F':
             xf = i
             yf = j

# fem un recorregut en amplada començant en I
# guardem les distàncies mínimes a D1 i el nombre de camins mínims a R1
D1 = [[INF for _ in range(m)] for _ in range(n)]
R1 = [[0 for _ in range(m)] for _ in range(n)]
D1[xi][yi] = 0
R1[xi][yi] = 1
cua = [(xi, yi)]
while len(cua) > 0:
     seg = []
     for (x, y) in cua:
         for d in range(4):
             x2 = x + inc_x[d]
             y2 = y + inc_y[d]
             if x2 >= 0 and x2 < n and y2 >= 0 and y2 < m and M[x2][y2] != '*':
                 if D1[x2][y2] == INF: # posició nova
                     seg.append((x2, y2))
                     D1[x2][y2] = D1[x][y] + 1
                     R1[x2][y2] = R1[x][y]
                 elif D1[x2][y2] == D1[x][y] + 1: # posició ja trobada, però amb la mateixa distància mínima
                     R1[x2][y2] += R1[x][y]
                     if R1[x2][y2] >= 256:
                         R1[x2][y2] = 256 # qualsevol nombre > 255 és irrelevant, així evitem que els nombres creixin innecessàriament
     cua = seg

# igual que el primer recorregut, però des del final
D2 = [[INF for _ in range(m)] for _ in range(n)]
R2 = [[0 for _ in range(m)] for _ in range(n)]
D2[xf][yf] = 0
R2[xf][yf] = 1
cua = [(xf, yf)]
while len(cua) > 0:
     seg = []
     for (x, y) in cua:
         for d in range(4):
             x2 = x + inc_x[d]
             y2 = y + inc_y[d]
             if x2 >= 0 and x2 < n and y2 >= 0 and y2 < m and M[x2][y2] != '*':
                 if D2[x2][y2] == INF:
                     seg.append((x2, y2))
                     D2[x2][y2] = D2[x][y] + 1
                     R2[x2][y2] = R2[x][y]
                 elif D2[x2][y2] == D2[x][y] + 1:
                     R2[x2][y2] += R2[x][y]
                     if R2[x2][y2] >= 256:
                         R2[x2][y2] = 256
     cua = seg

img = Image.new('RGB', (B*m, B*n), 'White')
dib = ImageDraw.Draw(img)

mx = R1[xf][yf]
q = 255//mx
for i in range(n):
     for j in range(m):
         if M[i][j] == '*':
             quadrat(j, i, "Blue")
         elif D1[i][j] + D2[i][j] > D1[xf][yf]: # D1[xf][yf] = distància mínima entre I i F
             quadrat(j, i, "Green")
         else:
             quadrat(j, i, (q*R1[i][j]*R2[i][j], 0, 0)) # R1[i][j]*R2[i][j] = nombre de camins mínims que passen per (i, j)

img.save("output.png")
```
</details>