Przedmiot: Wspomaganie Decyzji (Z2023)
Karta przedmiotu: 639B-INSWS-MEP-WDEUZ
Koordynator przedmiotu: dr inż. Tomasz Śliwiński

Wykonał: Szymon Kwieciński nr. albumu 324159
Data: 11.01.2024


### Dane
1.Zapotrzebowanie $j$ na produkcje produktu $i$, gdzie $i,j\in P$ 

|  | S | M | C |
| ---- | ---- | ---- | ---- |
| S | 0 | 0.05 | 0.08 |
| M | 0.75 | 0 | 0.12 |
| C | 1 | 0.1 | 0 |
2.Zasoby pracy $h=600000$
3.Koszty wyprodukowania prodktu  $i$, gdzie $i\in P$ 

|  | K |
| ---- | ---- |
| S | 300 |
| M | 150 |
| C | 500 |

4.Potrzebna praca do wyprodukowania produktu $i$, $i\in P$  

|  | H |
| ---- | ---- |
| S | 0.5 |
| M | 5 |
| C | 3 |
5.Maksymalna możliwość wyprodukowania produktu $i$, $i\in P$  

|     | $P^{max}$ |
|:--- |:--------- |
| S   | 400000    |
| M   | 50000     |
| C   | 550000    |
6.Jednostkowe ceny sprzedaży produktu $i$ dla puli $n$, gdzie $i\in P$, $n\in N$ 

|  | 1 | 2 | 3 | 4 |
| ---- | ---- | ---- | ---- | ---- |
| S | 0 | 0 | 0 | 900 |
| M | 0 | 0 | 0 | 2500 |
| C | 4000 | 3500 | 3000 | 2700 |
7.Maksymalna sprzedaż towaru $i$ dla puli $n$,  $i\in P$, $n\in N$ 

|  | 1 | 2 | 3 |
| ---- | ---- | ---- | ---- |
| S | 0 | 0 | 0 |
| M | 0 | 0 | 0 |
| C | 50000 | 25000 | 75000 |
# Zadanie 1
Sformułować możliwie najprostszy model programowania matematycznego dopuszczając podzielność jednostek produktów
### Parametry
$P = \{S,M,C\}$ = zbiór wytwarzanych produktów
$N$ = {1,2,3,4} = pula produktów
$Z_{ij}$  = zapotrzebowanie  produktu  j $\in P$ dla wyprodukowania produktu $i \in P$ 
$K_i$ = koszty pieniężne wyprodukowania produktu $i \in P$ 
$H_i$ = koszty pracy wyprodukowania produktu $i \in P$ 
$P^{max}_i$ = Maksymalna ilość wyprodukowania produktu $i \in P$ 
$h$ = zasoby pracy
$C_{in}$ = cena sprzedaży produktu $i \in P$ dla puli $n \in N$   
$S_{in}$ = maksymalna ilość produktu do sprzedania $i \in P$ dla puli $n \in \{1,N-1\}$

### Zmienne decyzyjne
$X^{prod}_i$ = produkcja produktu $i \in P$ 
$X^{zap}_i$ = zapotrzebowanie produktu $i \in P$ do celów produkcyjnych
$X^{sprz}_{in}$ = sprzedaż produktu $i \in P$ dla puli $n \in N$   

