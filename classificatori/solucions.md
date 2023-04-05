# Solucions Classificatori OICat 2023

## Taula de continguts
* [Problema C1. Ping-pong](#C1)
* [Problema C2. L'ase d'en Buridan](#C2)
* [Problema Q1. Productes acabats en cinc](#Q1)
* [Problema C3. Redistribució de la riquesa](#C3)
* [Problema G1. Trencaclosques](#G1)
* [Problema C4. Camp de visió](#C4)
* [Problema Q2. Classificació de l'OICat](#Q2)
* [Problema G2. Ordenant llibres](#G2)
* [Problema C5. Distàncies a les fulles](#C5)
* [Problema G3. Pizzeries](#G3)
* [Problema C6. Comptant permutacions](#C6)
* [Problema C7. Retransmissió](#C7)


## [Problema C1. Ping-pong](https://jutge.org/problems/P90280_ca) <a name="C1"></a>

Podem dividir el problema en 3 casos:
- Si el guanyador té més de 21 punts, la puntuació serà vàlida sempre i quan la diferència sigui de 2 punts.
- Si el guanyador té 21 punts, la puntuació serà vàlida si el perdedor té 19 o menys punts.
- Si el guanyador té menys de 21 punts, la puntuació mai pot ser vàlida.

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    int a, b;
    while(cin >> a >> b) {
        if(a > 21 or b > 21) {
            // Cas on el guanyador ha fet mes de 21 punts. 
            // Sera valid si la diferencia es de 2 punts.
            if(abs(a-b) == 2) {
                cout << "SI" << endl;
            } 
            else {
                cout << "NO" << endl;
            }
        }
        else if(a == 21) {
            // Cas on A guanya amb 21 punts.
            // Sera valid si B fa menys de 20 punts.
            if(b < 20) {
                cout << "SI" << endl;
            }
            else {
                cout << "NO" << endl;
            }
        }
        else if(b == 21) {
            // Cas on B guanya amb 21 punts.
            // Sera valid si A fa menys de 20 punts.
            if(a < 20) {
                cout << "SI" << endl;
            }
            else {
                cout << "NO" << endl;
            }
        }
        else {
            // Cap dels 2 jugadors arriba a 21 punts.
            // Mai pot ser una puntuacio valida.
            cout << "NO" << endl;
        } 
    }
}
```
</details>
<details>
  <summary><b>El mateix codi però una mica més elegant (C++)</b></summary>

```cpp
#include <iostream>
using namespace std;

int main() {
    int a, b;
    while(cin >> a >> b) {
        if(a < b) swap(a, b);
        if(a > 21) cout << (a-b == 2? "SI" : "NO") << endl;
        else if(a == 21) cout << (b <= 19? "SI" : "NO") << endl;
        else cout << "NO" << endl;
    }
}
```
</details>

<details>
    <summary><b>Codi (Python3)</b></summary>

```python
from easyinput import read

a, b = read(int,int) # llegim dos enters.
if max(a, b) > 21:
    # Si el guanyador ha fet més de 21 punts, 
    # hem de comprovar que la diferència és de 2 punts.
    if min(a, b) + 2 == max(a, b):
        print("SI")
    else:
        print("NO")
elif max(a, b) == 21:
    # Si el guanyador ha fet 21 punts exactes,
    # hem de comprovar que el perdedor hagi fet 19 o menys.
    if min(a, b) <= 19:
        print("SI")
    else:
        print("NO")
else:
    # Si el guanyador ha fet menys de 21 punts, la puntuació no és vàlida.
    print("NO")

```
</details>

## [Problema C2. L'ase d'en Buridan](https://jutge.org/problems/P86377_ca) <a name="C2"/>

Ens podem guardar les mides dels sacs en un vector (o una llista en Python) i ordenar-les. Llavors, iterem de gran a petita fins que trobem un element repetit (o fins al final del vector, si no hi ha cap repetit), guardant-nos la suma dels sacs que portem menjats.

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    while(cin >> n) {
        vector<int> v(n); // Declarem un vector de mida n.
        for(int i = 0; i < n; ++i) {
            cin >> v[i]; // Llegim els elements del vector.
        }
        sort(v.begin(), v.end()); // Ordenem el vector de petit a gran.
        int menjat = 0; // En aquesta variable anem sumant la mida dels sacs que ens mengem.
        for(int i = n-1; i >= 0; --i) {
            // Iterem pels elements del vector de gran a petit.
            if(i > 0 and v[i] == v[i-1]) {
                // La condicio i > 0 es necessaria perque, si no, quan i == 0,
                // al fer v[i-1] estariem accedint a la posicio -1 del vector
                // (que no existeix) i el programa podria fallar. 

                break; // Hem trobat un element repetit, per tant parem.
            }
            menjat += v[i]; // Sumem la mida del sac que ens mengem.
        }
        cout << menjat << endl;
    }
}
```
</details>

<details><summary><b>Codi (Python3)</b></summary>

```python
from easyinput import read

n = read(int)
while n is not None:
    a = [read(int) for _ in range(n)] # llegim n enters i els col·loquem en una llista.
    a.sort() # ordenem la llista (per defecte, de petit a gran).
    a.reverse() # invertim l'ordre dels elements de la llista (ara aniran de gran a petit).
    suma = 0 # en aquesta variable anirem guardant la suma dels sacs que es menja l'ase.
    for i in range(n):
        # Primer comprovem si el següent element és igual i, en cas que sí, sortim del for.
        if i < n-1: # hem de comprovar això abans d'accedir a a[i+1], ja que l'element a[n] no existeix.
            if a[i+1] == a[i]:
                break # sortim del for.

        # Sumem el valor del sac actual.
        suma += a[i]

    print(suma)

    n = read(int) # llegim el nombre d'elements per a la següent iteració del while.
```
</details>

## [Problema Q1. Productes acabats en cinc](https://jutge.org/problems/P14803_ca) <a name="Q1"/>

La clau d'aquest problema era veure que, per tal que un nombre acabi en 5, ha de ser múltiple de 5 però no de 2.

Per tant, hem de comptar el nombre de subconjunts de { $1, 2, \dots, 50$ } que tenen algun múltiple de 5 però no tenen cap múltiple de 2 (és a dir, on tots els nombres son senars). Entre 1 i 50 hi ha 25 nombres senars, així que el nombre de subconjunts de nombres senars serà $2^{25}$ (per cada nombre senar tenim dues opcions: incloure'l o no incloure'l). Si a aquests els hi restem els $2^{20}$ que no tenen cap múltiple de 5, obtenim $2^{25} - 2^{20} = 32505856$. 

Una altra manera de pensar-ho és que per construir el subconjunt que volem hem de triar uns quants múltiples de 5 senars (tenim $2^5 - 1$ maneres de fer-ho, ja que per cada un dels 5 candidats el podem incloure o no incloure (això ens dona el terme $2^5$), però hem de restar 1 per no comptar el conjunt buit) i un conjunt de nombres senars no múltiples de 5 (d'aquests n'hi ha $2^{20}$ maneres, ja que no ens importa si està buit). Per tant, la resposta és $(2^{5} - 1) \cdot 2^{20}$, que dona el mateix que abans.

## [Problema C3. Redistribució de la riquesa](https://jutge.org/problems/P57539_ca) <a name="C3"/>

L'estratègia òptima per minimitzar la diferència entre el més ric i el més pobre és repetir el següent procediment $k$ vegades:
1. Buscar la persona més rica (si hi ha un empat, agafem una qualsevol de les que tinguin més diners), a la que anomenem $r$.
2. Buscar la persona més pobra (si hi ha un empat, agafem una qualsevol de les que tinguin menys diners), a la que anomenem $p$.
3. Si $a_r > a_p$, transferim $1$ euro de $r$ a $p$.

El problema és que, com que $k$ pot ser molt gran (fins a $10^{18}$), no podem simular-ho directament, sinó que hem de trobar una manera més ràpida de calcular el resultat. El que farem és calcular per separat la màxima riquesa de la persona més pobra i la mínima riquesa de la persona més rica que es pot aconseguir transferint només $k$ euros. Si la riquesa de la persona més pobra és $m$ i la riquesa de la persona més rica és $M$, la resposta serà $M-m$ (a no ser que $m \geq M$, llavors la resposta serà o $0$ o $1$, depenent de si la suma total de diners és divisible pel nombre de persones). 

En primer lloc, ordenem les persones de menys a més diners (de manera que $a_1 \leq a_2 \leq \dots \leq a_n$). Un cop les tenim ordenades, observem que el procediment anterior el que començarà fent és transferir diners de $a_n$ fins a $a_1$. Si $k$ és prou gran, arribarà un moment en el que $a_1 = a_2$. Llavors, en lloc de donar-li tots els diners a $a_1$, el que farem és anar alternant entre donar-li un euro a $a_1$ i donar-li un euro a $a_2$, mantenint-los el màxim d'igualats possibles en tot moment per tal de maximitzar la mínima riquesa. Similarment, un cop tinguem que $a_3 = a_2 = a_1$, passarem a alternar entre els $3$, mantenint-los el màxim d'igualats possibles.

Així doncs, mentre tinguem que $a_1 < a_2$, ens costarà $1$ euro augmentar en $1$ unitat la mínima riquesa. Un cop arribem a $a_1 = a_2$, ens costarà $2$ euros augmentar la mínima riquesa en $1$ unitat. Un cop arribem a $a_1 = a_2 = a_3$, ens costarà $3$ euros augmentar la mínima riquesa en $1$ unitat. I així successivament, un cop arribem a $a_1 = a_2 = \dots = a_i$, ens costarà $i$ euros augmentar la mínima riquesa en $1$ unitat.

Entre les persones riques passa el mateix: mentre $a_n > a_{n-1}$, ens costarà $1$ euro disminuir en $1$ unitat la màxima riquesa, però un cop arribem a $a_n = a_{n-1}$ ens passarà a costar $2$ euros, un cop $a_n  = a_{n-1} = a_{n-2}$ ens costarà $3$ euros, etc.

Tenint això en ment, una manera de resoldre el problema és anar iterant d'esquerra a dreta del vector ordenat, calculant quants diners ens costarà haver igualat els primers $i$ elements, per tot $i$ des de $1$ fins a $n$. Sigui $v_i$ aquest valor. A continuació, trobem el màxim índex $j$ tal que podem igualar els primers $j$ elements amb $k$ euros o menys, però no podem igualar els primers $j+1$ elements amb $k$ euros o menys (és a dir, el màxim índex $j$ tal que $v_j \leq k$). Aleshores, la màxima riquesa de la persona més pobra que podem aconseguir serà $m = a_j + \lfloor (k - v_j)/j \rfloor$, perquè repartim diners a les $j$ persones més pobres fins que estan totes a $a_j$ (havent gastat $v_j$ euros), i amb els $k-v_j$ euros restants calculem quantes unitats podem augmentar $m$ abans de quedar-nos sense diners.

Similarment, si $w_i$ són els diners que hem de transferir per fer que $a_n = a_{n-1} = \dots = a_i$, i $j$ és el mínim índex tal que $w_j \leq k$, tenim que la mínima riquesa de la persona més rica serà $M = a_j - \lfloor (k - w_j) / (n-j+1) \rfloor$, ja que ara tindrem $n-j+1$ persones entre les quals repartir els diners sobrants.

<details>
  <summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ll = long long;

int main() {
    int n;
    ll k;
    while(cin >> n >> k) {
        vector<int> a(n);
        for(int& x : a)
            cin >> x;
        sort(a.begin(), a.end());

        // cost_esquerra[i] := diners que hem d'afegir per fer que els elements 
        //                     a l'esquerra d'a[i] siguin iguals a a[i].
        vector<ll> cost_esquerra(n, 0);
        for(int i = 1; i < n; ++i) {
            cost_esquerra[i] = cost_esquerra[i-1] + ll(a[i] - a[i-1]) * i;
        }

        // cost_dreta[i] := diners que hem de treure per fer que els elements 
        //                  a la dreta d'a[i] siguin iguals a a[i].
        vector<ll> cost_dreta(n, 0);
        for(int i = n-2; i >= 0; --i) {
            cost_dreta[i] = cost_dreta[i+1] + ll(a[i+1] - a[i]) * (n-1 - i);
        }

        int petit = -1; // maxim minim del vector que podem aconseguir afegint k euros.
        for(int i = 1; i < n; ++i) {
            if(cost_esquerra[i] > k) {
                ll sobrants = k - cost_esquerra[i-1];
                petit = a[i-1] + sobrants/i;
                break;
            }
        }
        if(petit == -1) {
            petit = a[n-1];
        }

        int gran = -1; // minim maxim del vector que podem aconseguir traient k euros.
        for(int i = n-2; i >= 0; --i) {
            if(cost_dreta[i] > k) {
                ll sobrants = k - cost_dreta[i+1];
                gran = a[i+1] - sobrants/(n-1 - i);
                break;
            }
        }
        if(gran == -1) {
            gran = a[0];
        }

        if(petit >= gran) {
            // Tot i que puguem moure molts diners, si la suma no 
            // es divisible per n no ho podrem igualar del tot.
            ll suma = 0;
            for(int& x : a) {
                suma += x;
            }
            cout << (suma%n ? 1 : 0) << endl;
        }
        else {
            cout << gran - petit << endl;
        }
    }
}
```
</details>

<details>
    <summary><b>Codi (Python3)</b></summary>

```py
from easyinput import read

n, k = read(int, int)
while n is not None:
    a = read(int, amount=n)
    if n == 1:
        a = [a] # si nomes hi ha un element, read(int) 
                # el llegeix com a enter, no com a llista
    a.sort()
    
    # cost_esquerra[i] := diners que hem d'afegir per fer que els elements 
    #                     a l'esquerra d'a[i] siguin iguals a a[i].
    cost_esquerra = [0] * n
    for i in range(1, n):
        cost_esquerra[i] = cost_esquerra[i-1] + (a[i] - a[i-1]) * i

    # cost_dreta[i] := diners que hem de treure per fer que els elements 
    #                  a la dreta d'a[i] siguin iguals a a[i].
    cost_dreta = [0] * n
    for i in range(n-2, -1, -1):
        cost_dreta[i] = cost_dreta[i+1] + (a[i+1] - a[i]) * (n-1 - i)

    petit = a[n-1] # maxim minim del vector que podem aconseguir afegint k euros.
    for i in range(1, n):
        if cost_esquerra[i] > k:
            sobrants = k - cost_esquerra[i-1]
            petit = a[i-1] + sobrants//i
            break

    gran = a[0] # minim maxim del vector que podem aconseguir traient k euros.
    for i in range(n-2, -1, -1):
        if cost_dreta[i] > k:
            sobrants = k - cost_dreta[i+1]
            gran = a[i+1] - sobrants//(n-1 - i)
            break

    if petit >= gran:
        # Tot i que puguem moure molts diners, si la suma no 
        # es divisible per n no ho podrem igualar del tot.
        if sum(a) % n == 0:
            print(0)
        else:
            print(1)
    else:
        print(gran - petit)

    n, k = read(int, int)
```
</details>

## [Problema G1. Trencaclosques](https://jutge.org/problems/P49148_ca) <a name="G1"/>

Per dibuixar la peça de puzzle es podia començar amb un quadrat i llavors afegir per cada costat un cercle del color del fons o del color de la peça, depenent de la forma del costat. Era fàcil equivocar-se amb els centres dels cercles, o dibuixant el cercle amb 1 píxel de diàmetre més o menys del que tocava.

Recordeu de cara a futurs problemes que les imatges de la llibreria `PIL` es representen com graelles amb $n$ files i $m$ columnes, on la casella superior esquerra té les coordenades $(0,0)$ i la casella inferior dreta té les coordenades $(m-1,n-1)$.

<details>
    <summary><b>Codi</b></summary>

```py
from PIL import Image, ImageDraw
from easyinput import read

# Dibuixa un rectangle amb vertex superior esquerre a (x1,y1) i vertex inferior dret a (x2, y2).
def rect(x1, y1, x2, y2, col):
    dib.polygon([(x1, y1), (x2, y1), (x2, y2), (x1, y2)], col)

# Dibuixa un quadrat amb vertex superior esquerre a (x, y) i costats de llargada c.
def quadrat(x, y, c, col):
    rect(x, y, x+c-1, y+c-1, col)

# Dibuixa un cercle amb diametre d.
def cercle(centre, d, col):
    (x,y) = centre
    r = d//2
    dib.ellipse([(x-r, y-r), (x+r, y+r)], col)

c = read(int)
d = read(int)
m = read(int)
color_fons = read(str)
color_dins = read(str)
forma = read(str)

img = Image.new('RGB', (c+2*m, c+2*m), color_fons)
dib = ImageDraw.Draw(img)

quadrat(m, m, c, color_dins) # dibuixa la peça quadrada.

# Índex del píxel de la meitat del costat de la peça de puzzle (començant a comptar per 0).
mig = m + (c-1)//2 

centres_fora = [(mig, m-1), (c+m, mig), (mig, c+m), (m-1, mig)]
centres_dins = [(mig, m), (c+m-1, mig), (mig, c+m-1), (m, mig)]

for i in range(4):
    if forma[i] == '(':
        cercle(centres_dins[i], d, color_fons)
    elif forma[i] == ')':
        cercle(centres_fora[i], d, color_dins)

img.save('output.png')
```
</details>

## [Problema C4. Camp de visió](https://jutge.org/problems/P66837_ca) <a name="C4"/>

La idea clau és calcular l'angle que forma cada punt des del centre respecte a l'eix horitzontal (o respecte a qualsevol altre punt de referència) i guardar-los en una llista d'angles ordenada.

Observem que és òptim posar la càmara de manera que un dels punts estigui tocant el límit del camp de visió (per què en cas que no, podríem girar la càmara una mica sense perdre cap punt). Així doncs, podem iterar per la llista calculant per cada possible posició de la càmara fins a quin punt arribaríem a veure. La manera trivial de fer-ho dona lloc a un algorisme $\mathcal O(n^2)$, però es pot optimitzar a $\mathcal O(n \log n)$ fent cerca binària, o a $\mathcal O(n)$ guardant-se l'índex de més a la dreta que hem arribat entre iteracions per a evitar tornar enrere. Només cal anar amb compte de tractar bé el cas en el que arribem al final de la llista i hem de tornar a començar pel principi.

La complexitat total serà $\mathcal O(n \log n)$, ja que hem d'incloure el cost d'ordenar la llista d'angles al principi. Per calcular l'angle d'un punt $(x, y)$ amb l'eix horitzontal podem utilitzar la funció $\texttt{atan(y, x)}$, que ens retorna l'angle en radians com un valor entre $-\pi$ i $\pi$.

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

// Angle del punt (x, y) amb l'eix X. Retorna un valor en l'interval [-180, 180]:
// (per ex. angle(1, 0) = 0, angle(0, 1) = 90, angle(0, -1) = -90).
double angle(int x, int y) {
    return atan2(y, x) * 180 / M_PI;
}

int main() {
    int n, theta;
    while(cin >> n >> theta) {
        vector<double> a(n); // vector on guardem els angles dels objectes.
        for(auto& d : a) {
            int x, y;
            cin >> x >> y;
            d = angle(x, y);
        }
        if(n == 1) {
            cout << 1 << endl;
            continue;
        }
        sort(a.begin(), a.end());
        int ans = 1; // màxim nombre d'objectes que podem veure.
        int r = 1;
        for(int l = 0; l < n; ++l) {
            // l := primer objecte que veiem en sentit antihorari.
            // r := primer objecte que no veiem en sentit antihorari des de l.

            // Augmentem r fins que no poguem veure l'objecte r.
            while(a[r%n] - a[l] + (r%n < l? 360 : 0) <= theta) {
                ++r;
                if(r%n == l) break;
            }
            ans = max(ans, r - l); // començant des de l podem veure un total de r-l objectes.
        }
        cout << ans << endl;
    }
}
```
</details>

<details><summary><b>Codi (Python3)</b></summary>

```python
from easyinput import read
from math import atan2, pi

# Angle del punt (x, y) amb l'eix X. Retorna un valor en l'interval [-180, 180].
# (per ex. angle(1, 0) = 0, angle(0, 1) = 90, angle(0, -1) = -90).
def angle(x, y):
    return atan2(y, x) * 180 / pi


# Angle en sentit antihorari entre els objectes l i r.
def diff(l, r):
    if r < l:
        return b[r] - b[l] + 360
    return b[r] - b[l]


n, theta = read(int,int)
while(n is not None):
    a = read(int, amount = 2*n) # llegim les coordenades dels n objectes com una llista de longitud 2n.
    b = [angle(a[2*i], a[2*i+1]) for i in range(n)]
    b.sort() # ens construim una llista ordenada amb els angles de cada un dels n objectes.
    ans = 1 # maxim nombre d'objectes que podem veure simultàniament.
    r = 1
    for l in range(n): 
        # l := primer objecte que podem veure en sentit antihorari.
        # r := primer objecte que no podem veure en sentit antihorari des de l.

        # Augmentem r fins que no poguem veure l'objecte r. 
        while(diff(l, r%n) <= theta and n != 1):
            r += 1
            if r%n == l:
                break
        ans = max(ans, r-l) # començant des de l podem veure un total de r-l objectes.
    print(ans)

    n, theta = read(int,int)
```
</details>

## [Problema Q2. Classificació de l'OIcat](https://jutge.org/problems/P20364_ca) <a name="Q2"/>

Siguin $C$, $Q$ i $G$ els conjunts d'estudiants que han resolt algun problema clàssic, quiz i gràfic, respectivament. Tal com és habitual, utilitzem el símbol $\cup$ per denotar la unió de dos conjunts (per ex. $C \cup G$ és el conjunt d'estudiants que han resolt un problema clàssic <b>o</b> un problema gràfic) i utilitzem el símbol $\cap$ per denotar la intersecció de dos conjunts (per ex. $C \cap G$ és el conjunt d'estudiants que han resolt un problema clàssic <b>i</b> un problema gràfic). Per referir-nos a la mida d'un conjunt utilitzem el símbol $\left \vert \cdot \right \vert$ (per ex. $\left\vert C \right\vert$ és la mida del conjunt $C$, és a dir, el nombre d'estudiants que han resolt algun problema clàssic).

Segons el [Principi d'Inclusió-Exclusió](https://ca.wikipedia.org/wiki/Principi_d%27inclusi%C3%B3-exclusi%C3%B3), tenim que 

$$\left\vert C \right\vert + \left\vert Q \right\vert + \left\vert G \right\vert - \left\vert C \cap G\right\vert  - \left\vert G \cap Q\right\vert  - \left\vert Q \cap C\right\vert  + \left\vert C \cap Q \cap G\right\vert = \left\vert C \cup Q \cup G \right\vert$$

Substituint les dades de l'enunciat, obtenim que

$$275 + 263 + 211 - 176 - 150 - 198 + \left \vert C \cap Q \cap G \right\vert = 352$$

d'on treiem que el nombre de persones que han resolt un problema dels 3 tipus és $127$.


## [Problema G2. Ordenant llibres](https://jutge.org/problems/P58245_ca) <a name="G2"/>

Aquest problema era llarg d'implementar, però la única dificultat era ordenar els llibres d'acord amb els criteris que es donen. Per fer-ho, es podia crear una $\texttt{dataclass}$ amb una funció $\texttt{compare}$ customitzada, o guardar-se cada llibre com una tupla i canviar el signe del nombre de pàgines i de l'alçada per tal de que l'ordenació per defecte de la tupla (de petit a gran) ens doni l'ordre que busquem.

<details><summary><b>Solució amb dataclass</b></summary>
    
```python
from PIL import Image, ImageDraw
from dataclasses import dataclass
from functools import cmp_to_key
from typing import List

@dataclass
class book:
    title: str
    author: str
    year: int
    month: int
    day: int
    pages: int
    height: int
    colour: str
    index: int

# Si retorna un nombre positiu, a anirà abans que b.
# Si retorna un nombre negatiu, b anirà abans que a.
def compare(a: book, b: book)->int:
    if a.title!=b.title:
        return 2*(a.title>b.title)-1
    if a.author!=b.author:
        return 2*(a.author>b.author)-1
    if a.pages!=b.pages:
        return b.pages-a.pages
    if a.year!=b.year:
        return a.year-b.year
    if a.month!=b.month:
        return a.month-b.month
    if a.day!=b.day:
        return a.day-b.day
    if a.height!=b.height:
        return b.height-a.height
    return a.index-b.index

n = int(input())
llibres: List[book] = [None for _ in range(n)] # inicialitzem una llista de llibres de la mida que toca
amplada_dibuix = 0
alcada_dibuix = 0
for i in range(n):
    # Llegim les dades de cada llibre:
    titol = input().lower()
    autor = input().lower()
    pagines = int(input())
    data = input()
    alcada = int(input())
    color = input()
    alcada_dibuix = max(alcada_dibuix,alcada)
    amplada_dibuix += pagines//4

    # Creem l'objecte del llibre.
    llibres[i] = book(title=titol, author=autor, year=int(data[6:]), month=int(data[3:5]), 
                    day=int(data[0:2]), pages=pagines, height=alcada, colour=color, index=i)

# Ordenem segons el criteri especificat a la funció compare.
llibres.sort(key=cmp_to_key(compare))

alcada_dibuix += 10
img = Image.new('RGB', (amplada_dibuix, alcada_dibuix), 'White')
dib = ImageDraw.Draw(img)
dib.polygon([(0,alcada_dibuix-1),(amplada_dibuix-1,alcada_dibuix-1),(amplada_dibuix-1,alcada_dibuix-10),(0,alcada_dibuix-10)],'Brown')

def dibuixa_llibre(x,amplada,alcada,color):
    dib.polygon([(x,alcada_dibuix-11),(x+amplada-1,alcada_dibuix-11),(x+amplada-1,alcada_dibuix-10-alcada),(x,alcada_dibuix-10-alcada)],color)

x_actual = 0 # coordenada horitzontal per la que anem.
for llibre in llibres:
    dibuixa_llibre(x_actual, llibre.pages//4, llibre.height, llibre.colour)
    x_actual += llibre.pages//4
img.save("output.png")
```
</details>

<details><summary><b>Solució sense dataclass</b></summary>

```python
from PIL import Image, ImageDraw
import math

def rect(x1, y1, x2, y2, col):
    dib.polygon([(x1, y1), (x2, y1), (x2, y2), (x1, y2)], col)

n = int(input())
llibres = []
altura_maxima = 0
ample_total = 0
for i in range(n):
    titol = input()
    autor = input()
    pagines = int(input())
    data = input()
    data = data.split("/")
    altura = int(input())
    color = input()

    altura_maxima = max(altura_maxima, altura)
    ample_total += pagines//4

    # Ens guardem els parametres que han d'anar ordenats de gran a petit amb signe negatiu per tal que 
    # l'ordenació per defecte de les tuples ens doni l'ordre que busquem.
    llibres.append((titol.lower(), autor.lower(), -pagines, data[2], data[1], data[0], -altura, i, color))

img = Image.new('RGB', (ample_total, altura_maxima+10), "White")
dib = ImageDraw.Draw(img)
rect(0, altura_maxima, ample_total, altura_maxima+10, "Brown")

llibres = sorted(llibres)
pos = 0
for (_, _, pagines, _, _, _, altura, _, col) in llibres:
    pagines = -pagines # recordeu que ens haviem guardat les pagines i l'altura amb signe negatiu i ho hem de canviar!
    pagines //= 4
    altura = -altura
    rect(pos, altura_maxima-1, pos+pagines-1, altura_maxima-altura, col)
    pos += pagines
img.save('output.png')
```
</details>


## [Problema C5. Distàncies a les fulles](https://jutge.org/problems/P38916_ca) <a name="C5"/>

Si només volguéssim trobar la distància de tots els vèrtexs a una fulla concreta, com ho faríeu?
<details><summary><b>Spoiler</b></summary>
    Per trobar les distàncies d'un vèrtex a tota la resta en un graf no dirigit sense pesos a les arestes, utilitzem l'algorisme BFS (Breadth-First Search). (De fet aquí com el graf és un arbre podríem utilitzar també un DFS, però el BFS també funciona per a grafs amb cicles.)
    L'algorisme funciona de la següent manera:
    <ol>
        <li>Creem una llista de distàncies de cada vèrtex, que inicialitzem a infinit, i creem una cua on anirem guardant els vèrtexos que hem de processar.</li>
        <li>Assignem distància $0$ al vèrtex des del que volem trobar les distàncies, i l'afegim a la cua.</li>
        <li>Mentre la cua no estigui buida, traiem el primer element de la cua (que anomenem $v$) i, per cada veí $u$ de $v$ que encara no haguem visitat (és a dir, que tingui distància infinit), l'afegim a la cua i actualitzem la seva distància: $\texttt{dist[u]} \gets \texttt{dist[v] + 1}$.</li>
        <li>Un cop la cua estigui buida, parem. Si queda algun vèrtex amb distància infinit, això voldrà dir que aquest vèrtex no és accessible des del vèrtex font (això no és possible en el nostre cas, ja que un arbre ha de ser connex per definició).</li>
    </ol>
</details>

Això ens dona una solució amb complexitat $\mathcal O(n^2)$, ja que hi poden haver fins a $\mathcal O(n)$ fulles i calcular les distàncies per cada fulla té cost $\mathcal O(n)$. Com ho podem fer més ràpid?

<details><summary><b>Pista</b></summary>
    El truc és calcular el BFS des de totes les fulles "a la vegada". Se us acut com fer-ho?
</details>

<details><summary><b>Spoiler</b></summary>
    Al principi de l'algorisme, afegim totes les fulles a la cua a la vegada, i posem la seva distància a 0. A partir d'aleshores continuem amb el BFS normal, i al final les distàncies que obtindrem seran la mínima distància a una de les fulles. Per què? Tal com funciona l'algorisme BFS, primer visitarem tots els vèrtexos a distància 0 (és a dir, les fulles que hem posat inicialment a la cua), després tots els vèrtexos a distància 1 (és a dir, els vèrtexos contigus a una fulla), després els vèrtexos a distància 2, etc. Si en algun moment visitem un vèrtex que ja ha estat visitat, tindrem que el nombre de passos amb que hi hem arribat originalment serà menor o igual que el nombre de passos amb que estem arribant ara, així que ja té la distància correcta i podem ignorar-lo. Així doncs, el fet de tenir múltiples punts d'origen no afecta a la validesa de l'algorisme.
</details>

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main() {
    int n;
    while(cin >> n) {
        // Ens guardem el graf com a llista d'adjacència,
        // és a dir: G[v] = {veins de v}.
        vector<vector<int>> G(n);

        // Un arbre té sempre n-1 arestes.
        for(int i = 0; i < n-1; ++i) {
            int u, v;
            cin >> u >> v;
            G[u].push_back(v); // afegim aresta u --> v
            G[v].push_back(u); // afegim aresta v --> u
        }

        const int INF = 1e9;
        vector<int> dist(n, INF); 
        // dist[v] := distancia minima del vertex v a una fulla.
        queue<int> q;

        // Afegim les fulles a la cua.
        for(int v = 0; v < n; ++v) {
            if(G[v].size() == 1) {
                q.push(v);
                dist[v] = 0;
            } 
        }

        // Fem el BFS.
        while (not q.empty()) {
            int v = q.front();
            q.pop();
            for(int u : G[v]) {
                // A diferència de quan fem el Dijkstra en un graf amb pesos,
                // aquí si arribem a un vèrtex que ja hem visitat, mai serà òptim
                // actualitzar la seva distància, així que només processem els 
                // vèrtexos u que encara no hem visitat.
                if(dist[u] == INF) {
                    dist[u] = dist[v] + 1;
                    q.push(u);
                }
            }
        }

        for(int v = 0; v < n; ++v) {
            cout << dist[v] << endl;
        }
        cout << string(10, '-') << endl;
    }
}
```
</details>


## [Problema G3. Pizzeries](https://jutge.org/problems/P48553_ca) <a name="G3"/>

Podem pensar la graella com un graf, on des de cada casella es pot anar a les 4 caselles adjacents. Així doncs, podem resoldre aquest problema igual que el problema C5, fent un BFS des de totes les pizzeries a la vegada.

<details open><summary><b>Truc per la implementació</b></summary>
Típicament en els problemes de grafs ens guardem la informació del graf en una llista d'adjacència $G$, on $G[v]$ és un vector (o llista en Python) amb els veïns del vèrtex $v$ en el graf. Quan el graf és una graella, no fa falta construir-se aquesta llista explícitament, sinó que ens podem definir vectors $\text{dx} = [0, 1, 0, -1]$ i $\text{dy} = [1, 0, -1, 0]$ de manera que en lloc de fer:

```python
for u in G[v]:
    # visita el vèrtex u
``` 

podem fer

```python
dx = [0, 1, 0, -1]
dy = [1, 0, -1, 0]
for k in range(4):
    u = [v[0] + dx[k], v[1] + dy[k]]
    # visita el vertex u.
```

Això acostuma a ser molt més simple que construir-se el graf $G$ explícitament.
</details>

<details><summary><b>Codi</b></summary>

```python
from PIL import Image, ImageDraw
from easyinput import read
from queue import Queue

n = read(int)
m = read(int)
r = read(int)
v = [read(str) for _ in range(n)]


# Retorna True si (x[0], x[1]) és una posició dins de la graella que no està coberta per aigua.
def IsValid(x):
    return x[0] >= 0 and x[0] < n and x[1] >= 0 and x[1] < m and v[x[0]][x[1]] != 'A'


img = Image.new('RGB', (10*m, 10*n), 'Grey')
dib = ImageDraw.Draw(img)

# Afegim totes les pizzeries a la cua.
q = Queue()
for i in range(n):
    for j in range(m):
        if v[i][j] == 'P':
            q.put((i, j))

inf = 1000000000 # valor més gran que qualsevol distància.

dist = [[0 if x == 'P' else inf for x in fila] for fila in v]
df = [1, 0, -1, 0] # moviments verticals
dc = [0, 1, 0, -1] # moviments horitzontals

# BFS.
while not q.empty():
    x = q.get()
    for k in range(4):
        # Tal com hem definit df i dc, tenim que:
        #   k = 0 -> moviment cap a baix
        #   k = 1 -> moviment cap a la dreta
        #   k = 2 -> moviment cap a dalt
        #   k = 3 -> moviment cap a l'esquerra 
        noux = (x[0] + df[k], x[1] + dc[k])
        if not IsValid(noux):
            continue
        if dist[noux[0]][noux[1]] > dist[x[0]][x[1]] + 1:
            dist[noux[0]][noux[1]] = dist[x[0]][x[1]] + 1
            q.put(noux)
        # També podríem aturar el BFS quan arribem a distància > r, 
        # fent que el codi vagi una mica més ràpid.

# Output. 
for i in range(n):
    for j in range(m):
        col = 'Grey'
        if v[i][j] == 'A':
            col = 'Aqua'
        elif v[i][j] == 'P':
            col = 'Brown'
        elif dist[i][j] <= r:
            col = 'Green'
        else:
            continue
        dib.polygon([(10*j, 10*i), (10*j + 9, 10*i), (10*j + 9, 10*i + 9), (10*j, 10*i + 9)], col)

img.save('output.png')
```
</details>


## [Problema C6. Comptant permutacions](https://jutge.org/problems/P38305_ca) <a name="C6"/>

Observem que en tenim prou amb saber calcular el nombre de permutacions més petites que una permutació fixada. Això és perquè el nombre de permutacions $p$ amb $p_1 \leq p \leq p_2$ serà igual a $f(p_2)+1 - f(p_1)$, on $f(p)$ és el nombre de permutacions estrictament més petites que $p$.

Per tant, suposem que tenim una permutació fixada $p$, de longitud $n$ i anem a calcular quantes permutacions hi ha que siguin estrictament més petites. Si $q < p$, això vol dir que existeix una posició $i$ entre $0$ i $n-1$ tal que $q_0 = p_0$, $q_1 = p_1$, $\dots$, $q_{i-1} = p_{i-1}$, i $q_i < p_i$. A partir d'aleshores, és igual com estiguin ordenats la resta d'elements, que sempre tindrem que $q < p$.

Així doncs, tenim que el nombre de permutacions $q$ més petites que $p$ que primer difereixen de $p$ en la posició $i$ és $c(i) \cdot (n-i-1)!$, on $c(i)$ és el nombre de caràcters que podem posar a la posició $i$-èssima que siguin més petits que $p_i$, i $(n-i-1)!$ són les maneres d'ordenar els $n-i-1$ caràcters restants. Per calcular $c(i)$, iterem per totes els $j$ amb $1 \leq j < p_i$ i comprovem quants d'aquests no han aparegut ja a l'esquerra de $p$ (és a dir, entre $p_0$, $p_1$, $\dots$, $p_{i-1}$).

La complexitat total és $\mathcal O(n^2)$, ja que per cada $i$ entre $0$ i $n-1$ hem d'iterar pels elements $j < i$ per veure si ja han estat utilitzats.

<details><summary><b>Repte</b></summary>
Sabríeu modificar la solució per tal que la complexitat sigui $\mathcal O(n \log n)$?
</details> 

<details><summary><b>Codi (C++)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

using ll = long long;

vector<ll> factorial;

// Precalcula els factorials fins a n.
// Necessitem que n! < 9e18 per tal que el resultat càpiga en un long long int. 
void precalcula_factorials(int n) {
    factorial = vector<ll>(n+1, 1);
    for(int k = 2; k <= n; ++k) {
        factorial[k] = k * factorial[k-1];
    }
}

// Donada una permutació, calcula el nombre de permutacions lexicogràficament més petites.
ll ordre(vector<int> const& perm) {
    int n = perm.size();
    ll ans = 0; // nombre de permutacions estrictament més petites que perm.

    // vist[k] = true si ja ha sortit el número k en el tros que portem vist 
    // de l'esquerra de la permutació.
    vector<bool> vist(n, false);
    for (int i = 0; i < n; ++i) {
        int nums_petits = 0; // nombres més petits que perm[i] que encara no han sortit.
        for(int j = 0; j < perm[i]; ++j) {
            if(not vist[j]) nums_petits++;
        }
        // Totes les permutacions que tinguin un nombre més petit que perm[i] a la 
        // posició i seran lexicogràficament més petites.
        ans += nums_petits * factorial[n-i-1];
        vist[perm[i]] = true;
    }
    return ans;
}

int main() {
    int n;
    while(cin >> n) {
        vector<int> p1(n);
        for(int& x : p1){
            cin >> x;
            x--;
        }
        vector<int> p2(n);
        for(int& x : p2){
            cin >> x;
            x--;
        }

        precalcula_factorials(n);
        cout << ordre(p2) - ordre(p1) + 1 << endl;
    }
}
```
</details>

## [Problema C7. Retransmissió](https://jutge.org/problems/P90287_ca) <a name="C7"/>

En primer lloc, observem que si $x = y$, la solució del problema és fàcil:

<details><summary><b>Spoiler</b></summary>
    Si $x = y$, necessitem $n-1$ arestes per tal que es pugui anar des de qualsevol vèrtex al vèrtex $x = y$ (ho podeu pensar com trobar un spanning tree del graf no dirigit, i llavors agafar la direcció de les arestes que "apunta cap a $x$").
</details>

Per tant, suposarem a partir d'ara que $x \neq y$. Considerem primer el subproblema en el qual $G$ és un arbre (el cas A de l'enunciat, que dona el 50% de la puntuació). En un arbre, per cada parella de vèrtexos només hi ha un únic camí que va entre els dos. Com hem de poder anar de $x$ a $y$ i de $y$ a $x$, necessitem agafar les arestes del camí entre $x$ i $y$ en les dues direccions. Per la resta d'arestes, en tenim prou amb agafar-les en una direcció (la que apunta cap a $x$ o $y$). Per tant, la solució serà $n-1 + \text{dist}(x, y)$. Per calcular la distància fem simplement un BFS o DFS, de manera que la complexitat és $\mathcal O(n)$.


En el cas del graf general, continua sent veritat que necessitem anar de $x$ a $y$ i de $y$ a $x$. La diferència és que ara pot haver-hi múltiples camins entre $x$ i $y$, de manera que no fa faltar repetir les mateixes arestes en les dues direccions, sinó que podem aprofitar per agafar arestes que passin per vèrtexos diferents en el camí d'anada i tornada. Per tant, la solució serà $n + k$, on $k$ és el nombre de vèrtexos entre $x$ i $y$ pels quals hem de passar en les dues direccions per força.

Per entendre millor com calcular aquest $k$, va bé introduir el concepte de <i>punts d'articulació</i>. En un graf no dirigit connex, diem que un vèrtex $v$ és un punt d'articulació si el graf es desconnecta quan eliminem el vèrtex $v$. Per exemple, en un arbre tots els vèrtexos excepte les fulles són punts d'articulació, però en un cicle no hi ha cap punt d'articulació. En aquest problema, ens interessen en particular els punts d'articulació que desconnecten $x$ i $y$ (és a dir, els punts d'articulació que, quan els eliminem del graf, deixen $x$ i $y$ en components connexos diferents). Per simplicitat, direm que aquests vèrtexos són <i>vèrtexos dolents</i>.

La gràcia d'aquesta definició és que tot vèrtex dolent ha d'aparèixer tant en el camí de $x$ a $y$ com en el camí de $y$ a $x$ (perquè no hi ha cap camí que no passi per ell). Per tant, $k$ és com a mínim el nombre de vèrtexos dolents. De fet, un pot veure que $k$ és exactament el nombre de vèrtexos dolents, ja que si no hi ha cap vèrtex dolent sempre podem agafar dos camins disjunts per l'anada i la tornada. Això és una conseqüència del [teorema de Menger](https://en.wikipedia.org/wiki/Menger's_theorem#Vertex_connectivity), que diu que si tenim un graf on fa falta eliminar $t$ vèrtexos per desconnectar-lo (en el nostre cas, per cada regió del graf on no hi hagi vèrtexos dolents, tindrem que $t = 2$), aleshores podem trobar $t$ camins entre $x$ i $y$ que no comparteixin cap vèrtex (assumint que $x$ i $y$ no són adjacents). 

Així doncs, una possible solució és calcular per cada vèrtex si és un vèrtex dolent (en $\mathcal O(n)$ amb un BFS o DFS), i donar com a resposta $n + \text{``nombre de vèrtexos dolents''}$. La complexitat total és $\mathcal O(n^2)$.

<details><summary><b>Codi(Puntuació parcial)</b></summary>

```cpp
#include<bits/stdc++.h>
using namespace std;

int main(){
    // PRECONDICIÓ: G es un arbre
    // Complexitat: O(n+m)
    int n, m;
    while(cin >> n >> m) {
        vector<vector<int>> G(n);
        for(int i = 0; i < m; ++i) {
            int a, b;
            cin >> a >> b;
            G[a].push_back(b);
            G[b].push_back(a);
        }
        int x, y;
        cin >> x >> y;

        // BFS des de x:
        vector<int> dist(n, -1);
        dist[x] = 0;
        queue<int> q;
        q.push(x);
        while(not q.empty()) {
            int v = q.front();
            q.pop();
            for(int u : G[v]) {
                if(dist[u] == -1) {
                    dist[u] = dist[v] + 1;
                    // Podem acabar el BFS si ja hem arribat a y, ja que 
                    // la seva distància no canviarà més.
                    if(u == y) 
                        break;
                    q.push(u);
                }
            }
        }
        cout << n-1 + dist[y] << endl;
    }
}
```
</details>

<details><summary><b>Codi(Puntuació total)</b></summary>

```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

vector<bool> vist;
vector<vector<int>> G;

// Fa un dfs des de u, sense visitar mai el vèrtex prohibit.
void dfs(int u, int prohibit) {
    vist[u] = true;
    for(int v : G[u]) {
        if(v != prohibit and not vist[v]) {
            dfs(v, prohibit);
        }
    }
}

int main() {
    int n, m;
    while(cin >> n >> m) {
        G = vector<vector<int>>(n);
        for(int i = 0; i < m; ++i) {
            int u, v;
            cin >> u >> v;
            G[u].push_back(v);
            G[v].push_back(u);
        }

        int x, y;
        cin >> x >> y;
        if(x == y) cout << n-1 << endl;
        else {
            int dolents = 0; // nombre de vèrtexos que desconnecten x i y quan els eliminem del graf.
            for(int v = 0; v < n; ++v) {
                if(v == x or v == y) continue;
                vist = vector<bool>(n, false);
                // Fem un DFS des de x on el vertex v està "prohibit" i no hi podem passar.
                dfs(x, v); 
                if(not vist[y]) dolents++;
            } 
            cout << n + dolents << endl;
        }
    }
}
```
</details>

<details><summary><b>Repte 1</b></summary>
    En el cas d'un arbre, si en lloc d'una única parella $(x, y)$ tenim $q$ parelles diferents per a les quals hem de calcular la solució, la solució anterior tindrà una complexitat de $\mathcal O(nq)$. Sabríeu resoldre el problema amb complexitat $\mathcal O((n+q)\log n)$?
</details>

<details><summary><b>Repte 2</b></summary>
    En el cas d'un graf general, sabríeu trobar tots els punts d'articulació amb complexitat $\mathcal O(n \log n)$ en lloc de $\mathcal O(n^2)$?
</details>

<details><summary><b>Repte 3</b></summary>
    Si admetem fins a $q$ queries diferents (com en el repte 1) però ara en un graf general, sabríeu resoldre el problema en $\mathcal O((n+m+q)\log n)$?
</details>
