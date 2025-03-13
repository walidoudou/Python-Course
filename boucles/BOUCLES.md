# Les Boucles en Python

## Table des matières

- [Introduction](#introduction)
- [Boucle for](#boucle-for)
  - [Syntaxe de base](#syntaxe-de-base)
  - [Boucle for avec range()](#boucle-for-avec-range)
  - [Boucle for avec les collections](#boucle-for-avec-les-collections)
  - [Boucle for avec enumerate()](#boucle-for-avec-enumerate)
  - [Boucle for avec zip()](#boucle-for-avec-zip)
- [Boucle while](#boucle-while)
  - [Syntaxe de base](#syntaxe-de-base-1)
  - [Boucle while avec condition de validation](#boucle-while-avec-condition-de-validation)
  - [Boucle while infinie](#boucle-while-infinie)
- [Contrôle de flux dans les boucles](#contrôle-de-flux-dans-les-boucles)
  - [break](#break)
  - [continue](#continue)
  - [else dans les boucles](#else-dans-les-boucles)
  - [pass](#pass)
- [Techniques d'itération avancées](#techniques-ditération-avancées)
  - [Compréhension de liste](#compréhension-de-liste)
  - [Compréhension de dictionnaire](#compréhension-de-dictionnaire)
  - [Compréhension d'ensemble](#compréhension-densemble)
  - [Expressions génératrices](#expressions-génératrices)
- [Patterns d'optimisation](#patterns-doptimisation)
  - [Optimization des boucles](#optimization-des-boucles)
  - [Boucles avec les itérateurs](#boucles-avec-les-itérateurs)
  - [Délégation de l'itération](#délégation-de-litération)
- [Cas d'utilisation courants](#cas-dutilisation-courants)
  - [Parcourir des structures imbriquées](#parcourir-des-structures-imbriquées)
  - [Lecture et traitement de fichiers](#lecture-et-traitement-de-fichiers)
  - [Itération infinie avec condition d'arrêt](#itération-infinie-avec-condition-darrêt)
- [Bonnes pratiques](#bonnes-pratiques)
- [Erreurs courantes](#erreurs-courantes)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

Les boucles sont des structures de contrôle fondamentales en programmation qui permettent d'exécuter un bloc de code plusieurs fois. Elles constituent l'un des mécanismes essentiels pour automatiser des tâches répétitives, parcourir des collections de données, et implémenter des algorithmes complexes.

Python propose deux types principaux de boucles : `for` et `while`. Chaque type a ses propres cas d'utilisation et avantages. Dans ce chapitre, nous explorerons en détail ces structures de boucle, ainsi que les techniques avancées d'itération qui font la puissance et l'élégance de Python.

## Boucle for

La boucle `for` en Python est conçue pour itérer sur une séquence d'éléments (comme une liste, un tuple, un dictionnaire, un ensemble, ou une chaîne de caractères). Contrairement aux boucles `for` dans certains autres langages, Python utilise une approche "pour chaque élément" plutôt qu'une approche basée sur un index.

### Syntaxe de base

```python
for élément in séquence:
    # Code à exécuter pour chaque élément
    print(élément)
```

### Boucle for avec range()

La fonction `range()` est souvent utilisée avec les boucles `for` pour générer une séquence de nombres. Elle est particulièrement utile lorsque vous avez besoin d'itérer un nombre spécifique de fois.

```python
# range(stop) - génère des nombres de 0 à stop-1
for i in range(5):
    print(i)
# Affiche: 0, 1, 2, 3, 4

# range(start, stop) - génère des nombres de start à stop-1
for i in range(2, 7):
    print(i)
# Affiche: 2, 3, 4, 5, 6

# range(start, stop, step) - génère des nombres de start à stop-1 avec un pas de step
for i in range(1, 10, 2):
    print(i)
# Affiche: 1, 3, 5, 7, 9

# Itération à rebours
for i in range(10, 0, -1):
    print(i)
# Affiche: 10, 9, 8, 7, 6, 5, 4, 3, 2, 1
```

### Boucle for avec les collections

Python permet d'itérer directement sur différents types de collections :

```python
# Itération sur une chaîne de caractères
for caractere in "Python":
    print(caractere)
# Affiche: P, y, t, h, o, n

# Itération sur une liste
fruits = ["pomme", "banane", "orange"]
for fruit in fruits:
    print(fruit)
# Affiche: pomme, banane, orange

# Itération sur un tuple
coordonnees = (10, 20, 30)
for coordonnee in coordonnees:
    print(coordonnee)
# Affiche: 10, 20, 30

# Itération sur un dictionnaire
personne = {"nom": "Dupont", "prenom": "Jean", "age": 30}

# Par défaut, itère sur les clés
for cle in personne:
    print(cle)
# Affiche: nom, prenom, age

# Itération sur les valeurs
for valeur in personne.values():
    print(valeur)
# Affiche: Dupont, Jean, 30

# Itération sur les paires clé-valeur
for cle, valeur in personne.items():
    print(f"{cle}: {valeur}")
# Affiche: nom: Dupont, prenom: Jean, age: 30

# Itération sur un ensemble
couleurs = {"rouge", "vert", "bleu"}
for couleur in couleurs:
    print(couleur)
# Affiche: rouge, vert, bleu (l'ordre peut varier car les ensembles sont non ordonnés)
```

### Boucle for avec enumerate()

La fonction `enumerate()` est très utile lorsque vous avez besoin à la fois de l'index et de la valeur lors de l'itération sur une séquence.

```python
fruits = ["pomme", "banane", "orange"]

# Sans enumerate
for i in range(len(fruits)):
    print(f"Index {i}: {fruits[i]}")

# Avec enumerate (plus pythonique)
for index, fruit in enumerate(fruits):
    print(f"Index {index}: {fruit}")
# Affiche:
# Index 0: pomme
# Index 1: banane
# Index 2: orange

# Avec un index de départ personnalisé
for index, fruit in enumerate(fruits, start=1):
    print(f"Fruit #{index}: {fruit}")
# Affiche:
# Fruit #1: pomme
# Fruit #2: banane
# Fruit #3: orange
```

### Boucle for avec zip()

La fonction `zip()` permet de parcourir simultanément plusieurs séquences.

```python
noms = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]
villes = ["Paris", "Lyon", "Marseille"]

for nom, age, ville in zip(noms, ages, villes):
    print(f"{nom}, {age} ans, habite à {ville}")
# Affiche:
# Alice, 25 ans, habite à Paris
# Bob, 30 ans, habite à Lyon
# Charlie, 35 ans, habite à Marseille

# Si les séquences sont de longueurs différentes, zip s'arrête à la plus courte
langues = ["français", "anglais"]
for nom, langue in zip(noms, langues):
    print(f"{nom} parle {langue}")
# Affiche:
# Alice parle français
# Bob parle anglais

# Avec Python 3.10+, on peut utiliser zip(..., strict=True) pour lever une exception
# si les séquences n'ont pas la même longueur
try:
    for nom, langue in zip(noms, langues, strict=True):
        print(f"{nom} parle {langue}")
except ValueError as e:
    print(f"Erreur: {e}")
# Affiche: Erreur: zip() argument 2 is shorter than argument 1
```

## Boucle while

Contrairement à la boucle `for` qui itère sur une séquence, la boucle `while` continue à s'exécuter tant qu'une condition spécifiée est vraie. C'est donc la structure à privilégier lorsque vous ne savez pas à l'avance combien d'itérations seront nécessaires.

### Syntaxe de base

```python
while condition:
    # Code à exécuter tant que la condition est vraie
    # N'oubliez pas de modifier la condition pour éviter une boucle infinie
```

Exemple simple :

```python
compteur = 0
while compteur < 5:
    print(compteur)
    compteur += 1
# Affiche: 0, 1, 2, 3, 4
```

### Boucle while avec condition de validation

Un cas d'utilisation courant de la boucle `while` est la validation d'entrées utilisateur :

```python
while True:
    reponse = input("Entrez un nombre entre 1 et 10: ")
    
    if reponse.isdigit():
        nombre = int(reponse)
        
        if 1 <= nombre <= 10:
            print(f"Vous avez entré {nombre}. Merci!")
            break
        else:
            print("Le nombre doit être entre 1 et 10.")
    else:
        print("Veuillez entrer un nombre valide.")
```

### Boucle while infinie

Une boucle `while` qui s'exécute indéfiniment peut être utile dans certains contextes, comme les serveurs ou les applications qui doivent fonctionner en permanence :

```python
# Exemple d'un serveur simplifié
import time

running = True
while running:
    # Traitement des requêtes
    print("Traitement des requêtes...")
    
    # Simuler une condition d'arrêt (dans un cas réel, cela pourrait être un signal système)
    user_input = input("Entrez 'stop' pour arrêter le serveur: ")
    if user_input.lower() == 'stop':
        running = False
    
    # Petite pause pour ne pas surcharger le CPU
    time.sleep(1)

print("Serveur arrêté.")
```

## Contrôle de flux dans les boucles

Python offre plusieurs instructions pour contrôler le flux d'exécution dans les boucles.

### break

L'instruction `break` permet de sortir immédiatement d'une boucle, quel que soit l'état de la condition :

```python
for i in range(10):
    if i == 5:
        print("Sortie de la boucle à i =", i)
        break
    print(i)
# Affiche: 0, 1, 2, 3, 4, Sortie de la boucle à i = 5

# Exemple: recherche dans une liste
nombres = [10, 20, 30, 40, 50]
valeur_cherchee = 30

for nombre in nombres:
    if nombre == valeur_cherchee:
        print(f"Trouvé: {nombre}")
        break
else:
    print("Valeur non trouvée")
# Affiche: Trouvé: 30
```

### continue

L'instruction `continue` passe à l'itération suivante sans exécuter le reste du code dans la boucle pour l'itération actuelle :

```python
for i in range(10):
    # Sauter les nombres pairs
    if i % 2 == 0:
        continue
    print(i)
# Affiche: 1, 3, 5, 7, 9

# Exemple: traiter uniquement les nombres positifs
nombres = [5, -2, 10, 0, -8, 15]
somme = 0

for nombre in nombres:
    if nombre <= 0:
        continue
    somme += nombre

print(f"Somme des nombres positifs: {somme}")  # 30
```

### else dans les boucles

Python permet d'ajouter une clause `else` aux boucles `for` et `while`. Le bloc `else` est exécuté après que la boucle ait terminé normalement (sans rencontrer d'instruction `break`) :

```python
# Vérifier si un nombre est premier
def est_premier(n):
    if n <= 1:
        return False
    
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    else:
        # Ce bloc est exécuté si aucun diviseur n'est trouvé
        return True

print(est_premier(17))  # True
print(est_premier(20))  # False

# Exemple avec un while
tentatives = 0
max_tentatives = 3

while tentatives < max_tentatives:
    mot_de_passe = input("Entrez votre mot de passe: ")
    
    if mot_de_passe == "secret":
        print("Accès autorisé")
        break
    
    tentatives += 1
    print(f"Mot de passe incorrect. Il vous reste {max_tentatives - tentatives} tentative(s).")
else:
    # Ce bloc est exécuté si le nombre maximal de tentatives est atteint
    print("Accès refusé. Trop de tentatives.")
```

### pass

L'instruction `pass` est un placeholder qui ne fait rien. Elle est utile lorsque vous avez besoin d'une instruction syntaxiquement, mais que vous ne voulez pas exécuter de code :

```python
# Ignorer certains éléments dans une boucle
for i in range(10):
    if i % 3 == 0:
        # Rien à faire pour les multiples de 3
        pass
    else:
        print(i)
# Affiche: 1, 2, 4, 5, 7, 8

# Peut aussi être utilisé comme placeholder pendant le développement
for item in complex_data_structure:
    # TODO: Implémenter le traitement
    pass
```

## Techniques d'itération avancées

Python offre des constructions élégantes pour simplifier les tâches d'itération courantes.

### Compréhension de liste

Les compréhensions de liste permettent de créer des listes de manière concise et expressive :

```python
# Approche traditionnelle
carrés = []
for i in range(1, 6):
    carrés.append(i ** 2)
print(carrés)  # [1, 4, 9, 16, 25]

# Avec une compréhension de liste
carrés = [i ** 2 for i in range(1, 6)]
print(carrés)  # [1, 4, 9, 16, 25]

# Avec une condition
nombres_pairs = [i for i in range(10) if i % 2 == 0]
print(nombres_pairs)  # [0, 2, 4, 6, 8]

# Avec une condition if-else (l'ordre change)
valeurs = [i if i % 2 == 0 else i * 100 for i in range(1, 6)]
print(valeurs)  # [100, 2, 300, 4, 500]

# Compréhension de liste imbriquée
matrice = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
aplatir = [num for ligne in matrice for num in ligne]
print(aplatir)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Compréhension de dictionnaire

De manière similaire, vous pouvez créer des dictionnaires avec des compréhensions :

```python
# Créer un dictionnaire de carrés
carres_dict = {i: i ** 2 for i in range(1, 6)}
print(carres_dict)  # {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# Inverser un dictionnaire (attention aux valeurs dupliquées !)
original = {"a": 1, "b": 2, "c": 3}
inverse = {valeur: cle for cle, valeur in original.items()}
print(inverse)  # {1: 'a', 2: 'b', 3: 'c'}

# Filtrer un dictionnaire
scores = {"Alice": 85, "Bob": 92, "Charlie": 78, "David": 95}
resultats_excellents = {nom: score for nom, score in scores.items() if score >= 90}
print(resultats_excellents)  # {'Bob': 92, 'David': 95}

# Transformation de valeurs
prix = {"pomme": 1.2, "banane": 0.5, "orange": 0.8}
prix_euros = {fruit: f"{prix:.2f} €" for fruit, prix in prix.items()}
print(prix_euros)  # {'pomme': '1.20 €', 'banane': '0.50 €', 'orange': '0.80 €'}
```

### Compréhension d'ensemble

Les compréhensions fonctionnent également avec les ensembles :

```python
# Créer un ensemble de carrés
carres_set = {i ** 2 for i in range(1, 6)}
print(carres_set)  # {1, 4, 9, 16, 25}

# Filtrer les voyelles
texte = "python programming language"
voyelles = {char for char in texte if char.lower() in 'aeiou'}
print(voyelles)  # {'a', 'e', 'i', 'o', 'u'}

# Extraire des caractéristiques uniques
noms = ["Alice", "Bob", "Charlie", "David", "Eva"]
premieres_lettres = {nom[0] for nom in noms}
print(premieres_lettres)  # {'A', 'B', 'C', 'D', 'E'}
```

### Expressions génératrices

Les expressions génératrices sont similaires aux compréhensions de liste mais créent un générateur au lieu d'une liste complète, ce qui est plus efficace en mémoire :

```python
# Compréhension de liste (crée une liste complète en mémoire)
liste_carres = [i ** 2 for i in range(1, 1000001)]
# Consomme beaucoup de mémoire

# Expression génératrice (génère les valeurs à la demande)
gen_carres = (i ** 2 for i in range(1, 1000001))
# Nécessite très peu de mémoire

# Calcul de la somme des carrés
somme = sum(i ** 2 for i in range(1, 101))
print(somme)  # 338350

# Utilisation avec des fonctions qui acceptent des itérables
maximum = max(len(mot) for mot in ["Python", "Programmation", "Développement"])
print(maximum)  # 13

# Conversion en liste si nécessaire
premiers_carres = list((i ** 2 for i in range(1, 6)))
print(premiers_carres)  # [1, 4, 9, 16, 25]
```

## Patterns d'optimisation

### Optimization des boucles

Voici quelques techniques pour optimiser les boucles en Python :

1. **Déplacer les opérations invariantes en dehors de la boucle**

```python
# Moins efficace
for i in range(100):
    result = some_expensive_function()  # Résultat identique à chaque itération
    print(i, result)

# Plus efficace
result = some_expensive_function()
for i in range(100):
    print(i, result)
```

2. **Utiliser des fonctions locales pour les appels fréquents**

```python
# Moins efficace
for item in large_list:
    transformed = some_module.some_function(item)
    # ...

# Plus efficace
transform = some_module.some_function  # Référence locale
for item in large_list:
    transformed = transform(item)
    # ...
```

3. **Préférer les méthodes intégrées et les fonctions du module `itertools`**

```python
import itertools

# Moins efficace
result = 0
for i in range(100):
    result += i

# Plus efficace
result = sum(range(100))

# Utilisation d'itertools pour des opérations complexes
combinations = list(itertools.combinations(range(5), 2))
print(combinations)  # [(0, 1), (0, 2), (0, 3), (0, 4), (1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)]
```

### Boucles avec les itérateurs

Les itérateurs permettent un parcours efficace des données, notamment pour les grands ensembles de données :

```python
# Créer un itérateur explicitement
liste = [1, 2, 3, 4, 5]
it = iter(liste)

print(next(it))  # 1
print(next(it))  # 2
print(next(it))  # 3

# Utilisation dans une boucle for
for val in it:
    print(val)  # Imprime 4, 5 (les valeurs restantes)

# Itérateurs personnalisés avec un générateur
def nombres_pairs(max):
    n = 0
    while n <= max:
        yield n
        n += 2

# Utilisation du générateur
for n in nombres_pairs(10):
    print(n)  # 0, 2, 4, 6, 8, 10
```

### Délégation de l'itération

Pour les objets personnalisés, vous pouvez implémenter `__iter__` et `__next__` pour les rendre itérables :

```python
class CompteurPairs:
    def __init__(self, max):
        self.max = max
        self.n = 0
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.n <= self.max:
            resultat = self.n
            self.n += 2
            return resultat
        else:
            raise StopIteration

# Utilisation
for n in CompteurPairs(10):
    print(n)  # 0, 2, 4, 6, 8, 10
```

## Cas d'utilisation courants

### Parcourir des structures imbriquées

```python
# Parcourir une matrice
matrice = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

for i, ligne in enumerate(matrice):
    for j, valeur in enumerate(ligne):
        print(f"matrice[{i}][{j}] = {valeur}")

# Parcourir un dictionnaire imbriqué
utilisateurs = {
    "alice": {
        "nom": "Alice Dupont",
        "age": 28,
        "role": "admin"
    },
    "bob": {
        "nom": "Bob Martin",
        "age": 32,
        "role": "user"
    }
}

for identifiant, infos in utilisateurs.items():
    print(f"Utilisateur: {identifiant}")
    for cle, valeur in infos.items():
        print(f"  {cle}: {valeur}")
```

### Lecture et traitement de fichiers

```python
# Lecture ligne par ligne
with open("exemple.txt", "r") as fichier:
    for ligne in fichier:
        print(ligne.strip())  # strip() pour enlever les sauts de ligne

# Traitement d'un fichier CSV
import csv

with open("donnees.csv", "r") as fichier:
    lecteur_csv = csv.reader(fichier)
    next(lecteur_csv)  # Ignorer l'en-tête
    
    for ligne in lecteur_csv:
        nom, age, ville = ligne
        print(f"{nom} a {age} ans et habite à {ville}")
```

### Itération infinie avec condition d'arrêt

```python
from random import randint

# Simulation d'un jeu de devinette
nombre_secret = randint(1, 100)
tentative = 0

while True:
    tentative += 1
    essai = int(input("Devinez le nombre (entre 1 et 100): "))
    
    if essai < nombre_secret:
        print("Trop petit!")
    elif essai > nombre_secret:
        print("Trop grand!")
    else:
        print(f"Bravo! Vous avez trouvé en {tentative} tentatives.")
        break
```

## Bonnes pratiques

1. **Choisir le bon type de boucle**
   - Utilisez `for` quand vous connaissez à l'avance le nombre d'itérations ou lorsque vous parcourez une collection.
   - Utilisez `while` quand la condition d'arrêt dépend d'un facteur dynamique.

2. **Éviter les boucles imbriquées profondes**
   - Les boucles imbriquées sur plusieurs niveaux réduisent la lisibilité et les performances.
   - Refactorisez le code en fonctions ou utilisez des compréhensions quand c'est possible.

3. **Utiliser les fonctions d'aide intégrées**
   - `enumerate()` pour accéder à l'index et à la valeur.
   - `zip()` pour itérer sur plusieurs séquences simultanément.
   - `reversed()` pour parcourir une séquence à l'envers.

4. **Préférer les compréhensions et les expressions génératrices pour les opérations simples**
   - Elles sont souvent plus lisibles et parfois plus rapides que les boucles explicites.

5. **Penser aux performances pour les grandes collections**
   - Utilisez des expressions génératrices plutôt que des compréhensions de liste pour les grands ensembles de données.
   - Évitez de construire des listes intermédiaires inutiles.

6. **Éviter de modifier une collection pendant l'itération**
   - Créez une nouvelle collection ou utilisez une copie pour éviter les comportements inattendus.

```python
# Dangereux (résultats imprévisibles)
liste = [1, 2, 3, 4, 5]
for item in liste:
    if item % 2 == 0:
        liste.remove(item)

# Approche sûre
liste = [1, 2, 3, 4, 5]
liste = [item for item in liste if item % 2 != 0]
```

7. **Utiliser `else` avec parcimonie**
   - La clause `else` dans les boucles peut être source de confusion pour ceux qui ne connaissent pas bien cette fonctionnalité.
   - Documentez clairement son utilisation si nécessaire.

8. **Éviter les boucles infinies accidentelles**
   - Assurez-vous toujours qu'il existe une condition de sortie atteignable dans les boucles `while`.
   - Incluez des mécanismes de sécurité comme un compteur de tentatives ou un timeout.

## Erreurs courantes

1. **Oubli de l'incrémentation dans les boucles while**

```python
# Boucle infinie
i = 0
while i < 5:
    print(i)
    # Oubli de i += 1
```

2. **Erreurs d'indentation**

```python
for i in range(5):
    print("Dans la boucle")
    print(i)
print("Toujours dans la boucle pour i =", i)  # Erreur d'indentation
```

3. **Modification d'une collection pendant l'itération**

```python
# Peut provoquer des erreurs ou un comportement inattendu
dico = {"a": 1, "b": 2, "c": 3}
for cle in dico:
    if cle == "b":
        dico.pop(cle)  # Modification pendant l'itération: RuntimeError
```

4. **Confusion entre `range(n)` et `range(1, n)`**

```python
# range(n) commence à 0
for i in range(5):
    print(i)  # 0, 1, 2, 3, 4

# range(1, n) commence à 1
for i in range(1, 5):
    print(i)  # 1, 2, 3, 4
```

5. **Utilisation inefficace des boucles**

```python
# Inefficace
caracteres = []
for c in "Hello":
    caracteres.append(c)

# Plus efficace
caracteres = list("Hello")

# Inefficace
somme = 0
for i in range(100):
    somme += i

# Plus efficace
somme = sum(range(100))
```

6. **Mauvaise compréhension des expressions conditionnelles dans les compréhensions**

```python
# Erreur de compréhension
nombres = [1, 2, 3, 4, 5]
pairs_ou_triples = [x if x % 2 == 0 else x * 3 for x in nombres]
# [3, 2, 9, 4, 15] - La condition if/else change la valeur, pas le filtrage
```

## Ressources supplémentaires

- [Documentation officielle Python - Les structures de contrôle](https://docs.python.org/fr/3/tutorial/controlflow.html)
- [PEP 8 - Guide de style pour le code Python](https://peps.python.org/pep-0008/)
- [Real Python - Les boucles en Python](https://realpython.com/python-for-loop/)
- [Python.org - Module itertools](https://docs.python.org/fr/3/library/itertools.html)
- [Python Tips - Compréhensions de liste](https://pythontips.com/2013/07/08/list-comprehensions-explained/)
- [Python Patterns - Patterns d'itération](https://python-patterns.guide/gang-of-four/iterator/)

---

Ce chapitre vous a présenté les bases et les techniques avancées pour maîtriser les boucles en Python. Les boucles sont des outils essentiels pour automatiser des tâches répétitives et parcourir des collections de données. Dans le prochain chapitre, nous explorerons les fonctions en Python, un autre concept fondamental qui vous permettra d'organiser votre code en modules réutilisables.