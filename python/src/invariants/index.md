# Raonament amb invariants

Dissenyar un bucle vol dir decidir com s'inicialitzen les variables, quina condició el fa continuar, què fa el cos a cada volta i com es pot saber que acabarà. Un **invariant** és la propietat que lliga aquestes quatre decisions.

Aquesta lliçó presenta el raonament amb invariants, una manera sistemàtica de dissenyar bucles i de convèncer-se que fan el que han de fer. L'apliquem a quatre exemples: el factorial, capgirar els dígits d'un nombre, classificar els elements d'una llista i fusionar dues llistes ordenades.

## Què és un invariant

Un invariant d'un bucle és una proposició sobre les variables del programa que es compleix:

- just abans de començar el bucle,
- al començament de cada iteració,
- al final de cada iteració.

Cada vegada que l'execució és a punt de comprovar la condició del `while`, l'invariant és cert. Dins del cos pot deixar de ser-ho un moment, però el cos l'ha de tornar a deixar establert abans de tornar a la condició.

L'invariant serveix per a quatre coses:

- diu com cal inicialitzar les variables abans del bucle,
- diu quina condició cal per arribar, en sortir, a la postcondició (allò que volem que sigui cert quan el bucle ha acabat),
- diu què ha de fer el cos del bucle,
- ajuda a detectar si el bucle acaba.

Triar-ne un de bo no sempre és fàcil, i és la part important del disseny. Convé raonar amb invariants en tots els bucles, ni que sigui de manera informal, i escriure'l explícitament quan el bucle no és trivial.

## Esquema general d'un bucle

El patró de qualsevol bucle raonat amb un invariant és aquest:

```python
# inicialització
# Invariant
while condició:
    # Invariant i condició
    # cos del bucle
    # Invariant
# Invariant i no condició
```

Hi ha quatre obligacions:

1. **Inicialització.** Les assignacions d'abans del bucle han de fer que l'invariant es compleixi.
2. **Cos.** Si a l'entrada del cos es compleixen l'invariant i la condició, al final del cos l'invariant ha de tornar a complir-se.
3. **Sortida.** L'invariant, juntament amb el fet que la condició ja no es compleix, ha d'implicar la postcondició. Això és el que fixa la condició del `while`.
4. **Terminació.** Cal una quantitat entera que decreixi a cada iteració i que no pugui decréixer per sempre. Llavors el bucle acaba.

Una manera pràctica de trobar l'invariant és aturar mentalment el programa al final del cos, mirar les variables i descriure què és veritat del seu contingut. Si aquesta descripció es manté a cada volta, és l'invariant.

<img src='./foto.png' alt='n és el prefix blau encara per tractar, r és el revés del sufix vermell, i la juxtaposició dels dos trossos de dalt és m.' style='width: 34em; max-width: 100%; display: block; margin: 1.2em auto;'/>

## El factorial

Donat un enter $n \ge 0$, volem calcular $n!$. El factorial es defineix així:

$$
n! = 1 \cdot 2 \cdot 3 \cdot \ldots \cdot n,
$$

amb el cas particular $0! = 1$.

Triem aquest invariant: a cada iteració mantindrem

$$
f = i! \quad\text{i}\quad i \le n.
$$

