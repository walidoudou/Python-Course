# Les Conditions en Python

## Table des matières

- [Introduction](#introduction)
- [Opérateurs de comparaison](#opérateurs-de-comparaison)
- [Instructions conditionnelles](#instructions-conditionnelles)
  - [if](#if)
  - [if...else](#ifelse)
  - [if...elif...else](#ifelifelse)
  - [Conditions imbriquées](#conditions-imbriquées)
- [Opérateurs logiques](#opérateurs-logiques)
  - [and (ET logique)](#and-et-logique)
  - [or (OU logique)](#or-ou-logique)
  - [not (NON logique)](#not-non-logique)
- [Expression conditionnelle](#expression-conditionnelle)
- [Valeurs truthy et falsy](#valeurs-truthy-et-falsy)
- [Opérateurs d'identité et d'appartenance](#opérateurs-didentité-et-dappartenance)
  - [is et is not](#is-et-is-not)
  - [in et not in](#in-et-not-in)
- [Techniques avancées](#techniques-avancées)
  - [Assignation conditionnelle avec walrus](#assignation-conditionnelle-avec-walrus)
  - [Chaînage de comparaisons](#chaînage-de-comparaisons)
  - [Structures conditionnelles alternatives](#structures-conditionnelles-alternatives)
- [Bonnes pratiques](#bonnes-pratiques)
- [Erreurs courantes](#erreurs-courantes)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

Les conditions sont fondamentales en programmation pour contrôler le flux d'exécution d'un programme. Elles permettent d'exécuter différentes parties de code en fonction de situations spécifiques, rendant ainsi les programmes plus flexibles et intelligents.

En Python, les conditions s'expriment de manière claire et lisible, reflétant la philosophie du langage qui privilégie la lisibilité et l'expressivité. Ce chapitre couvre tous les aspects des conditions en Python, de leurs formes les plus simples aux techniques les plus avancées.

## Opérateurs de comparaison

Python offre plusieurs opérateurs de comparaison qui renvoient une valeur booléenne (`True` ou `False`) :

```python
# Égalité
x = 5
y = 5
print(x == y)  # True

# Inégalité
x = 5
y = 10
print(x != y)  # True

# Supérieur à
print(x > 3)   # True

# Inférieur à
print(x < 10)  # True

# Supérieur ou égal à
print(x >= 5)  # True

# Inférieur ou égal à
print(x <= 5)  # True
```

Voici un résumé des opérateurs de comparaison en Python :

| Opérateur | Description                  | Exemple    | Résultat |
|-----------|------------------------------|------------|----------|
| `==`      | Égal à                       | `5 == 5`   | `True`   |
| `!=`      | Différent de                 | `5 != 3`   | `True`   |
| `>`       | Supérieur à                  | `5 > 3`    | `True`   |
| `<`       | Inférieur à                  | `5 < 10`   | `True`   |
| `>=`      | Supérieur ou égal à          | `5 >= 5`   | `True`   |
| `<=`      | Inférieur ou égal à          | `5 <= 5`   | `True`   |

Ces opérateurs fonctionnent sur différents types de données :

```python
# Comparaison de nombres
print(5 == 5.0)  # True (int comparé à float)

# Comparaison de chaînes (ordre lexicographique)
print("apple" < "banana")  # True
print("a" < "A")           # False (majuscules avant minuscules en ASCII)

# Comparaison de listes (élément par élément)
print([1, 2] < [1, 3])     # True
print([1, 2] == [1, 2])    # True
```

## Instructions conditionnelles

### if

L'instruction `if` est la base des conditions en Python. Elle exécute un bloc de code uniquement si une condition est vraie.

```python
age = 18

if age >= 18:
    print("Vous êtes majeur.")
    print("Vous pouvez voter.")
    
# Résultat:
# Vous êtes majeur.
# Vous pouvez voter.
```

Notez que Python utilise l'indentation (généralement 4 espaces) pour définir les blocs de code, contrairement à d'autres langages qui utilisent des accolades.

### if...else

La structure `if...else` permet d'exécuter un bloc de code si la condition est vraie, et un autre bloc si elle est fausse :

```python
age = 16

if age >= 18:
    print("Vous êtes majeur.")
else:
    print("Vous êtes mineur.")
    
# Résultat:
# Vous êtes mineur.
```

### if...elif...else

La structure `if...elif...else` permet de tester plusieurs conditions successives :

```python
note = 85

if note >= 90:
    print("Excellent (A)")
elif note >= 80:
    print("Très bien (B)")
elif note >= 70:
    print("Bien (C)")
elif note >= 60:
    print("Passable (D)")
else:
    print("Échec (F)")
    
# Résultat:
# Très bien (B)
```

L'interpréteur Python vérifie chaque condition dans l'ordre. Dès qu'une condition est vraie, le bloc correspondant est exécuté et le reste des conditions est ignoré.

### Conditions imbriquées

Il est possible d'imbriquer des conditions les unes dans les autres :

```python
age = 25
revenu = 30000

if age >= 18:
    print("Vous êtes majeur.")
    
    if revenu >= 25000:
        print("Vous êtes éligible pour un prêt.")
    else:
        print("Votre revenu est insuffisant pour un prêt.")
else:
    print("Vous êtes mineur, pas de prêt possible.")
    
# Résultat:
# Vous êtes majeur.
# Vous êtes éligible pour un prêt.
```

Bien que cette approche fonctionne, il est souvent préférable d'utiliser des opérateurs logiques pour combiner des conditions et éviter une indentation excessive.

## Opérateurs logiques

Les opérateurs logiques permettent de combiner plusieurs conditions.

### and (ET logique)

L'opérateur `and` renvoie `True` si toutes les conditions sont vraies :

```python
age = 25
a_permis = True

if age >= 18 and a_permis:
    print("Vous pouvez conduire.")
else:
    print("Vous ne pouvez pas conduire.")
    
# Résultat:
# Vous pouvez conduire.
```

### or (OU logique)

L'opérateur `or` renvoie `True` si au moins une des conditions est vraie :

```python
est_etudiant = False
est_retraite = True

if est_etudiant or est_retraite:
    print("Vous bénéficiez d'une réduction.")
else:
    print("Tarif normal.")
    
# Résultat:
# Vous bénéficiez d'une réduction.
```

### not (NON logique)

L'opérateur `not` inverse la valeur booléenne d'une expression :

```python
est_connecte = False

if not est_connecte:
    print("Veuillez vous connecter.")
else:
    print("Vous êtes déjà connecté.")
    
# Résultat:
# Veuillez vous connecter.
```

Ces opérateurs peuvent être combinés pour créer des conditions complexes :

```python
age = 25
a_permis = True
a_assurance = False

if age >= 18 and a_permis and not a_assurance:
    print("Vous pouvez conduire mais vous devez obtenir une assurance.")
    
# Résultat:
# Vous pouvez conduire mais vous devez obtenir une assurance.
```

## Expression conditionnelle

Python offre une syntaxe concise pour les conditions simples : l'opérateur ternaire (ou expression conditionnelle).

```python
age = 20
statut = "majeur" if age >= 18 else "mineur"
print(statut)  # "majeur"

# Équivalent à:
if age >= 18:
    statut = "majeur"
else:
    statut = "mineur"
```

Cette syntaxe est particulièrement utile pour des assignations conditionnelles simples ou des retours de fonction.

```python
# Assignation conditionnelle
abs_value = x if x >= 0 else -x

# Dans un retour de fonction
def obtenir_salutation(heure):
    return "Bonjour" if heure < 18 else "Bonsoir"

# Dans une liste par compréhension
nombres = [1, -2, 3, -4, 5]
valeurs_absolues = [n if n >= 0 else -n for n in nombres]
print(valeurs_absolues)  # [1, 2, 3, 4, 5]
```

## Valeurs truthy et falsy

En Python, les valeurs sont évaluées comme booléennes dans un contexte conditionnel selon ces règles :

**Valeurs considérées comme `False` (falsy) :**
- `False`
- `None`
- Zéro de tout type numérique : `0`, `0.0`, `0j`
- Séquences et collections vides : `''`, `()`, `[]`, `{}`, `set()`, `range(0)`

**Toutes les autres valeurs sont considérées comme `True` (truthy).**

```python
# Exemples de valeurs falsy
if not False:
    print("False est falsy")

if not None:
    print("None est falsy")

if not 0:
    print("0 est falsy")

if not "":
    print("Chaîne vide est falsy")

if not []:
    print("Liste vide est falsy")

# Exemples de valeurs truthy
if True:
    print("True est truthy")

if 1:
    print("1 est truthy")

if "texte":
    print("Chaîne non vide est truthy")

if [0]:
    print("Liste non vide est truthy")

if {0}:
    print("Ensemble non vide est truthy")
```

Cette propriété permet des expressions concises pour vérifier si une variable contient une valeur significative :

```python
nom = ""
# Affiche "Nom inconnu" si nom est vide
print(nom or "Nom inconnu")  # "Nom inconnu"

liste = []
# Exécute seulement si la liste n'est pas vide
if liste:
    print("La liste contient des éléments")
else:
    print("La liste est vide")
```

## Opérateurs d'identité et d'appartenance

### is et is not

Les opérateurs `is` et `is not` vérifient si deux variables font référence au même objet en mémoire :

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a == b)  # True (même contenu)
print(a is b)  # False (objets différents en mémoire)
print(a is c)  # True (même objet en mémoire)

# Utilisation courante avec None
x = None
if x is None:
    print("x est None")

if x is not None:
    print("x n'est pas None")
```

L'opérateur `is` est particulièrement utile pour vérifier si une variable est `None`, `True` ou `False`, car ces objets sont uniques en Python.

### in et not in

Les opérateurs `in` et `not in` vérifient si une valeur est présente dans une séquence (chaîne, liste, tuple) ou une collection (dictionnaire, ensemble) :

```python
# Vérifier l'appartenance dans une liste
fruits = ["pomme", "banane", "orange"]
print("banane" in fruits)  # True
print("kiwi" in fruits)    # False
print("kiwi" not in fruits)  # True

# Vérifier l'appartenance dans une chaîne
message = "Bonjour"
print("jour" in message)   # True

# Vérifier l'appartenance dans un dictionnaire (vérifie les clés)
personne = {"nom": "Dupont", "age": 30}
print("nom" in personne)    # True
print("adresse" in personne)  # False

# Vérifier l'appartenance dans un ensemble
couleurs = {"rouge", "vert", "bleu"}
print("rouge" in couleurs)  # True
```

Ces opérateurs sont très utiles dans les conditions :

```python
utilisateur = "admin"
droits_admin = ["admin", "superuser", "root"]

if utilisateur in droits_admin:
    print("Accès administrateur accordé")
else:
    print("Accès refusé")
```

## Techniques avancées

### Assignation conditionnelle avec walrus

L'opérateur d'assignation `:=` (introduit dans Python 3.8, surnommé "opérateur morse" ou "walrus operator") permet d'assigner une valeur à une variable dans une expression :

```python
# Sans l'opérateur walrus
data = get_data()
if data:
    process(data)

# Avec l'opérateur walrus
if (data := get_data()):
    process(data)
```

Cet opérateur est particulièrement utile dans les boucles et les compréhensions :

```python
# Exemple dans une boucle while
while (line := input("Entrez un texte (ou 'q' pour quitter): ")) != 'q':
    print(f"Vous avez entré: {line}")

# Exemple dans une compréhension de liste
filtered_data = [result for x in data if (result := process(x)) is not None]
```

### Chaînage de comparaisons

Python permet de chaîner des comparaisons de manière intuitive :

```python
# Vérifier si une valeur est dans une plage
age = 25
if 18 <= age <= 65:
    print("Âge de travail actif")

# Équivalent à:
if age >= 18 and age <= 65:
    print("Âge de travail actif")

# Autres exemples
x = 10
print(1 < x < 20)  # True
print(10 < x < 20)  # False
print(x < 10 < x*2)  # False
print(x <= 10 <= x*2)  # True
```

Cette fonctionnalité permet d'écrire des conditions plus lisibles et plus proches du langage naturel.

### Structures conditionnelles alternatives

Il existe d'autres façons d'exprimer des conditions en Python :

**Dictionnaires comme alternatives aux structures if/elif :**

```python
def obtenir_jour(numero):
    jours = {
        1: "Lundi",
        2: "Mardi",
        3: "Mercredi",
        4: "Jeudi",
        5: "Vendredi",
        6: "Samedi",
        7: "Dimanche"
    }
    return jours.get(numero, "Jour invalide")

print(obtenir_jour(3))  # "Mercredi"
print(obtenir_jour(8))  # "Jour invalide"
```

**Dispatch table (table de répartition) avec des fonctions :**

```python
def addition(a, b):
    return a + b

def soustraction(a, b):
    return a - b

def multiplication(a, b):
    return a * b

def division(a, b):
    if b == 0:
        return "Division par zéro impossible"
    return a / b

operations = {
    '+': addition,
    '-': soustraction,
    '*': multiplication,
    '/': division
}

def calculer(a, operateur, b):
    # Obtient la fonction correspondante ou renvoie une fonction par défaut
    operation = operations.get(operateur, lambda x, y: "Opération non supportée")
    return operation(a, b)

print(calculer(10, '+', 5))  # 15
print(calculer(10, '/', 2))  # 5.0
print(calculer(10, '%', 3))  # "Opération non supportée"
```

## Bonnes pratiques

1. **Privilégiez la lisibilité** : Utilisez des conditions claires et explicites.

   ```python
   # Moins lisible
   if a and b or c and not d:
       ...
   
   # Plus lisible
   if (a and b) or (c and not d):
       ...
   ```

2. **Évitez les doubles négations** qui rendent le code difficile à comprendre :

   ```python
   # Moins lisible
   if not is_not_valid:
       ...
   
   # Plus lisible
   if is_valid:
       ...
   ```

3. **Utilisez des noms de variables explicites** pour les conditions :

   ```python
   # Moins explicite
   if u:
       ...
   
   # Plus explicite
   if utilisateur_authentifie:
       ...
   ```

4. **Préférez les expressions positives** aux expressions négatives :

   ```python
   # Moins intuitif
   if not age < 18:
       ...
   
   # Plus intuitif
   if age >= 18:
       ...
   ```

5. **Utilisez `is` plutôt que `==` pour comparer avec `None`, `True` ou `False`** :

   ```python
   # Correct
   if resultat is None:
       ...
   
   # Moins fiable
   if resultat == None:
       ...
   ```

6. **Exploitez les valeurs truthy/falsy** quand c'est approprié :

   ```python
   # Sans exploiter truthy/falsy
   if len(liste) > 0:
       ...
   
   # En exploitant truthy/falsy
   if liste:
       ...
   ```

7. **Utilisez des conditions de garde** (early returns) pour simplifier le code :

   ```python
   # Avec conditions imbriquées (moins lisible)
   def traiter_donnees(donnees):
       if donnees:
           if valider(donnees):
               resultat = transformer(donnees)
               if resultat:
                   return resultat
               else:
                   return None
           else:
               return None
       else:
           return None
   
   # Avec conditions de garde (plus lisible)
   def traiter_donnees(donnees):
       if not donnees:
           return None
       
       if not valider(donnees):
           return None
       
       resultat = transformer(donnees)
       if not resultat:
           return None
       
       return resultat
   ```

## Erreurs courantes

1. **Confusion entre `=` (assignation) et `==` (comparaison)** :

   ```python
   # Erreur (assigne 5 à x, puis évalue x qui est truthy)
   if x = 5:
       print("x vaut 5")
   
   # Correct (compare x à 5)
   if x == 5:
       print("x vaut 5")
   ```

2. **Oubli des deux-points `:` après une condition** :

   ```python
   # Erreur
   if x > 5
       print("x est supérieur à 5")
   
   # Correct
   if x > 5:
       print("x est supérieur à 5")
   ```

3. **Indentation incorrecte** :

   ```python
   # Erreur (indentation incohérente)
   if x > 5:
       print("x est supérieur à 5")
         print("x est positif")  # IndentationError
   
   # Correct
   if x > 5:
       print("x est supérieur à 5")
       print("x est positif")
   ```

4. **Confusion entre `is` et `==`** :

   ```python
   a = [1, 2, 3]
   b = [1, 2, 3]
   
   # Incorrect dans ce contexte
   if a is b:  # Toujours False car ce sont des objets différents
       print("Listes identiques")
   
   # Correct
   if a == b:  # True car les contenus sont égaux
       print("Contenus égaux")
   ```

5. **Évaluation non sécurisée de valeurs potentiellement None** :

   ```python
   # Risque d'AttributeError si resultat est None
   if resultat.valeur > 10:
       ...
   
   # Approche sécurisée
   if resultat is not None and resultat.valeur > 10:
       ...
   ```

## Ressources supplémentaires

- [Documentation officielle Python - Instructions conditionnelles](https://docs.python.org/fr/3/tutorial/controlflow.html#if-statements)
- [PEP 8 - Guide de style pour le code Python](https://peps.python.org/pep-0008/)
- [PEP 572 - Opérateur d'assignation (:=)](https://peps.python.org/pep-0572/)
- [Real Python - Conditions et expressions conditionnelles](https://realpython.com/python-conditional-statements/)
- [Python.org - Tutoriel Python](https://docs.python.org/fr/3/tutorial/)

---

Ce chapitre vous a présenté les concepts fondamentaux et avancés des conditions en Python. Avec ces outils, vous pouvez maintenant créer des programmes capables de prendre des décisions complexes basées sur différentes situations. Dans le prochain chapitre, nous explorerons les boucles qui vous permettront de répéter des actions et de parcourir des collections de données.