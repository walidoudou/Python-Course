# Les Classes en Python

## Table des matières

- [Introduction](#introduction)
- [Les fondamentaux des classes](#les-fondamentaux-des-classes)
  - [Définition d'une classe](#définition-dune-classe)
  - [Création d'objets (instanciation)](#création-dobjets-instanciation)
  - [Attributs d'instance](#attributs-dinstance)
  - [Méthodes d'instance](#méthodes-dinstance)
  - [La méthode `__init__` (constructeur)](#la-méthode-__init__-constructeur)
  - [Le paramètre `self`](#le-paramètre-self)
- [Encapsulation](#encapsulation)
  - [Attributs publics, protégés et privés](#attributs-publics-protégés-et-privés)
  - [Getters et setters](#getters-et-setters)
  - [Propriétés](#propriétés)
- [Héritage](#héritage)
  - [Héritage simple](#héritage-simple)
  - [Appel du constructeur parent](#appel-du-constructeur-parent)
  - [Héritage multiple](#héritage-multiple)
  - [Ordre de résolution des méthodes (MRO)](#ordre-de-résolution-des-méthodes-mro)
  - [Fonctions `super()`, `isinstance()` et `issubclass()`](#fonctions-super-isinstance-et-issubclass)
- [Polymorphisme](#polymorphisme)
  - [Surcharge de méthodes](#surcharge-de-méthodes)
  - [Méthodes abstraites et classes abstraites](#méthodes-abstraites-et-classes-abstraites)
  - [Duck Typing](#duck-typing)
- [Méthodes et attributs spéciaux](#méthodes-et-attributs-spéciaux)
  - [Méthodes de représentation (`__str__`, `__repr__`)](#méthodes-de-représentation-__str__-__repr__)
  - [Méthodes opérateurs (`__add__`, `__eq__`, etc.)](#méthodes-opérateurs-__add__-__eq__-etc)
  - [Méthodes de conteneur (`__len__`, `__getitem__`, etc.)](#méthodes-de-conteneur-__len__-__getitem__-etc)
  - [Méthodes de contexte (`__enter__`, `__exit__`)](#méthodes-de-contexte-__enter__-__exit__)
- [Attributs et méthodes de classe](#attributs-et-méthodes-de-classe)
  - [Attributs de classe](#attributs-de-classe)
  - [Méthodes de classe (`@classmethod`)](#méthodes-de-classe-classmethod)
  - [Méthodes statiques (`@staticmethod`)](#méthodes-statiques-staticmethod)
- [Composition et agrégation](#composition-et-agrégation)
  - [Composition vs. héritage](#composition-vs-héritage)
  - [Conception par agrégation](#conception-par-agrégation)
- [Métaclasses](#métaclasses)
  - [La métaclasse `type`](#la-métaclasse-type)
  - [Création de métaclasses personnalisées](#création-de-métaclasses-personnalisées)
  - [Cas d'utilisation des métaclasses](#cas-dutilisation-des-métaclasses)
- [Bonnes pratiques](#bonnes-pratiques)
- [Exercices pratiques](#exercices-pratiques)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

La programmation orientée objet (POO) est un paradigme de programmation fondamental qui permet d'organiser le code en structures réutilisables appelées classes. Ces classes servent de modèles pour créer des objets ayant des attributs (données) et des méthodes (fonctions) spécifiques. Python est un langage qui prend en charge la POO tout en restant très accessible.

La POO offre plusieurs avantages :

- **Modularité** : Diviser le code en unités indépendantes facilite la maintenance.
- **Réutilisabilité** : Les classes créées peuvent être réutilisées dans différents contextes.
- **Évolutivité** : Le code peut être étendu en ajoutant de nouvelles classes dérivées.
- **Abstraction** : La POO permet de masquer les détails complexes et de se concentrer sur l'essentiel.

Dans ce chapitre, nous explorerons comment Python implémente la POO et comment utiliser efficacement ses fonctionnalités, des bases jusqu'aux concepts avancés.

## Les fondamentaux des classes

### Définition d'une classe

En Python, une classe est définie à l'aide du mot-clé `class`, suivi du nom de la classe et de deux-points. Par convention, les noms de classes utilisent la notation PascalCase (chaque mot commence par une majuscule, sans underscore).

```python
class Personne:
    """Classe représentant une personne."""
    pass  # Une classe vide pour l'instant
```

### Création d'objets (instanciation)

Une fois une classe définie, vous pouvez créer (ou instancier) des objets à partir de cette classe :

```python
# Création d'objets (instances) de la classe Personne
personne1 = Personne()
personne2 = Personne()

print(type(personne1))  # <class '__main__.Personne'>
```

### Attributs d'instance

Les attributs d'instance sont des variables qui stockent des données propres à chaque objet. Ils peuvent être ajoutés à un objet lors de sa création ou ultérieurement.

```python
class Personne:
    """Classe représentant une personne."""

    def __init__(self, nom, age):
        # Initialisation des attributs d'instance
        self.nom = nom
        self.age = age

# Création d'objets avec des attributs
personne1 = Personne("Alice", 30)
personne2 = Personne("Bob", 25)

# Accès aux attributs
print(personne1.nom)  # Alice
print(personne2.age)  # 25

# Modification d'attributs
personne1.age = 31
print(personne1.age)  # 31

# Ajout d'un nouvel attribut à un objet existant
personne1.email = "alice@example.com"
print(personne1.email)  # alice@example.com
# print(personne2.email)  # AttributeError: 'Personne' object has no attribute 'email'
```

### Méthodes d'instance

Les méthodes d'instance sont des fonctions définies à l'intérieur d'une classe qui peuvent manipuler les attributs de l'objet et effectuer des opérations.

```python
class Personne:
    """Classe représentant une personne."""

    def __init__(self, nom, age):
        self.nom = nom
        self.age = age

    def se_presenter(self):
        """Méthode qui présente la personne."""
        return f"Bonjour, je m'appelle {self.nom} et j'ai {self.age} ans."

    def avoir_anniversaire(self):
        """Méthode qui incrémente l'âge de la personne."""
        self.age += 1
        return f"{self.nom} a maintenant {self.age} ans."

# Utilisation des méthodes
personne = Personne("Charlie", 35)
print(personne.se_presenter())  # Bonjour, je m'appelle Charlie et j'ai 35 ans.
print(personne.avoir_anniversaire())  # Charlie a maintenant 36 ans.
```

### La méthode `__init__` (constructeur)

La méthode `__init__` est un constructeur qui est automatiquement appelé lorsqu'un nouvel objet est créé. Elle permet d'initialiser les attributs de l'objet.

```python
class Rectangle:
    """Classe représentant un rectangle."""

    def __init__(self, longueur=1, largeur=1):
        """
        Initialise un rectangle avec une longueur et une largeur.

        Args:
            longueur (float, optional): La longueur du rectangle. Par défaut 1.
            largeur (float, optional): La largeur du rectangle. Par défaut 1.
        """
        self.longueur = longueur
        self.largeur = largeur

    def calculer_aire(self):
        """Calcule l'aire du rectangle."""
        return self.longueur * self.largeur

    def calculer_perimetre(self):
        """Calcule le périmètre du rectangle."""
        return 2 * (self.longueur + self.largeur)

# Création d'objets avec différents paramètres
rect1 = Rectangle()  # Utilise les valeurs par défaut
rect2 = Rectangle(5, 3)  # Valeurs personnalisées

print(rect1.calculer_aire())  # 1 (1 × 1)
print(rect2.calculer_aire())  # 15 (5 × 3)
```

### Le paramètre `self`

Le premier paramètre de chaque méthode d'instance, conventionnellement nommé `self`, fait référence à l'instance actuelle de la classe. Il permet d'accéder aux attributs et aux autres méthodes de l'objet.

```python
class Compteur:
    """Classe implémentant un compteur simple."""

    def __init__(self, valeur_initiale=0):
        self.valeur = valeur_initiale

    def incrementer(self):
        """Incrémente le compteur de 1."""
        self.valeur += 1
        return self.valeur

    def decrementer(self):
        """Décrémente le compteur de 1."""
        self.valeur -= 1
        return self.valeur

    def reinitialiser(self):
        """Réinitialise le compteur à 0."""
        self.valeur = 0
        return self.valeur

    def obtenir_valeur(self):
        """Retourne la valeur actuelle du compteur."""
        return self.valeur

# Utilisation du compteur
compteur = Compteur(10)
print(compteur.obtenir_valeur())  # 10
print(compteur.incrementer())     # 11
print(compteur.incrementer())     # 12
print(compteur.decrementer())     # 11
print(compteur.reinitialiser())   # 0
```

## Encapsulation

L'encapsulation est un principe fondamental de la POO qui consiste à regrouper les données (attributs) et les méthodes qui les manipulent au sein d'une classe, tout en contrôlant l'accès à ces données.

### Attributs publics, protégés et privés

Python utilise des conventions pour indiquer le niveau d'accès aux attributs et méthodes :

1. **Attributs publics** : Accessibles de l'extérieur de la classe (sans préfixe)
2. **Attributs protégés** : Indiqués par un underscore, ils ne devraient être accédés que par la classe et ses sous-classes (convention)
3. **Attributs privés** : Indiqués par un double underscore, ils sont soumis au "name mangling" pour limiter l'accès direct

```python
class CompteBancaire:
    """Classe représentant un compte bancaire."""

    def __init__(self, proprietaire, solde_initial=0):
        self.proprietaire = proprietaire      # Attribut public
        self._solde = solde_initial           # Attribut protégé (convention)
        self.__numero_compte = "123456789"    # Attribut privé

    def deposer(self, montant):
        """Dépose un montant sur le compte."""
        if montant > 0:
            self._solde += montant
            return f"Dépôt de {montant}€ effectué. Nouveau solde: {self._solde}€"
        return "Le montant du dépôt doit être positif."

    def retirer(self, montant):
        """Retire un montant du compte si le solde est suffisant."""
        if montant > 0:
            if self._solde >= montant:
                self._solde -= montant
                return f"Retrait de {montant}€ effectué. Nouveau solde: {self._solde}€"
            return "Solde insuffisant."
        return "Le montant du retrait doit être positif."

    def afficher_solde(self):
        """Affiche le solde actuel du compte."""
        return f"Solde actuel du compte de {self.proprietaire}: {self._solde}€"

    def _operation_interne(self):
        """Méthode protégée pour les opérations internes."""
        return "Opération interne en cours..."

    def __operation_securisee(self):
        """Méthode privée pour les opérations sécurisées."""
        return f"Opération sécurisée sur le compte {self.__numero_compte}"

    def effectuer_operation_securisee(self):
        """Interface publique pour accéder à une méthode privée."""
        return self.__operation_securisee()

# Utilisation du compte bancaire
compte = CompteBancaire("Alice", 1000)

# Accès aux attributs et méthodes publics
print(compte.proprietaire)        # Alice
print(compte.afficher_solde())    # Solde actuel du compte de Alice: 1000€
print(compte.deposer(500))        # Dépôt de 500€ effectué. Nouveau solde: 1500€

# Accès à un attribut protégé (possible mais déconseillé)
print(compte._solde)              # 1500

# Accès à une méthode protégée (possible mais déconseillé)
print(compte._operation_interne())  # Opération interne en cours...

# Tentative d'accès à un attribut privé
# print(compte.__numero_compte)   # AttributeError: 'CompteBancaire' object has no attribute '__numero_compte'

# Mais l'accès est possible via le nom transformé (name mangling)
print(compte._CompteBancaire__numero_compte)   # 123456789

# Interface publique pour accéder à une fonctionnalité privée
print(compte.effectuer_operation_securisee())  # Opération sécurisée sur le compte 123456789
```

Le "name mangling" transforme `__attribut` en `_NomClasse__attribut`, ce qui permet techniquement d'accéder aux attributs privés, mais signale clairement que vous ne devriez pas le faire.

### Getters et setters

Les getters et setters sont des méthodes qui permettent de contrôler l'accès et la modification des attributs d'une classe :

```python
class Employe:
    """Classe représentant un employé d'une entreprise."""

    def __init__(self, nom, salaire):
        self.__nom = nom
        self.__salaire = salaire

    # Getter pour nom
    def get_nom(self):
        return self.__nom

    # Setter pour nom
    def set_nom(self, nouveau_nom):
        if isinstance(nouveau_nom, str) and nouveau_nom:
            self.__nom = nouveau_nom
        else:
            raise ValueError("Le nom doit être une chaîne non vide")

    # Getter pour salaire
    def get_salaire(self):
        return self.__salaire

    # Setter pour salaire
    def set_salaire(self, nouveau_salaire):
        if isinstance(nouveau_salaire, (int, float)) and nouveau_salaire > 0:
            self.__salaire = nouveau_salaire
        else:
            raise ValueError("Le salaire doit être un nombre positif")

    # Méthode pour augmenter le salaire
    def augmenter_salaire(self, pourcentage):
        if pourcentage > 0:
            self.__salaire += self.__salaire * (pourcentage / 100)
            return f"Nouveau salaire après augmentation: {self.__salaire}€"
        return "Le pourcentage d'augmentation doit être positif"

# Utilisation des getters et setters
employe = Employe("Alice", 50000)
print(employe.get_nom())           # Alice
print(employe.get_salaire())       # 50000

employe.set_nom("Alicia")
print(employe.get_nom())           # Alicia

employe.set_salaire(55000)
print(employe.get_salaire())       # 55000

print(employe.augmenter_salaire(10))  # Nouveau salaire après augmentation: 60500.0€

# Validation intégrée aux setters
try:
    employe.set_salaire(-1000)
except ValueError as e:
    print(f"Erreur: {e}")          # Erreur: Le salaire doit être un nombre positif
```

### Propriétés

Python propose un mécanisme de propriétés qui permet un accès plus élégant aux attributs tout en conservant la capacité de contrôle :

```python
class Temperature:
    """Classe pour représenter et convertir des températures."""

    def __init__(self, celsius=0):
        self._celsius = celsius

    @property
    def celsius(self):
        """Getter pour la température en Celsius."""
        return self._celsius

    @celsius.setter
    def celsius(self, valeur):
        """Setter pour la température en Celsius."""
        if valeur < -273.15:
            raise ValueError("La température ne peut pas être inférieure au zéro absolu (-273.15°C)")
        self._celsius = valeur

    @property
    def fahrenheit(self):
        """Getter pour la température en Fahrenheit."""
        return (self._celsius * 9/5) + 32

    @fahrenheit.setter
    def fahrenheit(self, valeur):
        """Setter pour la température en Fahrenheit."""
        celsius = (valeur - 32) * 5/9
        if celsius < -273.15:
            raise ValueError("La température ne peut pas être inférieure au zéro absolu (-459.67°F)")
        self._celsius = celsius

    @property
    def kelvin(self):
        """Getter pour la température en Kelvin."""
        return self._celsius + 273.15

    @kelvin.setter
    def kelvin(self, valeur):
        """Setter pour la température en Kelvin."""
        if valeur < 0:
            raise ValueError("La température en Kelvin ne peut pas être négative")
        self._celsius = valeur - 273.15

# Utilisation des propriétés
temp = Temperature(25)  # 25°C

# Accès aux propriétés comme si c'étaient des attributs
print(f"Celsius: {temp.celsius}°C")       # Celsius: 25°C
print(f"Fahrenheit: {temp.fahrenheit}°F") # Fahrenheit: 77.0°F
print(f"Kelvin: {temp.kelvin}K")          # Kelvin: 298.15K

# Modification des propriétés
temp.celsius = 30
print(f"Fahrenheit après modification: {temp.fahrenheit}°F")  # Fahrenheit: 86.0°F

temp.fahrenheit = 50
print(f"Celsius après modification: {temp.celsius}°C")        # Celsius: 10.0°C

temp.kelvin = 300
print(f"Celsius après modification: {temp.celsius}°C")        # Celsius: 26.85°C

# Validation dans les setters
try:
    temp.celsius = -300  # Inférieur au zéro absolu
except ValueError as e:
    print(f"Erreur: {e}")  # Erreur: La température ne peut pas être inférieure...
```

## Héritage

L'héritage est un mécanisme fondamental de la POO qui permet à une classe (classe enfant) d'hériter des attributs et méthodes d'une autre classe (classe parent).

### Héritage simple

```python
class Animal:
    """Classe de base représentant un animal."""

    def __init__(self, nom, age):
        self.nom = nom
        self.age = age

    def manger(self):
        return f"{self.nom} est en train de manger."

    def dormir(self):
        return f"{self.nom} est en train de dormir."

    def se_deplacer(self):
        return f"{self.nom} se déplace."

class Chien(Animal):
    """Classe dérivée représentant un chien."""

    def aboyer(self):
        return f"{self.nom} aboie: Wouaf! Wouaf!"

    # Surcharge de méthode
    def se_deplacer(self):
        return f"{self.nom} court sur ses quatre pattes."

class Oiseau(Animal):
    """Classe dérivée représentant un oiseau."""

    def chanter(self):
        return f"{self.nom} chante une mélodie."

    # Surcharge de méthode
    def se_deplacer(self):
        return f"{self.nom} vole dans les airs."

# Utilisation des classes héritées
rex = Chien("Rex", 3)
piaf = Oiseau("Piaf", 1)

# Méthodes héritées
print(rex.manger())    # Rex est en train de manger.
print(piaf.dormir())   # Piaf est en train de dormir.

# Méthodes spécifiques
print(rex.aboyer())    # Rex aboie: Wouaf! Wouaf!
print(piaf.chanter())  # Piaf chante une mélodie.

# Méthodes surchargées
print(rex.se_deplacer())  # Rex court sur ses quatre pattes.
print(piaf.se_deplacer()) # Piaf vole dans les airs.
```

### Appel du constructeur parent

Il est souvent nécessaire d'appeler le constructeur de la classe parent depuis la classe enfant :

```python
class Vehicule:
    """Classe de base représentant un véhicule."""

    def __init__(self, marque, modele, annee):
        self.marque = marque
        self.modele = modele
        self.annee = annee
        self.kilometrage = 0

    def afficher_infos(self):
        return f"{self.marque} {self.modele} ({self.annee}) - {self.kilometrage} km"

    def conduire(self, distance):
        self.kilometrage += distance
        return f"Le véhicule a parcouru {distance} km. Total: {self.kilometrage} km."

class Voiture(Vehicule):
    """Classe dérivée représentant une voiture."""

    def __init__(self, marque, modele, annee, nombre_portes, type_carburant):
        # Appel du constructeur parent
        super().__init__(marque, modele, annee)

        # Attributs spécifiques à la voiture
        self.nombre_portes = nombre_portes
        self.type_carburant = type_carburant

    def afficher_infos(self):
        infos_base = super().afficher_infos()
        return f"{infos_base}, {self.nombre_portes} portes, {self.type_carburant}"

class Moto(Vehicule):
    """Classe dérivée représentant une moto."""

    def __init__(self, marque, modele, annee, type_moto):
        # Appel du constructeur parent
        super().__init__(marque, modele, annee)

        # Attribut spécifique à la moto
        self.type_moto = type_moto

    def faire_un_wheeling(self):
        return f"La {self.marque} {self.modele} fait un wheeling !"

# Utilisation des classes héritées
ma_voiture = Voiture("Toyota", "Corolla", 2020, 5, "Hybride")
ma_moto = Moto("Kawasaki", "Ninja", 2019, "Sportive")

print(ma_voiture.afficher_infos())  # Toyota Corolla (2020) - 0 km, 5 portes, Hybride
print(ma_moto.afficher_infos())     # Kawasaki Ninja (2019) - 0 km

print(ma_voiture.conduire(150))     # Le véhicule a parcouru 150 km. Total: 150 km.
print(ma_moto.faire_un_wheeling())  # La Kawasaki Ninja fait un wheeling !
```

### Héritage multiple

Python prend en charge l'héritage multiple, où une classe peut hériter de plusieurs classes parentes :

```python
class Dispositif:
    """Classe représentant un dispositif électronique."""

    def __init__(self, nom, puissance):
        self.nom = nom
        self.puissance = puissance
        self.est_allume = False

    def allumer(self):
        if not self.est_allume:
            self.est_allume = True
            return f"{self.nom} est maintenant allumé."
        return f"{self.nom} est déjà allumé."

    def eteindre(self):
        if self.est_allume:
            self.est_allume = False
            return f"{self.nom} est maintenant éteint."
        return f"{self.nom} est déjà éteint."

class Portable:
    """Classe représentant un appareil portable."""

    def __init__(self, poids, autonomie):
        self.poids = poids
        self.autonomie = autonomie

    def verifier_batterie(self):
        return f"Autonomie restante: {self.autonomie} heures."

    def transporter(self):
        return f"L'appareil de {self.poids}g est facile à transporter."

class Telephone(Dispositif, Portable):
    """Classe représentant un téléphone (héritage multiple)."""

    def __init__(self, modele, puissance, poids, autonomie, numero):
        # Appel des constructeurs des classes parentes
        Dispositif.__init__(self, f"Téléphone {modele}", puissance)
        Portable.__init__(self, poids, autonomie)

        # Attributs spécifiques au téléphone
        self.numero = numero
        self.est_en_appel = False

    def appeler(self, contact):
        if self.est_allume:
            self.est_en_appel = True
            return f"Appel en cours vers {contact}..."
        return f"Impossible d'appeler: le téléphone est éteint."

    def terminer_appel(self):
        if self.est_en_appel:
            self.est_en_appel = False
            return "Appel terminé."
        return "Aucun appel en cours."

# Utilisation de la classe avec héritage multiple
mon_telephone = Telephone("iPhone 13", 15, 180, 24, "0123456789")

# Méthodes héritées de Dispositif
print(mon_telephone.allumer())  # Téléphone iPhone 13 est maintenant allumé.

# Méthodes héritées de Portable
print(mon_telephone.verifier_batterie())  # Autonomie restante: 24 heures.
print(mon_telephone.transporter())        # L'appareil de 180g est facile à transporter.

# Méthodes spécifiques
print(mon_telephone.appeler("Alice"))     # Appel en cours vers Alice...
print(mon_telephone.terminer_appel())     # Appel terminé.
```

### Ordre de résolution des méthodes (MRO)

Lorsqu'il y a héritage multiple, Python utilise un algorithme appelé "C3 linearization" pour déterminer l'ordre dans lequel les classes parentes sont consultées lors de la recherche d'attributs et de méthodes.

```python
class A:
    def methode(self):
        return "Méthode de A"

class B(A):
    def methode(self):
        return "Méthode de B"

class C(A):
    def methode(self):
        return "Méthode de C"

class D(B, C):
    pass

class E(C, B):
    pass

# Afficher le MRO (Method Resolution Order)
print(D.__mro__)  # (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
print(E.__mro__)  # (<class '__main__.E'>, <class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)

# Quelle méthode est appelée ?
d = D()
e = E()
print(d.methode())  # Méthode de B
print(e.methode())  # Méthode de C
```

### Fonctions `super()`, `isinstance()` et `issubclass()`

Python fournit plusieurs fonctions utiles pour travailler avec l'héritage :

```python
class Forme:
    def __init__(self, nom):
        self.nom = nom

    def calculer_aire(self):
        return 0

    def afficher_infos(self):
        return f"Forme: {self.nom}, Aire: {self.calculer_aire()}"

class Rectangle(Forme):
    def __init__(self, longueur, largeur):
        super().__init__("Rectangle")
        self.longueur = longueur
        self.largeur = largeur

    def calculer_aire(self):
        return self.longueur * self.largeur

class Cercle(Forme):
    def __init__(self, rayon):
        super().__init__("Cercle")
        self.rayon = rayon

    def calculer_aire(self):
        import math
        return math.pi * self.rayon ** 2

# Création d'objets
rectangle = Rectangle(5, 3)
cercle = Cercle(2)

# Utilisation de super() (déjà vue dans les constructeurs)
print(rectangle.afficher_infos())  # Forme: Rectangle, Aire: 15
print(cercle.afficher_infos())     # Forme: Cercle, Aire: 12.566370614359172

# Utilisation de isinstance()
print(isinstance(rectangle, Rectangle))  # True
print(isinstance(rectangle, Forme))      # True
print(isinstance(rectangle, Cercle))     # False

# Utilisation de issubclass()
print(issubclass(Rectangle, Forme))     # True
print(issubclass(Cercle, Forme))        # True
print(issubclass(Rectangle, Cercle))    # False
```

## Polymorphisme

Le polymorphisme permet aux objets de différentes classes d'être traités comme des objets d'une classe commune. En Python, le polymorphisme est principalement réalisé par la surcharge de méthodes et le duck typing.

### Surcharge de méthodes

La surcharge de méthodes permet à une sous-classe de fournir une implémentation spécifique d'une méthode déjà définie dans sa classe parente :

```python
class Instrument:
    def __init__(self, nom):
        self.nom = nom

    def jouer(self):
        return f"Joue de l'instrument: {self.nom}"

class Guitare(Instrument):
    def __init__(self):
        super().__init__("Guitare")

    def jouer(self):
        return "Gratte les cordes de la guitare"

class Piano(Instrument):
    def __init__(self):
        super().__init__("Piano")

    def jouer(self):
        return "Appuie sur les touches du piano"

class Batterie(Instrument):
    def __init__(self):
        super().__init__("Batterie")

    def jouer(self):
        return "Frappe les tambours et les cymbales"

# Utilisation polymorphique
def jouer_morceau(instrument):
    return instrument.jouer()

# Création d'instruments
instruments = [Guitare(), Piano(), Batterie()]

# Même méthode, comportements différents
for instrument in instruments:
    print(jouer_morceau(instrument))

# Affiche:
# Gratte les cordes de la guitare
# Appuie sur les touches du piano
# Frappe les tambours et les cymbales
```

### Méthodes abstraites et classes abstraites

Python propose le module `abc` (Abstract Base Classes) pour définir des classes abstraites et des méthodes abstraites :

```python
from abc import ABC, abstractmethod

class FormeAbstraite(ABC):
    """Classe abstraite représentant une forme géométrique."""

    @abstractmethod
    def calculer_aire(self):
        """Calcule l'aire de la forme."""
        pass

    @abstractmethod
    def calculer_perimetre(self):
        """Calcule le périmètre de la forme."""
        pass

    def afficher_infos(self):
        """Affiche les informations de la forme."""
        return f"Aire: {self.calculer_aire()}, Périmètre: {self.calculer_perimetre()}"

class RectangleAbs(FormeAbstraite):
    """Implémentation concrète d'un rectangle."""

    def __init__(self, longueur, largeur):
        self.longueur = longueur
        self.largeur = largeur

    def calculer_aire(self):
        return self.longueur * self.largeur

    def calculer_perimetre(self):
        return 2 * (self.longueur + self.largeur)

class CercleAbs(FormeAbstraite):
    """Implémentation concrète d'un cercle."""

    def __init__(self, rayon):
        self.rayon = rayon

    def calculer_aire(self):
        import math
        return math.pi * self.rayon ** 2

    def calculer_perimetre(self):
        import math
        return 2 * math.pi * self.rayon

# Essayer d'instancier la classe abstraite
try:
    forme = FormeAbstraite()  # Erreur: Can't instantiate abstract class
except TypeError as e:
    print(f"Erreur: {e}")

# Utilisation des classes concrètes
rectangle = RectangleAbs(5, 3)
cercle = CercleAbs(2)

print(rectangle.afficher_infos())  # Aire: 15, Périmètre: 16
print(cercle.afficher_infos())     # Aire: 12.566370614359172, Périmètre: 12.566370614359172
```

### Duck Typing

Le duck typing est un concept où le type d'un objet est déterminé par ce qu'il peut faire (ses méthodes) plutôt que par son héritage. "Si ça marche comme un canard et que ça fait coin-coin comme un canard, alors c'est probablement un canard."

```python
class Canard:
    def faire_du_bruit(self):
        return "Coin coin!"

    def nager(self):
        return "Le canard nage paisiblement."

class Personne:
    def faire_du_bruit(self):
        return "Bonjour!"

    def nager(self):
        return "La personne nage le crawl."

class Robot:
    def faire_du_bruit(self):
        return "Bip bip!"

    def nager(self):
        return "Le robot est étanche et se déplace sous l'eau."

# Fonction qui utilise le duck typing
def faire_nager_et_bruiter(entite):
    print(entite.nager())
    print(entite.faire_du_bruit())

# Utilisation avec différents types
canard = Canard()
personne = Personne()
robot = Robot()

print("Le canard:")
faire_nager_et_bruiter(canard)

print("\nLa personne:")
faire_nager_et_bruiter(personne)

print("\nLe robot:")
faire_nager_et_bruiter(robot)
```

## Méthodes et attributs spéciaux

Python utilise des méthodes spéciales (également appelées méthodes magiques ou dunder methods) pour permettre à vos classes d'interagir avec les fonctionnalités intégrées du langage.

### Méthodes de représentation (`__str__`, `__repr__`)

```python
class Point:
    """Classe représentant un point en 2D."""

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        """Retourne une représentation lisible pour l'utilisateur."""
        return f"Point({self.x}, {self.y})"

    def __repr__(self):
        """Retourne une représentation non ambiguë pour le développeur."""
        return f"Point(x={self.x}, y={self.y})"

# Utilisation des méthodes de représentation
p = Point(3, 4)
print(p)           # Point(3, 4) (appelle __str__)
print(repr(p))     # Point(x=3, y=4) (appelle __repr__)
print(f"{p!r}")    # Point(x=3, y=4) (appelle __repr__ via la syntaxe f-string)
```

### Méthodes opérateurs (`__add__`, `__eq__`, etc.)

```python
class Vecteur:
    """Classe représentant un vecteur en 2D."""

    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"Vecteur({self.x}, {self.y})"

    def __add__(self, autre):
        """Permet l'addition de vecteurs avec l'opérateur +."""
        if isinstance(autre, Vecteur):
            return Vecteur(self.x + autre.x, self.y + autre.y)
        return NotImplemented

    def __sub__(self, autre):
        """Permet la soustraction de vecteurs avec l'opérateur -."""
        if isinstance(autre, Vecteur):
            return Vecteur(self.x - autre.x, self.y - autre.y)
        return NotImplemented

    def __mul__(self, scalaire):
        """Permet la multiplication par un scalaire avec l'opérateur *."""
        if isinstance(scalaire, (int, float)):
            return Vecteur(self.x * scalaire, self.y * scalaire)
        return NotImplemented

    def __rmul__(self, scalaire):
        """Permet la multiplication par un scalaire lorsque le scalaire est à gauche."""
        return self.__mul__(scalaire)

    def __eq__(self, autre):
        """Permet la comparaison d'égalité avec l'opérateur ==."""
        if isinstance(autre, Vecteur):
            return self.x == autre.x and self.y == autre.y
        return NotImplemented

    def __abs__(self):
        """Retourne la magnitude du vecteur avec la fonction abs()."""
        return (self.x ** 2 + self.y ** 2) ** 0.5

# Utilisation des opérateurs surchargés
v1 = Vecteur(3, 4)
v2 = Vecteur(1, 2)

print(v1 + v2)    # Vecteur(4, 6)
print(v1 - v2)    # Vecteur(2, 2)
print(v1 * 2)     # Vecteur(6, 8)
print(3 * v2)     # Vecteur(3, 6)
print(v1 == v2)   # False

v3 = Vecteur(3, 4)
print(v1 == v3)   # True

print(abs(v1))    # 5.0 (magnitude du vecteur)
```

### Méthodes de conteneur (`__len__`, `__getitem__`, etc.)

```python
class ListePersonnalisee:
    """Une classe qui se comporte comme une liste."""

    def __init__(self, elements=None):
        self.elements = elements if elements is not None else []

    def __len__(self):
        """Permet d'utiliser la fonction len()."""
        return len(self.elements)

    def __getitem__(self, index):
        """Permet l'accès par index avec la syntaxe []."""
        return self.elements[index]

    def __setitem__(self, index, valeur):
        """Permet l'assignation par index avec la syntaxe []."""
        self.elements[index] = valeur

    def __delitem__(self, index):
        """Permet la suppression par index avec del."""
        del self.elements[index]

    def __iter__(self):
        """Permet d'itérer sur l'objet."""
        return iter(self.elements)

    def __contains__(self, item):
        """Permet d'utiliser l'opérateur in."""
        return item in self.elements

    def __str__(self):
        return str(self.elements)

# Utilisation des méthodes de conteneur
ma_liste = ListePersonnalisee([1, 2, 3, 4, 5])

print(len(ma_liste))    # 5
print(ma_liste[2])      # 3

ma_liste[1] = 10
print(ma_liste)         # [1, 10, 3, 4, 5]

del ma_liste[0]
print(ma_liste)         # [10, 3, 4, 5]

print(3 in ma_liste)    # True
print(6 in ma_liste)    # False

# Itération
for element in ma_liste:
    print(element, end=" ")  # 10 3 4 5
```

### Méthodes de contexte (`__enter__`, `__exit__`)

```python
class GestionnaireContexte:
    """Une classe qui peut être utilisée avec la déclaration 'with'."""

    def __init__(self, nom):
        self.nom = nom

    def __enter__(self):
        """Appelé au début du bloc with."""
        print(f"Entrée dans le contexte de {self.nom}")
        return self  # L'objet retourné est assigné à la variable après 'as'

    def __exit__(self, exc_type, exc_val, exc_tb):
        """Appelé à la fin du bloc with."""
        print(f"Sortie du contexte de {self.nom}")

        # Si une exception s'est produite dans le bloc
        if exc_type is not None:
            print(f"Une exception de type {exc_type.__name__} s'est produite: {exc_val}")
            # Retourner True pour supprimer l'exception, ou False pour la propager
            return False

# Utilisation du gestionnaire de contexte
with GestionnaireContexte("mon_contexte") as ctx:
    print("À l'intérieur du bloc with")
    print(f"Contexte: {ctx.nom}")

# Gestion d'exception
try:
    with GestionnaireContexte("contexte_erreur") as ctx:
        print("À l'intérieur du bloc with avec erreur")
        raise ValueError("Une erreur s'est produite")
except ValueError:
    print("L'exception a été propagée")
```

## Attributs et méthodes de classe

### Attributs de classe

Les attributs de classe sont partagés par toutes les instances de la classe :

```python
class Compteur:
    """Une classe avec un compteur partagé."""

    # Attribut de classe
    nombre_instances = 0

    def __init__(self, nom):
        self.nom = nom
        # Incrémentation de l'attribut de classe
        Compteur.nombre_instances += 1

    def __del__(self):
        """Destructeur appelé lorsque l'objet est supprimé."""
        Compteur.nombre_instances -= 1

# Utilisation de l'attribut de classe
print(f"Instances initiales: {Compteur.nombre_instances}")  # 0

compteur1 = Compteur("Premier")
print(f"Après création compteur1: {Compteur.nombre_instances}")  # 1

compteur2 = Compteur("Deuxième")
compteur3 = Compteur("Troisième")
print(f"Après création de 3 compteurs: {Compteur.nombre_instances}")  # 3

# Accès via une instance (mais référence toujours l'attribut de classe)
print(f"Via compteur1: {compteur1.nombre_instances}")  # 3

# Suppression d'un compteur
del compteur3
print(f"Après suppression: {Compteur.nombre_instances}")  # 2
```

### Méthodes de classe (`@classmethod`)

Les méthodes de classe sont liées à la classe plutôt qu'à une instance spécifique :

```python
class Date:
    """Une classe représentant une date."""

    def __init__(self, jour, mois, annee):
        self.jour = jour
        self.mois = mois
        self.annee = annee

    def __str__(self):
        return f"{self.jour:02d}/{self.mois:02d}/{self.annee}"

    @classmethod
    def depuis_chaine(cls, chaine_date):
        """Crée une instance de Date à partir d'une chaîne au format 'JJ-MM-AAAA'."""
        jour, mois, annee = map(int, chaine_date.split('-'))
        return cls(jour, mois, annee)

    @classmethod
    def aujourdhui(cls):
        """Crée une instance de Date représentant la date du jour."""
        import datetime
        aujourdhui = datetime.datetime.now()
        return cls(aujourdhui.day, aujourdhui.month, aujourdhui.year)

# Utilisation des méthodes de classe
date1 = Date(15, 3, 2023)
print(date1)  # 15/03/2023

# Création via méthode de classe
date2 = Date.depuis_chaine("25-12-2023")
print(date2)  # 25/12/2023

# Autre méthode de classe
date_aujourdhui = Date.aujourdhui()
print(f"Aujourd'hui: {date_aujourdhui}")
```

### Méthodes statiques (`@staticmethod`)

Les méthodes statiques sont des méthodes qui ne dépendent ni de la classe ni de l'instance :

```python
class MathUtils:
    """Classe utilitaire pour des opérations mathématiques."""

    @staticmethod
    def est_nombre_premier(n):
        """Vérifie si un nombre est premier."""
        if n <= 1:
            return False
        if n <= 3:
            return True
        if n % 2 == 0 or n % 3 == 0:
            return False
        i = 5
        while i * i <= n:
            if n % i == 0 or n % (i + 2) == 0:
                return False
            i += 6
        return True

    @staticmethod
    def pgcd(a, b):
        """Calcule le plus grand commun diviseur de deux nombres."""
        while b:
            a, b = b, a % b
        return a

    @staticmethod
    def ppcm(a, b):
        """Calcule le plus petit commun multiple de deux nombres."""
        return a * b // MathUtils.pgcd(a, b)

# Utilisation des méthodes statiques
print(MathUtils.est_nombre_premier(17))  # True
print(MathUtils.est_nombre_premier(20))  # False
print(MathUtils.pgcd(48, 18))           # 6
print(MathUtils.ppcm(12, 18))           # 36

# Pas besoin d'instancier la classe
# mu = MathUtils()
# print(mu.est_nombre_premier(17))  # Fonctionne aussi, mais moins clair
```

## Composition et agrégation

La composition et l'agrégation sont des façons d'établir des relations entre classes sans utiliser l'héritage.

### Composition vs. héritage

La composition ("a-part-of") établit une relation où une classe contient une ou plusieurs instances d'autres classes comme attributs :

```python
class Moteur:
    """Représente le moteur d'une voiture."""

    def __init__(self, cylindree, puissance):
        self.cylindree = cylindree  # en litres
        self.puissance = puissance  # en chevaux

    def demarrer(self):
        return "Vroooom!"

    def arreter(self):
        return "Le moteur s'arrête..."

class Roue:
    """Représente une roue de voiture."""

    def __init__(self, taille, pression):
        self.taille = taille      # en pouces
        self.pression = pression  # en bar

    def gonfler(self, pression_ajoutee):
        self.pression += pression_ajoutee
        return f"Pression actuelle: {self.pression} bar"

class Voiture:
    """Représente une voiture (composition)."""

    def __init__(self, marque, modele, cylindree, puissance):
        self.marque = marque
        self.modele = modele
        # Composition: la voiture "a un" moteur
        self.moteur = Moteur(cylindree, puissance)
        # Composition: la voiture "a" 4 roues
        self.roues = [Roue(17, 2.5) for _ in range(4)]

    def demarrer(self):
        return f"{self.marque} {self.modele}: {self.moteur.demarrer()}"

    def verifier_pression_pneus(self):
        pressions = [roue.pression for roue in self.roues]
        return f"Pression des pneus: {pressions}"

# Utilisation de la composition
ma_voiture = Voiture("Toyota", "Corolla", 1.8, 140)
print(ma_voiture.demarrer())  # Toyota Corolla: Vroooom!
print(ma_voiture.verifier_pression_pneus())  # Pression des pneus: [2.5, 2.5, 2.5, 2.5]

# La voiture contrôle le cycle de vie du moteur et des roues
# Si la voiture est supprimée, le moteur et les roues le sont aussi
```

### Conception par agrégation

L'agrégation ("has-a") est similaire à la composition, mais les objets contenus peuvent exister indépendamment de l'objet conteneur :

```python
class Auteur:
    """Représente un auteur de livres."""

    def __init__(self, nom, biographie):
        self.nom = nom
        self.biographie = biographie

    def __str__(self):
        return self.nom

class Livre:
    """Représente un livre écrit par un auteur."""

    def __init__(self, titre, auteur, annee):
        self.titre = titre
        self.auteur = auteur  # Agrégation: référence à un objet Auteur existant
        self.annee = annee

    def __str__(self):
        return f"{self.titre} ({self.annee}) par {self.auteur}"

class Bibliotheque:
    """Représente une collection de livres."""

    def __init__(self, nom):
        self.nom = nom
        self.livres = []  # Agrégation: contient des références à des objets Livre

    def ajouter_livre(self, livre):
        self.livres.append(livre)
        return f"{livre.titre} ajouté à la bibliothèque {self.nom}"

    def lister_livres(self):
        return [str(livre) for livre in self.livres]

# Utilisation de l'agrégation
# Les auteurs existent indépendamment
tolkien = Auteur("J.R.R. Tolkien", "Auteur britannique né en 1892...")
rowling = Auteur("J.K. Rowling", "Auteure britannique née en 1965...")

# Création de livres liés aux auteurs
seigneur_anneaux = Livre("Le Seigneur des Anneaux", tolkien, 1954)
hobbit = Livre("Le Hobbit", tolkien, 1937)
harry_potter = Livre("Harry Potter à l'école des sorciers", rowling, 1997)

# Ajout de livres à la bibliothèque
ma_bibliotheque = Bibliotheque("Ma Collection")
ma_bibliotheque.ajouter_livre(seigneur_anneaux)
ma_bibliotheque.ajouter_livre(hobbit)
ma_bibliotheque.ajouter_livre(harry_potter)

# Affichage des livres
for livre in ma_bibliotheque.lister_livres():
    print(livre)

# Les auteurs et les livres peuvent exister sans la bibliothèque
# Si la bibliothèque est supprimée, les auteurs et les livres existent toujours
```

## Métaclasses

Les métaclasses sont des "classes de classes" qui permettent de personnaliser la création de classes.

### La métaclasse `type`

En Python, `type` est la métaclasse par défaut qui crée des classes :

```python
# Création dynamique d'une classe avec type
def dire_bonjour(self, nom):
    return f"Bonjour, {nom}!"

# type(nom_classe, bases, dictionnaire_attributs)
ClasseDynamique = type('ClasseDynamique', (object,), {
    'x': 10,
    'dire_bonjour': dire_bonjour
})

# Utilisation de la classe créée dynamiquement
obj = ClasseDynamique()
print(obj.x)  # 10
print(obj.dire_bonjour("Alice"))  # Bonjour, Alice!
```

### Création de métaclasses personnalisées

Une métaclasse personnalisée permet de contrôler la création et l'initialisation des classes :

```python
class Meta(type):
    """Une métaclasse personnalisée."""

    def __new__(mcs, name, bases, attrs):
        """Appelée lors de la création d'une classe."""
        print(f"Création de la classe {name}")

        # Ajout d'un attribut à toutes les classes créées avec cette métaclasse
        attrs['meta_creee'] = True

        # Convertir toutes les méthodes non spéciales en majuscules
        for key, value in list(attrs.items()):
            if callable(value) and not key.startswith('__'):
                attrs[key.upper()] = value
                del attrs[key]

        # Création de la classe
        return super().__new__(mcs, name, bases, attrs)

    def __init__(cls, name, bases, attrs):
        """Appelée après la création de la classe."""
        print(f"Initialisation de la classe {name}")
        super().__init__(name, bases, attrs)

# Utilisation de la métaclasse
class MaClasse(metaclass=Meta):
    x = 10

    def methode(self):
        return "Méthode originale"

# La création et l'initialisation ont déjà été loguées
print(MaClasse.meta_creee)  # True

# La méthode a été renommée en majuscules
obj = MaClasse()
# print(obj.methode())  # AttributeError: 'MaClasse' object has no attribute 'methode'
print(obj.METHODE())  # Méthode originale
```

### Cas d'utilisation des métaclasses

Les métaclasses sont utiles pour :

- L'enregistrement automatique des classes
- La validation des attributs et méthodes
- La modification des classes au moment de leur création
- L'implémentation de modèles de conception comme le singleton

Exemple de métaclasse pour un registre de classes :

```python
class RegistryMeta(type):
    """Métaclasse qui enregistre toutes les classes qui l'utilisent."""

    registry = {}

    def __new__(mcs, name, bases, attrs):
        cls = super().__new__(mcs, name, bases, attrs)
        # Ne pas enregistrer les classes abstraites (avec attribut __abstract__ = True)
        if not attrs.get('__abstract__', False):
            mcs.registry[name] = cls
        return cls

class Base(metaclass=RegistryMeta):
    """Classe de base utilisant la métaclasse RegistryMeta."""
    __abstract__ = True  # Cette classe ne sera pas enregistrée

class A(Base):
    pass  # Sera enregistrée

class B(Base):
    pass  # Sera enregistrée

class C(Base):
    __abstract__ = True  # Ne sera pas enregistrée

class D(C):
    pass  # Sera enregistrée (car n'a pas __abstract__ = True)

# Affichage des classes enregistrées
print("Classes enregistrées:")
for name, cls in RegistryMeta.registry.items():
    print(f"- {name}: {cls}")
```

## Bonnes pratiques

1. **Utiliser des noms significatifs**

   - Nommez les classes avec des substantifs en PascalCase (ex: `CompteBancaire`).
   - Nommez les méthodes avec des verbes en snake_case (ex: `calculer_interet`).

2. **Suivre le principe de responsabilité unique**

   - Une classe ne devrait avoir qu'une seule raison de changer.
   - Préférez plusieurs petites classes spécialisées à une grande classe qui fait tout.

3. **Préférer la composition à l'héritage**

   - L'héritage crée un couplage fort entre les classes.
   - La composition offre plus de flexibilité et de modularité.

4. **Utiliser l'encapsulation appropriée**

   - Utilisez des attributs protégés (`_attr`) ou privés (`__attr`) lorsque nécessaire.
   - Fournissez des interfaces publiques (méthodes, propriétés) pour accéder aux données.

5. **Documenter vos classes**

   - Ajoutez des docstrings pour décrire le but de la classe et de ses méthodes.
   - Documentez les paramètres, les retours et les exceptions.

6. **Éviter les classes trop complexes**

   - Si une classe devient trop grande, envisagez de la diviser.
   - Visez un maximum de 500-700 lignes de code par classe.

7. **Initialiser correctement les objets**

   - Assurez-vous que `__init__` met l'objet dans un état valide.
   - Utilisez des valeurs par défaut ou levez des exceptions claires.

8. **Implémenter les méthodes spéciales appropriées**

   - Implémentez `__str__` pour une représentation lisible.
   - Implémentez `__repr__` pour une représentation non ambiguë.
   - Ajoutez des méthodes comme `__eq__`, `__hash__`, etc. selon les besoins.

9. **Utiliser des validations de données**

   - Validez les entrées dans les méthodes et les setters.
   - Levez des exceptions spécifiques avec des messages clairs.

10. **Tester vos classes**
    - Écrivez des tests pour vérifier le comportement de vos classes.
    - Testez les cas limites et les situations d'erreur.

## Exercices pratiques

1. **Système de gestion bancaire**
   Créez un système avec des classes pour `Compte`, `Client`, `Transaction`, etc.

2. **Jeu de cartes**
   Implémentez des classes pour `Carte`, `Paquet`, `Joueur`, `Jeu`, etc.

3. **Système de gestion de bibliothèque**
   Créez des classes pour `Livre`, `Auteur`, `Membre`, `Bibliothèque`, etc.

4. **Formes géométriques**
   Implémentez une hiérarchie de classes pour différentes formes avec calcul d'aire, périmètre, etc.

5. **Zoo virtuel**
   Créez une hiérarchie d'animaux avec différents comportements.

## Ressources supplémentaires

- [Documentation officielle Python - Classes](https://docs.python.org/fr/3/tutorial/classes.html)
- [Python Cookbook - Recettes sur les objets et les classes](https://www.oreilly.com/library/view/python-cookbook-3rd/9781449357337/)
- [Fluent Python - Chapitre sur les objets](https://www.oreilly.com/library/view/fluent-python-2nd/9781492056348/)
- [Design Patterns en Python](https://refactoring.guru/design-patterns/python)
- [Raymond Hettinger - Python's Class Development Toolkit](https://www.youtube.com/watch?v=HTLu2DFOdTg)
- [David Beazley - Python 3 Metaprogramming](https://www.youtube.com/watch?v=sPiWg5jSoZI)

---

Ce chapitre vous a présenté les concepts fondamentaux et avancés de la programmation orientée objet en Python. Les classes sont l'un des outils les plus puissants pour structurer et organiser votre code, permettant de créer des programmes modulaires, extensibles et maintenables. Dans le prochain chapitre, nous explorerons le monde de la programmation asynchrone à travers les coroutines en Python.