La inicialització en surt de seguida. Volem $f = i!$ amb $i \le n$, i sabem que $0! = 1$. La precondició (allò que podem suposar de l'entrada) diu que $n \ge 0$, així que podem posar `i = 0` i `f = 1`.

La condició del bucle surt de la postcondició. En acabar volem $f = n!$. Si l'invariant ens dóna $f = i!$ i $i \le n$, n'hi ha prou que en sortir es compleixi $i = n$. Per tant la condició del `while` és `i != n`: mentre sigui certa encara no hem acabat, i quan deixa de ser-ho l'invariant obliga $i = n$ i, amb ell, $f = n!$.

El cos ha de restaurar l'invariant. Si fem `i = i + 1`, el nou valor d'`i` és una unitat més gran i el factorial que li correspon és l'antic multiplicat per aquest `i` nou. Cal, doncs, incrementar primer i multiplicar després:

```python
i = i + 1
f = f * i
```

El bucle acaba perquè `i` comença a 0, creix d'un en un i la precondició assegura $n \ge 0$: `i` no pot avançar per sempre sense arribar a `n`.

Tot plegat:

```python
def factorial(n: int) -> int:
    """Retorna n!. Precondició: n ≥ 0."""

    i = 0
    f = 1
    # Invariant: f = i! i i ≤ n
    while i != n:
        # f = i! i i < n
        i = i + 1
        f = f * i
        # f = i! i i ≤ n
    # f = i! i i = n, per tant f = n!
    return f
```

## Capgirar els dígits

Volem una funció que, donat un enter $m \ge 0$, en retorni un altre amb els mateixos dígits en base 10 escrits del revés.

|   `m` | resultat |
| ----: | -------: |
| 35276 |    67253 |
|    19 |       91 |
|     3 |        3 |
|     0 |        0 |
|   100 |        1 |

La idea és anar traient l'últim dígit del que queda del nombre i afegir-lo al final del resultat. Per descriure on som a mitja feina fan falta dues variables: `n`, amb els dígits que encara no hem tractat, i `r`, amb els dígits ja extrets, escrits del revés.

L'invariant és gràfic:

<img src='./capgira-digits.png' alt='n és el prefix blau encara per tractar, r és el revés del sufix vermell, i la juxtaposició dels dos trossos de dalt és m.' style='width: 34em; max-width: 100%; display: block; margin: 1.2em auto;'/>

Els dígits de `m` són els dígits de `n` (el tros blau) seguits dels dígits ja extrets. Aquests dígits extrets es dibuixen en vermell en l'ordre original, `xyz`, just darrere de `n`, de manera que els dos trossos de dalt reconstrueixen `m`. La variable `r` no els guarda en aquest ordre: en guarda el revés, `zyx`. El tros vermell de dalt no és una variable; només fa visible la relació entre `m`, `n` i `r`.

Quan `n` val 0 ja no queda cap dígit per extreure, així que `r` és `m` escrit del revés. La condició del bucle és, per tant, `n != 0`.

Al començament no hem extret res: `n` és tot `m` i `r` és 0, que aquí vol dir «cap dígit», no pas el dígit 0. El cos treu l'últim dígit de `n` i l'afegeix al final de `r`:

```python
r = 10 * r + n % 10
n = n // 10
```

`n` és un enter no negatiu i a cada volta es divideix per 10, de manera que arriba a 0 i el bucle acaba.

```python
def capgira_digits(m: int) -> int:
    """Retorna l'enter obtingut escrivint els dígits de m del revés.
    Precondició: m ≥ 0.
    """

    n, r = m, 0
    # Invariant: els dígits de m són els de n seguits del revés dels de r
    while n != 0:
        r = 10 * r + n % 10
        n = n // 10
    return r
```

## Classificar els elements d'una llista

Tenim una llista `L` i un interval `[x, y]`, amb $x \le y$. Volem reordenar `L` de manera que els elements menors que `x` quedin a la part esquerra, els majors que `y` a la part dreta, i els de l'interval al mig. Dins de cada part, l'ordre no importa.

Per a l'interval `[6, 9]` i la llista `[15, 7, 3, 0, 10, 6, 11, 9, 1, 13]`, un resultat possible és `[1, 3, 0, 7, 9, 6, 11, 10, 13, 15]`: els tres primers són menors que 6, els tres següents cauen dins de `[6, 9]` i els quatre últims són majors que 9.

Per anar classificant sense fer servir una llista auxiliar, mantindrem quatre zones:

<img src='./classificacio.png' alt='La llista queda partida en quatre zones: més petits, dins de [x, y], per classificar i més grans, amb els índexs esq, mig i dre als límits.' style='width: 42em; max-width: 100%; display: block; margin: 1.2em auto;'/>

Els índexs `esq`, `mig` i `dre` marquen els límits de les zones:

- `L[0:esq]` conté elements estrictament menors que `x`,
- `L[esq:mig]` conté elements de l'interval `[x, y]`,
- `L[mig:dre + 1]` conté elements encara per classificar,
- `L[dre + 1:]` conté elements estrictament majors que `y`.

Això és l'invariant. La fletxa `dre` és al límit dret de la zona pendent: `dre` és l'índex de l'últim element per classificar, i tot el que queda a la seva dreta ja és més gran que `y`.

Al començament tota la llista està per classificar, així que `esq = mig = 0` i `dre = len(L) - 1`. Acabem quan la zona pendent és buida, és a dir, quan `mig > dre`. La condició del bucle és, doncs, `mig <= dre`.

A cada volta mirem l'element `L[mig]`, el primer dels pendents:

- Si és menor que `x`, l'intercanviem amb `L[esq]` i avancem `esq` i `mig`. L'element petit queda a la zona de l'esquerra. Si la zona del mig no era buida, l'element que hi havia a `esq` ja era de l'interval i, en avançar `mig`, continua dins d'aquesta zona. Si era buida, `esq` i `mig` coincideixen i l'intercanvi no mou res.
- Si és major que `y`, l'intercanviem amb `L[dre]` i reculem `dre`. L'element gran passa a la zona de la dreta. L'element que arriba a la posició `mig` encara no l'hem mirat, i per això `mig` no avança.
- Si cau dins de `[x, y]`, no es mou. Només avancem `mig`, i la zona del mig el va absorbint.

Cada volta fa créixer `mig` o fa decréixer `dre`, de manera que la distància `dre - mig` baixa. El bucle acaba.

```python
def classifica(L: list[int], x: int, y: int) -> None:
    """Reordena L: a l'esquerra els elements menors que x,
    a la dreta els majors que y, i al mig la resta.
    Precondició: x ≤ y.
    """

    esq, mig, dre = 0, 0, len(L) - 1
    # Invariant: vegeu les quatre zones de la figura
    while mig <= dre:
        if L[mig] < x:
            L[mig], L[esq] = L[esq], L[mig]
            esq, mig = esq + 1, mig + 1
        elif L[mig] > y:
            L[mig], L[dre] = L[dre], L[mig]
            dre = dre - 1
        else:
            mig = mig + 1
```

Sobre l'exemple anterior, aquesta funció deixa exactament `[1, 3, 0, 7, 9, 6, 11, 10, 13, 15]`.

## Fusió de dues llistes ordenades

Volem una funció que rebi dues llistes ordenades de manera creixent i en retorni la fusió, també ordenada. Per exemple, la fusió de

```text
A = [-9, -7, 0, 1, 3, 4]
B = [-8, -7, 1, 2, 2, 4, 5]
```

és

```text
C = [-9, -8, -7, -7, 0, 1, 1, 2, 2, 3, 4, 4, 5]
```

Com que `A` i `B` ja estan ordenades, la fusió es pot fer en una sola passada, amb dos índexs `i` i `j` sobre les dues llistes.

Aquest és un instant intermedi de l'algorisme. Els elements blaus ja s'han consumit; els vermells encara no:

<img src='./fusio-invariant.png' alt='A té i al límit entre els elements ja fusionats i els pendents, B té j al mateix límit, i C conté la fusió del prefix blau.' style='width: 36em; max-width: 100%; display: block; margin: 1.2em auto;'/>

L'invariant té dues parts:

- `C` és la fusió ordenada de `A[0:i]` i `B[0:j]`,
- tots els elements blaus són més petits o iguals que tots els vermells.

A la figura, `i` val 2 i `j` val 1: la fusió de `[-9, -7]` i `[-8]` és justament `[-9, -8, -7]`. El blau més gran és $-7$ i el vermell més petit també és $-7$, de manera que cap element pendent pot haver d'anar davant del que ja hi ha a `C`.

Per això el següent element de `C` és el més petit entre `A[i]` i `B[j]`. S'afegeix a `C` i s'avança l'índex corresponent. L'invariant es manté, perquè les dues llistes estan ordenades: tot el que queda en una llista és més gran o igual que el seu primer element pendent, i aquest era més gran o igual que tot el que ja hem posat a `C`.

El bucle continua mentre `i` i `j` apuntin tots dos a un element. Quan un dels dos arriba al final, el tros que queda de l'altra llista ja està ordenat i és més gran o igual que tot `C`, i es pot afegir d'un cop. Cada volta avança `i` o `j`, així que una de les dues llistes s'acaba.

```python
def fusio(A: list[int], B: list[int]) -> list[int]:
    """Retorna la fusió ordenada d'A i de B.
    Precondició: A i B estan ordenades de manera creixent.
    """

    C: list[int] = []
    i, j = 0, 0
    # Invariant: C és la fusió de A[0:i] i B[0:j],
    # i els elements ja consumits són ≤ que els pendents
    while i < len(A) and j < len(B):
        if A[i] <= B[j]:
            C.append(A[i])
            i = i + 1
        else:
            C.append(B[j])
            j = j + 1
    C.extend(A[i:])
    C.extend(B[j:])
    return C
```

Al començament `i` i `j` valen 0 i `C` és buida: la fusió de dos prefixos buits és la llista buida, i encara no hi ha cap element blau. En sortir del bucle, un dels dos trossos `A[i:]` o `B[j:]` és buit, de manera que les dues crides a `extend` només afegeixen el que realment queda.

## Com trobar un bon invariant

Els invariants són una manera efectiva d'obtenir algorismes iteratius correctes. Per trobar-ne un de bo en un bucle nou, aquest ordre funciona bé:

1. Penseu com avança l'algorisme, volta a volta.
2. Descriviu l'estat del programa al començament d'una iteració qualsevol. Aquesta descripció és l'invariant.
3. Declareu les variables que fan falta per escriure'l.
4. Deduïu-ne la inicialització, la condició i el cos. L'ordre en què ho feu no és important: cadascun ha de complir l'obligació que li toca a l'esquema general, i cal comprovar que el bucle acaba.

<Autors autors="jordic jpetit"/>