### Ograniczenia
- Zmienne decyzyjne nie są ujemne
$$X^{prod}_{i}\ge0, \forall_{i\in P} \quad (1)$$
$$X^{zap}_{i}\ge0, \forall_{i\in P} \quad (2)$$
$$X^{sprz}_{in}\ge0, \forall_{i\in P} \wedge\forall_{n\in N}\quad (3)$$
- Nie zostały przekroczone dostępne zasoby pracy $h$
$$ \sum\limits_{i=1}^{3}H_{i}X^{prod}_i \le h \quad (4)$$
- Nie zostały przekroczone dostępne możliwości produkcyjne $P^{max}$
$$X^{prod}_i\le P^{max}_{i,}, \forall_{i\in P} \quad (5)$$
- Nie została przekroczona sprzedaż towaru $i$ dla danej puli $n$
$$X^{sprz}_{in} \le S_{in}, \forall_{i\in P}\wedge\forall_{n \in \{1...N-1\}} \quad (6)$$
- Wykorzystano wszystkie wyprodukowane produkty $i$
$$X^{prod}_i=X^{zap}_i+{\sum\limits_{n=1}^{4}X^{sprz}_{in}}, \forall_{i\in P} \quad (7)$$
- Zgodność wykorzystanych produktów
$$X^{zap}_i=\sum\limits_{j=1}^{3}X^{prod}_jZ_{ji}, \forall_{i\in P}  \quad (8)$$
### Funkcje celu
* Funkcja przychodu 
$$f_{p}(X) = \sum\limits_{i=1}^{3}\sum\limits_{n=1}^{4}X^{sprz}_{in}C_{in} \quad (9)$$
- Funkcja kosztów 
$$f_{k}(X) = \sum\limits_{i=1}^{3}X^{prod}_{i}K_{i}\quad (10)$$
- Funkcja zysków 
$$ f(X) = f_{p}(X)-f_{k}(X) \quad (11)$$

```bash
set P; # zbiór wytwarzanych produktów {S,M,C} 
 
# params
param N; # pula produktów
param Z {P, P} >= 0; # zapotrzebowanie  produktu  j dla wyprodukowania produktu
param K {P} >= 0; # koszty pieniężne wyprodukowania produktu
param H {P} >= 0; # koszty pracy wyprodukowania produktu
param P_max {P} >= 0; # Maksymalna ilość wyprodukowania produktu 
param h >= 0; # zasoby pracy
param C {P, 1..N} >= 0; # cena sprzedaży produktu dla danej puli
param S {P, 1..N-1} >= 0; # maksymalna ilość produktu do sprzedania dla danej puli

# vars
var X_prod {P} >= 0; # produkcja produktu i
var X_zap {P} >= 0; # zapotrzebowanie produktu i do celów produkcyjnych
var X_sprz {P, 1..N} >= 0; #  sprzedaż produktu i z puli n
var przychod = sum{i in P, n in 1..N} X_sprz[i,n]*C[i,n];
var koszt = sum{i in P} X_prod[i]*K[i];


# constrains
# Nie zostały przekroczone dostępne zasoby pracy
subject to ZasobyPracy:
	sum {i in P} H[i] * X_prod[i] <= h;
# Nie zostały przekroczone dostępne możliwości produkcyjne
subject to LimityProdukcji {i in P}: 
	X_prod[i] <= P_max[i];
# Nie została przekroczona sprzedaż towaru $i$ dla danej puli
subject to LimitySprzedDlaPuli {i in P, n in 1..N-1}: 
	X_sprz[i,n] <= S[i,n];
# Wykorzystano wszystkie wyprodukowane produkty 
subject to WykorzystanieProdukcji {i in P}: 
	X_prod[i] = X_zap[i] + sum{n in 1..N} X_sprz[i,n];
# Zgodność wykorzystanych produktów
subject to Zgodnosc {i in P}: 
	X_zap[i] = sum{j in P} Z[j,i] * X_prod[j];

# objectives
maximize Zysk: przychod - koszt;
```
$$zad1.mod$$

```bash
set P := S,M,C;
param N := 4;

param Z:  S		M 		C :=
		S 0		0.05	0.08
		M 0.75	0		0.12
		C 1		0.1		0
		;

param K :=
	S 300 
	M 150 
	C 500
	;
	
param H :=
	S 0.5 
	M 5 
	C 3
	;

param P_max :=
	S 400000 
	M 50000
	C 550000
	;
	
param h := 600000;

param C : 1		2 		3		4 	:=
		S 0		0		0		900
		M 0		0		0		2500
		C 4000	3500	3000	2700		
		;
		
param S:  1		2 		3	:=
		S 0		0		0
		M 0		0		0
		C 50000	25000	75000	
		;
```
$$zad1.dat$$
# Zadanie 2
Wyznaczyć rozwiązanie optymalne (przy pomocy dowolnego solwera) i zbadać jego wrażliwość na zmiany wielkości zasobu pracy

## Rozwiązanie optymalne
```bash
reset;
model p1.mod;
data p1.dat;
option solver cplex;
solve;
display Zysk, przychod, koszt;
```
$$zad1.run$$

