# Les Fonctions Usuelles en Python

## Table des matières

- [Introduction](#introduction)
- [Manipulation avancée de chaînes de caractères](#manipulation-avancée-de-chaînes-de-caractères)
  - [Méthodes principales des chaînes](#méthodes-principales-des-chaînes)
  - [Formatage de chaînes](#formatage-de-chaînes)
  - [Expressions régulières](#expressions-régulières)
  - [Fonctions de traitement textuel](#fonctions-de-traitement-textuel)
- [Opérations sur les listes](#opérations-sur-les-listes)
  - [Méthodes de base](#méthodes-de-base)
  - [Fonctions de haut niveau](#fonctions-de-haut-niveau)
  - [Compréhensions de liste avancées](#compréhensions-de-liste-avancées)
  - [Opérations sur plusieurs listes](#opérations-sur-plusieurs-listes)
- [Techniques pour dictionnaires](#techniques-pour-dictionnaires)
  - [Méthodes essentielles](#méthodes-essentielles)
  - [DefaultDict et Counter](#defaultdict-et-counter)
  - [Fusion et comparaison](#fusion-et-comparaison)
  - [Dictionnaires ordonnés](#dictionnaires-ordonnés)
- [Ensembles et opérations ensemblistes](#ensembles-et-opérations-ensemblistes)
  - [Création et modification](#création-et-modification)
  - [Opérations ensemblistes](#opérations-ensemblistes)
  - [Cas d'utilisation courants](#cas-dutilisation-courants)
- [Fonctions pour tuples et sequences immuables](#fonctions-pour-tuples-et-sequences-immuables)
  - [Méthodes disponibles](#méthodes-disponibles)
  - [Named tuples](#named-tuples)
  - [Utilisations avancées](#utilisations-avancées)
- [Fonctions générales sur les itérables](#fonctions-générales-sur-les-itérables)
  - [Fonctions de base (any, all, len)](#fonctions-de-base-any-all-len)
  - [Tri et recherche](#tri-et-recherche)
  - [Agrégation et statistiques](#agrégation-et-statistiques)
- [Techniques de conversion de types](#techniques-de-conversion-de-types)
  - [Conversions numériques](#conversions-numériques)
  - [Convertir entre structures de données](#convertir-entre-structures-de-données)
  - [Sérialisation et désérialisation](#sérialisation-et-désérialisation)
- [Fonctions pour la gestion de fichiers](#fonctions-pour-la-gestion-de-fichiers)
  - [Lecture et écriture de fichiers texte](#lecture-et-écriture-de-fichiers-texte)
  - [Manipulation de fichiers binaires](#manipulation-de-fichiers-binaires)
  - [Gestion des chemins de fichiers](#gestion-des-chemins-de-fichiers)
- [Fonctions mathématiques](#fonctions-mathématiques)
  - [Module math](#module-math)
  - [Fonctions numériques intégrées](#fonctions-numériques-intégrées)
  - [Arrondis et troncatures](#arrondis-et-troncatures)
  - [Opérations statistiques](#opérations-statistiques)
- [Fonctions du module itertools](#fonctions-du-module-itertools)
  - [Itérateurs infinis](#itérateurs-infinis)
  - [Combinatoires](#combinatoires)
  - [Chaînage et groupement](#chaînage-et-groupement)
- [Fonctions du module functools](#fonctions-du-module-functools)
  - [Cache et mémoïsation](#cache-et-mémoïsation)
  - [Fonctions partielles](#fonctions-partielles)
  - [Réduction et plis](#réduction-et-plis)
- [Fonctions du module collections](#fonctions-du-module-collections)
  - [Structures de données spécialisées](#structures-de-données-spécialisées)
  - [Comptage et regroupement](#comptage-et-regroupement)
- [Bonnes pratiques](#bonnes-pratiques)
- [Erreurs courantes](#erreurs-courantes)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

Les fonctions usuelles en Python sont un ensemble d'outils essentiels qui permettent de manipuler efficacement les données et de réaliser des opérations courantes. Elles constituent la boîte à outils de tout développeur Python et permettent d'écrire un code concis, lisible et performant.

Ce chapitre présente les fonctions les plus utiles et fréquemment utilisées pour manipuler les types de données fondamentaux de Python : chaînes de caractères, listes, dictionnaires, ensembles, et plus encore. Une bonne maîtrise de ces fonctions est essentielle pour écrire un code Python élégant et efficace. Plutôt que de réinventer la roue, un bon développeur Python sait tirer parti de ces fonctions intégrées et des modules de la bibliothèque standard.

Nous explorerons également des techniques avancées et des modules spécialisés qui offrent des fonctionnalités puissantes pour des tâches spécifiques. En fin de compte, ce chapitre vise à élargir votre arsenal de techniques pour résoudre efficacement des problèmes courants en programmation Python.

## Manipulation avancée de chaînes de caractères

Les chaînes de caractères sont l'un des types de données les plus couramment manipulés en programmation. Python offre de nombreuses méthodes et fonctions pour travailler efficacement avec le texte.

### Méthodes principales des chaînes

Python offre de nombreuses méthodes intégrées pour manipuler les chaînes de caractères :

```python
# Transformation de casse
text = "Python Programming"
print(text.upper())          # 'PYTHON PROGRAMMING'
print(text.lower())          # 'python programming'
print(text.title())          # 'Python Programming'
print(text.capitalize())     # 'Python programming'
print(text.swapcase())       # 'pYTHON pROGRAMMING'

# Recherche et remplacement
text = "Python is amazing and Python is powerful"
print(text.count("Python"))  # 2 (nombre d'occurrences)
print(text.find("Python"))   # 0 (première position)
print(text.rfind("Python"))  # 21 (dernière position)
print(text.replace("Python", "Ruby", 1))  # 'Ruby is amazing and Python is powerful'

# Vérification de contenu
print("Python".startswith("Py"))  # True
print("Python".endswith("on"))    # True
print("Python".isalpha())         # True (uniquement des lettres)
print("Python3".isalpha())        # False (contient des chiffres)
print("Python3".isalnum())        # True (alphanumérique)
print("123".isdigit())            # True (uniquement des chiffres)
print("  ".isspace())             # True (uniquement des espaces)

# Manipulation de structure
text = "  Python  "
print(text.strip())          # 'Python' (supprime les espaces au début et à la fin)
print(text.lstrip())         # 'Python  ' (supprime les espaces au début)
print(text.rstrip())         # '  Python' (supprime les espaces à la fin)

words = "apple,banana,orange"
print(words.split(","))      # ['apple', 'banana', 'orange']

lines = "line1\nline2\nline3"
print(lines.splitlines())    # ['line1', 'line2', 'line3']

parts = ["Python", "is", "awesome"]
print(" ".join(parts))       # 'Python is awesome'
print("-".join(parts))       # 'Python-is-awesome'

# Alignement et remplissage
text = "Python"
print(text.center(10, '*'))  # '**Python**'
print(text.ljust(10, '-'))   # 'Python----'
print(text.rjust(10, '0'))   # '0000Python'
print("42".zfill(5))         # '00042' (remplissage à gauche avec des zéros)
```

### Formatage de chaînes

Python propose plusieurs méthodes pour formater les chaînes, notamment les chaînes formatées (f-strings) introduites dans Python 3.6, qui sont désormais la méthode recommandée :

```python
# 1. Formatage avec f-strings (Python 3.6+)
name = "Alice"
age = 30
print(f"Hello, {name}. You are {age} years old.")
# Expressions dans les f-strings
print(f"In 5 years, you'll be {age + 5} years old.")
# Formatage des nombres
pi = 3.14159
print(f"Pi rounded to 2 decimals: {pi:.2f}")  # 3.14
# Alignement avec f-strings
for i in range(1, 4):
    print(f"{i:2} - {i*i:3} - {i*i*i:4}")

# 2. Méthode format()
print("Hello, {}. You are {} years old.".format(name, age))
# Avec des indices
print("Hello, {0}. You are {1} years old. {0} is a nice name.".format(name, age))
# Avec des noms
print("Hello, {name}. You are {age} years old.".format(name="Bob", age=25))
# Formatage avancé
print("Binary: {0:b}, Octal: {0:o}, Hexadecimal: {0:x}".format(42))

# 3. Formatage de style C (ancien style, mais toujours utilisé)
print("Hello, %s. You are %d years old." % (name, age))
# Formatage des nombres
print("Pi: %.2f" % pi)  # Pi: 3.14
```

Le formatage des nombres mérite une attention particulière :

```python
# Formatage des nombres avec f-strings
num = 1234.56789

# Arrondi
print(f"{num:.2f}")      # 1234.57

# Séparateur de milliers
print(f"{num:,.2f}")     # 1,234.57

# Signe et zéros en préfixe
print(f"{num:+010.2f}")  # +001234.57

# Pourcentage
print(f"{0.25:.1%}")     # 25.0%

# Notation scientifique
print(f"{num:.2e}")      # 1.23e+03

# Binaire, octal et hexadécimal
n = 42
print(f"{n:b}")          # 101010 (binaire)
print(f"{n:#b}")         # 0b101010 (binaire avec préfixe)
print(f"{n:o}")          # 52 (octal)
print(f"{n:x}")          # 2a (hexadécimal)
print(f"{n:#x}")         # 0x2a (hexadécimal avec préfixe)
```

### Expressions régulières

Le module `re` est utilisé pour travailler avec des expressions régulières, qui sont puissantes pour la recherche et la manipulation de texte selon des motifs complexes :

```python
import re

text = "Contact: email@example.com and another.email@domain.co.uk"

# Recherche simple
if re.search(r"example\.com", text):
    print("Le domaine example.com a été trouvé")

# Trouver toutes les occurrences
emails = re.findall(r"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}", text)
print(emails)  # ['email@example.com', 'another.email@domain.co.uk']

# Substitution
censored = re.sub(r"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}", "[EMAIL REDACTED]", text)
print(censored)  # Contact: [EMAIL REDACTED] and [EMAIL REDACTED]

# Extraction avec groupes
pattern = r"([a-zA-Z0-9._%+-]+)@([a-zA-Z0-9.-]+\.[a-zA-Z]{2,})"
matches = re.finditer(pattern, text)
for match in matches:
    print(f"User: {match.group(1)}, Domain: {match.group(2)}")

# Compilation d'expressions régulières pour la réutilisation
email_pattern = re.compile(r"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}")
emails = email_pattern.findall(text)
print(emails)

# Options des expressions régulières
case_insensitive = re.search(r"EXAMPLE\.COM", text, re.IGNORECASE)
print(case_insensitive.group())  # example.com

# Remplacement avec fonction
def censor_domain(match):
    user, domain = match.groups()
    return f"{user}@[DOMAIN REDACTED]"

result = re.sub(pattern, censor_domain, text)
print(result)  # Contact: email@[DOMAIN REDACTED] and another.email@[DOMAIN REDACTED]
```

Quelques motifs d'expressions régulières courants :

```python
# Expressions régulières courantes
patterns = {
    "Email": r"[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}",
    "URL": r"https?://(?:[-\w.]|(?:%[\da-fA-F]{2}))+[/\w\.-]*",
    "IP address": r"\b(?:\d{1,3}\.){3}\d{1,3}\b",
    "Date (yyyy-mm-dd)": r"\d{4}-\d{2}-\d{2}",
    "Phone number": r"\+?\d{1,3}[-.\s]?\(?\d{1,4}\)?[-.\s]?\d{1,4}[-.\s]?\d{1,9}",
    "HTML tag": r"<[^>]+>",
    "Python comment": r"#.*?$"
}

# Exemple de texte contenant plusieurs éléments à détecter
sample = """
Visit our website at https://example.com for more information.
Contact us at support@example.com or call +1 (555) 123-4567.
The server IP is 192.168.1.1 and was last updated on 2023-04-15.
<!-- This is an HTML comment --> and # This is a Python comment
"""

# Tester chaque motif sur l'exemple
for name, pattern in patterns.items():
    matches = re.findall(pattern, sample, re.MULTILINE)
    if matches:
        print(f"{name}: {matches}")
```

### Fonctions de traitement textuel

Python dispose également de modules spécialisés pour le traitement de texte :

```python
# Module string - constantes utiles
import string

print(string.ascii_lowercase)  # 'abcdefghijklmnopqrstuvwxyz'
print(string.ascii_uppercase)  # 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
print(string.digits)           # '0123456789'
print(string.punctuation)      # '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'
print(string.whitespace)       # ' \t\n\r\x0b\x0c'

# Module textwrap - formatage de paragraphes
import textwrap

long_text = "This is a very long text that needs to be wrapped to fit within a certain width. Python provides the textwrap module to handle this kind of text processing efficiently."

# Wrap à 40 caractères
wrapped = textwrap.wrap(long_text, width=40)
for line in wrapped:
    print(line)

# Alternative avec fill (retourne une seule chaîne)
print(textwrap.fill(long_text, width=40))

# Indentation
print(textwrap.fill(long_text, width=40, initial_indent="    ", subsequent_indent="  "))

# Troncature
print(textwrap.shorten(long_text, width=50, placeholder="..."))  # Tronque à 50 caractères

# Module difflib - comparaison de texte
import difflib

text1 = "Python is an amazing programming language."
text2 = "Python is a powerful programming language."

# Comparer deux chaînes
d = difflib.Differ()
diff = d.compare(text1.splitlines(), text2.splitlines())
print('\n'.join(diff))

# Trouver la similitude
s = difflib.SequenceMatcher(None, text1, text2)
print(f"Ratio de similitude : {s.ratio():.2f}")  # Un ratio entre 0 et 1

# Module unicodedata - traitement Unicode
import unicodedata

# Normalisation de caractères Unicode
s1 = 'café'  # é comme caractère unique
s2 = 'cafe\u0301'  # e suivi d'un accent combinant

print(s1 == s2)  # False
print(unicodedata.normalize('NFC', s1) == unicodedata.normalize('NFC', s2))  # True

# Informations sur les caractères
for char in "Python★":
    print(f"{char}: {unicodedata.name(char, 'Unknown')}")
```

## Opérations sur les listes

Les listes sont l'une des structures de données les plus polyvalentes en Python, et il existe de nombreuses techniques pour les manipuler efficacement.

### Méthodes de base

```python
# Création et manipulation de base
fruits = ["apple", "banana", "cherry"]
fruits.append("orange")      # Ajoute un élément à la fin
fruits.insert(1, "blueberry")  # Insère à l'indice spécifié
fruits.extend(["grape", "kiwi"])  # Ajoute plusieurs éléments
print(fruits)  # ['apple', 'blueberry', 'banana', 'cherry', 'orange', 'grape', 'kiwi']

# Suppression d'éléments
fruits.remove("banana")      # Supprime par valeur
popped = fruits.pop()        # Supprime et retourne le dernier élément
popped_index = fruits.pop(1)  # Supprime et retourne l'élément à l'index spécifié
print(popped)                # kiwi
print(popped_index)          # blueberry
print(fruits)                # ['apple', 'cherry', 'orange', 'grape']

# Recherche
fruits = ["apple", "banana", "cherry", "banana"]
print(fruits.index("banana"))       # 1 (première occurrence)
print(fruits.index("banana", 2))    # 3 (commence la recherche à l'index 2)
print(fruits.count("banana"))       # 2 (nombre d'occurrences)

# Tri et inversion
numbers = [3, 1, 4, 1, 5, 9, 2, 6]
numbers.sort()                # Tri en place
print(numbers)                # [1, 1, 2, 3, 4, 5, 6, 9]
numbers.sort(reverse=True)    # Tri décroissant
print(numbers)                # [9, 6, 5, 4, 3, 2, 1, 1]
numbers.reverse()             # Inverse l'ordre
print(numbers)                # [1, 1, 2, 3, 4, 5, 6, 9]

# Copie de liste
original = [1, 2, 3]
copy1 = original.copy()       # Méthode 1
copy2 = list(original)        # Méthode 2
copy3 = original[:]           # Méthode 3
original.append(4)
print(original)               # [1, 2, 3, 4]
print(copy1)                  # [1, 2, 3] (non affecté)
```

### Fonctions de haut niveau

```python
# Fonctions intégrées pour les listes
numbers = [1, 2, 3, 4, 5]
print(sum(numbers))           # 15 (somme des éléments)
print(max(numbers))           # 5 (valeur maximale)
print(min(numbers))           # 1 (valeur minimale)
print(len(numbers))           # 5 (nombre d'éléments)
print(sorted(numbers, reverse=True))  # [5, 4, 3, 2, 1] (nouveau tri)

# Zip pour combiner des listes
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
for name, age in zip(names, ages):
    print(f"{name} is {age} years old")

# Énumération pour obtenir les indices
for i, name in enumerate(names):
    print(f"Index {i}: {name}")

# Énumération avec décalage (commencer à 1 au lieu de 0)
for i, name in enumerate(names, 1):
    print(f"Person {i}: {name}")

# Liste à partir d'itérations
numbers = list(range(1, 6))   # [1, 2, 3, 4, 5]
chars = list("Python")        # ['P', 'y', 't', 'h', 'o', 'n']

# Filter - filtrer selon un prédicat
even = list(filter(lambda x: x % 2 == 0, numbers))
print(even)                   # [2, 4]

# Map - appliquer une fonction à chaque élément
squares = list(map(lambda x: x ** 2, numbers))
print(squares)                # [1, 4, 9, 16, 25]

# Any/All - vérifier des conditions
print(any(x > 3 for x in numbers))  # True (au moins un élément > 3)
print(all(x > 0 for x in numbers))  # True (tous les éléments > 0)
```

### Compréhensions de liste avancées

Les compréhensions de liste sont une fonctionnalité puissante pour créer et transformer des listes de manière concise :

```python
# Compréhension de liste simple
squares = [x ** 2 for x in range(1, 6)]
print(squares)  # [1, 4, 9, 16, 25]

# Avec condition (filtre)
even_squares = [x ** 2 for x in range(1, 11) if x % 2 == 0]
print(even_squares)  # [4, 16, 36, 64, 100]

# Avec condition if-else (transformation conditionnelle)
numbers = [1, 2, 3, 4, 5]
result = ["even" if x % 2 == 0 else "odd" for x in numbers]
print(result)  # ['odd', 'even', 'odd', 'even', 'odd']

# Compréhensions imbriquées (matrice)
matrix = [[i * j for j in range(1, 4)] for i in range(1, 4)]
print(matrix)  # [[1, 2, 3], [2, 4, 6], [3, 6, 9]]

# Aplatir une liste de listes
nested = [[1, 2], [3, 4], [5, 6]]
flattened = [item for sublist in nested for item in sublist]
print(flattened)  # [1, 2, 3, 4, 5, 6]

# Compréhension avec zip
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
name_ages = [f"{name} is {age}" for name, age in zip(names, ages)]
print(name_ages)  # ['Alice is 25', 'Bob is 30', 'Charlie is 35']

# Compréhension avec dictionnaires
word = "python"
letter_count = {letter: word.count(letter) for letter in set(word)}
print(letter_count)  # {'p': 1, 'y': 1, 't': 1, 'h': 1, 'o': 1, 'n': 1}

# Filtrage avancé avec plusieurs conditions
numbers = list(range(1, 20))
filtered = [x for x in numbers if x % 2 == 0 and x % 3 == 0]
print(filtered)  # [6, 12, 18]

# Transformation complexe
words = ["apple", "banana", "cherry", "date"]
processed = [
    f"{word.upper()}: {len(word)}"
    for word in words
    if len(word) > 5
]
print(processed)  # ['BANANA: 6', 'CHERRY: 6']
```

Les compréhensions de liste sont souvent plus rapides et plus lisibles que les boucles `for` équivalentes, mais il faut veiller à ne pas créer des expressions trop complexes qui nuiraient à la lisibilité.

### Opérations sur plusieurs listes

```python
# Fusion de listes
list1 = [1, 2, 3]
list2 = [4, 5, 6]
merged = list1 + list2                # Concaténation
print(merged)                         # [1, 2, 3, 4, 5, 6]

# Opération élément par élément avec zip
summed = [a + b for a, b in zip(list1, list2)]
print(summed)                         # [5, 7, 9]

# Intersection et différence (conversion en ensembles)
set1 = set([1, 2, 3, 4])
set2 = set([3, 4, 5, 6])
print(list(set1 & set2))              # [3, 4] (intersection)
print(list(set1 - set2))              # [1, 2] (différence)

# Entrelacement de deux listes
def interleave(list1, list2):
    result = []
    for a, b in zip(list1, list2):
        result.extend([a, b])
    return result

print(interleave([1, 3, 5], [2, 4, 6]))  # [1, 2, 3, 4, 5, 6]

# Produit cartésien
import itertools
colors = ["red", "green"]
sizes = ["S", "M", "L"]
combinations = list(itertools.product(colors, sizes))
print(combinations)  # [('red', 'S'), ('red', 'M'), ('red', 'L'), ('green', 'S'), ('green', 'M'), ('green', 'L')]

# Transposition de matrices
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
transposed = list(zip(*matrix))  # Astuce avec * pour décompresser
print(transposed)  # [(1, 4, 7), (2, 5, 8), (3, 6, 9)]
```

## Techniques pour dictionnaires

Les dictionnaires sont des structures de données clé-valeur très puissantes en Python.

### Méthodes essentielles

```python
# Création et accès
user = {
    "name": "Alice",
    "age": 30,
    "email": "alice@example.com"
}

# Accès aux valeurs
print(user["name"])           # Alice
print(user.get("age"))        # 30
print(user.get("address"))    # None (clé inexistante)
print(user.get("address", "Unknown"))  # Unknown (valeur par défaut)

# Modification
user["age"] = 31
user.update({"email": "alice.new@example.com", "address": "123 Main St"})
print(user)  # Dictionnaire mis à jour

# Suppression
email = user.pop("email")     # Supprime et retourne la valeur
print(email)                  # alice.new@example.com
del user["address"]           # Suppression directe
user.clear()                  # Vide le dictionnaire
print(user)                   # {}

# Méthodes de dictionnaire
user = {"name": "Bob", "age": 25, "active": True}
print(user.keys())            # dict_keys(['name', 'age', 'active'])
print(user.values())          # dict_values(['Bob', 25, True])
print(user.items())           # dict_items([('name', 'Bob'), ('age', 25), ('active', True)])

# Copie
copy1 = user.copy()           # Copie superficielle
copy2 = dict(user)            # Autre façon de copier

# Itération
for key in user:
    print(f"{key}: {user[key]}")

for key, value in user.items():
    print(f"{key}: {value}")

# setdefault - obtenir une valeur ou définir une valeur par défaut
email = user.setdefault("email", "default@example.com")
print(email)                  # default@example.com
print(user)                   # Inclut maintenant 'email' avec valeur par défaut
```

### DefaultDict et Counter

Le module `collections` offre des variantes spécialisées de dictionnaires :

```python
from collections import defaultdict, Counter

# defaultdict - fournit une valeur par défaut pour les clés manquantes
# Exemple : compter les occurrences de lettres dans une chaîne
letter_counts = defaultdict(int)  # Valeur par défaut: 0
for char in "mississippi":
    letter_counts[char] += 1
print(dict(letter_counts))  # {'m': 1, 'i': 4, 's': 4, 'p': 2}

# Grouper des éléments
animals = [
    ("cat", "mammal"),
    ("dog", "mammal"),
    ("snake", "reptile"),
    ("lizard", "reptile"),
    ("eagle", "bird"),
    ("sparrow", "bird")
]

animal_groups = defaultdict(list)
for animal, group in animals:
    animal_groups[group].append(animal)

print(dict(animal_groups))
# {'mammal': ['cat', 'dog'], 'reptile': ['snake', 'lizard'], 'bird': ['eagle', 'sparrow']}

# Counter - spécialisé pour compter les occurrences
inventory = Counter(["apple", "orange", "apple", "banana", "apple", "orange"])
print(inventory)  # Counter({'apple': 3, 'orange': 2, 'banana': 1})

# Méthodes spécifiques à Counter
print(inventory.most_common(2))  # [('apple', 3), ('orange', 2)]

# Opérations mathématiques
inventory2 = Counter(["apple", "pear", "banana", "pear"])
print(inventory + inventory2)  # Combine les compteurs
print(inventory - inventory2)  # Soustrait (conserve uniquement les positifs)
```

### Fusion et comparaison

```python
# Fusion de dictionnaires (Python 3.5+)
dict1 = {"a": 1, "b": 2}
dict2 = {"b": 3, "c": 4}

# Avant Python 3.9
merged = {**dict1, **dict2}  # {'a': 1, 'b': 3, 'c': 4}
print(merged)

# Python 3.9+
merged = dict1 | dict2       # {'a': 1, 'b': 3, 'c': 4}
print(merged)

# Mise à jour (Python 3.9+)
dict1 |= dict2
print(dict1)                 # {'a': 1, 'b': 3, 'c': 4}

# Fusion avec une logique personnalisée
def merge_with_sum(dict1, dict2):
    result = dict1.copy()
    for key, value in dict2.items():
        if key in result:
            result[key] += value
        else:
            result[key] = value
    return result

d1 = {"a": 1, "b": 2, "c": 3}
d2 = {"b": 5, "c": 7, "d": 9}
print(merge_with_sum(d1, d2))  # {'a': 1, 'b': 7, 'c': 10, 'd': 9}

# Comparaison de dictionnaires
dict1 = {"a": 1, "b": 2}
dict2 = {"b": 2, "a": 1}
print(dict1 == dict2)        # True (même contenu, ordre n'importe pas)

# Différence de clés
dict1 = {"a": 1, "b": 2, "c": 3}
dict2 = {"b": 2, "c": 3, "d": 4}
only_in_dict1 = dict1.keys() - dict2.keys()  # {'a'}
only_in_dict2 = dict2.keys() - dict1.keys()  # {'d'}
common_keys = dict1.keys() & dict2.keys()    # {'b', 'c'}
```

### Dictionnaires ordonnés

Depuis Python 3.7, les dictionnaires préservent l'ordre d'insertion des éléments :

```python
# Dictionnaire ordonné (par défaut depuis Python 3.7)
ordered = {"first": 1, "second": 2, "third": 3, "fourth": 4}
for key, value in ordered.items():
    print(f"{key}: {value}")  # L'ordre d'insertion est préservé

# Tri de dictionnaire par clés
sorted_by_key = dict(sorted(ordered.items()))
print(sorted_by_key)

# Tri par valeurs
sorted_by_value = dict(sorted(ordered.items(), key=lambda item: item[1]))
print(sorted_by_value)

# Tri par valeurs en ordre décroissant
sorted_desc = dict(sorted(ordered.items(), key=lambda item: item[1], reverse=True))
print(sorted_desc)

# OrderedDict (collections) - pour la compatibilité avec les versions Python < 3.7
from collections import OrderedDict
od = OrderedDict([("first", 1), ("second", 2), ("third", 3)])
```

## Ensembles et opérations ensemblistes

Les ensembles sont des collections non ordonnées d'éléments uniques, idéales pour les tests d'appartenance et les opérations mathématiques d'ensemble.

### Création et modification

```python
# Création d'ensembles
fruits = {"apple", "banana", "cherry"}  # Syntaxe littérale
colors = set(["red", "green", "blue"])  # À partir d'une liste
vowels = set("aeiou")                  # À partir d'une chaîne

# Ensemble vide (doit utiliser set() car {} crée un dictionnaire vide)
empty_set = set()

# Ajout et suppression d'éléments
fruits.add("orange")          # Ajoute un élément
fruits.update(["grape", "kiwi"])  # Ajoute plusieurs éléments
print(fruits)

fruits.remove("banana")       # Supprime un élément (lève une erreur si absent)
fruits.discard("watermelon")  # Supprime si présent (sinon ne fait rien)
popped = fruits.pop()         # Supprime et retourne un élément (arbitraire)
print(popped)
print(fruits)

fruits.clear()                # Vide l'ensemble
print(fruits)                 # set()
```

### Opérations ensemblistes

```python
# Opérations ensemblistes de base
A = {1, 2, 3, 4, 5}
B = {4, 5, 6, 7, 8}

# Union - éléments dans A OU B
union1 = A | B                # Opérateur
union2 = A.union(B)           # Méthode
print(union1)                 # {1, 2, 3, 4, 5, 6, 7, 8}

# Intersection - éléments dans A ET B
intersection1 = A & B         # Opérateur
intersection2 = A.intersection(B)  # Méthode
print(intersection1)          # {4, 5}

# Différence - éléments dans A mais pas dans B
difference1 = A - B           # Opérateur
difference2 = A.difference(B)  # Méthode
print(difference1)            # {1, 2, 3}

# Différence symétrique - éléments dans A ou B mais pas les deux
sym_diff1 = A ^ B             # Opérateur
sym_diff2 = A.symmetric_difference(B)  # Méthode
print(sym_diff1)              # {1, 2, 3, 6, 7, 8}

# Sous-ensemble et sur-ensemble
C = {1, 2}
print(C.issubset(A))          # True (C ⊆ A)
print(A.issuperset(C))        # True (A ⊇ C)
print(A.isdisjoint(C))        # False (A ∩ C ≠ ∅)
print(A.isdisjoint({10, 11}))  # True (pas d'éléments communs)

# Opérations avec mise à jour (modifie en place)
D = {1, 2, 3}
E = {3, 4, 5}
D |= E                        # Union avec mise à jour
print(D)                      # {1, 2, 3, 4, 5}

D = {1, 2, 3}
D &= E                        # Intersection avec mise à jour
print(D)                      # {3}
```

### Cas d'utilisation courants

```python
# Élimination des doublons dans une liste
duplicates = [1, 2, 2, 3, 4, 4, 5]
unique = list(set(duplicates))
print(unique)                 # [1, 2, 3, 4, 5]

# Appartenance (très efficace pour les grands ensembles)
primes = {2, 3, 5, 7, 11, 13, 17, 19}
print(7 in primes)            # True
print(12 in primes)           # False

# Comptage d'éléments uniques
text = "to be or not to be that is the question"
unique_words = set(text.split())
print(len(unique_words))      # 8
print(unique_words)

# Filtrage (garder uniquement les éléments communs à deux listes)
list1 = [1, 2, 3, 4, 5]
list2 = [3, 4, 5, 6, 7]
common = list(set(list1) & set(list2))
print(common)                 # [3, 4, 5]

# Recherche de doublons
def find_duplicates(items):
    seen = set()
    duplicates = set()
    for item in items:
        if item in seen:
            duplicates.add(item)
        else:
            seen.add(item)
    return duplicates

print(find_duplicates([1, 2, 2, 3, 4, 4, 5]))  # {2, 4}

# Ensemble immuable (frozenset)
immutable = frozenset([1, 2, 3])
# immutable.add(4)  # AttributeError: 'frozenset' object has no attribute 'add'

# Utilisation comme clé de dictionnaire
sets_dict = {frozenset([1, 2]): "Set A", frozenset([3, 4]): "Set B"}
print(sets_dict[frozenset([1, 2])])  # Set A
```

## Fonctions pour tuples et sequences immuables

Les tuples sont des séquences immuables similaires aux listes, mais qui ne peuvent pas être modifiées après leur création.

### Méthodes disponibles

```python
# Création de tuples
t1 = (1, 2, 3)                # Parenthèses
t2 = 1, 2, 3                  # Parenthèses facultatives
t3 = tuple([4, 5, 6])         # À partir d'une liste
singleton = (42,)             # Un tuple à un élément (la virgule est nécessaire)

# Opérations de base
print(t1 + t2)                # (1, 2, 3, 1, 2, 3) - concaténation
print(t1 * 3)                 # (1, 2, 3, 1, 2, 3, 1, 2, 3) - répétition
print(2 in t1)                # True - appartenance
print(t1[1])                  # 2 - accès par index
print(t1[1:])                 # (2, 3) - tranche

# Méthodes
print(t1.count(2))            # 1 - nombre d'occurrences
print(t1.index(3))            # 2 - indice de la première occurrence

# Déballage
a, b, c = t1
print(a, b, c)                # 1 2 3

# Déballage étendu
first, *rest = t2
print(first, rest)            # 1 [2, 3]

*start, last = t3
print(start, last)            # [4, 5] 6

# Échange de variables
x, y = 10, 20
x, y = y, x                   # Échange
print(x, y)                   # 20 10
```

### Named tuples

Les named tuples sont une façon élégante de créer des objets simples de type enregistrement :

```python
from collections import namedtuple

# Définition d'un named tuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print(p)                      # Point(x=1, y=2)
print(p.x, p.y)               # 1 2
print(p[0], p[1])             # 1 2 (compatible avec l'indexation de tuple)

# Création avec des mots-clés
q = Point(y=4, x=3)
print(q)                      # Point(x=3, y=4)

# Conversion en dictionnaire
print(p._asdict())            # {'x': 1, 'y': 2}

# Création d'une copie modifiée
r = p._replace(x=10)
print(r)                      # Point(x=10, y=2)

# Définition avec valeurs par défaut
# Nécessite Python 3.7+ ou un workaround
Person = namedtuple('Person', 'name age')
Person.__new__.__defaults__ = (None, 0)
p1 = Person('Alice')
print(p1)                     # Person(name='Alice', age=0)

# Définition à partir d'une chaîne
Employee = namedtuple('Employee', 'name position department')
e = Employee('Bob', 'Developer', 'IT')
print(e)                      # Employee(name='Bob', position='Developer', department='IT')

# Accès aux champs par nom
print(e.position)             # Developer
```

### Utilisations avancées

```python
# Tuples comme clés de dictionnaire
locations = {
    (40.7128, -74.0060): "New York",
    (34.0522, -118.2437): "Los Angeles",
    (51.5074, -0.1278): "London"
}
print(locations[(40.7128, -74.0060)])  # New York

# Tuple non modifiable mais avec éléments mutables
t = (1, 2, [3, 4])
# t[0] = 10  # TypeError: 'tuple' object does not support item assignment
t[2][0] = 30  # Modification de la liste à l'intérieur
print(t)                      # (1, 2, [30, 4])

# Utilisation dans une fonction avec nombre variable d'arguments
def sum_all(*args):
    return sum(args)

print(sum_all(1, 2, 3, 4))    # 10

# Comparaison de tuples (lexicographique)
print((1, 2, 3) < (1, 3, 0))  # True (2 < 3 au deuxième élément)
print((1, 2, 3) < (1, 2, 3, 0))  # True (le plus court est considéré inférieur)

# Tri de liste par multiples critères
students = [
    ('John', 'A', 15),
    ('Jane', 'B', 12),
    ('Dave', 'A', 10),
    ('Alice', 'C', 15)
]
# Tri par note, puis par âge (ordre décroissant)
sorted_students = sorted(students, key=lambda s: (s[1], -s[2]))
print(sorted_students)  # [('Dave', 'A', 10), ('John', 'A', 15), ('Jane', 'B', 12), ('Alice', 'C', 15)]
```

## Fonctions générales sur les itérables

Python offre de nombreuses fonctions pour travailler avec des itérables de manière générique, indépendamment de leur type spécifique.

### Fonctions de base (any, all, len)

```python
# any() - renvoie True si au moins un élément est vrai
print(any([False, False, True]))  # True
print(any([]))                 # False (vide)
print(any([0, "", False]))     # False (tous faux)

# all() - renvoie True si tous les éléments sont vrais
print(all([True, True, True])) # True
print(all([]))                 # True (vide)
print(all([1, "string", True, [1]]))  # True (tous vrais)
print(all([1, "", True]))      # False (un élément faux)

# len() - nombre d'éléments
print(len([1, 2, 3]))          # 3
print(len("Python"))           # 6
print(len({"a": 1, "b": 2}))   # 2 (nombre de paires clé-valeur)
print(len(range(10)))          # 10

# Utilisation pratique de any/all
numbers = [1, 2, 3, 4, 5]
print(any(n > 10 for n in numbers))        # False
print(all(n > 0 for n in numbers))         # True

# Valider une liste d'objets
users = [
    {"name": "Alice", "age": 25, "active": True},
    {"name": "Bob", "age": 30, "active": False},
    {"name": "Charlie", "age": 35, "active": True}
]
any_inactive = any(not user["active"] for user in users)
all_adults = all(user["age"] >= 18 for user in users)
print(f"Any inactive users? {any_inactive}")  # True
print(f"All users are adults? {all_adults}")  # True
```

### Tri et recherche

```python
# sorted() - retourne une nouvelle liste triée
numbers = [3, 1, 4, 1, 5, 9, 2]
print(sorted(numbers))         # [1, 1, 2, 3, 4, 5, 9]
print(sorted(numbers, reverse=True))  # [9, 5, 4, 3, 2, 1, 1]

# Tri avec une clé personnalisée
names = ["alice", "Bob", "Charlie", "david"]
print(sorted(names))           # ['Bob', 'Charlie', 'alice', 'david'] (tri par ASCII)
print(sorted(names, key=str.lower))  # ['alice', 'Bob', 'Charlie', 'david'] (alphabétique)

# Tri d'objets complexes
students = [
    {"name": "Alice", "grade": 85},
    {"name": "Bob", "grade": 92},
    {"name": "Charlie", "grade": 78}
]
print(sorted(students, key=lambda s: s["grade"], reverse=True))
# [{'name': 'Bob', 'grade': 92}, {'name': 'Alice', 'grade': 85}, {'name': 'Charlie', 'grade': 78}]

# Tri à plusieurs niveaux
data = [("a", 3), ("b", 2), ("a", 1), ("c", 4)]
print(sorted(data))            # [('a', 1), ('a', 3), ('b', 2), ('c', 4)]
# Tri par second élément puis par premier élément
from operator import itemgetter
print(sorted(data, key=itemgetter(1, 0)))
# [('a', 1), ('b', 2), ('a', 3), ('c', 4)]

# min() et max() - avec clé personnalisée
print(min(students, key=lambda s: s["grade"]))  # Charlie
print(max(students, key=lambda s: s["grade"]))  # Bob

# Recherche dans un itérable
from bisect import bisect_left

# Recherche par bisection (liste doit être triée)
sorted_numbers = sorted(numbers)  # [1, 1, 2, 3, 4, 5, 9]
position = bisect_left(sorted_numbers, 4)
print(position)                # 5 (insertion point)
print(sorted_numbers[position] == 4)  # True (élément trouvé)

# Recherche personnalisée
def binary_search(arr, x):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] < x:
            left = mid + 1
        elif arr[mid] > x:
            right = mid - 1
        else:
            return mid
    return -1

print(binary_search(sorted_numbers, 5))  # 5
print(binary_search(sorted_numbers, 6))  # -1 (non trouvé)
```

### Agrégation et statistiques

```python
# sum() - somme des éléments
print(sum([1, 2, 3, 4, 5]))    # 15
print(sum([0.1, 0.2, 0.3], 0.0))  # 0.6 (avec valeur initiale)

# sum() avec objets non numériques
words = ["Hello", "World"]
# print(sum(words))  # TypeError
print("".join(words))          # HelloWorld

# min() et max() - valeurs minimale et maximale
print(min([5, 3, 8, 1, 4]))    # 1
print(max([5, 3, 8, 1, 4]))    # 8
print(min(["apple", "banana", "cherry"], key=len))  # apple (plus courte)
print(max(["apple", "banana", "cherry"], key=len))  # banana (plus longue)

# min/max avec dictionnaires
prices = {"apple": 0.5, "banana": 0.3, "cherry": 0.8}
cheapest = min(prices.items(), key=lambda x: x[1])
most_expensive = max(prices.items(), key=lambda x: x[1])
print(cheapest)                # ('banana', 0.3)
print(most_expensive)          # ('cherry', 0.8)

# Module statistics (Python 3.4+)
import statistics

data = [1, 3, 3, 5, 7]
print(statistics.mean(data))   # 3.8 (moyenne)
print(statistics.median(data)) # 3 (médiane)
print(statistics.mode(data))   # 3 (mode - valeur la plus fréquente)
print(statistics.stdev(data))  # 2.28 (écart type)
print(statistics.variance(data))  # 5.2 (variance)

# Agrégation personnalisée
def product(iterable):
    result = 1
    for i in iterable:
        result *= i
    return result

print(product([1, 2, 3, 4]))   # 24 (1*2*3*4)

# Module itertools pour l'agrégation
import itertools

# accumulate() - agrégation progressive
acc = list(itertools.accumulate([1, 2, 3, 4, 5]))
print(acc)                     # [1, 3, 6, 10, 15] (sommes cumulées)

# accumulate() avec fonction personnalisée
acc_prod = list(itertools.accumulate([1, 2, 3, 4], lambda x, y: x * y))
print(acc_prod)                # [1, 2, 6, 24] (produits cumulés)
```

## Techniques de conversion de types

La conversion entre différents types de données est une opération courante en Python.

### Conversions numériques

```python
# Conversions numériques de base
i = int("123")                # Chaîne -> entier
f = float("3.14")             # Chaîne -> flottant
s = str(42)                   # Entier -> chaîne
b = bool(0)                   # Nombre -> booléen (0 est False)

# Conversions avec bases numériques
print(int("1010", 2))         # 10 (binaire -> décimal)
print(int("A0", 16))          # 160 (hexadécimal -> décimal)
print(bin(10))                # '0b1010' (décimal -> binaire)
print(hex(160))               # '0xa0' (décimal -> hexadécimal)
print(oct(10))                # '0o12' (décimal -> octal)

# Conversion vers/depuis des caractères ASCII
print(ord('A'))               # 65 (caractère -> code ASCII)
print(chr(65))                # 'A' (code ASCII -> caractère)

# Arrondis et troncatures
import math
print(round(3.7))             # 4 (arrondi à l'entier le plus proche)
print(round(3.5))             # 4 (arrondi bancaire - les .5 vont vers l'entier pair le plus proche)
print(round(2.5))             # 2
print(round(3.14159, 2))      # 3.14 (arrondi à 2 décimales)
print(math.floor(3.7))        # 3 (arrondi vers le bas)
print(math.ceil(3.2))         # 4 (arrondi vers le haut)
print(math.trunc(3.7))        # 3 (troncature - supprime la partie décimale)
print(int(3.7))               # 3 (conversion en entier - troncature)

# Conversion de nombres complexes
c = complex(1, 2)             # 1+2j
print(c.real)                 # 1.0 (partie réelle)
print(c.imag)                 # 2.0 (partie imaginaire)
print(abs(c))                 # 2.23... (module)
```

### Convertir entre structures de données

```python
# Liste <-> Tuple
my_list = [1, 2, 3]
my_tuple = tuple(my_list)     # [1, 2, 3] -> (1, 2, 3)
back_to_list = list(my_tuple) # (1, 2, 3) -> [1, 2, 3]

# Chaîne <-> Liste
my_str = "Python"
my_list = list(my_str)        # "Python" -> ['P', 'y', 't', 'h', 'o', 'n']
back_to_str = "".join(my_list) # ['P', 'y', ...] -> "Python"

# Chaîne <-> Liste de mots
words = "Hello world Python"
word_list = words.split()     # "Hello world Python" -> ['Hello', 'world', 'Python']
back_to_str = " ".join(word_list) # ['Hello', ...] -> "Hello world Python"

# Liste <-> Ensemble
my_list = [1, 2, 2, 3, 4, 4]
my_set = set(my_list)         # [1, 2, 2, 3, 4, 4] -> {1, 2, 3, 4}
back_to_list = list(my_set)   # {1, 2, 3, 4} -> [1, 2, 3, 4] (ordre non garanti)

# Dictionnaire <-> Liste de tuples
my_dict = {"a": 1, "b": 2, "c": 3}
items_list = list(my_dict.items()) # {"a": 1, ...} -> [('a', 1), ('b', 2), ('c', 3)]
back_to_dict = dict(items_list) # [('a', 1), ...] -> {"a": 1, "b": 2, "c": 3}

# Création de dictionnaire à partir de listes
keys = ["a", "b", "c"]
values = [1, 2, 3]
my_dict = dict(zip(keys, values)) # -> {"a": 1, "b": 2, "c": 3}

# Dictionnaire à partir d'une liste avec des index comme clés
my_list = ["apple", "banana", "cherry"]
my_dict = {i: val for i, val in enumerate(my_list)}
print(my_dict)                # {0: 'apple', 1: 'banana', 2: 'cherry'}

# Conversions imbriquées
nested_list = [[1, 2], [3, 4], [5, 6]]
flattened = [item for sublist in nested_list for item in sublist]
print(flattened)              # [1, 2, 3, 4, 5, 6]

# Transposition de matrice
matrix = [[1, 2, 3], [4, 5, 6]]
transposed = list(zip(*matrix)) # [(1, 4), (2, 5), (3, 6)]
# Conversion en liste de listes
transposed = [list(row) for row in zip(*matrix)] # [[1, 4], [2, 5], [3, 6]]
```

### Sérialisation et désérialisation

```python
# JSON - JavaScript Object Notation
import json

# Objets Python -> JSON (sérialisation)
data = {
    "name": "John",
    "age": 30,
    "city": "New York",
    "languages": ["Python", "JavaScript", "C++"],
    "active": True,
    "height": 1.85
}

json_str = json.dumps(data)
print(json_str)
# '{"name": "John", "age": 30, "city": "New York", "languages": ["Python", "JavaScript", "C++"], "active": true, "height": 1.85}'

# Avec formatage pour lisibilité
pretty_json = json.dumps(data, indent=2, sort_keys=True)
print(pretty_json)

# Écriture dans un fichier
with open("data.json", "w") as f:
    json.dump(data, f, indent=4)

# JSON -> Objets Python (désérialisation)
parsed_data = json.loads(json_str)
print(parsed_data["name"])    # John
print(parsed_data["languages"][0])  # Python

# Lecture depuis un fichier
with open("data.json", "r") as f:
    loaded_data = json.load(f)

# Pickle - Sérialisation spécifique à Python
import pickle

# Objets Python -> binaire (sérialisation)
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self):
        return f"Hello, I'm {self.name}"

p = Person("Alice", 30)
serialized = pickle.dumps(p)
print(type(serialized))       # <class 'bytes'>

# Écriture dans un fichier
with open("person.pickle", "wb") as f:
    pickle.dump(p, f)

# Binaire -> Objets Python (désérialisation)
deserialized = pickle.loads(serialized)
print(deserialized.name)      # Alice
print(deserialized.greet())   # Hello, I'm Alice

# Lecture depuis un fichier
with open("person.pickle", "rb") as f:
    loaded_p = pickle.load(f)
    print(loaded_p.age)       # 30

# CSV - Comma-Separated Values
import csv

# Liste -> CSV
data = [
    ["Name", "Age", "City"],
    ["John", 30, "New York"],
    ["Alice", 25, "Boston"],
    ["Bob", 22, "Chicago"]
]

with open("people.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerows(data)

# Lecture du CSV
with open("people.csv", "r", newline="") as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)

# Lecture avec en-têtes
with open("people.csv", "r", newline="") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["Name"], row["Age"])  # John 30, etc.
```

## Fonctions pour la gestion de fichiers

La manipulation de fichiers est une opération courante dans de nombreux programmes.

### Lecture et écriture de fichiers texte

```python
# Écriture dans un fichier (crée ou écrase)
with open("example.txt", "w") as file:
    file.write("Hello, World!\n")
    file.write("Python is awesome.\n")

# Ajout à un fichier existant
with open("example.txt", "a") as file:
    file.write("This line is appended.\n")

# Lecture d'un fichier complet
with open("example.txt", "r") as file:
    content = file.read()
    print(content)

# Lecture ligne par ligne
with open("example.txt", "r") as file:
    for line in file:
        print(line.strip())  # strip() pour enlever les sauts de ligne

# Lecture par blocs
with open("example.txt", "r") as file:
    chunk = file.read(10)  # Lire 10 caractères
    print(chunk)

# Lecture de toutes les lignes dans une liste
with open("example.txt", "r") as file:
    lines = file.readlines()
    print(lines)  # ['Hello, World!\n', 'Python is awesome.\n', 'This line is appended.\n']

# Encodage des fichiers
with open("unicode.txt", "w", encoding="utf-8") as file:
    file.write("こんにちは\n")  # "Bonjour" en japonais
    file.write("Привет\n")     # "Bonjour" en russe

# Lecture avec encodage spécifique
with open("unicode.txt", "r", encoding="utf-8") as file:
    content = file.read()
    print(content)

# Gestion des erreurs d'encodage
try:
    with open("unicode.txt", "r", encoding="ascii", errors="replace") as file:
        content = file.read()
        print(content)  # Caractères non-ASCII remplacés par �
except UnicodeError as e:
    print(f"Erreur d'encodage: {e}")
```

### Manipulation de fichiers binaires

```python
# Écriture de données binaires
data = bytes([0x48, 0x65, 0x6C, 0x6C, 0x6F])  # "Hello" en hexadécimal

with open("binary.dat", "wb") as file:
    file.write(data)

# Lecture de données binaires
with open("binary.dat", "rb") as file:
    binary_data = file.read()
    print(binary_data)        # b'Hello'
    print(list(binary_data))  # [72, 101, 108, 108, 111]

# Gestion de la position dans le fichier
with open("binary.dat", "rb") as file:
    first_byte = file.read(1)
    print(first_byte)         # b'H'

    # Position actuelle
    position = file.tell()
    print(position)           # 1

    # Déplacement dans le fichier
    file.seek(3)              # Aller à la position 3
    print(file.read(1))       # b'l'

    # Déplacement relatif
    file.seek(-2, 1)          # Reculer de 2 positions
    print(file.read(1))       # b'l'

    # Déplacement depuis la fin
    file.seek(-1, 2)          # 1 position avant la fin
    print(file.read(1))       # b'o'

# Écriture et lecture de structures binaires
import struct

# "i" pour integer, "f" pour float, "4s" pour chaîne de 4 caractères
data = struct.pack("if4s", 42, 3.14, b"ABCD")

with open("struct.dat", "wb") as file:
    file.write(data)

with open("struct.dat", "rb") as file:
    binary_data = file.read()
    unpacked = struct.unpack("if4s", binary_data)
    print(unpacked)           # (42, 3.14, b'ABCD')
```

### Gestion des chemins de fichiers

```python
import os
import shutil
import glob
from pathlib import Path  # Module pathlib (Python 3.4+)

# Manipulation de chemins avec os.path
current_dir = os.getcwd()      # Répertoire de travail actuel
print(current_dir)

file_path = os.path.join(current_dir, "example.txt")
print(file_path)

# Informations sur les fichiers
file_exists = os.path.exists(file_path)
is_file = os.path.isfile(file_path)
is_dir = os.path.isdir(current_dir)
file_size = os.path.getsize(file_path) if file_exists else 0
print(f"File exists: {file_exists}, Size: {file_size} bytes")

# Décomposition de chemins
dirname = os.path.dirname(file_path)  # Répertoire
basename = os.path.basename(file_path)  # Nom du fichier
filename, extension = os.path.splitext(basename)  # Nom et extension
print(f"Dir: {dirname}, File: {filename}, Ext: {extension}")

# Création et suppression de répertoires
new_dir = os.path.join(current_dir, "new_directory")
if not os.path.exists(new_dir):
    os.mkdir(new_dir)         # Crée un répertoire unique
    # os.makedirs(new_dir)    # Crée des répertoires imbriqués si nécessaire

os.rmdir(new_dir)             # Supprime un répertoire vide
# shutil.rmtree(new_dir)      # Supprime un répertoire et son contenu

# Copie et déplacement
if os.path.exists("example.txt"):
    shutil.copy("example.txt", "example_copy.txt")  # Copie de fichier
    # os.rename("example_copy.txt", "example_renamed.txt")  # Renommage/déplacement
    # shutil.move("example_renamed.txt", "moved_file.txt")  # Déplacement

# Lister les fichiers
files = os.listdir(current_dir)  # Liste tous les fichiers et répertoires
print(files)

# Filtrage avec glob
python_files = glob.glob("*.py")  # Tous les fichiers .py
print(python_files)

text_files = glob.glob("*.txt")  # Tous les fichiers .txt
print(text_files)

# Recherche récursive
all_python_files = glob.glob("**/*.py", recursive=True)  # Tous les .py dans tous les sous-répertoires
print(all_python_files)

# Module pathlib (approche orientée objet)
path = Path("example.txt")
print(path.exists())
print(path.is_file())
print(path.suffix)            # Extension
print(path.stem)              # Nom sans extension
print(path.parent)            # Répertoire parent

# Création et parcours avec pathlib
new_path = Path("test_dir") / "subdir" / "file.txt"
print(new_path)

if not new_path.parent.exists():
    new_path.parent.mkdir(parents=True)  # Crée les répertoires parents

# Parcours du système de fichiers
for file_path in Path(current_dir).glob("*.txt"):
    print(file_path)
```

## Fonctions mathématiques

### Module math

```python
import math

# Constantes
print(math.pi)                # 3.141592653589793
print(math.e)                 # 2.718281828459045
print(math.inf)               # Infini positif
print(-math.inf)              # Infini négatif
print(math.nan)               # Not a Number

# Fonctions trigonométriques (radians)
print(math.sin(math.pi/2))    # 1.0
print(math.cos(math.pi))      # -1.0
print(math.tan(math.pi/4))    # 1.0

# Conversion radians/degrés
print(math.degrees(math.pi))  # 180.0
print(math.radians(180))      # 3.141592653589793

# Logarithmes et exponentielles
print(math.log(2.718))        # 1.0 (logarithme naturel, base e)
print(math.log10(100))        # 2.0 (logarithme base 10)
print(math.log2(8))           # 3.0 (logarithme base 2)
print(math.exp(1))            # 2.718281828459045 (e^1)

# Racines
print(math.sqrt(16))          # 4.0 (racine carrée)
print(16 ** 0.5)              # 4.0 (alternative)
print(math.pow(4, 0.5))       # 2.0 (4^0.5)

# Arrondis
print(math.floor(3.7))        # 3 (arrondi inférieur)
print(math.ceil(3.2))         # 4 (arrondi supérieur)
print(math.trunc(3.7))        # 3 (troncature)
print(round(3.5))             # 4 (arrondi au plus proche)
print(round(2.5))             # 2 (arrondi bancaire)

# Valeur absolue et signe
print(math.fabs(-3.5))        # 3.5 (valeur absolue)
print(abs(-3.5))              # 3.5 (fonction intégrée)
print(math.copysign(1.0, -5)) # -1.0 (copie le signe)

# Fonctions hyperboliques
print(math.sinh(1))           # 1.1752011936438014
print(math.cosh(0))           # 1.0
print(math.tanh(1))           # 0.7615941559557649

# Factorielle et combinatoires
print(math.factorial(5))      # 120 (5!)
print(math.comb(7, 3))        # 35 (combinaisons)
print(math.perm(7, 3))        # 210 (permutations)

# Fonctions spéciales
print(math.gcd(12, 18))       # 6 (plus grand commun diviseur)
print(math.lcm(12, 18))       # 36 (plus petit commun multiple, Python 3.9+)
print(math.gamma(5))          # 24.0 (fonction gamma)
print(math.erf(1))            # 0.8427007929497149 (fonction d'erreur)
```

### Fonctions numériques intégrées

```python
# Fonctions intégrées pour les opérations numériques
print(abs(-5))                # 5 (valeur absolue)
print(pow(2, 3))              # 8 (puissance)
print(pow(2, 3, 5))           # 3 (puissance avec modulo: (2^3) % 5)
print(divmod(13, 5))          # (2, 3) (quotient et reste)
print(round(3.14159, 2))      # 3.14 (arrondi à n décimales)

# min et max
print(min(5, 3, 8, 1, 4))     # 1
print(max([5, 3, 8, 1, 4]))   # 8

# sum
print(sum([1, 2, 3, 4, 5]))   # 15

# Conversions
print(bin(42))                # '0b101010' (binaire)
print(oct(42))                # '0o52' (octal)
print(hex(42))                # '0x2a' (hexadécimal)
print(int('101010', 2))       # 42 (binaire vers décimal)
print(int('0x2a', 16))        # 42 (hexadécimal vers décimal)

# Module decimal pour l'arithmétique décimale de précision
from decimal import Decimal, getcontext

# Problème classique avec les flottants
print(0.1 + 0.2)              # 0.30000000000000004 (imprécision)

# Avec decimal
getcontext().prec = 28        # Définir la précision
d1 = Decimal('0.1')
d2 = Decimal('0.2')
print(d1 + d2)                # 0.3 (précis)

# Calculs financiers
print(Decimal('1.45') * Decimal('1.1'))  # 1.595

# Module fractions pour l'arithmétique rationnelle
from fractions import Fraction

# Création de fractions
f1 = Fraction(1, 3)           # 1/3
f2 = Fraction(2, 5)           # 2/5
print(f1 + f2)                # 11/15
print(f1 * f2)                # 2/15

# Conversion de décimal en fraction
print(Fraction('0.25'))       # 1/4
print(Fraction(0.1))          # 3602879701896397/36028797018963968 (imprécision)
print(Fraction('0.1'))        # 1/10 (précis)

# Nombre complexe
c1 = 1 + 2j
c2 = complex(3, 4)
print(c1 + c2)                # (4+6j)
print(abs(c2))                # 5.0 (module)
```

### Arrondis et troncatures

```python
# Types d'arrondis
import math
import decimal

x = 3.14159

# Arrondi standard (bancaire)
print(round(x))               # 3 (arrondi à l'entier le plus proche)
print(round(x, 2))            # 3.14 (arrondi à 2 décimales)
print(round(2.5))             # 2 (arrondi bancaire: .5 va vers le pair le plus proche)
print(round(3.5))             # 4

# Arrondi vers le bas
print(math.floor(x))          # 3

# Arrondi vers le haut
print(math.ceil(x))           # 4

# Troncature (supprime la partie décimale)
print(math.trunc(x))          # 3
print(math.trunc(-3.7))       # -3
print(int(x))                 # 3 (conversion en entier = troncature)

# Arrondi avec decimal (pour le contrôle du mode d'arrondi)
from decimal import Decimal, ROUND_HALF_UP, ROUND_HALF_DOWN

decimal.getcontext().rounding = ROUND_HALF_UP  # Arrondi classique
d = Decimal("2.5")
print(d.quantize(Decimal("1")))  # 3

decimal.getcontext().rounding = ROUND_HALF_DOWN  # .5 arrondi vers le bas
print(d.quantize(Decimal("1")))  # 2

# Format avec spécification d'arrondi
print(f"{x:.2f}")             # 3.14 (arrondi à 2 décimales)
```

### Opérations statistiques

```python
import statistics
import random
import numpy as np  # Bibliothèque externe, nécessite installation

# Données d'exemple
data = [1, 2, 3, 3, 3, 4, 5, 5, 9]

# Statistiques de base avec le module statistics
print(statistics.mean(data))        # 3.888... (moyenne)
print(statistics.median(data))      # 3 (médiane)
print(statistics.mode(data))        # 3 (mode - valeur la plus fréquente)
print(statistics.stdev(data))       # 2.26... (écart type)
print(statistics.variance(data))    # 5.11... (variance)

# Statistiques avec fonctions intégrées
print(sum(data) / len(data))        # 3.888... (moyenne)
print(min(data))                    # 1 (minimum)
print(max(data))                    # 9 (maximum)

# Tri et médiane manuelle
sorted_data = sorted(data)
n = len(sorted_data)
if n % 2 == 0:
    median = (sorted_data[n//2 - 1] + sorted_data[n//2]) / 2
else:
    median = sorted_data[n//2]
print(median)                       # 3

# Génération de nombres aléatoires
print(random.random())              # Nombre aléatoire entre 0 et 1
print(random.uniform(1, 10))        # Nombre aléatoire réel entre 1 et 10
print(random.randint(1, 10))        # Entier aléatoire entre 1 et 10 inclus
print(random.choice(data))          # Élément aléatoire de data
print(random.sample(data, 3))       # 3 éléments aléatoires sans répétition
random.shuffle(data)                # Mélange les éléments de data en place
print(data)

# Statistiques avancées avec numpy
if 'np' in globals():
    numpy_data = np.array(data)
    print(np.mean(numpy_data))      # Moyenne
    print(np.median(numpy_data))    # Médiane
    print(np.std(numpy_data))       # Écart type
    print(np.var(numpy_data))       # Variance
    print(np.percentile(numpy_data, 75))  # 75e percentile
    print(np.corrcoef([1, 2, 3], [2, 3, 4]))  # Coefficient de corrélation
```

## Fonctions du module itertools

Le module `itertools` offre des outils puissants pour créer et manipuler des itérables de manière efficace.

### Itérateurs infinis

```python
import itertools

# count() - compte à partir d'une valeur avec un pas facultatif
counter = itertools.count(10, 2)  # Commence à 10, incrémente de 2
for i in range(5):
    print(next(counter))  # 10, 12, 14, 16, 18

# cycle() - cycle indéfiniment à travers un itérable
cycler = itertools.cycle("ABC")
for i in range(7):
    print(next(cycler))  # A, B, C, A, B, C, A

# repeat() - répète un élément un nombre de fois défini ou indéfiniment
repeater = itertools.repeat("Hello", 3)
for item in repeater:
    print(item)  # Hello, Hello, Hello

# Combinaison avec map
squares = map(pow, range(10), itertools.repeat(2))
print(list(squares))  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

### Combinatoires

```python
import itertools

# Permutations - tous les arrangements possibles
perms = itertools.permutations("ABC", 2)
print(list(perms))  # [('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]

# Combinaisons - sous-ensembles de r éléments
combs = itertools.combinations("ABC", 2)
print(list(combs))  # [('A', 'B'), ('A', 'C'), ('B', 'C')]

# Combinaisons avec répétition
combs_r = itertools.combinations_with_replacement("ABC", 2)
print(list(combs_r))  # [('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')]

# Produit cartésien
prod = itertools.product("AB", "12")
print(list(prod))  # [('A', '1'), ('A', '2'), ('B', '1'), ('B', '2')]

# Produit cartésien répété
repeated_prod = itertools.product("AB", repeat=3)
print(list(repeated_prod))  # [('A', 'A', 'A'), ('A', 'A', 'B'), ...]

# Utilisation pratique pour tester des combinaisons de paramètres
def test_function(p1, p2, p3):
    return f"Test: {p1}-{p2}-{p3}"

parameters = [
    ["a", "b"],    # options pour p1
    [1, 2],        # options pour p2
    [True, False]  # options pour p3
]

for p1, p2, p3 in itertools.product(*parameters):
    result = test_function(p1, p2, p3)
    print(result)
```

### Chaînage et groupement

```python
import itertools

# chain() - chaîne plusieurs itérables en un seul
result = itertools.chain("ABC", [1, 2, 3], (True, False))
print(list(result))  # ['A', 'B', 'C', 1, 2, 3, True, False]

# chain.from_iterable() - chaîne les éléments d'un itérable d'itérables
nested = [["A", "B"], [1, 2], [True, False]]
result = itertools.chain.from_iterable(nested)
print(list(result))  # ['A', 'B', 1, 2, True, False]

# zip_longest() - comme zip() mais continue jusqu'au plus long itérable
result = itertools.zip_longest("ABCD", [1, 2], fillvalue="X")
print(list(result))  # [('A', 1), ('B', 2), ('C', 'X'), ('D', 'X')]

# groupby() - groupe les éléments consécutifs par une clé
animals = ["duck", "dog", "deer", "cat", "cow"]
# Note: les éléments doivent être triés pour un regroupement efficace
animals.sort(key=lambda x: x[0])  # Trier par première lettre
for key, group in itertools.groupby(animals, key=lambda x: x[0]):
    print(key, list(group))
# c ['cat', 'cow']
# d ['deer', 'dog', 'duck']

# Exemple de groupby() sur un dataset
data = [
    {"name": "Alice", "department": "HR", "salary": 50000},
    {"name": "Bob", "department": "IT", "salary": 60000},
    {"name": "Charlie", "department": "HR", "salary": 55000},
    {"name": "David", "department": "IT", "salary": 65000},
    {"name": "Eve", "department": "Finance", "salary": 70000}
]

# Trier par département pour le groupby
data.sort(key=lambda x: x["department"])

for dept, employees in itertools.groupby(data, key=lambda x: x["department"]):
    print(f"Department: {dept}")
    for emp in employees:
        print(f"  - {emp['name']}: ${emp['salary']}")

# islice() - découpe un itérable
counter = itertools.count()  # Itérateur infini
first_five = itertools.islice(counter, 5)  # Premiers 5 éléments
print(list(first_five))  # [0, 1, 2, 3, 4]

letters = "ABCDEFGHIJ"
subset = itertools.islice(letters, 2, 7, 2)  # Du 2e au 7e élément par pas de 2
print(list(subset))  # ['C', 'E', 'G']

# dropwhile() et takewhile() - filtrage conditionnel
data = [1, 2, 3, 10, 4, 5, 6, 7]
result = itertools.dropwhile(lambda x: x < 5, data)
print(list(result))  # [10, 4, 5, 6, 7] (supprime jusqu'à ce que la condition soit fausse)

result = itertools.takewhile(lambda x: x < 5, data)
print(list(result))  # [1, 2, 3] (prend jusqu'à ce que la condition soit fausse)

# filterfalse() - filtre les éléments pour lesquels la fonction renvoie False
result = itertools.filterfalse(lambda x: x % 2 == 0, range(10))
print(list(result))  # [1, 3, 5, 7, 9] (nombres impairs)

# accumulate() - accumulation progressive
result = itertools.accumulate([1, 2, 3, 4, 5])
print(list(result))  # [1, 3, 6, 10, 15] (sommes cumulées)

# Avec une fonction personnalisée
result = itertools.accumulate([1, 2, 3, 4, 5], lambda x, y: x * y)
print(list(result))  # [1, 2, 6, 24, 120] (produits cumulés)
```

## Fonctions du module functools

Le module `functools` fournit des outils pour la programmation fonctionnelle.

### Cache et mémoïsation

```python
import functools
import time

# lru_cache - mise en cache des résultats pour éviter les calculs répétés
@functools.lru_cache(maxsize=128)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Sans cache, fibonacci(35) prendrait des secondes
start = time.time()
print(fibonacci(35))  # 9227465
end = time.time()
print(f"Temps d'exécution: {end - start:.6f} secondes")

# Informations sur le cache
print(fibonacci.cache_info())  # CacheInfo(hits=68, misses=36, maxsize=128, currsize=36)

# Réinitialisation du cache
fibonacci.cache_clear()
print(fibonacci.cache_info())  # CacheInfo(hits=0, misses=0, maxsize=128, currsize=0)

# cache - mise en cache simple (Python 3.9+)
@functools.cache
def factorial(n):
    return n * factorial(n-1) if n else 1

print(factorial(10))  # 3628800
print(factorial.cache_info())

# Mémoïsation manuelle (alternative pour les versions plus anciennes)
def memoize(func):
    cache = {}

    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        key = str(args) + str(kwargs)
        if key not in cache:
            cache[key] = func(*args, **kwargs)
        return cache[key]

    return wrapper

@memoize
def slow_function(x, y):
    time.sleep(1)  # Simulation d'une fonction lente
    return x + y

print(slow_function(1, 2))  # Première exécution: prend 1 seconde
print(slow_function(1, 2))  # Exécution mise en cache: instantanée
```

### Fonctions partielles

```python
import functools

# partial - création d'une nouvelle fonction avec certains arguments fixés
from functools import partial

def power(base, exponent):
    return base ** exponent

# Création de fonctions spécialisées
square = partial(power, exponent=2)
cube = partial(power, exponent=3)
raise_to_4 = partial(power, exponent=4)

print(square(5))      # 25 (5^2)
print(cube(5))        # 125 (5^3)
print(raise_to_4(5))  # 625 (5^4)

# Utilisation avec des fonctions intégrées
import operator

add_5 = partial(operator.add, 5)  # Ajoute 5 à l'argument
multiply_by_3 = partial(operator.mul, 3)  # Multiplie l'argument par 3

print(add_5(10))       # 15
print(multiply_by_3(7)) # 21

# Cas pratique : configuration d'une fonction
def create_file(filename, mode='w', encoding='utf-8', content=''):
    with open(filename, mode, encoding=encoding) as f:
        f.write(content)

# Création d'une fonction pré-configurée
create_log_file = partial(
    create_file,
    mode='a',
    encoding='utf-8',
    content='--- Log started ---\n'
)

# Utilisation
create_log_file('app.log')  # Crée un fichier log avec contenu prédéfini

# Utilisation avec sorted()
pairs = [(1, 'one'), (2, 'two'), (3, 'three')]

# Tri normal par le premier élément
print(sorted(pairs))  # [(1, 'one'), (2, 'two'), (3, 'three')]

# Tri par le second élément
sort_by_second = partial(sorted, key=lambda x: x[1])
print(sort_by_second(pairs))  # [(1, 'one'), (3, 'three'), (2, 'two')]
```

### Réduction et plis

```python
import functools
import operator

# reduce - applique une fonction cumul
```
