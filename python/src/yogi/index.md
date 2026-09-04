# Lectura de dades amb `yogi`

<img src='./lectura.png' style='height: 10em; float: right; margin: 2em 0 1em 1em;'/>

La lectura de dades en Python és, admetem-ho, massa difícil per als
principiants. Per solucionar-ho, es pot usar el mòdul
[`yogi`](https://github.com/jutge-org/yogi). Aquest petit mòdul
ofereix funcions senzilles però potents per llegir dades des de Python i és molt útil per resoldre els
problemes de [Jutge.org](https:/jutge.org).

Aquesta lliçó explica com instal·lar i utilitzar el mòdul `yogi`.

## Instal·lació

-   Instal·leu amb `python3 -m pip install yogi`.

## Ús bàsic

La funció `read()` de `yogi` retorna el següent element (_token_) de l'entrada.
El tipus d'aquest element s'ha de donar com a paràmetre i pot ser un d'aquests tres:

-   `read(int)` retorna el següent enter,
-   `read(float)` retorna el següent real, i
-   `read(str)` retorna la següent paraula.

Els elements es separen per paraules. Els blancs i salts de línia no es poden obtenir ja que `read()` els ignora. Si l'entrada no té més elements, `read()` provoca una desagradable excepció.

La funció `scan()` de `yogi` també retorna el següent element (_token_) de l'entrada, però en el cas que aquesta acabi (o que el valor llegit no sigui compatible amb el demanat), retorna `None`. Igual que `read()`, cal demanar a `scan()` què es vol llegir:

-   `scan(int)` retorna el següent enter o `None` si no hi és,
-   `scan(float)` retorna el següent real, o `None` si no hi és, i
-   `scan(str)` retorna la següent paraula o `None` si no hi és.

A més, es pot usar el generador `tokens()` per anar obtenint el següent element de l'entrada o acabar el bucle quan ja no n'ha més.

## Exemples

### Programa que saluda

```python
from yogi import read

print('Com et dius?')
nom = read(str)
print('Hola', nom)
```

Fixeu-vos que amb aquest programa s'aconsegueix la primera paraula escrita per l'usuari,
amb independència dels blancs o salts de línia que tingui davant o darrera. Això no es podria
haver fet senzillament amb `input()`!

### Programa que llegeix dos nombres i n'escriu la seva suma

```python
from yogi import read

x = read(int)
y = read(int)
print(x + y)
```

### Programa que llegeix una seqüència de reals i n'escriu la seva suma (amb `scan()`)

```python
from yogi import scan

s = 0.0
x = scan(float)
while x is not None:
    s = s + x
    x = scan(float)
print(s)
```

Fixeu-vos que quan s'usa un terminal, cal senyalar el final de l'entrada amb
<kbd>Control</kbd> + <kbd>d</kbd> a Linux i Mac o amb <kbd>Control</kbd> +
<kbd>z</kbd> a Windows.

### Programa que llegeix una seqüència de reals i n'escriu la seva suma (amb `tokens()`)

El programa anterior seria més fàcil d'escriure utilitzant `tokens()`:

```python
from yogi import tokens

s = 0.0
for x in tokens(float):
    s = s + x
print(s)
```

## Informació addicional

Es pot trobar una informació més exhaustiva sobre aquest mòdul a [la seva
documentació](https://github.com/jutge-org/yogi/blob/master/README.md).

<Autors autors="jpetit"/>