```
CPLEX 22.1.1.0: optimal solution; objective 313889221.6
9 dual simplex iterations (5 in phase I)
Zysk = 313889000
przychod = 432632000
koszt = 118743000
```
$$zad1:\: consol\:output$$
Po uruchomieniu programu dla modelu $zad1.mod$ i dla danych $zad1.dat$ otrzymujemy optymalne rozwiązanie $Zysk=313889000$ .

## Wrażliwość na zmianę zasobu pracy

Wrażliwość na zminę $h$ zasobu pracy  zbadano powtarzając symulacje dla wartości $50000<=h<=2000000$ dla kroku równemu 20000. Do obliczeń zostało wykorzystane API dla języka python dostępne dla AMPL. W programie wykorzystujemy wcześniej zdefiniowane $zad1.mod$ oraz  $zad1.dat$. Symulacje powtarzamy dla wielu parametrów $h$ otrzymując rozwiązanie poniżej $Rys.1$ .

```python
from amplpy import AMPL, ampl_notebook
import pandas as pd
import numpy as np

ampl.read("zad1.mod")
ampl.read_data("zad1.dat")

start = 50000
stop = 2000000
step = 20000
h_values = [x for x in range(start, stop, step)]
h_values_len = len(h_values)

arr = np.zeros((h_values_len, 4))
for i, h in enumerate(h_values):
    ampl.get_parameter("h").set(h)
    ampl.option["solver"] = "cplex"
    ampl.solve()
    zysk = math.floor(ampl.getObjective("Zysk").getValues().to_list()[0])
    koszt = math.floor(ampl.get_variable("koszt").getValues().to_list()[0])
    przychod = math.floor(ampl.get_variable("przychod").getValues().to_list()[0])
    
    arr[i,0] = zysk
    arr[i,1] = koszt
    arr[i,2] = przychod
    arr[i,3] = h
    
df = pd.DataFrame(arr, columns=['zysk', 'koszt', 'przychod', 'praca'])
df.plot(x="praca", grid=True, ylabel="wartość", xlabel="zasoby pracy") 
```
![[Pasted image 20240110230636.png]]
$$Rys.1 \: \: Funckcje\:celu\:dla\:wektora\:zasobów\:pracy\:h $$
Po analizie wykresu możemy zaobserwować że wzrost kosztów do wartości około 130000 wzrasta liniowo po czym staje się wartością stało

![[Pasted image 20240111225419.png]]
$$Rys.2\: \: Funckcje\:celu\:dla\:wektora\:zasobów\:pracy\:h$$
Po ograniczeniu zakresu dla $1200000<=h<=1500000$ dla kroku 10000 możemy lepiej zaobserwować zatrzymanie się wartości zysku pomimo wzrostu zasobu pracy. Ta wartość to $604733333$. Aby zwiększać zyski należy rozważyć zmienne innych parametrów.

# Zadanie 3
Jaki wpływ na rozwiązanie optymalne miałoby dodatkowe wymaganie pełnego wykorzystania zasobów pracy?

Aby zbadać wpływ pełnego wykorzystania zasobów pracy należy zmienić równie $4$ na:
$$ \sum\limits_{i=1}^{3}H_{i}X^{prod}_i = h \quad (12)$$
```
CPLEX 22.1.1.0: optimal solution; objective 313889221.6
9 dual simplex iterations (5 in phase I)
Zysk = 313889000
przychod = 432632000
koszt = 118743000
```
$$zad3:\: consol\:output$$
Po zmianie modelu matematycznego i uruchomieniu solver dowiadujemy się że zmiana wymagań na pełne wykorzystanie zasobów pracy nie wpływa na rozwiązanie optymalne.

# Zadanie 4
Jakie są ograniczenia i jakie możliwości rozszerzania opracowanego modelu?

Aktualny model jest uproszczeniem rzeczywistości. Można by go rozszerzyć o:
- uwzględnienie magazynowania produktów
	- pojemność magazynów
	- koszt magazynowania
	- wielkość zajmowanego miejsca przez dany produkt
- kosztu i czasu transportu
- zapotrzebowania na rynku na poszczególne produkty
- minimalne stany magazynowe dla każdego towaru
