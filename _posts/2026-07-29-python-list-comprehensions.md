---
layout: post
title: Python List Comprehensions und Generatoren
tags: [python, programmierung]
published: false
---

Python bietet eine elegante Syntax, um Sequenzen zu erzeugen und zu transformieren: List Comprehensions und Generatoren. Beide Konzepte erlauben es, kompakten und lesbaren Code zu schreiben, unterscheiden sich aber grundlegend in ihrer Funktionsweise.

Eine **List Comprehension** erzeugt sofort eine vollständige Liste im Arbeitsspeicher. Die Syntax folgt dem Muster `[ausdruck for element in iterable if bedingung]`:

```python
# Alle geraden Quadratzahlen von 1 bis 20
quadrate = [x**2 for x in range(1, 11) if x % 2 == 0]
print(quadrate)  # [4, 16, 36, 64, 100]
```

Der Ausdruck `x**2` wird auf jedes `x` angewendet, das die Bedingung `x % 2 == 0` erfüllt. In einer klassischen Schleife würde dasselbe vier Zeilen erfordern; die Comprehension fasst es in eine lesbare Zeile.

List Comprehensions lassen sich auch verschachteln, etwa um eine zweidimensionale Matrix zu glätten:

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flach = [element for zeile in matrix for element in zeile]
print(flach)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Die äußere Schleife iteriert über die Zeilen, die innere über die Elemente jeder Zeile – die Reihenfolge in der Comprehension entspricht der Reihenfolge geschachtelter for-Schleifen.

Ein **Generator** hingegen berechnet die Werte nicht sofort, sondern erst dann, wenn sie angefordert werden. Dies spart Speicher bei großen oder potenziell unendlichen Folgen. Syntaktisch unterscheidet sich ein Generator-Ausdruck von einer List Comprehension nur durch runde statt eckige Klammern:

```python
# Generator für die ersten 10 Millionen Quadratzahlen
gen = (x**2 for x in range(10_000_000))

# Nur der erste Wert wird berechnet – kein Speicherproblem
print(next(gen))  # 0
print(next(gen))  # 1
```

Mit `next()` wird jeweils der nächste Wert aus dem Generator abgerufen. Der gesamte Bereich von zehn Millionen Zahlen existiert nie gleichzeitig im Speicher.

Noch mächtiger sind **Generatorfunktionen**, die das Schlüsselwort `yield` verwenden. Sie pausieren ihre Ausführung bei jedem `yield` und setzen beim nächsten Aufruf genau dort fort:

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
erste_zehn = [next(fib) for _ in range(10)]
print(erste_zehn)  # [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

Die Funktion `fibonacci()` definiert eine unendliche Folge, ohne dass diese jemals vollständig berechnet wird. Jeder Aufruf von `next()` führt die Funktion bis zum nächsten `yield` aus und gibt den Wert zurück.

Die Wahl zwischen Liste und Generator hängt vom Anwendungsfall ab: Wenn alle Werte mehrfach gebraucht werden oder der Index wichtig ist, ist eine Liste sinnvoll. Sollen große Datenmengen der Reihe nach verarbeitet werden, ist ein Generator die sparsamere Wahl.
