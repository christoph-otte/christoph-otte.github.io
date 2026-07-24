---
layout: post
title: Klassen und objektorientierte Programmierung in Python
tags: [python, programmierung]
published: false
---

Objektorientierte Programmierung (OOP) strukturiert Code um Objekte herum: Datenstrukturen, die sowohl Zustand (Attribute) als auch Verhalten (Methoden) vereinen. Python unterstützt dieses Paradigma vollständig, ohne es zu erzwingen – man kann es einsetzen, wo es Sinn ergibt.

Eine **Klasse** ist die Vorlage, ein **Objekt** (oder eine Instanz) die konkrete Ausprägung. Als Beispiel modellieren wir einen zweidimensionalen Vektor:

```python
class Vektor:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def laenge(self):
        return (self.x**2 + self.y**2) ** 0.5

    def __repr__(self):
        return f"Vektor({self.x}, {self.y})"

v = Vektor(3, 4)
print(v)          # Vektor(3, 4)
print(v.laenge()) # 5.0
```

Die Methode `__init__` ist der Konstruktor: Sie wird beim Erstellen eines Objekts aufgerufen und initialisiert dessen Attribute. `self` verweist auf die jeweilige Instanz. `__repr__` definiert die Textdarstellung des Objekts – eines von vielen sogenannten *dunder methods* (double underscore), mit denen Python-Klassen nahtlos in die Sprache integriert werden.

Weitere dunder methods erlauben es, mathematische Operatoren zu überladen:

```python
class Vektor:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vektor(self.x + other.x, self.y + other.y)

    def __mul__(self, skalar):
        return Vektor(self.x * skalar, self.y * skalar)

    def __repr__(self):
        return f"Vektor({self.x}, {self.y})"

a = Vektor(1, 2)
b = Vektor(3, 4)
print(a + b)   # Vektor(4, 6)
print(a * 3)   # Vektor(3, 6)
```

Durch `__add__` und `__mul__` lassen sich Instanzen der Klasse mit `+` und `*` verknüpfen, als wären es eingebaute Typen.

**Vererbung** erlaubt es, eine Klasse auf Basis einer anderen zu definieren. Die abgeleitete Klasse erbt alle Attribute und Methoden der Elternklasse und kann sie erweitern oder überschreiben:

```python
class Form:
    def __init__(self, farbe):
        self.farbe = farbe

    def beschreibung(self):
        return f"Eine {self.farbe} Form"

class Kreis(Form):
    def __init__(self, farbe, radius):
        super().__init__(farbe)
        self.radius = radius

    def flaeche(self):
        import math
        return math.pi * self.radius**2

    def beschreibung(self):
        return f"Ein {self.farbe} Kreis mit Radius {self.radius}"

k = Kreis("blauer", 5)
print(k.beschreibung())  # Ein blauer Kreis mit Radius 5
print(f"{k.flaeche():.2f}")  # 78.54
```

`super().__init__(farbe)` ruft den Konstruktor der Elternklasse auf. So muss `Kreis` die Logik für `farbe` nicht duplizieren. Die Methode `beschreibung` wird überschrieben (*overriding*): Der Aufruf `k.beschreibung()` verwendet die Version der Unterklasse.

In modernem Python empfiehlt sich die Verwendung von `@dataclass` für einfache Datenklassen. Der Dekorator generiert automatisch `__init__`, `__repr__` und `__eq__`:

```python
from dataclasses import dataclass, field

@dataclass
class Punkt:
    x: float
    y: float
    bezeichnung: str = "P"

    def abstand_zum_ursprung(self):
        return (self.x**2 + self.y**2) ** 0.5

p1 = Punkt(3.0, 4.0, "A")
p2 = Punkt(3.0, 4.0, "A")
print(p1)          # Punkt(x=3.0, y=4.0, bezeichnung='A')
print(p1 == p2)    # True – __eq__ wird automatisch generiert
print(p1.abstand_zum_ursprung())  # 5.0
```

Die Typannotationen (`x: float`) sind für `@dataclass` notwendig, dienen aber auch der Dokumentation. Der generierte `__eq__` vergleicht alle Felder, sodass `p1 == p2` hier `True` ergibt – etwas, das bei gewöhnlichen Klassen ohne eigenen `__eq__` nicht der Fall wäre.

OOP ist kein Selbstzweck. Sinnvoll eingesetzt modelliert es reale Zusammenhänge präzise und hält Code wartbar; übertrieben angewendet führt es zu unnötiger Komplexität. Python lässt die Wahl – und das ist einer seiner Stärken.
