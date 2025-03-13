# Les Fonctions en Python

## Table des matières

- [Introduction](#introduction)
- [Déclaration et appel de fonctions](#déclaration-et-appel-de-fonctions)
  - [Syntaxe de base](#syntaxe-de-base)
  - [Retour de valeurs](#retour-de-valeurs)
  - [Fonctions sans retour](#fonctions-sans-retour)
  - [Retours multiples](#retours-multiples)
- [Arguments et paramètres](#arguments-et-paramètres)
  - [Arguments positionnels](#arguments-positionnels)
  - [Arguments nommés](#arguments-nommés)
  - [Valeurs par défaut](#valeurs-par-défaut)
  - [Arguments variables avec *args](#arguments-variables-avec-args)
  - [Arguments mot-clés variables avec **kwargs](#arguments-mot-clés-variables-avec-kwargs)
  - [Ordre des paramètres](#ordre-des-paramètres)
  - [Arguments uniquement positionnels et uniquement nommés](#arguments-uniquement-positionnels-et-uniquement-nommés)
- [Portée des variables](#portée-des-variables)
  - [Portée locale](#portée-locale)
  - [Portée englobante](#portée-englobante)
  - [Portée globale](#portée-globale)
  - [Résolution des noms (règle LEGB)](#résolution-des-noms-règle-legb)
- [Closures](#closures)
  - [Définition et concept](#définition-et-concept)
  - [Création de closures](#création-de-closures)
  - [Applications pratiques](#applications-pratiques)
- [Fonctions d'ordre supérieur](#fonctions-dordre-supérieur)
  - [Fonctions comme arguments](#fonctions-comme-arguments)
  - [Fonctions retournant des fonctions](#fonctions-retournant-des-fonctions)
  - [Fonctions intégrées d'ordre supérieur](#fonctions-intégrées-dordre-supérieur)
- [Fonctions pures et effets de bord](#fonctions-pures-et-effets-de-bord)
  - [Caractéristiques des fonctions pures](#caractéristiques-des-fonctions-pures)
  - [Avantages des fonctions pures](#avantages-des-fonctions-pures)
  - [Identifier et éviter les effets de bord](#identifier-et-éviter-les-effets-de-bord)
- [Fonctions anonymes (lambda)](#fonctions-anonymes-lambda)
  - [Syntaxe et limites](#syntaxe-et-limites)
  - [Cas d'utilisation](#cas-dutilisation)
- [Décorateurs](#décorateurs)
  - [Concept et utilisation de base](#concept-et-utilisation-de-base)
  - [Création de décorateurs](#création-de-décorateurs)
  - [Décorateurs avec paramètres](#décorateurs-avec-paramètres)
  - [Décorateurs multiples](#décorateurs-multiples)
- [Fonctions récursives](#fonctions-récursives)
  - [Concept de récursivité](#concept-de-récursivité)
  - [Cas de base et cas récursif](#cas-de-base-et-cas-récursif)
  - [Limites et optimisations](#limites-et-optimisations)
- [Documentation des fonctions](#documentation-des-fonctions)
  - [Docstrings](#docstrings)
  - [Annotations de type](#annotations-de-type)
- [Bonnes pratiques](#bonnes-pratiques)
- [Erreurs courantes](#erreurs-courantes)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

Les fonctions sont l'un des piliers fondamentaux de la programmation en Python. Elles permettent d'encapsuler des blocs de code réutilisables, améliorant ainsi la lisibilité, la maintenabilité et la modularité des programmes. Une fonction bien conçue effectue une tâche spécifique, peut être utilisée dans différents contextes, et facilite les tests et le débogage.

En Python, les fonctions sont considérées comme des objets de première classe, ce qui signifie qu'elles peuvent être manipulées comme n'importe quelle autre variable : assignées à des variables, stockées dans des collections, passées comme arguments à d'autres fonctions, et même retournées par d'autres fonctions.

Ce chapitre couvre tous les aspects des fonctions en Python, des concepts de base aux techniques avancées telles que les closures, les fonctions d'ordre supérieur et les décorateurs.

## Déclaration et appel de fonctions

### Syntaxe de base

En Python, vous déclarez une fonction en utilisant le mot-clé `def`, suivi du nom de la fonction, de parenthèses (qui peuvent contenir des paramètres), et d'un deux-points. Le corps de la fonction est indenté sous la ligne de déclaration.

```python
def saluer():
    """Cette fonction affiche un message de salutation."""
    print("Bonjour, bienvenue !")

# Appel de la fonction
saluer()  # Affiche: Bonjour, bienvenue !
```

### Retour de valeurs

Les fonctions peuvent renvoyer des valeurs en utilisant le mot-clé `return`. Une fois qu'une instruction `return` est exécutée, la fonction se termine immédiatement et renvoie la valeur spécifiée.

```python
def additionner(a, b):
    """Cette fonction renvoie la somme de deux nombres."""
    return a + b

resultat = additionner(3, 5)
print(resultat)  # Affiche: 8
```

### Fonctions sans retour

Si une fonction ne contient pas d'instruction `return`, ou si elle contient un `return` sans valeur, elle renvoie implicitement `None`.

```python
def afficher_info(message):
    """Cette fonction affiche un message mais ne renvoie rien."""
    print(f"Info: {message}")

resultat = afficher_info("Opération complétée")  # Affiche: Info: Opération complétée
print(resultat)  # Affiche: None
```

### Retours multiples

En Python, une fonction peut renvoyer plusieurs valeurs en les séparant par des virgules. En réalité, Python crée un tuple avec ces valeurs.

```python
def obtenir_dimensions():
    """Renvoie la largeur et la hauteur d'un objet."""
    largeur = 100
    hauteur = 50
    return largeur, hauteur

dimensions = obtenir_dimensions()
print(dimensions)  # Affiche: (100, 50)
print(type(dimensions))  # Affiche: <class 'tuple'>

# Unpacking du tuple lors de l'appel
largeur, hauteur = obtenir_dimensions()
print(f"Largeur: {largeur}, Hauteur: {hauteur}")  # Affiche: Largeur: 100, Hauteur: 50
```

## Arguments et paramètres

En Python, les termes "paramètre" et "argument" sont souvent utilisés de manière interchangeable, mais ils ont des significations légèrement différentes :
- Les **paramètres** sont les variables listées dans la définition de la fonction.
- Les **arguments** sont les valeurs que vous passez à la fonction lorsque vous l'appelez.

### Arguments positionnels

Les arguments positionnels sont passés à une fonction dans l'ordre dans lequel les paramètres sont définis.

```python
def afficher_informations(nom, age, ville):
    """Affiche les informations d'une personne."""
    print(f"Nom: {nom}")
    print(f"Âge: {age}")
    print(f"Ville: {ville}")

# Appel avec arguments positionnels
afficher_informations("Alice", 30, "Paris")
```

### Arguments nommés

Les arguments nommés sont spécifiés avec le nom du paramètre suivi de la valeur. Cela permet de passer les arguments dans n'importe quel ordre.

```python
# Appel avec arguments nommés
afficher_informations(ville="Lyon", nom="Bob", age=25)

# Mélange d'arguments positionnels et nommés
# Les arguments positionnels doivent toujours venir avant les arguments nommés
afficher_informations("Charlie", ville="Marseille", age=35)
```

### Valeurs par défaut

Vous pouvez spécifier des valeurs par défaut pour les paramètres, qui seront utilisées si aucun argument n'est fourni pour ces paramètres.

```python
def saluer(nom, message="Bonjour"):
    """Salue une personne avec un message personnalisable."""
    print(f"{message}, {nom} !")

# Utilisation de la valeur par défaut pour message
saluer("David")  # Affiche: Bonjour, David !

# Remplacement de la valeur par défaut
saluer("Eva", "Bonsoir")  # Affiche: Bonsoir, Eva !
```

> **Important** : Les paramètres avec des valeurs par défaut doivent être placés après les paramètres sans valeurs par défaut dans la définition de la fonction.

```python
# Correct
def fonction(a, b, c=10, d=20):
    pass

# Incorrect - SyntaxError
def fonction(a, b=10, c, d=20):
    pass
```

### Arguments variables avec *args

L'opérateur `*` permet de passer un nombre variable d'arguments positionnels à une fonction. Ces arguments sont collectés dans un tuple.

```python
def calculer_somme(*nombres):
    """Calcule la somme de tous les nombres fournis."""
    resultat = 0
    for nombre in nombres:
        resultat += nombre
    return resultat

# Appel avec différents nombres d'arguments
print(calculer_somme(1, 2))  # Affiche: 3
print(calculer_somme(1, 2, 3, 4, 5))  # Affiche: 15

# Passage d'une liste dépaquetée
nombres = [10, 20, 30]
print(calculer_somme(*nombres))  # Affiche: 60
```

### Arguments mot-clés variables avec **kwargs

L'opérateur `**` permet de passer un nombre variable d'arguments nommés à une fonction. Ces arguments sont collectés dans un dictionnaire.

```python
def afficher_profil(**infos):
    """Affiche toutes les informations fournies sur un profil."""
    print("Profil:")
    for cle, valeur in infos.items():
        print(f"  {cle}: {valeur}")

# Appel avec différents arguments nommés
afficher_profil(nom="Frank", age=40, ville="Bordeaux", profession="Ingénieur")

# Passage d'un dictionnaire dépaqueté
infos_personne = {"nom": "Grace", "age": 28, "ville": "Lille"}
afficher_profil(**infos_personne)
```

### Ordre des paramètres

Lorsque vous définissez une fonction avec différents types de paramètres, ils doivent suivre cet ordre spécifique :

1. Paramètres positionnels normaux
2. Paramètres avec valeurs par défaut
3. Paramètres variables (`*args`)
4. Paramètres nommés uniquement (après `*args` ou `*`)
5. Paramètres mot-clés variables (`**kwargs`)

```python
def fonction_complete(
    pos1, pos2,                # Paramètres positionnels
    defaut1="a", defaut2="b",  # Paramètres avec valeurs par défaut
    *args,                     # Arguments positionnels variables
    nomme1, nomme2,            # Paramètres nommés uniquement
    **kwargs                   # Arguments mot-clés variables
):
    print(f"Positionnels: {pos1}, {pos2}")
    print(f"Par défaut: {defaut1}, {defaut2}")
    print(f"Args: {args}")
    print(f"Nommés uniquement: {nomme1}, {nomme2}")
    print(f"Kwargs: {kwargs}")

fonction_complete(
    1, 2,
    "c", "d",
    3, 4, 5,
    nomme1="x", nomme2="y",
    extra1="e1", extra2="e2"
)
```

### Arguments uniquement positionnels et uniquement nommés

À partir de Python 3.8, vous pouvez spécifier explicitement quels paramètres doivent être positionnels ou nommés :

```python
def fonction_moderne(
    pos_only1, pos_only2, /,  # Paramètres uniquement positionnels (avant /)
    standard1, standard2,     # Paramètres standards (peuvent être positionnels ou nommés)
    *, kw_only1, kw_only2     # Paramètres uniquement nommés (après *)
):
    print(f"Uniquement positionnels: {pos_only1}, {pos_only2}")
    print(f"Standards: {standard1}, {standard2}")
    print(f"Uniquement nommés: {kw_only1}, {kw_only2}")

# Appel valide
fonction_moderne(
    1, 2,                     # pos_only1, pos_only2 (doivent être positionnels)
    3, standard2=4,           # standard1, standard2 (peuvent être positionnels ou nommés)
    kw_only1=5, kw_only2=6    # kw_only1, kw_only2 (doivent être nommés)
)

# Appel invalide - TypeError
# fonction_moderne(pos_only1=1, 2, 3, 4, kw_only1=5, kw_only2=6)
```

## Portée des variables

La portée d'une variable détermine d'où cette variable est accessible dans votre code.

### Portée locale

Les variables définies à l'intérieur d'une fonction ont une portée locale et ne sont accessibles qu'à l'intérieur de cette fonction.

```python
def calculer():
    x = 10  # Variable locale
    y = 20  # Variable locale
    somme = x + y
    return somme

resultat = calculer()
print(resultat)  # Affiche: 30

# print(x)  # NameError: name 'x' is not defined
```

### Portée englobante

Les fonctions imbriquées peuvent accéder aux variables de la fonction qui les contient.

```python
def externe():
    x = "externe"
    
    def interne():
        print(f"Dans interne, x = {x}")
    
    interne()
    print(f"Dans externe, x = {x}")

externe()
# Affiche:
# Dans interne, x = externe
# Dans externe, x = externe
```

### Portée globale

Les variables définies au niveau supérieur d'un module ont une portée globale et sont accessibles dans tout le module.

```python
x = "globale"  # Variable globale

def afficher_x():
    print(f"Dans afficher_x, x = {x}")

afficher_x()  # Affiche: Dans afficher_x, x = globale
print(f"Au niveau global, x = {x}")  # Affiche: Au niveau global, x = globale
```

Pour modifier une variable globale à l'intérieur d'une fonction, vous devez utiliser le mot-clé `global`.

```python
compteur = 0  # Variable globale

def incrementer():
    global compteur
    compteur += 1
    print(f"Compteur: {compteur}")

incrementer()  # Affiche: Compteur: 1
incrementer()  # Affiche: Compteur: 2
print(compteur)  # Affiche: 2
```

De même, pour modifier une variable de la portée englobante dans une fonction imbriquée, vous devez utiliser le mot-clé `nonlocal`.

```python
def compteur_externe():
    compteur = 0
    
    def incrementer():
        nonlocal compteur
        compteur += 1
        return compteur
    
    return incrementer

incrementer = compteur_externe()
print(incrementer())  # Affiche: 1
print(incrementer())  # Affiche: 2
print(incrementer())  # Affiche: 3
```

### Résolution des noms (règle LEGB)

Python suit la règle LEGB pour résoudre les noms de variables :

1. **L**ocal - Variables définies dans la fonction actuelle
2. **E**nclosing - Variables définies dans les fonctions englobantes
3. **G**lobal - Variables définies au niveau du module
4. **B**uilt-in - Noms prédéfinis dans Python (comme `print`, `len`, etc.)

```python
x = "globale"

def externe():
    x = "englobante"
    
    def interne():
        x = "locale"
        print(f"interne: {x}")  # Utilise la variable locale
    
    interne()
    print(f"externe: {x}")  # Utilise la variable de la portée englobante

externe()
print(f"globale: {x}")  # Utilise la variable globale

# Affiche:
# interne: locale
# externe: englobante
# globale: globale
```

## Closures

### Définition et concept

Une closure est une fonction qui conserve l'accès aux variables de la portée dans laquelle elle a été définie, même après que cette portée ait fini d'être exécutée. En d'autres termes, la fonction "capture" et conserve l'environnement dans lequel elle a été créée.

### Création de closures

Pour créer une closure en Python, vous devez :
1. Définir une fonction à l'intérieur d'une autre fonction
2. La fonction interne doit référencer une variable de la fonction externe
3. La fonction externe doit retourner la fonction interne

```python
def creer_multiplicateur(facteur):
    """Crée une fonction qui multiplie son argument par un facteur donné."""
    def multiplicateur(nombre):
        return nombre * facteur  # Utilise 'facteur' de la portée englobante
    
    return multiplicateur

# Création de deux multiplicateurs différents
doubler = creer_multiplicateur(2)
tripler = creer_multiplicateur(3)

print(doubler(5))  # Affiche: 10
print(tripler(5))  # Affiche: 15

# La variable 'facteur' est préservée dans la closure
print(doubler.__closure__[0].cell_contents)  # Affiche: 2
print(tripler.__closure__[0].cell_contents)  # Affiche: 3
```

### Applications pratiques

Les closures sont utiles dans de nombreux contextes :

1. **Création de fonctions configurables**

```python
def formater_nombre(decimales):
    """Crée une fonction de formatage avec un nombre spécifique de décimales."""
    format_string = f"{{:.{decimales}f}}"
    
    def formateur(nombre):
        return format_string.format(nombre)
    
    return formateur

formater_2_decimales = formater_nombre(2)
formater_4_decimales = formater_nombre(4)

print(formater_2_decimales(3.14159))  # Affiche: 3.14
print(formater_4_decimales(3.14159))  # Affiche: 3.1416
```

2. **Maintien d'un état interne**

```python
def creer_compteur(depart=0, pas=1):
    """Crée un compteur avec une valeur de départ et un pas spécifiés."""
    compteur = depart
    
    def incrementer():
        nonlocal compteur
        valeur_actuelle = compteur
        compteur += pas
        return valeur_actuelle
    
    return incrementer

compteur = creer_compteur(10, 5)
print(compteur())  # Affiche: 10
print(compteur())  # Affiche: 15
print(compteur())  # Affiche: 20
```

3. **Décorateurs personnalisés** (voir section sur les décorateurs)

## Fonctions d'ordre supérieur

Les fonctions d'ordre supérieur sont des fonctions qui peuvent prendre d'autres fonctions comme arguments et/ou renvoyer des fonctions comme résultats.

### Fonctions comme arguments

```python
def appliquer_operation(fonction, valeur):
    """Applique une fonction à une valeur."""
    return fonction(valeur)

def doubler(x):
    return x * 2

def carre(x):
    return x ** 2

# Passage de fonctions comme arguments
print(appliquer_operation(doubler, 5))  # Affiche: 10
print(appliquer_operation(carre, 5))    # Affiche: 25
```

### Fonctions retournant des fonctions

Nous avons déjà vu plusieurs exemples de fonctions retournant des fonctions dans la section sur les closures.

```python
def creer_verificateur(condition):
    """Crée une fonction qui vérifie si une valeur satisfait une condition."""
    def verificateur(valeur):
        return condition(valeur)
    
    return verificateur

# Création de fonctions de vérification spécifiques
est_positif = creer_verificateur(lambda x: x > 0)
est_pair = creer_verificateur(lambda x: x % 2 == 0)

print(est_positif(5))   # Affiche: True
print(est_positif(-3))  # Affiche: False
print(est_pair(4))      # Affiche: True
print(est_pair(7))      # Affiche: False
```

### Fonctions intégrées d'ordre supérieur

Python propose plusieurs fonctions d'ordre supérieur intégrées :

1. **map()** - Applique une fonction à chaque élément d'un itérable

```python
nombres = [1, 2, 3, 4, 5]

# Doubler chaque nombre
resultats = map(lambda x: x * 2, nombres)
print(list(resultats))  # Affiche: [2, 4, 6, 8, 10]

# Utilisation avec une fonction nommée
def carre(x):
    return x ** 2

resultats = map(carre, nombres)
print(list(resultats))  # Affiche: [1, 4, 9, 16, 25]
```

2. **filter()** - Filtre les éléments d'un itérable selon une fonction de prédicat

```python
nombres = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Filtrer les nombres pairs
nombres_pairs = filter(lambda x: x % 2 == 0, nombres)
print(list(nombres_pairs))  # Affiche: [2, 4, 6, 8, 10]

# Filtrer les nombres premiers
def est_premier(n):
    if n <= 1:
        return False
    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False
    return True

nombres_premiers = filter(est_premier, nombres)
print(list(nombres_premiers))  # Affiche: [2, 3, 5, 7]
```

3. **reduce()** - Réduit un itérable à une seule valeur en appliquant une fonction d'agrégation

```python
from functools import reduce

nombres = [1, 2, 3, 4, 5]

# Calculer la somme
somme = reduce(lambda x, y: x + y, nombres)
print(somme)  # Affiche: 15

# Calculer le produit
produit = reduce(lambda x, y: x * y, nombres)
print(produit)  # Affiche: 120

# Trouver le maximum
maximum = reduce(lambda x, y: x if x > y else y, nombres)
print(maximum)  # Affiche: 5
```

4. **sorted()** - Trie un itérable en utilisant une fonction clé

```python
personnes = [
    {"nom": "Alice", "age": 30},
    {"nom": "Bob", "age": 25},
    {"nom": "Charlie", "age": 35}
]

# Tri par âge
tri_par_age = sorted(personnes, key=lambda p: p["age"])
for p in tri_par_age:
    print(f"{p['nom']}: {p['age']} ans")
# Affiche:
# Bob: 25 ans
# Alice: 30 ans
# Charlie: 35 ans

# Tri par nom
tri_par_nom = sorted(personnes, key=lambda p: p["nom"])
for p in tri_par_nom:
    print(f"{p['nom']}: {p['age']} ans")
# Affiche:
# Alice: 30 ans
# Bob: 25 ans
# Charlie: 35 ans
```

## Fonctions pures et effets de bord

### Caractéristiques des fonctions pures

Une fonction pure est une fonction qui :
1. Renvoie toujours le même résultat pour les mêmes arguments (déterminisme)
2. N'a pas d'effets de bord, c'est-à-dire qu'elle ne modifie pas d'état en dehors de sa portée locale

```python
# Fonction pure
def additionner(a, b):
    return a + b

# Fonction pure
def calculer_cercle(rayon):
    return {
        "diametre": rayon * 2,
        "circonference": 2 * 3.14159 * rayon,
        "aire": 3.14159 * rayon ** 2
    }

# Fonction avec effets de bord
def ajouter_element(liste, element):
    liste.append(element)  # Modifie l'état externe (la liste)
    return liste
```

### Avantages des fonctions pures

Les fonctions pures offrent plusieurs avantages :
1. **Facilité de test** - Les entrées et sorties sont prévisibles
2. **Facilité de raisonnement** - Pas d'interactions complexes avec l'état global
3. **Parallélisation** - Peuvent être exécutées en parallèle sans risque de conflits
4. **Mise en cache** - Les résultats peuvent être mis en cache pour les mêmes entrées
5. **Modularité** - Peuvent être facilement combinées et composées

### Identifier et éviter les effets de bord

Les effets de bord courants incluent :
1. Modification de variables globales
2. Modification d'arguments mutables
3. Opérations d'entrée/sortie (E/S)
4. Génération d'exceptions
5. Opérations sur le système de fichiers
6. Requêtes réseau

Exemple de transformation d'une fonction avec effets de bord en fonction pure :

```python
# Fonction avec effets de bord
def ajouter_taxe(produit):
    produit["prix_ttc"] = produit["prix_ht"] * 1.2
    return produit

# Version pure
def calculer_prix_ttc(produit):
    return {
        **produit,  # Crée une copie de produit
        "prix_ttc": produit["prix_ht"] * 1.2
    }

# Utilisation
produit_original = {"nom": "Ordinateur", "prix_ht": 1000}

# Avec effets de bord - modifie l'original
produit_modifie = ajouter_taxe(produit_original)
print(produit_original)  # Affiche: {'nom': 'Ordinateur', 'prix_ht': 1000, 'prix_ttc': 1200.0}
print(produit_original is produit_modifie)  # Affiche: True

# Version pure - crée un nouvel objet
produit_original = {"nom": "Ordinateur", "prix_ht": 1000}
produit_avec_ttc = calculer_prix_ttc(produit_original)
print(produit_original)  # Affiche: {'nom': 'Ordinateur', 'prix_ht': 1000}
print(produit_avec_ttc)  # Affiche: {'nom': 'Ordinateur', 'prix_ht': 1000, 'prix_ttc': 1200.0}
print(produit_original is produit_avec_ttc)  # Affiche: False
```

## Fonctions anonymes (lambda)

### Syntaxe et limites

Les fonctions lambda sont des fonctions anonymes, définies en une seule ligne, avec une syntaxe concise.

```python
# Syntaxe: lambda parametres: expression
doubler = lambda x: x * 2
print(doubler(5))  # Affiche: 10
```

Limites des fonctions lambda :
1. Elles sont limitées à une seule expression
2. Elles ne peuvent pas contenir d'instructions (comme `if`, `for`, etc.)
3. Elles ne peuvent pas avoir de docstring

### Cas d'utilisation

Les fonctions lambda sont particulièrement utiles :
1. Comme arguments de fonctions d'ordre supérieur
2. Pour des transformations simples
3. Pour des tris personnalisés
4. Pour des filtrages rapides

```python
# Avec sorted()
personnes = [("Alice", 30), ("Bob", 25), ("Charlie", 35)]
tri_par_age = sorted(personnes, key=lambda personne: personne[1])
print(tri_par_age)  # Affiche: [('Bob', 25), ('Alice', 30), ('Charlie', 35)]

# Avec filter()
nombres = [1, 2, 3, 4, 5, 6]
pairs = list(filter(lambda x: x % 2 == 0, nombres))
print(pairs)  # Affiche: [2, 4, 6]

# Avec map()
carres = list(map(lambda x: x ** 2, nombres))
print(carres)  # Affiche: [1, 4, 9, 16, 25, 36]

# Dans des expressions conditionnelles
valeur_absolue = lambda x: x if x >= 0 else -x
print(valeur_absolue(-5))  # Affiche: 5
```

## Décorateurs

### Concept et utilisation de base

Les décorateurs sont une façon élégante d'étendre ou de modifier le comportement d'une fonction sans modifier son code. Un décorateur est essentiellement une fonction qui prend une fonction comme argument et renvoie une nouvelle fonction qui enveloppe la fonction originale.

```python
def decorateur_simple(fonction):
    def wrapper(*args, **kwargs):
        print("Avant l'appel de la fonction")
        resultat = fonction(*args, **kwargs)
        print("Après l'appel de la fonction")
        return resultat
    return wrapper

# Application manuelle du décorateur
def saluer():
    print("Bonjour !")

saluer_decoree = decorateur_simple(saluer)
saluer_decoree()  # Affiche trois lignes

# Utilisation de la syntaxe @decorateur
@decorateur_simple
def au_revoir():
    print("Au revoir !")

au_revoir()  # Affiche trois lignes
```

### Création de décorateurs

Exemple de décorateur qui mesure le temps d'exécution d'une fonction :

```python
import time

def mesurer_temps(fonction):
    def wrapper(*args, **kwargs):
        debut = time.time()
        resultat = fonction(*args, **kwargs)
        fin = time.time()
        print(f"La fonction {fonction.__name__} a pris {fin - debut:.4f} secondes")
        return resultat
    return wrapper

@mesurer_temps
def operation_lente():
    time.sleep(1)
    return "Opération terminée"

print(operation_lente())
# Affiche:
# La fonction operation_lente a pris 1.0010 secondes
# Opération terminée
```

### Décorateurs avec paramètres

Pour créer un décorateur qui accepte des paramètres, vous devez ajouter un niveau d'imbrication supplémentaire :

```python
def repetition(nombre=1):
    def decorateur(fonction):
        def wrapper(*args, **kwargs):
            resultats = []
            for _ in range(nombre):
                resultats.append(fonction(*args, **kwargs))
            return resultats
        return wrapper
    return decorateur

@repetition(3)
def saluer(nom):
    return f"Bonjour, {nom} !"

print(saluer("Alice"))
# Affiche: ['Bonjour, Alice !', 'Bonjour, Alice !', 'Bonjour, Alice !']
```

### Décorateurs multiples

Vous pouvez appliquer plusieurs décorateurs à une même fonction. Ils sont appliqués de bas en haut.

```python
def decorateur_a(fonction):
    def wrapper(*args, **kwargs):
        print("Décorateur A - Avant")
        resultat = fonction(*args, **kwargs)
        print("Décorateur A - Après")
        return resultat
    return wrapper

def decorateur_b(fonction):
    def wrapper(*args, **kwargs):
        print("Décorateur B - Avant")
        resultat = fonction(*args, **kwargs)
        print("Décorateur B - Après")
        return resultat
    return wrapper

@decorateur_a
@decorateur_b
def fonction_decoree():
    print("Fonction originale")

fonction_decoree()
# Affiche:
# Décorateur A - Avant
# Décorateur B - Avant
# Fonction originale
# Décorateur B - Après
# Décorateur A - Après
```

## Fonctions récursives

### Concept de récursivité

Une fonction récursive est une fonction qui s'appelle elle-même pour résoudre un problème en le décomposant en sous-problèmes plus simples.

### Cas de base et cas récursif

Toute fonction récursive doit avoir :
1. Un **cas de base** - une condition d'arrêt pour éviter une récursion infinie
2. Un **cas récursif** - où la fonction s'appelle elle-même avec un problème plus petit

```python
# Calcul factoriel avec récursion
def factoriel(n):
    # Cas de base
    if n == 0 or n == 1:
        return 1
    # Cas récursif
    else:
        return n * factoriel(n - 1)

print(factoriel(5))  # Affiche: 120 (5 * 4 * 3 * 2 * 1)
```

### Limites et optimisations

Les fonctions récursives peuvent atteindre la limite de récursion de Python (par défaut 1000) et peuvent être inefficaces pour des problèmes complexes en raison de l'empilement des appels.

```python
import sys
print(sys.getrecursionlimit())  # Affiche la limite de récursion actuelle

# Modification de la limite (à utiliser avec prudence)
# sys.setrecursionlimit(2000)
```

Techniques d'optimisation :

1. **Mémoïsation** - Stockage des résultats intermédiaires

```python
# Fibonacci avec mémoïsation
def fibonacci_memo(n, memo={}):
    if n in memo:
        return memo[n]
    
    if n <= 1:
        resultat = n
    else:
        resultat = fibonacci_memo(n-1, memo) + fibonacci_memo(n-2, memo)
    
    memo[n] = resultat
    return resultat

print(fibonacci_memo(40))  # Rapide
```

2. **Décorateur `lru_cache`** - Mémoïsation automatique

```python
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(40))  # Très rapide
```

3. **Récursion terminale** - Optimisation pour certains langages (pas Python)

```python
def factoriel_tail(n, acc=1):
    if n <= 1:
        return acc
    return factoriel_tail(n-1, n*acc)
```

## Documentation des fonctions

### Docstrings

Les docstrings sont des chaînes de documentation placées juste après la déclaration d'une fonction. Elles décrivent l'objectif, les paramètres et la valeur de retour de la fonction.

```python
def calculer_moyenne(nombres):
    """
    Calcule la moyenne d'une liste de nombres.
    
    Args:
        nombres (list): Une liste de nombres.
        
    Returns:
        float: La moyenne des nombres.
        
    Raises:
        ValueError: Si la liste est vide.
        TypeError: Si la liste contient des éléments non numériques.
    """
    if not nombres:
        raise ValueError("La liste ne peut pas être vide")
    
    return sum(nombres) / len(nombres)
```

Accès à la documentation :

```python
print(calculer_moyenne.__doc__)  # Affiche la docstring
help(calculer_moyenne)  # Affiche une aide formatée
```

### Annotations de type

Les annotations de type permettent de spécifier les types attendus des paramètres et de la valeur de retour d'une fonction.

```python
def saluer(nom: str, age: int) -> str:
    """Renvoie un message de salutation personnalisé."""
    return f"Bonjour {nom}, vous avez {age} ans."

# Les annotations sont stockées dans l'attribut __annotations__
print(saluer.__annotations__)  # Affiche: {'nom': <class 'str'>, 'age': <class 'int'>, 'return': <class 'str'>}
```

Annotations pour les types complexes (nécessite le module `typing`) :

```python
from typing import List, Dict, Tuple, Optional, Union, Callable

def fonction_complexe(
    nombres: List[int],
    mapping: Dict[str, int],
    coordonnees: Tuple[int, int],
    optionnel: Optional[str] = None,
    union: Union[int, str] = 0,
    callback: Callable[[int], bool] = lambda x: x > 0
) -> Dict[str, List[int]]:
    """Exemple de fonction avec des annotations de type complexes."""
    return {"resultats": []}
```

## Bonnes pratiques

1. **Écrivez des fonctions qui font une seule chose**
   - Une fonction doit avoir une responsabilité unique et clairement définie.
   - Si une fonction devient trop longue ou complexe, divisez-la en plusieurs fonctions plus petites.

2. **Choisissez des noms de fonctions descriptifs**
   - Les noms de fonctions doivent être des verbes ou des expressions verbales.
   - Le nom doit clairement indiquer ce que fait la fonction.
   - Exemple : `calculer_moyenne`, `verifier_autorisation`, `obtenir_utilisateur`

3. **Utilisez des arguments nommés pour la clarté**
   - Spécialement pour les fonctions avec de nombreux paramètres.
   - Exemple : `plot(x=data, y=labels, color='blue', size=12)`

4. **Limitez le nombre de paramètres**
   - Une fonction avec trop de paramètres devient difficile à utiliser et à tester.
   - Considérez l'utilisation d'objets ou de dictionnaires pour regrouper les paramètres connexes.

5. **Renvoyez des résultats plutôt que de modifier des objets**
   - Préférez les fonctions pures qui renvoient de nouveaux objets.
   - Si la modification est nécessaire, documentez clairement ce comportement.

6. **Documentez vos fonctions**
   - Utilisez des docstrings pour décrire le but, les paramètres et le retour.
   - Incluez des exemples d'utilisation pour les fonctions complexes.

7. **Écrivez des fonctions défensives**
   - Validez les entrées au début de la fonction.
   - Renvoyez des erreurs claires en cas d'entrées invalides.

```python
def diviser(a, b):
    """
    Divise a par b.
    
    Args:
        a (float): Le numérateur
        b (float): Le dénominateur
        
    Returns:
        float: Le résultat de la division
        
    Raises:
        ValueError: Si b est zéro
        TypeError: Si a ou b ne sont pas des nombres
    """
    if not isinstance(a, (int, float)) or not isinstance(b, (int, float)):
        raise TypeError("Les arguments doivent être des nombres")
    
    if b == 0:
        raise ValueError("Division par zéro impossible")
    
    return a / b
```

8. **Utilisez des valeurs par défaut sensées**
   - Choisissez des valeurs par défaut qui couvrent les cas d'utilisation les plus courants.
   - Évitez d'utiliser des objets mutables comme valeurs par défaut.

```python
# Problématique
def ajouter_element(element, liste=[]):
    liste.append(element)
    return liste

print(ajouter_element(1))  # [1]
print(ajouter_element(2))  # [1, 2] !

# Correct
def ajouter_element(element, liste=None):
    if liste is None:
        liste = []
    liste.append(element)
    return liste
```

9. **Préférez des fonctions explicites aux fonctions implicites**
   - Évitez les fonctions qui ont des comportements cachés ou qui dépendent d'un état global.
   - Rendez visible tout ce qui est important pour comprendre le comportement de la fonction.

10. **Testez vos fonctions**
    - Écrivez des tests unitaires pour vérifier que vos fonctions fonctionnent comme prévu.
    - Testez les cas limites et les conditions d'erreur.

## Erreurs courantes

1. **Oublier de renvoyer une valeur**

```python
# Problème
def calculer_somme(a, b):
    resultat = a + b
    # Oubli du return

# Solution
def calculer_somme(a, b):
    resultat = a + b
    return resultat
```

2. **Variables globales modifiées accidentellement**

```python
# Problème
total = 0

def ajouter(valeur):
    total += valeur  # UnboundLocalError: local variable 'total' referenced before assignment

# Solution
def ajouter(valeur):
    global total
    total += valeur
```

3. **Objets mutables comme valeurs par défaut**

```python
# Problème (déjà vu ci-dessus)
def ajouter_element(element, liste=[]):
    liste.append(element)
    return liste

# Solution (déjà vue ci-dessus)
def ajouter_element(element, liste=None):
    if liste is None:
        liste = []
    liste.append(element)
    return liste
```

4. **Mauvaise ordre des paramètres**

```python
# Problème
def diviser(diviseur, dividende):
    return dividende / diviseur

# Appel confus
resultat = diviser(10, 2)  # 0.2 (attendu: 5)

# Solution
def diviser(dividende, diviseur):
    return dividende / diviseur

# Ou utiliser des arguments nommés
resultat = diviser(diviseur=2, dividende=10)  # 5
```

5. **Récursion sans condition d'arrêt**

```python
# Problème
def recursive_sans_fin(n):
    return 1 + recursive_sans_fin(n-1)  # RecursionError

# Solution
def recursive_avec_fin(n):
    if n <= 0:
        return 0
    return 1 + recursive_avec_fin(n-1)
```

6. **Mauvaise gestion des exceptions**

```python
# Problème
def convertir_en_entier(texte):
    try:
        return int(texte)
    except:  # Attrape toutes les exceptions, y compris SystemExit, KeyboardInterrupt, etc.
        return 0

# Solution
def convertir_en_entier(texte):
    try:
        return int(texte)
    except ValueError:  # Attrape spécifiquement les erreurs de conversion
        return 0
```

7. **Shadowing des fonctions intégrées**

```python
# Problème
def sum(liste):  # Masque la fonction intégrée sum()
    total = 0
    for element in liste:
        total += element
    return total

# Solution
def calculer_somme(liste):  # Nom différent
    total = 0
    for element in liste:
        total += element
    return total
```

8. **Effet de bord non documenté**

```python
# Problème
def trier_liste(liste):
    liste.sort()  # Modifie la liste originale sans le mentionner
    return liste

# Solution
def trier_liste(liste):
    """
    Trie la liste en place et retourne la liste triée.
    
    Note: Cette fonction modifie la liste originale.
    """
    liste.sort()
    return liste

# Ou mieux
def obtenir_liste_triee(liste):
    """
    Retourne une nouvelle liste triée sans modifier l'originale.
    """
    return sorted(liste)  # Crée une nouvelle liste
```

## Ressources supplémentaires

- [Documentation officielle Python - Fonctions](https://docs.python.org/fr/3/tutorial/controlflow.html#defining-functions)
- [PEP 8 - Guide de style pour le code Python](https://peps.python.org/pep-0008/)
- [PEP 257 - Conventions pour les docstrings](https://peps.python.org/pep-0257/)
- [PEP 484 - Annotations de type](https://peps.python.org/pep-0484/)
- [Real Python - Les fonctions en Python](https://realpython.com/defining-your-own-python-function/)
- [Python.org - Programmation fonctionnelle en Python](https://docs.python.org/fr/3/howto/functional.html)
- [Python Cookbook - Recettes sur les fonctions](https://www.oreilly.com/library/view/python-cookbook-3rd/9781449357337/)

---

Ce chapitre vous a présenté les concepts fondamentaux et avancés des fonctions en Python. Les fonctions sont essentielles pour structurer votre code, le rendre plus lisible, maintenable et réutilisable. Dans le prochain chapitre, nous explorerons la programmation orientée objet à travers les classes en Python, ce qui vous permettra d'organiser votre code de manière encore plus modulaire et intuitive.