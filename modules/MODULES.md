# Les Modules en Python

## Table des matières

- [Introduction](#introduction)
- [Concepts fondamentaux](#concepts-fondamentaux)
  - [Qu'est-ce qu'un module](#quest-ce-quun-module)
  - [Qu'est-ce qu'un package](#quest-ce-quun-package)
  - [L'écosystème des modules Python](#lécosystème-des-modules-python)
- [Utilisation des modules](#utilisation-des-modules)
  - [Import de modules](#import-de-modules)
  - [Formes d'import](#formes-dimport)
  - [Alias d'import](#alias-dimport)
  - [Imports relatifs et absolus](#imports-relatifs-et-absolus)
  - [Imports conditionnels](#imports-conditionnels)
- [Modules standards](#modules-standards)
  - [Modules essentiels](#modules-essentiels)
  - [Modules pour la manipulation de données](#modules-pour-la-manipulation-de-données)
  - [Modules système](#modules-système)
  - [Modules pour le développement web](#modules-pour-le-développement-web)
- [Création de modules](#création-de-modules)
  - [Structure de base d'un module](#structure-de-base-dun-module)
  - [Dunder names (\_\_name\_\_, \_\_all\_\_, etc.)](#dunder-names-__name__-__all__-etc)
  - [Initialisation de module](#initialisation-de-module)
  - [Variables et fonctions publiques et privées](#variables-et-fonctions-publiques-et-privées)
- [Création de packages](#création-de-packages)
  - [Structure de répertoire](#structure-de-répertoire)
  - [Fichier \_\_init\_\_.py](#fichier-__init__py)
  - [Sous-packages](#sous-packages)
  - [Packages à espace de noms](#packages-à-espace-de-noms)
- [Système d'importation de Python](#système-dimportation-de-python)
  - [Mécanisme de recherche de modules](#mécanisme-de-recherche-de-modules)
  - [Chemin de recherche des modules (sys.path)](#chemin-de-recherche-des-modules-syspath)
  - [Importlib et personnalisation de l'importation](#importlib-et-personnalisation-de-limportation)
  - [Modules compilés et extension C](#modules-compilés-et-extension-c)
- [Organisation de projets](#organisation-de-projets)
  - [Structure recommandée](#structure-recommandée)
  - [Séparation des préoccupations](#séparation-des-préoccupations)
  - [Stratégies de versionnage](#stratégies-de-versionnage)
  - [Tests et documentation](#tests-et-documentation)
- [Distribution de packages](#distribution-de-packages)
  - [Création d'un package distribuable](#création-dun-package-distribuable)
  - [setup.py et setup.cfg](#setuppy-et-setupcfg)
  - [pyproject.toml (PEP 517/518)](#pyprojecttoml-pep-517518)
  - [Publication sur PyPI](#publication-sur-pypi)
- [Gestion des dépendances](#gestion-des-dépendances)
  - [Pip et requirements.txt](#pip-et-requirementstxt)
  - [Environnements virtuels (venv, virtualenv)](#environnements-virtuels-venv-virtualenv)
  - [Pipenv](#pipenv)
  - [Poetry](#poetry)
  - [Conda](#conda)
- [Bonnes pratiques](#bonnes-pratiques)
  - [Organisation de code](#organisation-de-code)
  - [Conventions de nommage](#conventions-de-nommage)
  - [Documentation des modules](#documentation-des-modules)
  - [API stables et évolutives](#api-stables-et-évolutives)
- [Erreurs courantes](#erreurs-courantes)
  - [Problèmes d'importation circulaire](#problèmes-dimportation-circulaire)
  - [Variables globales mutables](#variables-globales-mutables)
  - [Mauvaise organisation du code](#mauvaise-organisation-du-code)
  - [Réutilisation des noms de modules standards](#réutilisation-des-noms-de-modules-standards)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

Les modules et packages sont au cœur de l'écosystème Python. Ils permettent d'organiser le code en unités logiques et réutilisables, facilitant ainsi la maintenance, la collaboration et le partage de code. La philosophie de Python "Batteries included" (batteries incluses) signifie que la bibliothèque standard est riche en modules utiles, tandis que l'écosystème plus large propose des milliers de packages tiers pour pratiquement tous les besoins.

Dans ce chapitre, nous explorerons en profondeur le système de modules de Python, depuis leur utilisation basique jusqu'à la création et distribution de vos propres packages. Vous apprendrez comment organiser efficacement vos projets, gérer les dépendances et mettre en œuvre les meilleures pratiques en matière de modularisation du code.

## Concepts fondamentaux

### Qu'est-ce qu'un module

Un module en Python est simplement un fichier contenant du code Python. Le nom du module est le nom du fichier sans l'extension `.py`. Les modules permettent d'organiser le code en compartiments logiques et réutilisables.

Par exemple, si vous avez un fichier `math_utils.py` :

```python
# math_utils.py
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b

PI = 3.14159
```

Vous pouvez l'importer et l'utiliser dans un autre fichier :

```python
import math_utils

result1 = math_utils.add(5, 3)       # 8
result2 = math_utils.multiply(4, 2)  # 8
print(math_utils.PI)                # 3.14159
```

Les modules offrent plusieurs avantages :

- **Organisation du code** : Diviser un programme en modules facilite sa compréhension et sa maintenance
- **Réutilisation** : Les modules peuvent être importés et utilisés dans différents programmes
- **Encapsulation** : Les modules aident à encapsuler les détails d'implémentation
- **Espace de noms** : Ils fournissent un espace de noms pour éviter les conflits de noms

### Qu'est-ce qu'un package

Un package est une façon d'organiser des modules connexes en une structure hiérarchique de répertoires. Techniquement, un package est un répertoire contenant un fichier spécial `__init__.py` et éventuellement d'autres modules ou sous-packages.

Voici un exemple de structure de package :

```
mon_package/
├── __init__.py
├── module1.py
├── module2.py
└── sous_package/
    ├── __init__.py
    └── module3.py
```

Cette structure permet d'organiser le code en hiérarchies logiques et facilite la gestion de projets plus importants.

Pour utiliser ce package :

```python
# Importer un module spécifique du package
import mon_package.module1

# Utiliser une fonction du module
mon_package.module1.some_function()

# Importer du sous-package
from mon_package.sous_package import module3
module3.another_function()
```

### L'écosystème des modules Python

L'écosystème Python est riche en modules et packages, répartis en trois grandes catégories :

1. **Modules intégrés** : Directement intégrés à l'interpréteur Python (`sys`, `builtins`)
2. **Modules de la bibliothèque standard** : Livrés avec Python mais doivent être importés (`os`, `datetime`, `json`)
3. **Modules tiers** : Développés par la communauté et installables via pip, conda, etc. (comme `numpy`, `pandas`, `requests`)

PyPI (Python Package Index) est le dépôt officiel de packages tiers et contient plus de 350 000 packages couvrant pratiquement tous les domaines d'application.

## Utilisation des modules

### Import de modules

L'importation de modules en Python se fait principalement via l'instruction `import`. Lorsqu'un module est importé pour la première fois, Python exécute tout le code du module et le rend disponible dans l'espace de noms actuel.

```python
# Importation basique
import math

# Utilisation
x = math.sqrt(16)  # 4.0
y = math.pi        # 3.141592...
```

Python recherche les modules dans une liste de répertoires définie dans `sys.path`, qui inclut :

- Le répertoire du script en cours d'exécution
- Les répertoires spécifiés par la variable d'environnement `PYTHONPATH`
- Les répertoires standard de la bibliothèque Python
- Les répertoires de packages tiers installés

### Formes d'import

Python offre plusieurs façons d'importer des modules et leurs composants :

```python
# 1. Import du module entier
import math
print(math.sqrt(16))  # Nécessite le préfixe math.

# 2. Import d'éléments spécifiques
from math import sqrt, pi
print(sqrt(16))  # Pas besoin du préfixe math.
print(pi)        # 3.141592...

# 3. Import de tous les éléments du module (à utiliser avec parcimonie)
from math import *
print(sqrt(16))  # 4.0
print(sin(0))    # 0.0
# Cette méthode peut créer des conflits de noms et réduire la lisibilité

# 4. Import avec sous-modules
import xml.etree.ElementTree
tree = xml.etree.ElementTree.parse('data.xml')

# Ou plus concis
from xml.etree import ElementTree
tree = ElementTree.parse('data.xml')
```

### Alias d'import

Pour simplifier l'utilisation de modules avec des noms longs ou éviter les conflits, Python permet de créer des alias lors de l'importation :

```python
# Alias pour un module entier
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

data = np.array([1, 2, 3])
df = pd.DataFrame({'A': [1, 2, 3]})
plt.plot([1, 2, 3])

# Alias pour des importations spécifiques
from math import sqrt as square_root
result = square_root(16)  # 4.0
```

Ces conventions d'alias sont largement adoptées dans l'écosystème Python, notamment dans la communauté scientifique.

### Imports relatifs et absolus

Dans un package, vous pouvez utiliser des imports relatifs ou absolus pour référencer d'autres modules du même package :

```
mon_package/
├── __init__.py
├── module_a.py
├── module_b.py
└── sous_package/
    ├── __init__.py
    └── module_c.py
```

**Imports absolus** (toujours à partir de la racine du package) :

```python
# Dans module_b.py
from mon_package import module_a
from mon_package.sous_package import module_c
```

**Imports relatifs** (à partir de l'emplacement actuel) :

```python
# Dans module_b.py
from . import module_a  # même niveau

# Dans sous_package/module_c.py
from .. import module_a  # niveau parent
from ..module_b import some_function  # module spécifique du parent
```

Les imports relatifs ne peuvent être utilisés que dans des modules au sein d'un package, pas dans des scripts autonomes.

### Imports conditionnels

Parfois, vous pourriez avoir besoin d'importer des modules de manière conditionnelle, par exemple pour gérer des dépendances optionnelles ou pour la compatibilité entre différentes versions de Python :

```python
# Gestion de dépendances optionnelles
try:
    import numpy as np
    HAS_NUMPY = True
except ImportError:
    HAS_NUMPY = False

def analyze_data(data):
    if HAS_NUMPY:
        # Utiliser numpy pour un calcul plus efficace
        return np.mean(data)
    else:
        # Implémentation de secours en Python pur
        return sum(data) / len(data)

# Compatibilité entre versions de Python
import sys
if sys.version_info >= (3, 10):
    # Fonctionnalités disponibles dans Python 3.10+
    from collections import Counter as counter
else:
    # Implémentation de secours pour les versions antérieures
    from backports.counter import Counter as counter
```

## Modules standards

Python inclut une vaste bibliothèque standard, souvent décrite comme "batteries included", qui offre de nombreux modules prêts à l'emploi pour diverses tâches.

### Modules essentiels

```python
import os          # Interaction avec le système d'exploitation
import sys         # Accès aux variables et fonctions spécifiques à l'interpréteur
import datetime    # Manipulation de dates et heures
import math        # Fonctions mathématiques
import random      # Génération de nombres aléatoires
import re          # Expressions régulières
import json        # Parsing et sérialisation JSON
import pickle      # Sérialisation d'objets Python
import time        # Accès au temps et pauses

# Exemples d'utilisation
print(os.getcwd())                  # Répertoire de travail actuel
print(sys.version)                  # Version de Python
print(datetime.datetime.now())      # Date et heure actuelles
print(math.factorial(5))            # 120
print(random.randint(1, 100))       # Nombre aléatoire entre 1 et 100
print(re.match(r'\d+', '123abc'))   # Recherche de motif
data = json.dumps({"name": "John"}) # Conversion en JSON
time.sleep(1)                       # Pause d'une seconde
```

### Modules pour la manipulation de données

```python
import csv         # Lecture et écriture de fichiers CSV
import sqlite3     # Interface pour la base de données SQLite
import xml         # Parsing XML
import html        # Manipulation HTML
import urllib      # Manipulation d'URLs et requêtes web
import base64      # Encodage et décodage base64
import hashlib     # Fonctions de hachage cryptographiques
import collections # Structures de données spécialisées

# Exemples
# Lecture d'un CSV
with open('data.csv', 'r') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)

# Utilisation de collections spécialisées
from collections import Counter, defaultdict
counter = Counter(['a', 'b', 'a', 'c', 'a'])
print(counter)  # Counter({'a': 3, 'b': 1, 'c': 1})

d = defaultdict(list)
d['key'].append(1)  # Crée automatiquement une liste vide si la clé n'existe pas
```

### Modules système

```python
import argparse    # Parsing d'arguments de ligne de commande
import logging     # Journalisation structurée
import threading   # Threads légers
import multiprocessing # Processus parallèles
import subprocess  # Exécution de commandes externes
import platform    # Informations sur la plate-forme
import shutil      # Opérations avancées sur les fichiers
import tempfile    # Création de fichiers temporaires
import io          # Manipulation de flux d'E/S

# Exemple de logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
logger.info("Ceci est un message d'information")
logger.warning("Ceci est un avertissement")

# Exemple d'arguments en ligne de commande
parser = argparse.ArgumentParser(description="Description du programme")
parser.add_argument('-f', '--file', help="Chemin du fichier")
args = parser.parse_args()
```

### Modules pour le développement web

```python
import http         # Client/serveur HTTP
import email        # Manipulation d'e-mails
import html         # Manipulation HTML
import urllib       # Manipulation d'URLs
import http.server  # Serveur HTTP simple
import wsgiref      # Référence d'implémentation WSGI
import cgi          # Interface de passerelle commune

# Exemple de serveur HTTP simple
from http.server import HTTPServer, SimpleHTTPServer

def run_server(port=8000):
    server_address = ('', port)
    httpd = HTTPServer(server_address, SimpleHTTPServer.SimpleHTTPRequestHandler)
    print(f"Serveur en cours d'exécution sur le port {port}")
    httpd.serve_forever()
```

## Création de modules

### Structure de base d'un module

Un module Python est simplement un fichier `.py`. Cependant, pour créer un module bien structuré et maintenable, certaines pratiques sont recommandées :

```python
"""
mon_module.py - Description courte du module

Ce module fournit des fonctions pour XYZ.
Il peut être utilisé comme ceci:
    import mon_module
    resultat = mon_module.ma_fonction(argument)

Auteur: Votre Nom
Version: 1.0.0
License: MIT
"""

# Imports standards
import math
import os

# Imports tiers (s'il y en a)
# import numpy as np

# Constantes du module
PI = 3.14159
MAX_SIZE = 100

# Fonctions publiques
def ma_fonction(argument):
    """Description de la fonction et ses arguments."""
    return _fonction_interne(argument)

def autre_fonction():
    """Description de cette fonction."""
    return "résultat"

# Fonctions "privées" (conventionnellement préfixées par _)
def _fonction_interne(argument):
    """Fonction interne non destinée à être appelée directement."""
    return argument * 2

# Classes
class MaClasse:
    """Documentation de la classe."""

    def __init__(self, param):
        self.param = param

    def methode(self):
        return self.param

# Code d'initialisation ou de test
if __name__ == "__main__":
    # Ce code s'exécute uniquement si le fichier est exécuté directement
    print("Test du module")
    print(ma_fonction(10))
```

Cette structure contient tous les éléments clés d'un module bien organisé : documentation, imports, constantes, fonctions, classes et un bloc de test.

### Dunder names (\_\_name\_\_, \_\_all\_\_, etc.)

Python utilise plusieurs variables spéciales (désignées par des doubles underscores ou "dunder") pour contrôler le comportement des modules :

#### \_\_name\_\_

La variable `__name__` contient le nom du module lors de l'import, ou la chaîne `"__main__"` si le module est exécuté directement. C'est la base du modèle courant :

```python
if __name__ == "__main__":
    # Code exécuté uniquement si le module est lancé directement
    # Idéal pour des tests ou comme point d'entrée
    main()
```

#### \_\_all\_\_

La liste `__all__` définit quels noms sont exportés lors de l'utilisation de `from module import *` :

```python
# module.py
__all__ = ['fonction_publique', 'AutreClasse']

def fonction_publique():
    return "Cette fonction est exportée avec from module import *"

def _fonction_interne():
    return "Cette fonction n'est pas exportée"

class AutreClasse:
    pass
```

Dans un autre fichier :

```python
from module import *  # Importe uniquement fonction_publique et AutreClasse
```

#### Autres dunder names utiles

```python
# __doc__ - Docstring du module
__doc__ = "Documentation alternative pour le module"

# __file__ - Chemin du fichier du module
print(__file__)  # Affiche le chemin du fichier

# __package__ - Nom du package contenant le module
print(__package__)

# __version__ - Convention pour indiquer la version du module
__version__ = "1.2.3"
```

### Initialisation de module

Lorsqu'un module est importé, tout son code est exécuté immédiatement. Cela permet une initialisation du module, mais peut aussi causer des effets de bord indésirables si cette initialisation est lourde ou modifie l'état global.

Exemple d'initialisation simple :

```python
"""Module d'analyse de données."""

print("Initialisation du module d'analyse")  # Attention aux effets de bord

# Configuration du module
DEBUG = False
DEFAULT_PRECISION = 2
_cache = {}  # Cache interne

# Fonction d'initialisation explicite
def init(debug=False, precision=None):
    """Initialise le module avec les paramètres donnés."""
    global DEBUG, DEFAULT_PRECISION
    DEBUG = debug
    if precision is not None:
        DEFAULT_PRECISION = precision

    if DEBUG:
        print(f"Module initialisé avec précision {DEFAULT_PRECISION}")
```

Pour les initialisations coûteuses, il est préférable d'utiliser un chargement paresseux (lazy loading) ou une initialisation explicite à l'appel.

### Variables et fonctions publiques et privées

Python n'a pas de mécanisme d'accès `private` comme d'autres langages, mais suit une convention :

- Les noms préfixés par un underscore (comme `_variable` ou `_fonction()`) sont considérés comme "privés" ou "internes"
- Les noms préfixés par deux underscores (comme `__variable`) déclenchent le "name mangling" pour éviter les collisions dans les sous-classes
- Les interfaces publiques sont celles sans préfixe underscore

Exemple :

```python
# Dans mon_module.py

# API publique
def fonction_publique():
    """Cette fonction fait partie de l'API publique."""
    return _helper() * 2

# Fonctions "privées"
def _helper():
    """
    Cette fonction est interne et n'est pas destinée à être utilisée directement.
    Elle pourrait changer sans avertissement.
    """
    return 42

class MaClasse:
    def methode_publique(self):
        return "public"

    def _methode_privee(self):
        return "privée"

    def __methode_tres_privee(self):
        return "très privée avec name mangling"
```

Utilisation :

```python
import mon_module

# Utilisation correcte
resultat = mon_module.fonction_publique()
instance = mon_module.MaClasse()
instance.methode_publique()

# Non recommandé (mais possible)
mon_module._helper()
instance._methode_privee()

# Difficile d'accès (mais pas impossible)
instance._MaClasse__methode_tres_privee()
```

## Création de packages

### Structure de répertoire

Un package simple a généralement une structure comme celle-ci :

```
mon_package/
├── __init__.py
├── module1.py
├── module2.py
└── sous_dossier/
    ├── __init__.py
    └── module3.py
```

Pour un projet plus complet destiné à être distribué, la structure pourrait être :

```
mon_projet/
├── README.md
├── setup.py
├── requirements.txt
├── mon_package/
│   ├── __init__.py
│   ├── module1.py
│   ├── module2.py
│   └── sous_package/
│       ├── __init__.py
│       └── module3.py
└── tests/
    ├── __init__.py
    ├── test_module1.py
    └── test_module2.py
```

### Fichier \_\_init\_\_.py

Le fichier `__init__.py` marque un répertoire comme étant un package Python. Il peut être vide, mais est souvent utilisé pour :

1. **Initialiser le package**
2. **Définir l'API publique du package**
3. **Remonter des éléments des sous-modules pour simplifier l'importation**

Exemple de fichier `__init__.py` :

```python
"""
mon_package - Description du package

Ce package fournit des fonctionnalités pour ...
"""

__version__ = '0.1.0'
__author__ = 'Votre Nom'

# Définir les éléments visibles avec from mon_package import *
__all__ = ['fonction_utile', 'AutreClasse', 'sous_module']

# Importer et exposer les éléments des sous-modules pour simplifier l'API
from .module1 import fonction_utile
from .module2 import AutreClasse
from . import sous_module  # Rendre disponible mon_package.sous_module

# Initialisation du package
print("Initialisation de mon_package")
```

Avec cette configuration, les utilisateurs peuvent faire :

```python
from mon_package import fonction_utile
from mon_package import AutreClasse
import mon_package.sous_module
```

au lieu de :

```python
from mon_package.module1 import fonction_utile
from mon_package.module2 import AutreClasse
import mon_package.sous_module
```

### Sous-packages

Les sous-packages sont simplement des packages imbriqués dans d'autres packages. Ils permettent d'organiser le code en hiérarchies plus profondes pour les projets complexes.

Exemple d'utilisation des sous-packages :

```python
# Structure
# mon_package/
# ├── __init__.py
# ├── core/
# │   ├── __init__.py
# │   └── fonctions.py
# └── utils/
#     ├── __init__.py
#     └── helpers.py

# Dans mon_package/__init__.py
from .core import fonction_principale  # Remonter l'API principale

# Dans mon_package/core/__init__.py
from .fonctions import fonction_principale

# Dans mon_package/core/fonctions.py
def fonction_principale():
    return "Résultat de la fonction principale"

# Dans mon_package/utils/__init__.py
from .helpers import helper_function

# Dans mon_package/utils/helpers.py
def helper_function():
    return "Aide"
```

Utilisation :

```python
# Import simple à cause de la remontée dans __init__.py
from mon_package import fonction_principale
fonction_principale()

# Import direct
from mon_package.utils import helper_function
helper_function()
```

### Packages à espace de noms

Depuis Python 3.3, il est possible de créer des "packages à espace de noms" (namespace packages) sans fichier `__init__.py`. Cela permet de répartir les parties d'un package sur plusieurs répertoires ou même sur plusieurs distributions.

```python
# Créer plusieurs répertoires pour le même espace de noms
# /chemin1/mon_namespace/module1.py
# /chemin2/mon_namespace/module2.py

# Ajouter les deux chemins au PYTHONPATH
import sys
sys.path.extend(['/chemin1', '/chemin2'])

# Importation qui combine les deux sources
import mon_namespace.module1
import mon_namespace.module2
```

Ces packages sont utiles pour les grands écosystèmes de logiciels avec plusieurs équipes ou distributions.

## Système d'importation de Python

### Mécanisme de recherche de modules

Lorsque vous importez un module, Python le recherche selon une séquence précise :

1. Vérifier s'il s'agit d'un module intégré (`sys.builtin_module_names`)
2. Rechercher un fichier correspondant dans la liste des répertoires de `sys.path`
3. Lever `ImportError` si le module n'est pas trouvé

Pour les packages, Python recherche un répertoire avec le nom du package contenant un fichier `__init__.py` (sauf pour les packages à espace de noms).

### Chemin de recherche des modules (sys.path)

`sys.path` est une liste de répertoires où Python recherche les modules. Elle est initialisée à partir de :

- Le répertoire du script principal
- La variable d'environnement `PYTHONPATH`
- Les répertoires de la bibliothèque standard
- Les répertoires des packages installés

Vous pouvez examiner et modifier cette liste :

```python
import sys

# Afficher le chemin de recherche
print(sys.path)

# Ajouter un répertoire au début du chemin
sys.path.insert(0, '/chemin/vers/mes/modules')

# Ajouter à la fin (moins prioritaire)
sys.path.append('/autre/chemin')
```

Cependant, modifier `sys.path` dans le code est généralement déconseillé. Il est préférable d'utiliser les mécanismes standard comme les environnements virtuels, `setup.py` ou les variables d'environnement.

### Importlib et personnalisation de l'importation

Le module `importlib` fournit des fonctionnalités pour personnaliser le processus d'importation :

```python
import importlib
import sys

# Recharger un module déjà importé
import mon_module
importlib.reload(mon_module)

# Import dynamique
module_name = "math"
math_module = importlib.import_module(module_name)
print(math_module.pi)

# Créer un finder et loader personnalisé
class MonImporteur:
    @classmethod
    def find_spec(cls, fullname, path, target=None):
        if fullname.startswith('virtuel.'):
            return importlib.machinery.ModuleSpec(
                fullname, cls, is_package=False)
        return None

    @classmethod
    def create_module(cls, spec):
        return None  # Utilise le module par défaut

    @classmethod
    def exec_module(cls, module):
        name = module.__name__
        # Remplir le module dynamiquement
        module.hello = f"Module virtuel {name}"

# Ajouter l'importeur à sys.meta_path
sys.meta_path.append(MonImporteur)

# Maintenant on peut importer des modules virtuels
import virtuel.test
print(virtuel.test.hello)  # "Module virtuel virtuel.test"
```

Cette technique avancée permet de créer des modules dynamiques, d'implémenter des formats de modules personnalisés ou de modifier le comportement d'importation.

### Modules compilés et extension C

Python peut importer des modules écrits en C ou dans d'autres langages, compilés en fichiers `.pyd` (Windows) ou `.so` (Unix/macOS).

```python
# Importer un module d'extension C
import numpy.core._multiarray_umath  # Module C interne de NumPy

# Modules standard avec implémentations C
import _csv       # Version C rapide du module csv
import _pickle    # Version C rapide du module pickle
```

Pour créer vos propres extensions C, vous pouvez utiliser :

- `ctypes` pour appeler des bibliothèques C existantes
- Le module `extension` de la bibliothèque standard
- Cython pour convertir du code Python en C
- PyBind11 pour intégrer du code C++

## Organisation de projets

### Structure recommandée

Pour un projet Python bien organisé, la structure suivante est recommandée :

```
nom_projet/                      # Dossier racine du projet
├── README.md                    # Documentation principale
├── LICENSE                      # Licence du projet
├── setup.py                     # Script d'installation
├── pyproject.toml               # Configuration du projet (PEP 518)
├── requirements.txt             # Dépendances pour l'installation
├── requirements-dev.txt         # Dépendances supplémentaires pour le développement
├── .gitignore                   # Fichiers à ignorer pour git
├── docs/                        # Documentation
│   ├── conf.py                  # Configuration Sphinx
│   └── index.rst                # Page d'accueil de la documentation
├── nom_package/                 # Package principal (code source)
│   ├── __init__.py
│   ├── module1.py
│   ├── module2.py
│   └── sous_package/
│       ├── __init__.py
│       └── module3.py
├── tests/                       # Tests unitaires et d'intégration
│   ├── __init__.py
│   ├── test_module1.py
│   └── test_module2.py
└── examples/                    # Exemples d'utilisation
    ├── exemple_simple.py
    └── exemple_avance.py
```

Cette structure est adaptable selon les besoins du projet.

### Séparation des préoccupations

Un bon design de module suit le principe de séparation des préoccupations :

1. **Modules fonctionnels** : Regroupez les fonctionnalités par domaine ou objectif, pas par type technique
2. **Cohésion élevée** : Chaque module doit avoir un objectif unique et bien défini
3. **Couplage faible** : Minimisez les interdépendances entre modules
4. **Interface claire** : Définissez clairement l'API publique de chaque module

Exemple :

```
mon_app/
├── models/             # Définition des données
│   ├── __init__.py
│   ├── user.py
│   └── product.py
├── services/           # Logique métier
│   ├── __init__.py
│   ├── auth.py
│   └── inventory.py
├── views/              # Présentation et UI
│   ├── __init__.py
│   ├── user_views.py
│   └── product_views.py
└── utils/              # Utilitaires génériques
    ├── __init__.py
    └── helpers.py
```

### Stratégies de versionnage

Pour gérer les versions de vos packages, suivez ces bonnes pratiques :

1. **Versionnage sémantique** : MAJEUR.MINEUR.CORRECTIF

   - MAJEUR : changements incompatibles avec l'API existante
   - MINEUR : ajouts de fonctionnalités rétrocompatibles
   - CORRECTIF : corrections de bugs rétrocompatibles

2. **Définir la version dans un seul endroit** :

```python
# mon_package/__init__.py
__version__ = '1.2.3'

# Puis dans setup.py
import mon_package
setup(
    name="mon_package",
    version=mon_package.__version__,
    # ...
)

# Alternative avec importlib.metadata pour Python 3.8+
try:
    from importlib.metadata import version
    __version__ = version("mon_package")
except ImportError:
    __version__ = "unknown"
```

3. **Journalisation des changements** : Maintenez un fichier `CHANGELOG.md` pour documenter les modifications.

### Tests et documentation

Les tests et la documentation sont des composantes essentielles d'un projet bien organisé :

**Tests** :

- Utilisez `unittest`, `pytest` ou un autre framework
- Organisez les tests en parallèle du code source
- Incluez des tests unitaires, d'intégration et fonctionnels

```python
# tests/test_module1.py
import unittest
from mon_package import module1

class TestModule1(unittest.TestCase):
    def test_fonction(self):
        self.assertEqual(module1.fonction(5), 10)

if __name__ == "__main__":
    unittest.main()
```

**Documentation** :

- Docstrings pour les modules, classes et fonctions
- README pour les instructions générales
- Sphinx pour la documentation complète

```python
def ma_fonction(arg1, arg2):
    """
    Description courte de la fonction.

    Description plus détaillée expliquant ce que fait la fonction,
    ses paramètres, valeurs de retour, exceptions, etc.

    Args:
        arg1 (int): Description du premier argument
        arg2 (str): Description du second argument

    Returns:
        bool: Description de la valeur de retour

    Raises:
        ValueError: Dans quelles conditions cette exception est levée

    Examples:
        >>> ma_fonction(5, "test")
        True
    """
    # Implémentation
    return True
```

## Distribution de packages

### Création d'un package distribuable

Pour créer un package distribuable, vous devez définir sa structure et ses métadonnées.

1. **Créer la structure du package** (comme vu précédemment)
2. **Définir les fichiers de configuration** (`setup.py`, `setup.cfg`, `pyproject.toml`)
3. **Compiler le package** pour la distribution

### setup.py et setup.cfg

Le fichier `setup.py` est traditionnellement utilisé pour définir comment installer le package :

```python
from setuptools import setup, find_packages

setup(
    name="mon_package",
    version="0.1.0",
    author="Votre Nom",
    author_email="votre.email@exemple.com",
    description="Description courte du package",
    long_description=open("README.md").read(),
    long_description_content_type="text/markdown",
    url="https://github.com/username/mon_package",
    packages=find_packages(),
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    python_requires=">=3.6",
    install_requires=[
        "requests>=2.25.0",
        "numpy>=1.19.0",
    ],
    entry_points={
        "console_scripts": [
            "mon-commande=mon_package.cli:main",
        ],
    },
)
```

`setup.cfg` peut compléter ou remplacer partiellement `setup.py` :

```ini
[metadata]
name = mon_package
version = 0.1.0
author = Votre Nom
author_email = votre.email@exemple.com
description = Description courte du package
long_description = file: README.md
long_description_content_type = text/markdown
url = https://github.com/username/mon_package
classifiers =
    Programming Language :: Python :: 3
    License :: OSI Approved :: MIT License
    Operating System :: OS Independent

[options]
packages = find:
python_requires = >=3.6
install_requires =
    requests>=2.25.0
    numpy>=1.19.0

[options.entry_points]
console_scripts =
    mon-commande = mon_package.cli:main
```

### pyproject.toml (PEP 517/518)

`pyproject.toml` est le format moderne recommandé pour la configuration des projets Python :

```toml
[build-system]
requires = ["setuptools>=42", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "mon_package"
version = "0.1.0"
description = "Description courte du package"
readme = "README.md"
authors = [
    {name = "Votre Nom", email = "votre.email@exemple.com"}
]
license = {text = "MIT"}
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
]
requires-python = ">=3.6"
dependencies = [
    "requests>=2.25.0",
    "numpy>=1.19.0",
]

[project.urls]
"Homepage" = "https://github.com/username/mon_package"
"Bug Tracker" = "https://github.com/username/mon_package/issues"

[project.scripts]
mon-commande = "mon_package.cli:main"
```

### Publication sur PyPI

Pour publier votre package sur PyPI (Python Package Index) :

1. **Préparer le package pour la distribution** :

```bash
# Installer les outils nécessaires
pip install build twine

# Générer les archives de distribution
python -m build
```

2. **Vérifier les archives générées** :

```bash
# Vérifier les archives avec twine
twine check dist/*
```

3. **Téléverser sur TestPyPI** (facultatif mais recommandé) :

```bash
# Téléverser sur TestPyPI
twine upload --repository-url https://test.pypi.org/legacy/ dist/*

# Installer depuis TestPyPI pour vérifier
pip install --index-url https://test.pypi.org/simple/ mon_package
```

4. **Téléverser sur PyPI** :

```bash
# Téléverser sur PyPI
twine upload dist/*
```

Après cela, n'importe qui pourra installer votre package avec :

```bash
pip install mon_package
```

## Gestion des dépendances

### Pip et requirements.txt

`pip` est l'outil standard pour installer des packages Python. Un fichier `requirements.txt` permet de spécifier les dépendances d'un projet :

```
# requirements.txt
requests>=2.25.0
numpy>=1.19.0
pandas==1.3.0
```

Installation des dépendances :

```bash
pip install -r requirements.txt
```

### Environnements virtuels (venv, virtualenv)

Les environnements virtuels sont essentiels pour isoler les dépendances des projets :

```bash
# Création avec venv (intégré à Python 3)
python -m venv mon_env

# Activation (Windows)
mon_env\Scripts\activate

# Activation (Unix/macOS)
source mon_env/bin/activate

# Installation des dépendances dans l'environnement
pip install -r requirements.txt

# Désactivation
deactivate
```

### Pipenv

Pipenv combine la gestion des packages et des environnements virtuels :

```bash
# Installation
pip install pipenv

# Créer un environnement et installer des dépendances
pipenv install requests numpy

# Installer des dépendances de développement
pipenv install --dev pytest black

# Activer l'environnement
pipenv shell

# Exécuter une commande dans l'environnement
pipenv run python script.py
```

Pipenv génère deux fichiers :

- `Pipfile` : Spécification des dépendances
- `Pipfile.lock` : Versions exactes pour une reproduction fidèle

### Poetry

Poetry est un outil moderne pour la gestion des dépendances et des packages :

```bash
# Installation
pip install poetry

# Initialiser un nouveau projet
poetry new mon_projet

# Ajouter des dépendances
poetry add requests numpy

# Ajouter des dépendances de développement
poetry add --dev pytest black

# Installer les dépendances
poetry install

# Exécuter une commande dans l'environnement
poetry run python script.py

# Activer l'environnement
poetry shell
```

Poetry utilise un seul fichier `pyproject.toml` pour toute la configuration.

### Conda

Conda est un gestionnaire de packages et d'environnements populaire, surtout dans la communauté scientifique :

```bash
# Création d'un environnement
conda create --name mon_env python=3.9

# Activation
conda activate mon_env

# Installation des packages
conda install numpy pandas scikit-learn

# Exportation des dépendances
conda env export > environment.yml

# Création à partir d'un fichier
conda env create -f environment.yml

# Désactivation
conda deactivate
```

Conda est particulièrement utile pour les packages comportant des extensions C complexes.

## Bonnes pratiques

### Organisation de code

1. **Un module, une responsabilité** : Chaque module doit avoir un objectif clair et unique
2. **Limiter la taille** : Si un module dépasse 500-1000 lignes, envisagez de le diviser
3. **Cohérence interne** : Les éléments d'un module doivent être liés
4. **Interfaces stables** : Définissez clairement l'API publique de chaque module
5. **Imports en haut** : Placez tous les imports au début du fichier
6. **Éviter les imports circulaires** : Restructurez si des modules dépendent mutuellement l'un de l'autre

### Conventions de nommage

Suivez les conventions PEP 8 pour le nommage :

1. **Modules et packages** : Noms courts, en minuscules, sans underscores (ex: `math`, `os`)
2. **Classes** : PascalCase (ex: `MyClass`, `NetworkManager`)
3. **Fonctions et variables** : snake_case (ex: `calculate_total`, `user_age`)
4. **Constantes** : MAJUSCULES_AVEC_UNDERSCORES (ex: `MAX_SIZE`, `DEFAULT_TIMEOUT`)
5. **Noms internes** : préfixé par un underscore (ex: `_internal_function`, `_helper`)

### Documentation des modules

Documentez efficacement vos modules :

1. **Docstring de module** : Au début du fichier, décrivant l'objectif et l'utilisation
2. **Docstrings pour fonctions et classes** : Pour toutes les API publiques
3. **Commentaires de code** : Pour les sections complexes uniquement
4. **README** : Instructions d'installation et exemples d'utilisation
5. **CHANGELOG** : Historique des modifications par version

Exemple de docstring de module :

```python
"""
database.py - Gestion de la connexion et des requêtes à la base de données

Ce module fournit une interface simplifiée pour interagir avec une base de données
SQL à travers l'ORM SQLAlchemy. Il gère la connexion, les transactions et les requêtes
courantes.

Utilisation typique:
    from myapp.database import get_session, User

    with get_session() as session:
        users = session.query(User).filter(User.active == True).all()

Dépendances:
    - SQLAlchemy>=1.4.0
"""
```

### API stables et évolutives

Pour maintenir une API stable tout en permettant l'évolution :

1. **Versionnement sémantique** : MAJEUR.MINEUR.CORRECTIF
2. **Dépréciation progressive** : Marquer les fonctions obsolètes avec warnings avant de les supprimer
3. **Compatibilité ascendante** : Conserver la compatibilité dans les versions mineures
4. **Paramètres optionnels** : Ajouter de nouvelles fonctionnalités via des paramètres optionnels

Exemple de dépréciation :

```python
import warnings

def old_function():
    warnings.warn(
        "old_function() est dépréciée et sera retirée dans la version 2.0.0. "
        "Utilisez new_function() à la place.",
        DeprecationWarning,
        stacklevel=2
    )
    # Implémentation ou appel à la nouvelle fonction
    return new_function()

def new_function():
    # Nouvelle implémentation
    return "résultat"
```

## Erreurs courantes

### Problèmes d'importation circulaire

Les imports circulaires se produisent lorsque le module A importe le module B, qui à son tour importe le module A.

```
# moduleA.py
from moduleB import fonctionB

def fonctionA():
    return "A" + fonctionB()

# moduleB.py
from moduleA import fonctionA  # Circulaire!

def fonctionB():
    return "B" + fonctionA()
```

Solutions :

1. **Restructurer** les modules pour éliminer la dépendance circulaire
2. **Import local** : Déplacer l'import à l'intérieur de la fonction où il est nécessaire
3. **Import différé** : Importer à l'exécution plutôt qu'à la définition

```python
# moduleB.py - Avec import local
def fonctionB():
    # Import seulement quand nécessaire
    from moduleA import fonctionA
    return "B" + fonctionA()
```

### Variables globales mutables

Les variables globales mutables peuvent causer des problèmes difficiles à déboguer :

```python
# Dans un module
CONFIG = {"timeout": 30, "retries": 3}  # Global mutable

def process():
    # Modification de la variable globale
    CONFIG["timeout"] = 60  # Affecte tous les utilisateurs du module!
```

Solutions :

1. **Utiliser des constantes immuables** quand possible
2. **Encapsuler l'état** dans des classes ou fonctions
3. **Copier les valeurs** avant modification
4. **Utiliser des gestionnaires de configuration** explicites

```python
# Meilleure approche
DEFAULT_CONFIG = {"timeout": 30, "retries": 3}  # Valeurs par défaut

def get_config():
    """Retourne une copie de la configuration."""
    return DEFAULT_CONFIG.copy()

def process(config=None):
    """Traite avec une configuration spécifique."""
    if config is None:
        config = get_config()

    # Modification locale uniquement
    config["timeout"] = 60
    # ...
```

### Mauvaise organisation du code

Une mauvaise organisation peut rendre le code difficile à maintenir :

```python
# Exemple de mauvaise organisation - tout dans un seul fichier
def calculer_moyenne(liste):
    # ...

def calculer_ecart_type(liste):
    # ...

def connecter_bd():
    # ...

def executer_requete(requete):
    # ...

class Utilisateur:
    # ...

class Produit:
    # ...

# Des centaines de lignes avec des fonctionnalités non liées...
```

Solutions :

1. **Séparer par fonctionnalité** en modules distincts
2. **Créer une hiérarchie logique** de packages
3. **Regrouper les classes et fonctions connexes**

Meilleure structure :

```
mon_app/
├── modeles/
│   ├── __init__.py
│   ├── utilisateur.py  # Classe Utilisateur
│   └── produit.py      # Classe Produit
├── database/
│   ├── __init__.py
│   ├── connexion.py    # connecter_bd, executer_requete
│   └── requetes.py     # Requêtes spécifiques
└── statistiques/
    ├── __init__.py
    └── calculs.py      # calculer_moyenne, calculer_ecart_type
```

### Réutilisation des noms de modules standards

Réutiliser les noms de modules standards peut créer des conflits d'importation :

```python
# Fichier email.py dans votre projet
def envoyer_email(destinataire, sujet, corps):
    # ...

# Ailleurs dans votre code
import email  # Importe votre module au lieu du module standard!
```

Solutions :

1. **Éviter de nommer** vos modules comme des modules standards
2. **Utiliser des préfixes spécifiques** à votre projet
3. **Vérifier les conflits potentiels** avant de nommer

## Ressources supplémentaires

- [Documentation officielle sur les modules](https://docs.python.org/fr/3/tutorial/modules.html)
- [Guide de la bibliothèque standard Python](https://docs.python.org/fr/3/library/index.html)
- [Python Packaging User Guide](https://packaging.python.org/)
- [Real Python - Python Modules and Packages](https://realpython.com/python-modules-packages/)
- [PEP 8 - Style Guide for Python Code](https://peps.python.org/pep-0008/)
- [PEP 328 - Imports: Multi-Line and Absolute/Relative](https://peps.python.org/pep-0328/)
- [PEP 517/518 - Spécifications pour la construction de packages](https://peps.python.org/pep-0517/)
- [Hitchhiker's Guide to Python - Structuring Your Project](https://docs.python-guide.org/writing/structure/)
- [Full Stack Python - Application Dependencies](https://www.fullstackpython.com/application-dependencies.html)

---

Ce chapitre vous a présenté en détail le système de modules et packages en Python, ainsi que les meilleures pratiques pour organiser et distribuer votre code. La modularisation est un pilier fondamental du développement logiciel, permettant de créer des applications maintenables, évolutives et réutilisables. Dans le prochain chapitre, nous aborderons les fonctions usuelles de Python qui facilitent les opérations courantes de manipulation de données.
