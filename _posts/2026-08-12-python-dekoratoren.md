---
layout: post
title: Dekoratoren in Python
tags: [python, programmierung]
published: false
---

Ein Dekorator ist eine Funktion, die eine andere Funktion entgegennimmt, ihr Verhalten erweitert und eine neue Funktion zurückgibt. Das Konzept beruht darauf, dass Funktionen in Python erstklassige Objekte sind: Sie können anderen Funktionen als Argument übergeben, in Variablen gespeichert oder von Funktionen zurückgegeben werden.

Das Grundprinzip lässt sich an einem einfachen Beispiel zeigen. Angenommen, man möchte messen, wie lange eine Funktion zur Ausführung braucht:

```python
import time

def zeitmessung(funktion):
    def huelle(*args, **kwargs):
        start = time.perf_counter()
        ergebnis = funktion(*args, **kwargs)
        ende = time.perf_counter()
        print(f"{funktion.__name__} dauerte {ende - start:.4f} Sekunden")
        return ergebnis
    return huelle
```

Die Funktion `zeitmessung` nimmt eine Funktion entgegen und gibt `huelle` zurück – eine neue Funktion, die die ursprüngliche umschließt und zusätzlich die Zeit misst. `*args` und `**kwargs` sorgen dafür, dass beliebige Argumente an die ursprüngliche Funktion weitergereicht werden.

Python bietet hierfür eine eigene Syntax mit dem `@`-Symbol:

```python
@zeitmessung
def langsame_summe(n):
    return sum(range(n))

ergebnis = langsame_summe(10_000_000)
# langsame_summe dauerte 0.2341 Sekunden
```

Die Zeile `@zeitmessung` ist gleichbedeutend mit `langsame_summe = zeitmessung(langsame_summe)`. Der Dekorator wird einmal beim Definieren der Funktion angewendet – nicht bei jedem Aufruf.

Ein häufiges Problem beim Dekorieren ist, dass die ursprüngliche Funktion ihre Metadaten verliert: `langsame_summe.__name__` würde nun `huelle` zurückgeben. Dies lässt sich mit `functools.wraps` beheben:

```python
import functools

def zeitmessung(funktion):
    @functools.wraps(funktion)
    def huelle(*args, **kwargs):
        start = time.perf_counter()
        ergebnis = funktion(*args, **kwargs)
        ende = time.perf_counter()
        print(f"{funktion.__name__} dauerte {ende - start:.4f} Sekunden")
        return ergebnis
    return huelle
```

`@functools.wraps(funktion)` kopiert den Namen, den Docstring und andere Attribute der ursprünglichen Funktion auf die Hülle.

Dekoratoren können auch parametrisiert werden. Dann muss man eine zusätzliche Ebene einziehen – eine Funktion, die die Parameter entgegennimmt und erst dann den eigentlichen Dekorator zurückgibt:

```python
def wiederholen(n):
    def dekorator(funktion):
        @functools.wraps(funktion)
        def huelle(*args, **kwargs):
            for _ in range(n):
                ergebnis = funktion(*args, **kwargs)
            return ergebnis
        return huelle
    return dekorator

@wiederholen(3)
def begruessung(name):
    print(f"Hallo, {name}!")

begruessung("Ada")
# Hallo, Ada!
# Hallo, Ada!
# Hallo, Ada!
```

Mehrere Dekoratoren lassen sich stapeln. Sie werden von innen nach außen angewendet, also von unten nach oben in der Schreibweise:

```python
@zeitmessung
@wiederholen(5)
def berechnung():
    return sum(range(100_000))
```

Hier wird `berechnung` zuerst mit `wiederholen(5)` dekoriert, dann mit `zeitmessung`. Der Zeitgeber misst also die Gesamtdauer aller fünf Wiederholungen.

Dekoratoren sind in der Python-Standardbibliothek allgegenwärtig: `@property` macht eine Methode zu einem Attribut, `@staticmethod` und `@classmethod` verändern die Bindung von Methoden an Klassen, und `@lru_cache` aus `functools` fügt automatisch einen Ergebnis-Cache hinzu.
