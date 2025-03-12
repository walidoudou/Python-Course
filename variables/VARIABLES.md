# Les Variables en Python

## Table des matières

- [Introduction](#introduction)
- [Déclaration de variables](#déclaration-de-variables)
- [Portée des variables](#portée-des-variables)
  - [Portée locale](#portée-locale)
  - [Portée globale](#portée-globale)
  - [Portée nonlocal](#portée-nonlocal)
- [Types de données](#types-de-données)
  - [Types primitifs](#types-primitifs)
  - [Types de séquences et collections](#types-de-séquences-et-collections)
  - [Autres types](#autres-types)
- [Conversion de types](#conversion-de-types)
- [Nommage des variables](#nommage-des-variables)
- [Assignation multiple et unpacking](#assignation-multiple-et-unpacking)
- [Variables et mutabilité](#variables-et-mutabilité)
- [Bonnes pratiques](#bonnes-pratiques)
- [Exemples avancés](#exemples-avancés)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

Les variables sont des espaces de stockage nommés qui contiennent des données. En Python, les variables sont dynamiquement typées, ce qui signifie que le type d'une variable est déterminé par la valeur qu'elle contient, et peut changer au cours de l'exécution du programme.

Contrairement à d'autres langages, Python ne nécessite pas de déclaration explicite du type de variable ou même de mot-clé pour définir une variable. Une variable Python est créée au moment où vous lui assignez une valeur.

Ce chapitre couvre tout ce que vous devez savoir sur les variables en Python, des concepts de base aux techniques avancées.

## Déclaration de variables

En Python, vous déclarez une variable simplement en lui assignant une valeur. Il n'est pas nécessaire de déclarer le type de données ou d'utiliser un mot-clé spécifique.

```python
# Déclaration de variables de différents types
nom = "Alice"           # chaîne de caractères (str)
age = 30                # entier (int)
taille = 1.75           # nombre à virgule flottante (float)
est_etudiant = True     # booléen (bool)
hobbies = ["lecture", "natation", "vélo"]  # liste (list)
informations = {"nom": "Alice", "age": 30}  # dictionnaire (dict)
```

Contrairement à certains langages comme JavaScript, Python n'a pas de mots-clés comme `var`, `let` ou `const`. Toutes les variables sont modifiables (à l'exception des constantes par convention).

Pour définir une constante (par convention), on utilise habituellement des majuscules :

```python
PI = 3.14159
MAX_UTILISATEURS = 100
URL_API = "https://api.exemple.com"
```

Python ne peut pas empêcher techniquement la modification de ces "constantes", mais par convention les développeurs comprennent que ces variables ne devraient pas être modifiées.

## Portée des variables

La portée (scope) d'une variable détermine où cette variable est accessible dans votre code.

### Portée locale

Les variables définies à l'intérieur d'une fonction ont une portée locale et ne sont accessibles qu'à l'intérieur de cette fonction.

```python
def calculer_somme():
    a = 5         # Variable locale
    b = 10        # Variable locale
    resultat = a + b
    print(resultat)  # 15

calculer_somme()
# print(a)  # Erreur : NameError: name 'a' is not defined
```

### Portée globale

Les variables définies au niveau supérieur d'un module ont une portée globale et sont accessibles dans tout le module.

```python
message = "Bonjour tout le monde"  # Variable globale

def afficher_message():
    print(message)  # Accès à la variable globale

afficher_message()  # "Bonjour tout le monde"
print(message)      # "Bonjour tout le monde"
```

Pour modifier une variable globale à l'intérieur d'une fonction, il faut utiliser le mot-clé `global` :

```python
compteur = 0  # Variable globale

def incrementer():
    global compteur  # Déclaration de l'utilisation de la variable globale
    compteur += 1
    print(compteur)

incrementer()  # 1
incrementer()  # 2
print(compteur)  # 2
```

### Portée nonlocal

Pour les fonctions imbriquées, on peut utiliser le mot-clé `nonlocal` pour accéder et modifier une variable de la fonction englobante :

```python
def fonction_externe():
    x = 10
    
    def fonction_interne():
        nonlocal x
        x += 5
        print("Valeur de x dans fonction_interne:", x)
    
    fonction_interne()
    print("Valeur de x dans fonction_externe:", x)

fonction_externe()
# Valeur de x dans fonction_interne: 15
# Valeur de x dans fonction_externe: 15
```

## Types de données

Python possède plusieurs types de données intégrés. Voici les principaux :

### Types primitifs

```python
# Nombres
entier = 42            # int (entier)
decimal = 3.14         # float (nombre à virgule flottante)
complexe = 1 + 2j      # complex (nombre complexe)

# Chaînes de caractères
nom = "Python"         # str (chaîne de caractères)
caractere = 'a'        # En Python, pas de type spécifique pour un caractère
chaine_multiligne = """Ceci est une chaîne
qui s'étend sur
plusieurs lignes"""

# Format string (f-string)
personne = "Marie"
age = 25
message = f"{personne} a {age} ans"  # "Marie a 25 ans"

# Booléens
vrai = True            # bool (booléen)
faux = False           # bool (booléen)

# Type NoneType
absence = None         # None (représente l'absence de valeur)
```

### Types de séquences et collections

```python
# Listes (ordonnées, modifiables, peuvent contenir des doublons)
fruits = ["pomme", "banane", "orange"]
mixte = [1, "deux", True, [3, 4]]

# Tuples (ordonnés, non modifiables)
coordonnees = (10, 20)
point3d = (1, 2, 3)

# Ensembles (non ordonnés, pas de doublons)
couleurs = {"rouge", "vert", "bleu"}
nombres_uniques = {1, 2, 3, 3}  # stocke {1, 2, 3}

# Dictionnaires (collections de paires clé-valeur)
personne = {
    "nom": "Durand",
    "prenom": "Pierre",
    "age": 35,
    "adresse": {
        "rue": "123 Avenue de l'Exemple",
        "ville": "Paris"
    }
}
```

### Autres types

```python
# Bytes et bytearray
donnees_bytes = b"hello"        # bytes (immutable)
donnees_modifiables = bytearray(b"hello")  # bytearray (mutable)

# Range
sequence = range(5)             # range(0, 5)
```

## Conversion de types

Python permet de convertir facilement entre différents types de données :

```python
# Conversion en entier
nombre_entier = int("42")         # 42
nombre_entier_hex = int("2A", 16) # 42 (conversion depuis hexadécimal)
nombre_entier_bin = int("101010", 2)  # 42 (conversion depuis binaire)
nombre_depuis_float = int(42.9)   # 42 (tronque la partie décimale)

# Conversion en float
nombre_decimal = float("3.14")    # 3.14
nombre_depuis_int = float(42)     # 42.0

# Conversion en chaîne
texte = str(42)                   # "42"
texte_bool = str(True)            # "True"
texte_liste = str([1, 2, 3])      # "[1, 2, 3]"

# Conversion en booléen
# Tout est True sauf : False, None, 0, "", [], (), {}, set()
bool_depuis_int = bool(0)         # False
bool_depuis_str = bool("")        # False
bool_depuis_liste = bool([])      # False
bool_depuis_dict = bool({})       # False

# Conversion en liste, tuple, set
liste_depuis_tuple = list((1, 2, 3))  # [1, 2, 3]
tuple_depuis_liste = tuple([1, 2, 3]) # (1, 2, 3)
ensemble_depuis_liste = set([1, 2, 2, 3])  # {1, 2, 3}

# Conversion en dictionnaire
dict_depuis_tuples = dict([("a", 1), ("b", 2)])  # {"a": 1, "b": 2}
```

## Nommage des variables

Python a des conventions de nommage spécifiques qui sont documentées dans la PEP 8 (Guide de style pour le code Python).

### Conventions et bonnes pratiques

```python
# snake_case pour les variables et fonctions (recommandé)
nombre_utilisateurs = 42
def calculer_total():
    pass

# PascalCase pour les classes
class UtilisateurService:
    pass

# UPPER_SNAKE_CASE pour les constantes
MAX_TENTATIVES = 3
URL_API = "https://api.exemple.com"

# Noms descriptifs
nb_articles_panier = 5  # Préférable à 'n' ou 'count'
est_authentifie = True  # Préférable à 'flag' ou 'b'

# Préfixes communs
est_valide = True       # Booléen (est_, a_, peut_)
a_permission = False
```

À éviter :

```python
# Éviter les noms de variables qui commencent par un chiffre
# 1variable = 10  # Erreur de syntaxe

# Éviter les noms réservés en Python
# class, def, if, else, import, from, etc.

# Éviter les caractères spéciaux (à l'exception de _)
# variable@ = "valeur"  # Erreur de syntaxe
```

## Assignation multiple et unpacking

Python permet d'assigner plusieurs variables simultanément :

```python
# Assignation multiple
x, y, z = 1, 2, 3
print(x, y, z)  # 1 2 3

# Échange de valeurs
a = 5
b = 10
a, b = b, a
print(a, b)  # 10 5

# Unpacking de liste ou tuple
coordonnees = (10, 20, 30)
x, y, z = coordonnees
print(x, y, z)  # 10 20 30

# Unpacking partiel avec *
premieres, *reste, derniere = [1, 2, 3, 4, 5]
print(premieres)  # 1
print(reste)      # [2, 3, 4]
print(derniere)   # 5

# Unpacking de dictionnaire
personne = {"nom": "Dupont", "age": 25}
nom, age = personne.items()
print(nom)  # ('nom', 'Dupont')
print(age)  # ('age', 25)

nom, age = personne
print(nom, age)  # 'nom' 'age' (les clés)

# Ignorer certaines valeurs avec _
x, _, z = (1, 2, 3)
print(x, z)  # 1 3
```

## Variables et mutabilité

En Python, certains types sont mutables (peuvent être modifiés après création) et d'autres sont immutables (ne peuvent pas être modifiés après création) :

**Types immutables :**
- int, float, complex
- str
- tuple
- frozenset
- bool
- None

**Types mutables :**
- list
- dict
- set
- bytearray

```python
# Exemple de type immutable (str)
nom = "Python"
print(id(nom))  # Adresse mémoire initiale
nom += " 3"  # Crée un nouvel objet string
print(id(nom))  # Nouvelle adresse mémoire

# Exemple de type mutable (list)
liste = [1, 2, 3]
print(id(liste))  # Adresse mémoire initiale
liste.append(4)   # Modifie l'objet liste existant
print(id(liste))  # Même adresse mémoire
```

Attention aux références lors de l'assignation d'objets mutables :

```python
# Création d'une liste
original = [1, 2, 3]

# Assignation par référence (pas une copie)
reference = original

# Modification via la référence
reference.append(4)

print(original)   # [1, 2, 3, 4] - L'original est aussi modifié

# Pour créer une vraie copie
copie_superficielle = original.copy()  # ou list(original)
copie_profonde = copy.deepcopy(original)  # Nécessite import copy
```

## Bonnes pratiques

Voici quelques bonnes pratiques à suivre pour l'utilisation des variables en Python :

1. **Utilisez des noms descriptifs** qui expliquent clairement le but de la variable :

   ```python
   # Mauvais
   x = 300
   
   # Bon
   delai_attente_ms = 300
   ```

2. **Suivez les conventions de nommage** de la PEP 8 :

   ```python
   # Variables et fonctions en snake_case
   nombre_utilisateurs = 42
   
   # Classes en PascalCase
   class UtilisateurService:
       pass
   
   # Constantes en UPPER_SNAKE_CASE
   MAX_CONNEXIONS = 100
   ```

3. **Évitez les variables globales** autant que possible :

   ```python
   # Évitez ceci
   total = 0
   
   def ajouter(montant):
       global total
       total += montant
   
   # Préférez ceci
   def ajouter(total, montant):
       return total + montant
   ```

4. **Utilisez des annotations de type** pour améliorer la lisibilité (Python 3.5+) :

   ```python
   age: int = 30
   nom: str = "Alice"
   
   def calculer_age(annee_naissance: int) -> int:
       return 2023 - annee_naissance
   ```

5. **Initialisez vos variables** lors de la déclaration quand c'est possible :

   ```python
   # Moins bien
   utilisateur = None
   utilisateur = obtenir_utilisateur()
   
   # Mieux
   utilisateur = obtenir_utilisateur()
   ```

6. **Utilisez `None` pour représenter l'absence de valeur** :

   ```python
   def chercher_utilisateur(id):
       # Si l'utilisateur n'est pas trouvé
       return None
   
   utilisateur = chercher_utilisateur(42)
   if utilisateur is None:
       print("Utilisateur non trouvé")
   ```

7. **Utilisez un unpacking explicite** pour les séquences de longueur connue :

   ```python
   # Moins clair
   user_data = get_user_data()
   name = user_data[0]
   email = user_data[1]
   
   # Plus clair
   name, email = get_user_data()
   ```

## Exemples avancés

### Variables locales, nonlocales et globales

```python
x = "global"

def fonction_externe():
    x = "nonlocal"
    
    def fonction_interne():
        x = "local"
        print("x local:", x)
    
    fonction_interne()
    print("x nonlocal:", x)

fonction_externe()
print("x global:", x)

# Résultat:
# x local: local
# x nonlocal: nonlocal
# x global: global
```

Avec modifications des variables :

```python
x = "global"

def fonction_externe():
    x = "nonlocal"
    
    def fonction_interne():
        nonlocal x
        x = "modifié par fonction_interne"
    
    fonction_interne()
    print("x nonlocal après modification:", x)

fonction_externe()
print("x global:", x)

# Résultat:
# x nonlocal après modification: modifié par fonction_interne
# x global: global
```

### Closures et variables

```python
def creer_compteur():
    compteur = 0
    
    def incrementer():
        nonlocal compteur
        compteur += 1
        return compteur
    
    return incrementer

# Création de deux compteurs indépendants
compteur1 = creer_compteur()
compteur2 = creer_compteur()

print(compteur1())  # 1
print(compteur1())  # 2
print(compteur2())  # 1 (indépendant de compteur1)
```

### Affectation conditionnelle

```python
# Opérateur ternaire
age = 20
statut = "majeur" if age >= 18 else "mineur"
print(statut)  # "majeur"

# Assignation avec or pour valeur par défaut
nom = donnees.get("nom") or "Anonyme"

# Assignation avec assignation d'expression (Python 3.8+)
if (resultat := fonction_couteuse()) is not None:
    traiter(resultat)
```

### Variables de classe et d'instance

```python
class Compteur:
    # Variable de classe (partagée par toutes les instances)
    compteur_global = 0
    
    def __init__(self):
        # Variable d'instance (spécifique à chaque instance)
        self.compteur_instance = 0
    
    def incrementer(self):
        Compteur.compteur_global += 1
        self.compteur_instance += 1

# Création de deux instances
c1 = Compteur()
c2 = Compteur()

c1.incrementer()
c1.incrementer()
c2.incrementer()

print(c1.compteur_instance)  # 2 (spécifique à c1)
print(c2.compteur_instance)  # 1 (spécifique à c2)
print(Compteur.compteur_global)  # 3 (partagé entre toutes les instances)
```

## Ressources supplémentaires

- [Documentation officielle Python - Variables et types de données](https://docs.python.org/fr/3/tutorial/introduction.html#using-python-as-a-calculator)
- [PEP 8 - Guide de style pour le code Python](https://peps.python.org/pep-0008/)
- [PEP 484 - Type Hints](https://peps.python.org/pep-0484/)
- [Real Python - Variables en Python](https://realpython.com/python-variables/)
- [Python.org - Tutoriel Python](https://docs.python.org/fr/3/tutorial/)

---

Ce chapitre vous a présenté les concepts fondamentaux des variables en Python, des bases aux techniques avancées. Dans le prochain chapitre, nous explorerons les structures conditionnelles qui vous permettront de contrôler le flux d'exécution de vos programmes.