
# Table of Contents

1. [Zanke](#1-zanke)
    - [while zanka](#while-zanka)
    - [for zanka](#for-zanka)
2. [Funkcije](#2-funkcije)
    - [Definicija funkcije](#definicija-funkcije)
3. [Rekurzija](#3-rekurzija)
    - [Primeri](#primeri-rekurzije)
4. [Objektno usmerjeno programiranje](#4-objektno-programiranje)
    - [Razredi in objekti](#razredi-in-objekti)
    - [Posebnosti](#posebnosti)
5. [Seznami](#5-seznami)
    - [Aritmeticne operacije](#aritmeticne-operacije)
    - [Odstranjevanje](#odstranjevanje)
    - [Dodajanje](#dodajanje)
    - [Iskanje](#iskanje)
    - [Urejanje-Sortiranje](#urejanje-sortiranje)
    - [Seznam razumov](#seznam-razumov)
6. [Dodatne funkcije](#6-dodatne-funkcije)
    - [Preverjanje tipa spremenljivke](#preverjanje-tipa-spremenljivke)
    - [Zip funkcija](#zip-funkcija)
    - [Enumerate](#enumerate)
    - [Pairwise](#pairwise)
    - [Random](#random)
7. [Bolj uporabljene funkcije](#7-uporabljene-funkcije)
    - [all in any](#all-in-any)
    - [Range](#range)
    - [Split](#split)
    - [Strip](#strip)
8. [Odpiranje datoteke](#8-odpiranje-datoteke)
    - [Pisanje v datoteko](#pisanje-v-datoteko)
    - [Branje datoreke](#branje-datoreke)
    - [Obstoj datoteke](#obstoj-datoteke)
9. [Nizi](#9-nizi)
    - [Iskanje](#iskanje)
    - [Urejanje](#urejanje)
    - [Odstranjevanje iz niza](#odstranjevanje-iz-niza)
    - [Preveranje vsebine](#preveranje-vsebine)
    - [Slicing](#slicing)
10. [Mnozice](#10-mnozice)
     - [Prazna množica](#prazna-množica)
     - [Pomembne metode](#pomembne-metode)
     - [Operacije z množicami](#operacije-z-množicami)
11. [Slovarji (Dictionaries)](#11-slovarji-dictionaries)
     - [Ustvarjanje slovarjev](#ustvarjanje-slovarjev)
     - [Pomembne metode](#pomembne-metode)
     - [Določene posebnosti](#določene-posebnosti)
12. [Regex](#regex)



# Zapiski za izpit iz Programiranja 1

### 1. Zanke
#### while zanka
```python
n = 5
while n > 0:
    print(n)
    n -= 1
```
- Poganja se, dokler je pogoj resničen.

#### for zanka
```python
for i in range(1, 6):
    print(i)
```
- Uporablja se za iteracijo preko zaporedij (seznamov, nizov, ...).
### 2. Funkcije:

#### definicija funkcije

```python
def pozdravi(ime):
    return f"Zivjo, {ime}!"

print(pozdravi("Ana"))
```

### 3. Rekurzija

#### Primeri rekurzije:
##### - Rodovnik

rodovnik = {
    "Adam": ["Matjaž", "Cilka", "Daniel"],
    "Matjaž": ["Viljem"],
    "Cilka": [],
    "Daniel": ["Elizabeta", "Hans"],
    "Viljem": ["Tadeja"],
    "Elizabeta": ["Ludvik", "Jurij", "Barbara", "Herman"],
    "Hans": ["Erik"],
    "Ludvik": ["Franc", "Jožef"],
    "Jurij": [],
    "Barbara": [],
    "Herman": ["Margareta"],
    "Erik": [],
    "Franc": ["Alenka"],
    "Jožef": ["Petra", "Aleksander"],
    "Alenka": [],
    "Petra": [],
    "Aleksander": [],
    "Tadeja": [],
    "Margareta": []
}
- Štetje oseb v rodovniku[oseba]:
```python
def stevilo_oseb(oseba, rodovnik):
    # Oseba sama šteje kot ena
    return 1 + sum(stevilo_oseb(otrok, rodovnik) for otrok in rodovnik[oseba])

# Primer uporabe
print(stevilo_oseb("Adam", rodovnik))  # Izpiše celotno število oseb v rodovniku
```

- Seštevanje starosti od potomcev[osebe]
```python
starosti = {
    "Adam": 111, "Matjaž": 90, "Cilka": 88, "Daniel": 85, "Viljem": 58,
    "Tadeja": 20, "Elizabeta": 67, "Ludvik": 50, "Jurij": 49, "Barbara": 45,
    "Herman": 39, "Erik": 83, "Franc": 30, "Jožef": 29, "Alenka": 9,
    "Petra": 7, "Aleksander": 5, "Margareta": 3
}

def skupna_starost(oseba, rodovnik, starosti):
    # Vrednost trenutne osebe + starosti vseh otrok
    return starosti[oseba] + sum(skupna_starost(otrok, rodovnik, starosti) for otrok in rodovnik[oseba])

# Primer uporabe
print(skupna_starost("Adam", rodovnik, starosti))  # Izpiše skupno starost vseh
```

- Iskanje najstarejšega od potomcev[osebe]
```python
def najstarejsa_oseba(oseba, rodovnik, starosti):
    najvecja_starost = starosti[oseba]
    najstarejsa = oseba

    for otrok in rodovnik[oseba]:
        otrok_starost, otrok_ime = najstarejsa_oseba(otrok, rodovnik, starosti)
        if otrok_starost > najvecja_starost:
            najvecja_starost = otrok_starost
            najstarejsa = otrok_ime

    return najvecja_starost, najstarejsa

# Primer uporabe
print(najstarejsa_oseba("Adam", rodovnik, starosti))  # Vrne (111, 'Adam')
```
- Seznam vseh potomcev[oseba]
```python
def seznam_potomcev(oseba, rodovnik):
    potomci = [oseba]  # Vključi trenutno osebo
    for otrok in rodovnik[oseba]:
        potomci.extend(seznam_potomcev(otrok, rodovnik))
    return potomci

# Primer uporabe
print(seznam_potomcev("Adam", rodovnik))  # Izpiše celoten seznam oseb
```

- Preštevanje generacije od potomcev[osebe] (globina)
```python
def stevilo_generacij(oseba, rodovnik):
    if not rodovnik[oseba]:  # Če nima otrok
        return 1
    return 1 + max(stevilo_generacij(otrok, rodovnik) for otrok in rodovnik[oseba])

# Primer uporabe
print(stevilo_generacij("Adam", rodovnik))  # Vrne število generacij
```

- Iskanje poti do osebe
```python
def pot_do_osebe(oseba, cilj, rodovnik):
    if oseba == cilj:
        return [oseba]
    for otrok in rodovnik[oseba]:
        pot = pot_do_osebe(otrok, cilj, rodovnik)
        if pot:
            return [oseba] + pot
    return []

# Primer uporabe
print(pot_do_osebe("Adam", "Petra", rodovnik))  # Vrne ['Adam', 'Daniel', 'Elizabeta', 'Jožef', 'Petra']
```


Primer za izračun faktoriela:
```python
def faktoriel(n):
    if n == 0:
        return 1
    return n * faktoriel(n - 1)

print(faktoriel(5))  # 120
```
Posebnosti rekurzije:
- Vedno imejte **bazični primer** (npr. `n == 0`).
- Previdno z globino rekurzije (lahko pride do napake `RecursionError`).
### 4. Objektno programiranje
#### razredi in objekti

```python
class Vozilo:
    def __init__(self, hitrost):
        self.hitrost = hitrost

    def premakni(self):
        return f"Premikam se s hitrostjo {self.hitrost} km/h."

class Avto(Vozilo):
    def __init__(self, hitrost, znamka):
        super().__init__(hitrost)
        self.znamka = znamka

avto = Avto(120, "Toyota")
print(avto.premakni())
```
#### posebnosti

- **`__init__`**: Konstruktor, kliče se ob ustvarjanju objekta.
- **`self`**: Referenca na trenutni objekt.
- **`super`**: Kličemo razred starša
### 6. Dodatne funkcije

#### preverjanje tipa spremenljivke

```python
x = 10
if isinstance(x, int):
    print("x je celo število.")
```


#### zip funkcija

Združevanje seznamov:
```python
imena = ["Ana", "Bojan"]
ocene = [10, 9]
for ime, ocena in zip(imena, ocene):
    print(f"{ime}: {ocena}")
```

#### enumerate

Omogoča iteracijo z indeksi:
```python
fruits = ['jabolko', 'banana', 'češnja']
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
```

#### pairwise

Omogoča iteracijo v parih:
```python
from itertools import pairwise
nums = [1, 2, 3, 4]
for a, b in pairwise(nums):
    print(a, b)
```

#### random

```python
import random
print(random.randint(1, 6))

# randrange(start, stop, step)
print(random.randrange(0, 10, 2))
```

##### choice(seznam)
Vrne naključni element iz sekvence (seznam, niz, itd.).
choices(seq, weights=None, k=1)
- Izbere k elementov z vračanjem, pri čemer lahko določiš uteži za izbor.

```python
colors = ['rdeča', 'modra', 'zelena']
print(random.choice(colors))
```

##### sample(seq)
sample(population, k)
- Vrne seznam k naključnih elementov brez vračanja.
numbers = [1, 2, 3, 4, 5]
print(random.sample(numbers, 3))

#### shuffle(seq)
Premeša elemente v seznamu (in situ, vrne None).
```python
numbers = [1, 2, 3, 4, 5]
random.shuffle(numbers)
print(numbers)
```

### 5. Seznami


#### aritmeticne operacije

```python
# Povezovanje seznamov: Seznama lahko povežeš s pomočjo operatorja +.
list1 = [1, 2, 3]
list2 = [4, 5, 6]
combined = list1 + list2
print(combined)  # [1, 2, 3, 4, 5, 6]

# Ponovno ponavljanje seznamov: Seznam lahko ponoviš večkrat z operatorjem *
my_list = [1, 2, 3]
repeated = my_list * 2
print(repeated)  # [1, 2, 3, 1, 2, 3]
```

#### odstranjevanje

##### discard()

Množice (set) v Pythonu uporabljajo metodo discard() za odstranjevanje elementov:
- Če element obstaja, ga odstrani.
- Če element ne obstaja, ne sproži izjeme (razlika od metode remove).

```python
# Uporaba metode discard
numbers_set = {1, 2, 3, 4}
numbers_set.discard(2)  # Odstrani 2, če obstaja
print(numbers_set)  # {1, 3, 4}

numbers_set.discard(10)  # Ne sproži izjeme, če element ne obstaja
print(numbers_set)  # {1, 3, 4}
```

##### remove(value)

Odstrani prvi pojav določenega elementa iz seznama.
Če element ne obstaja, sproži izjemo ValueError.
```python
numbers = [1, 2, 3, 2, 4]
numbers.remove(2)  # Odstrani prvi pojav 2
print(numbers)  # [1, 3, 2, 4]

# Če element ne obstaja:
# numbers.remove(10)  # ValueError: list.remove(x): x not in list
```

##### pop(index)

Odstrani element na določenem indeksu in ga vrne.
- Če je indeks izpuščen, odstrani in vrne zadnji element.
- Če indeks ni veljaven, sproži izjemo IndexError.

```python
numbers = [1, 2, 3, 4]
last_item = numbers.pop()  # Odstrani zadnji element
print(numbers)  # [1, 2, 3]
print(last_item)  # 4

second_item = numbers.pop(1)  # Odstrani element na indeksu 1
print(numbers)  # [1, 3]
print(second_item)  # 2
```

##### del

Odstrani element na določenem indeksu ali celoten seznam.
```python
numbers = [1, 2, 3, 4]
del numbers[1]  # Odstrani element na indeksu 1
print(numbers)  # [1, 3, 4]

# Odstranitev celotnega seznama
del numbers
```

#### dodajanje

##### append()

- Dodaja en element na konec seznama kot celoto.
- Če dodaš iterabilen objekt (npr. seznam, niz), bo celoten objekt dodan kot en sam element.

```python
# Dodajanje enega elementa
seznam = [1, 2, 3]
seznam.append(4)  # Doda 4 kot en element
print(seznam)  # [1, 2, 3, 4]

# Dodajanje seznama kot enega elementa
seznam.append([5, 6])  # Celoten seznam [5, 6] postane en element
print(seznam)  # [1, 2, 3, 4, [5, 6]]
```
##### insert()

Dodajanje elementa na določeni poziciji

```python
my_list.insert(1, "new")
print(my_list)  # [1, 'new', 2, 3, 4, 5]
```

##### extend()

- Dodaja elemente iz iterabilnega objekta (npr. seznama, niza) posamično na konec seznama.
- Elementi iterabilnega objekta se "razširijo" in dodajo posamezno.

```python
# Razširitev seznama z elementi drugega seznama
seznam = [1, 2, 3]
seznam.extend([4, 5])  # Doda 4 in 5 kot posamezne elemente
print(seznam)  # [1, 2, 3, 4, 5]

# Razširitev seznama z elementi iz niza
seznam.extend("AB")  # Doda 'A' in 'B' kot posamezne elemente
print(seznam)  # [1, 2, 3, 4, 5, 'A', 'B']
```

#### iskanje

##### index()

- Uporablja se za iskanje prvega indeksa določenega elementa v seznamu.
- Če element ni v seznamu, sproži izjemo ValueError.

element: Element, ki ga iščemo.
start: Začetni indeks iskanja (privzeto 0).
end: Končni indeks iskanja (privzeto dolžina seznama).

```python
list.index(element, start=0, end=len(list))

# Iskanje indeksa elementa
seznam = [10, 20, 30, 40, 50, 20]
print(seznam.index(20))  # 1 (prvi pojav 20)

# Iskanje z določenim začetkom in koncem
print(seznam.index(20, 2))  # 5 (najde 20 od 2. indeksa dalje)

# Če element ne obstaja
# print(seznam.index(60))  # ValueError: 60 is not in list
```

##### count()
Preštej, kolikokrat se določen element pojavi v seznamu.

```python
my_list = [1, 2, 3, 1, 1]
print(my_list.count(1))  # 3
```

#### urejanje (sortiranje)

##### sort()

Uredi seznam v naraščajočem vrstnem redu (spremeni seznam v mestu).

```python
my_list = [4, 2, 3, 1]
my_list.sort()
print(my_list)  # [1, 2, 3, 4]

seznam.sort(reverse=True)
print(seznam)  # [9, 5, 2, 1]
```

##### reverse() 

Obrne vrstni red elementov v seznamu.

```python
my_list.reverse()
print(my_list)  # [4, 3, 2, 1]
```

#### seznam razumov (List Comprehensions)

```python
kvadrati = [x**2 for x in range(1, 6)]
print(kvadrati)  # [1, 4, 9, 16, 25]
```

### 7. Uporabljene funkcije

#### `all` in `any`

```python
# Preveri, ali so vsi elementi resnični
print(all([True, True, False]))  # False

# Preveri, ali je vsaj en element resničen
print(any([False, False, True]))  # True
```
#### range()

Deluje na principu vstavljanja: range(start, stop, step)
```python
for i in range(10, 0, -2):
    print(i)
```
#### split()

Razdeli niz na seznam podnizov glede na določen ločilni znak (privzeto presledek).
- `separator` (neobvezno): Ločilni znak, po katerem se niz deli. Privzeto je presledek (' ').
- `maxsplit` (neobvezno): Največje število delitev. Privzeto -1 (ni omejitve).

```python
text = "Python je odličen!"
words = text.split()
print(words)
```

#### strip()

Lahko mu tudi podamo znak
```python
text = "   Python   "
cleaned = text.strip()
print(cleaned)
# Izhod: 'Python'
```
### 8. Odpiranje datoteke
#### pisanje v datoteko

```python
# Pisanje v datoteko (prepiše vsebino, če datoteka obstaja)
with open("primer.txt", "w") as file:
    file.write("To je prva vrstica.\n")
    file.write("To je druga vrstica.\n")
```

#### branje datoreke

```python
# Z while:
with open("primer.txt", "r") as file:
    line = file.readline()  # Preberi prvo vrstico
    while line:  # Nadaljuj, dokler obstajajo vrstice
        print(line.strip())
        line = file.readline()  # Preberi naslednjo vrstico

# Z with in for:
with open("primer.txt", "r") as file:
    for line in file:
        print(line.strip())  # Uporaba strip za odstranitev \n
```

#### obstoj datoteke

Preverjanje obstoja datoteke: Pred branjem lahko preveriš, ali datoteka obstaja, z uporabo modula os ali pathlib.

```python
import os
if os.path.exists("primer.txt"):
    with open("primer.txt", "r") as file:
        print(file.read())
else:
    print("Datoteka ne obstaja.")
```
### 9. Nizi


#### iskanje

##### find()

- Uporablja se za iskanje prvega indeksa podniza v nizu.
- Če podniz ne obstaja, vrne -1 (ne sproži izjeme kot index()).

substring: Podniz, ki ga iščemo.
start: Začetni indeks iskanja (privzeto 0).
end: Končni indeks iskanja (privzeto dolžina niza).

```python
string.find(substring, start=0, end=len(string))

# Iskanje v nizu
text = "Python programming"
print(text.find("pro"))  # 7 (indeks začetka "pro")
print(text.find("z"))    # -1 (podniza "z" ni)

# Iskanje z določenim obsegom
print(text.find("o", 5))  # 7 (najde "o" po 5. indeksu)
```

#### urejanje

- lower(): Pretvori vse znake v male črke.
- upper(): Pretvori vse znake v velike črke.
- capitalize(): Pretvori prvo črko v veliko, ostale v male.
- title(): Pretvori prvo črko vsake besede v veliko.
- swapcase(): Zamenja velike črke z malimi in obratno.
- center(): Poravna niz na sredino, znotraj določenega širine.
- ljust() / rjust(): Poravna niz na levo ali desno, znotraj določene širine.

```python
s = "hello"
print(s.center(10, "*"))  # '**hello***'

s = "hello"
print(s.ljust(10, "*"))  # 'hello*****'
print(s.rjust(10, "*"))  # '*****hello'
```


Odstranjevanje iz niza:
- strip(): Odstrani presledke ali določene znake z začetka in konca.
- lstrip(): Odstrani znake samo z začetka.
- rstrip(): Odstrani znake samo s konca.

Uporabne metode:
- find(substring): Poišče indeks prvega podniza (vrne -1, če ne obstaja).
- replace(old, new): Zamenja vse pojavitve podniza z drugim.
- count(substring): Prešteje pojavitve določenega podniza.
- split(separator): Razdeli niz glede na separator in vrne seznam.
- join(iterable): Združi elemente iterabilnega objekta (npr. seznam) v niz.

Preveranje vsebine:
- startswith(prefix): Preveri, ali se niz začne z določenim nizom.
- endswith(suffix): Preveri, ali se niz konča z določenim nizom.
- isalnum(), isalpha(), isdigit(), isspace(), itd.: Preverijo vsebino niza.

#### isalnum()

 Preveri, ali niz vsebuje samo alfanumerične znake (črke in števke) ter nima nobenih presledkov ali posebnih znakov.
- Vrne True, če so vsi znaki v nizu alfanumerični (vsaj en znak mora biti prisoten).
- Vrne False, če vsebuje karkoli drugega (npr. presledke, ločila, itd.).

```python
text1 = "Python3"
text2 = "Python 3"
text3 = "12345"
text4 = "Hello!"

print(text1.isalnum())  # True (vse so črke in števke)
print(text2.isalnum())  # False (ima presledek)
print(text3.isalnum())  # True (vse so števke, kar je dovoljeno)
print(text4.isalnum())  # False (ima poseben znak '!')
```

#### isnumeric()

Opis: Preveri, ali niz vsebuje samo števke (numerične znake). Sprejema tudi določene posebne numerične znake, kot so eksponenti ali znaki za razlomke.
- Vrne True, če so vsi znaki numerični.
- Vrne False, če vsebuje karkoli drugega (črke, presledke, posebne znake).

```python
num1 = "12345"
num2 = "123.45"
num3 = "⅔"  # Razlomek (Unicode)
num4 = "123abc"

print(num1.isnumeric())  # True (vse so števke)
print(num2.isnumeric())  # False (vsebuje piko, ki ni števka)
print(num3.isnumeric())  # True (Unicode znak za razlomke je numeričen)
print(num4.isnumeric())  # False (vsebuje črke)
```

#### slicing

```python
s = "hello"
print(s[1:4])  # 'ell'
print(s[:3])   # 'hel'
print(s[3:])   # 'lo'
```
### 10. Mnozice
Množice so zbirke, ki ne dovolijo ponavljajočih se elementov in so neurejene.
Pomembne metode za delo z množicami:

#### prazna množica

empty_set = set()
print(empty_set)  # set()

#### pomembne metode

##### add()

- Dodajanje elementa v množico
```python
my_set.add(4)
print(my_set)  # {1, 2, 3, 4}
```

##### remove()

- Odstrani element iz množice (vrne napako, če element ni prisoten)
```python
my_set.remove(2)
print(my_set)  # {1, 3, 4}
```

##### discard()

- Odstrani element iz množice, če obstaja
```python
my_set.discard(5)  # Ne vrne napake, če element ni prisoten.
print(my_set)  # {1, 3, 4}
```

##### pop()

- Odstrani in vrne en naključen element iz množice
```python
removed = my_set.pop()
print(removed)  # Naključen element
print(my_set)
```

##### clear()

- Odstrani vse elemente iz množice
```python
my_set.clear()
print(my_set)  # set()
```

#### operacije z mnozicami

set1 = {1, 2, 3}
set2 = {3, 4, 5}

```python
# Unija (|) - Združitev dveh množic
union_set = set1 | set2
print(union_set)  # {1, 2, 3, 4, 5}

# Presek (&) - Prvi skupni element v obeh množicah
intersection_set = set1 & set2
print(intersection_set)  # {3}

# Razlika (-) - Elementi, ki so v prvi množici, vendar ne v drugi
difference_set = set1 - set2
print(difference_set)  # {1, 2}

# Simetrična razlika (^) - Elementi, ki so samo v eni od množic
symmetric_difference_set = set1 ^ set2
print(symmetric_difference_set)  # {1, 2, 4, 5}
```


### 11. Slovarji (Dictionaries)


Slovarji so podatkovne strukture, ki shranijo podatke v obliki parov ključ-vrednost.

#### ustvarjanje slovarjev

```python
my_dict = {"name": "John", "age": 25, "city": "New York"}
print(my_dict)  # {'name': 'John', 'age': 25, 'city': 'New York'}
```

#### pomembne metode

##### get() 

- Poišče vrednost, ki pripada določenemu ključu. Če ključ ne obstaja, vrne None ali drugo privzeto vrednost
```python
print(my_dict.get("name"))  # John
print(my_dict.get("address", "Unknown"))  # Unknown
```

###### update()

- Posodobi slovar z novimi ključi in vrednostmi
```python
my_dict.update({"age": 26, "address": "123 Street"})
print(my_dict)  # {'name': 'John', 'age': 26, 'city': 'New York', 'address': '123 Street'}
```

###### keys()

- Vrne seznam vseh ključev v slovarju
```python
print(my_dict.keys())  # dict_keys(['name', 'age', 'city', 'address'])
```

###### values()

- Vrne seznam vseh vrednosti v slovarju
```python
print(my_dict.values())  # dict_values(['John', 26, 'New York', '123 Street'])
```

###### items()

- Vrne seznam parov ključ-vrednost
```python
print(my_dict.items())  # dict_items([('name', 'John'), ('age', 26), ('city', 'New York'), ('address', '123 Street')])
```

###### pop()

- Odstrani in vrne vrednost, ki pripada določenemu ključu
```python
address = my_dict.pop("address")
print(address)  # 123 Street
print(my_dict)  # {'name': 'John', 'age': 26, 'city': 'New York'}
```

###### popitem()

- Odstrani in vrne zadnji par ključ-vrednost
```python
last_item = my_dict.popitem()
print(last_item)  # ('city', 'New York')
print(my_dict)  # {'name': 'John', 'age': 26}
```

###### clear()

- Odstrani vse elemente iz slovarja
```python
my_dict.clear()
print(my_dict)  # {}
```

#### določene posebnosti

Ključi slovarjev morajo biti ne-spremenljive (immutable) tipe
```python
valid_dict = {1: "one", "name": "Alice", (1, 2): "tuple"}
# invalid_dict = {[1, 2]: "list"}  # Napaka: seznam ni mogoč kot ključ
```
### Regex

Osnovna Sintaksa Regularnih Izrazov

- `[]`: Seznam znakov (match-a kateri koli znak znotraj oglatih oklepajev).
  - [a-z]: Ujema se z vsemi malimi črkami.
  - [0-9]: Ujema se z vsemi številkami.
  - [^a-z]: Ujema se z vsemi znaki, ki niso male črke.

- `()`: Skupina, uporablja se za določanje delov vzorca, ki jih želimo ujeti (capture groups).
  - `(abc)`: Ujema se z besedo "abc" in jo ujame kot skupino.

- `.`: Ujema se z katerim koli znakom, razen nove vrstice.

- `^`: Ujema se z začetkom besedila (na začetku vzorca).
  - `^abc`: Se ujema z besedilom, ki se začne z "abc".

- `$`: Ujema se z koncem besedila (na koncu vzorca).
  - `abc$`: Se ujema z besedilom, ki se konča z "abc".

- `*`: Ujema se z nič ali več ponovitvami prejšnjega elementa.
  - `a*`: Se ujema z nič ali več "a".

- `+`: Ujema se z eno ali več ponovitvami prejšnjega elementa.
  - `a+`: Se ujema z eno ali več "a".

- `?`: Ujema se z nič ali eno ponovitvijo prejšnjega elementa.
  - `a?`: Se ujema z nič ali eno "a".

- `{m,n}`: Ujema se z vsaj m in največ n ponovitvami prejšnjega elementa.
  - `a{2,4}`: Se ujema z "aa", "aaa" ali "aaaa".

- `|`: Ali (or), omogoča izbiro med več vzorci.
  - `abc|def`: Ujema se z "abc" ali "def".

Primeri Uporabe Regex-a

1. Iskanje telefonskih številk

Poiščimo vse telefonske številke v besedilu. Oblika številke je lahko npr. "123-456-7890".
```python
import re

text = "Moji telefoni so 123-456-7890 in 987-654-3210."
result = re.findall(r'\d{3}-\d{3}-\d{4}', text)
print(result)  # ['123-456-7890', '987-654-3210']
```
2. Preverjanje elektronskega naslova

Preverimo, ali besedilo vsebuje veljaven elektronski naslov.
```python
email = "user@example.com"
if re.match(r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$', email):
    print("Veljaven e-naslov")
else:
    print("Neveljaven e-naslov")
```
3. Zamenjava besedila (substitucija)

Zamenjajmo vse številke v besedilu z "#".
```python
text = "Moja številka je 123456 in tvoja je 654321."
new_text = re.sub(r'\d', '#', text)
print(new_text)  # Moja številka je ###### in tvoja je ######
```
4. Razdelitev besedila po presledkih

Razdelimo besedilo na besede, ki so ločene s presledki.
```python
text = "To je primer besedila"
words = re.split(r'\s+', text)
print(words)  # ['To', 'je', 'primer', 'besedila']
```
Uporabni Vzorce

`^\d{3}-\d{2}-\d{4}$`: Ujema se s številko socialne varnosti v obliki "123-45-6789".

`\b\w+\b`: Ujema se z vsemi besedami (beseda = niz, ki je sestavljen iz alfanumeričnih znakov).

`\d{2,4}`: Ujema se z dvema do štirimi številkami.

`[A-Za-z0-9]+`: Ujema se z nizom, ki vsebuje samo črke in številke.

Primer Regex-a v Jupyter Notebooku

import re

#### iskanje vseh stevilk v besedilu
```python
text = "Račun: 100, Popust: 20, Skupaj: 80"
numbers = re.findall(r'\d+', text)
print(numbers)  # ['100', '20', '80']
```
#### zamenjava stevilk z "#"
```python
new_text = re.sub(r'\d', '#', text)
print(new_text)  # Račun: ###, Popust: ##, Skupaj: ##
```
