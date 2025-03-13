# Les Coroutines en Python

## Table des matières

- [Introduction](#introduction)
- [Programmation synchrone vs asynchrone](#programmation-synchrone-vs-asynchrone)
  - [Limites de la programmation synchrone](#limites-de-la-programmation-synchrone)
  - [Avantages de la programmation asynchrone](#avantages-de-la-programmation-asynchrone)
  - [Cas d'utilisation idéaux](#cas-dutilisation-idéaux)
- [Coroutines: concepts fondamentaux](#coroutines-concepts-fondamentaux)
  - [Définition et principes](#définition-et-principes)
  - [Générateurs vs coroutines](#générateurs-vs-coroutines)
  - [Évolution des coroutines en Python](#évolution-des-coroutines-en-python)
- [Syntaxe async/await](#syntaxe-asyncawait)
  - [Définition d'une coroutine avec `async def`](#définition-dune-coroutine-avec-async-def)
  - [Attendre une coroutine avec `await`](#attendre-une-coroutine-avec-await)
  - [Objets attendables (awaitables)](#objets-attendables-awaitables)
- [Le module asyncio](#le-module-asyncio)
  - [Boucles d'événements](#boucles-dévénements)
  - [Tâches (Tasks)](#tâches-tasks)
  - [Futures](#futures)
  - [Exécution de coroutines](#exécution-de-coroutines)
- [Création et chaînage de coroutines](#création-et-chaînage-de-coroutines)
  - [Coroutines simples](#coroutines-simples)
  - [Composition de coroutines](#composition-de-coroutines)
  - [Exécution parallèle avec `gather`](#exécution-parallèle-avec-gather)
  - [Exécution de la première coroutine terminée avec `wait_for`](#exécution-de-la-première-coroutine-terminée-avec-wait_for)
- [Gestion des erreurs dans les coroutines](#gestion-des-erreurs-dans-les-coroutines)
  - [Try/except avec await](#tryexcept-avec-await)
  - [Propagation des exceptions](#propagation-des-exceptions)
  - [Annulation de tâches](#annulation-de-tâches)
- [E/S asynchrones](#es-asynchrones)
  - [Opérations réseau](#opérations-réseau)
  - [Opérations sur les fichiers](#opérations-sur-les-fichiers)
  - [Opérations de temporisation](#opérations-de-temporisation)
- [Synchronisation entre coroutines](#synchronisation-entre-coroutines)
  - [Locks](#locks)
  - [Events](#events)
  - [Semaphores](#semaphores)
  - [Queues](#queues)
- [Streams](#streams)
  - [StreamReader et StreamWriter](#streamreader-et-streamwriter)
  - [Serveurs et clients asynchrones](#serveurs-et-clients-asynchrones)
- [Subprocessus asynchrones](#subprocessus-asynchrones)
  - [Exécution de commandes externes](#exécution-de-commandes-externes)
  - [Communication avec les subprocessus](#communication-avec-les-subprocessus)
- [Patterns avancés](#patterns-avancés)
  - [Producer-Consumer](#producer-consumer)
  - [Fan-out/Fan-in](#fan-outfan-in)
  - [Timeouts et gestion d'erreurs avancée](#timeouts-et-gestion-derreurs-avancée)
- [Asyncio et autres bibliothèques](#asyncio-et-autres-bibliothèques)
  - [aiohttp pour les requêtes HTTP](#aiohttp-pour-les-requêtes-http)
  - [asyncpg pour PostgreSQL](#asyncpg-pour-postgresql)
  - [aiomysql pour MySQL](#aiomysql-pour-mysql)
  - [aioredis pour Redis](#aioredis-pour-redis)
  - [Autres bibliothèques notables](#autres-bibliothèques-notables)
- [Débogage de code asynchrone](#débogage-de-code-asynchrone)
  - [Comprendre les traces d'exécution](#comprendre-les-traces-dexécution)
  - [Utilisation de `asyncio.debug`](#utilisation-de-asynciodebug)
  - [Logging dans les coroutines](#logging-dans-les-coroutines)
- [Bonnes pratiques](#bonnes-pratiques)
- [Pièges courants](#pièges-courants)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

La programmation asynchrone est devenue l'un des paradigmes fondamentaux du développement moderne, particulièrement pour les applications qui nécessitent un grand nombre d'opérations d'entrée/sortie concurrentes. Python, avec son module `asyncio` et sa syntaxe `async`/`await`, offre un modèle élégant et puissant pour écrire du code asynchrone.

Les coroutines sont au cœur de ce modèle. Ce sont des fonctions spéciales qui peuvent suspendre leur exécution à certains points, permettant à d'autres coroutines de s'exécuter pendant qu'elles attendent la complétion d'opérations lentes, comme des requêtes réseau, des lectures de fichiers ou des délais temporisés.

Dans ce chapitre, nous explorerons en profondeur le concept des coroutines en Python, comment elles fonctionnent, comment les utiliser efficacement, et les patterns avancés qu'elles permettent d'implémenter.

## Programmation synchrone vs asynchrone

### Limites de la programmation synchrone

Dans un programme synchrone traditionnel, les opérations sont exécutées séquentiellement, une après l'autre. Lorsqu'une opération bloquante est rencontrée (comme une requête réseau ou une lecture de fichier), le programme entier s'arrête et attend que cette opération se termine.

```python
# Exemple de code synchrone
import requests
import time

def télécharger_site(url):
    print(f"Téléchargement de {url}")
    response = requests.get(url)  # Opération bloquante
    print(f"Terminé {url}")
    return response.text

def télécharger_tous_les_sites(sites):
    début = time.time()
    for url in sites:
        html = télécharger_site(url)
    fin = time.time()
    print(f"Téléchargement terminé en {fin - début} secondes")

sites = [
    "https://www.google.com",
    "https://www.yahoo.com",
    "https://www.python.org",
    "https://www.github.com",
]

télécharger_tous_les_sites(sites)  # Prend plusieurs secondes
```

Ce code est simple à comprendre, mais inefficace lorsqu'il s'agit d'opérations d'E/S. Pendant que le programme attend la réponse d'un serveur, le processeur reste inactif, gaspillant des ressources précieuses.

### Avantages de la programmation asynchrone

La programmation asynchrone permet à un programme de continuer son exécution pendant qu'il attend la complétion d'opérations d'E/S. Cela se traduit par:

1. **Meilleure utilisation des ressources**: Le CPU peut effectuer d'autres tâches pendant les opérations d'E/S.
2. **Concurrence sans multithreading**: Évite les complexités et les problèmes potentiels des threads.
3. **Scalabilité accrue**: Peut gérer un grand nombre de connexions simultanées avec une empreinte mémoire réduite.
4. **Modèle mental clair**: Avec `async`/`await`, le code asynchrone peut être presque aussi lisible que le code synchrone.

```python
# Exemple de code asynchrone équivalent
import asyncio
import aiohttp
import time

async def télécharger_site(session, url):
    print(f"Téléchargement de {url}")
    async with session.get(url) as response:
        print(f"Terminé {url}")
        return await response.text()

async def télécharger_tous_les_sites(sites):
    async with aiohttp.ClientSession() as session:
        tâches = []
        for url in sites:
            tâche = asyncio.create_task(télécharger_site(session, url))
            tâches.append(tâche)

        await asyncio.gather(*tâches)

async def main():
    sites = [
        "https://www.google.com",
        "https://www.yahoo.com",
        "https://www.python.org",
        "https://www.github.com",
    ]

    début = time.time()
    await télécharger_tous_les_sites(sites)
    fin = time.time()

    print(f"Téléchargement terminé en {fin - début} secondes")

# Python 3.7+
asyncio.run(main())  # Beaucoup plus rapide que la version synchrone
```

### Cas d'utilisation idéaux

La programmation asynchrone est particulièrement adaptée pour:

1. **Applications web hautement concurrentes**: Serveurs pouvant gérer des milliers de connexions simultanées.
2. **Microservices**: Communication entre services distribuée et efficace.
3. **APIs hautes performances**: API REST ou GraphQL avec temps de réponse faible.
4. **Scraping web**: Collecte de données de multiples sources en parallèle.
5. **Applications temps réel**: Chat, notifications push, mises à jour en temps réel.
6. **IoT (Internet des objets)**: Gestion de nombreux appareils connectés.

À l'inverse, la programmation asynchrone n'est généralement pas avantageuse pour:

- Tâches à forte intensité CPU (calculs scientifiques, traitement d'images, etc.)
- Applications simples avec peu d'opérations d'E/S
- Programmes à courte durée de vie

## Coroutines: concepts fondamentaux

### Définition et principes

Une coroutine en Python est une fonction spéciale qui peut suspendre son exécution et rendre le contrôle à l'appelant sans perdre son état. Contrairement aux fonctions traditionnelles qui suivent un modèle d'entrée-sortie "entrée-puis-sortie", les coroutines peuvent se suspendre et reprendre à plusieurs points différents.

Les coroutines fonctionnent sur un principe de coopération, où chaque coroutine cède volontairement le contrôle lorsqu'elle attend une opération potentiellement bloquante. Cette approche diffère du multithreading, où la préemption peut se produire à tout moment.

### Générateurs vs coroutines

Les coroutines modernes en Python ont évolué à partir des générateurs. Un générateur est une fonction qui utilise `yield` pour produire une séquence de valeurs:

```python
def simple_generator():
    yield 1
    yield 2
    yield 3

for value in simple_generator():
    print(value)  # Affiche 1, 2, 3
```

Les générateurs peuvent également recevoir des valeurs avec `send()`, ce qui a conduit au concept initial de coroutines en Python:

```python
def echo_coroutine():
    while True:
        value = yield
        print(f"Reçu: {value}")

coro = echo_coroutine()
next(coro)  # Amorcer la coroutine jusqu'au premier yield
coro.send("Hello")  # Affiche "Reçu: Hello"
coro.send("World")  # Affiche "Reçu: World"
```

### Évolution des coroutines en Python

L'évolution des coroutines en Python peut être résumée comme suit:

1. **Python 2.5**: Introduction des générateurs améliorés avec `send()`, `throw()` et `close()`
2. **PEP 342**: Ajout du support pour les coroutines basées sur les générateurs
3. **Python 3.3**: Introduction du module `yield from` (PEP 380)
4. **Python 3.4**: Introduction du module `asyncio` (PEP 3156)
5. **Python 3.5**: Introduction de la syntaxe `async`/`await` (PEP 492)
6. **Python 3.6**: Améliorations des générateurs asynchrones et compréhensions asynchrones
7. **Python 3.7**: Ajout de `asyncio.run()` et d'autres améliorations de l'API
8. **Python 3.8**: Support du protocole ASGI, améliorations de performance
9. **Python 3.9+**: Affinements continus de l'API asyncio

## Syntaxe async/await

### Définition d'une coroutine avec `async def`

Une coroutine est définie à l'aide du mot-clé `async def`:

```python
async def ma_coroutine():
    print("Début de la coroutine")
    # Corps de la coroutine
    print("Fin de la coroutine")
    return "Résultat"
```

Appeler cette fonction ne l'exécute pas immédiatement, mais renvoie un objet coroutine:

```python
coro = ma_coroutine()
print(type(coro))  # <class 'coroutine'>
```

Pour exécuter une coroutine, vous devez l'attendre dans une autre coroutine avec `await` ou l'exécuter dans une boucle d'événements.

### Attendre une coroutine avec `await`

Le mot-clé `await` permet d'attendre la complétion d'une coroutine ou d'un objet "awaitable":

```python
async def coroutine_1():
    print("Coroutine 1 démarre")
    await asyncio.sleep(1)  # Simule une opération d'E/S
    print("Coroutine 1 se termine")
    return "Résultat de coroutine 1"

async def coroutine_2():
    print("Coroutine 2 démarre")
    résultat = await coroutine_1()  # Attend coroutine_1
    print(f"Coroutine 2 a obtenu: {résultat}")
    print("Coroutine 2 se termine")
```

Lorsqu'une coroutine atteint un `await`, elle suspend son exécution jusqu'à ce que l'opération attendue soit terminée. Pendant ce temps, la boucle d'événements peut exécuter d'autres coroutines.

### Objets attendables (awaitables)

En Python, un objet est "awaitable" s'il peut être utilisé avec le mot-clé `await`. Les types attendables incluent:

1. **Coroutines**: Objets créés par les fonctions `async def`
2. **Tâches** (`asyncio.Task`): Wrappers autour des coroutines pour leur exécution concurrente
3. **Futures** (`asyncio.Future`): Objets de bas niveau pour représenter un résultat qui sera disponible plus tard

```python
# Exemple d'utilisation des différents attendables
async def demo_awaitables():
    # Attendre une coroutine
    résultat_1 = await asyncio.sleep(1)

    # Attendre une tâche
    tâche = asyncio.create_task(asyncio.sleep(2))
    résultat_2 = await tâche

    # Attendre un future
    future = asyncio.get_event_loop().create_future()
    # Future sera résolu ailleurs
    asyncio.create_task(résoudre_future(future))
    résultat_3 = await future

    return résultat_1, résultat_2, résultat_3

async def résoudre_future(future):
    await asyncio.sleep(0.5)
    future.set_result("Future résolu")
```

## Le module asyncio

### Boucles d'événements

La boucle d'événements est le mécanisme central qui gère l'exécution asynchrone en Python. Elle:

- Planifie l'exécution des coroutines
- Surveille les E/S pour détecter quand elles sont prêtes
- Réveille les coroutines en attente lorsque leurs opérations sont terminées
- Gère les callbacks et les timeouts

```python
# Obtention et manipulation explicite de la boucle d'événements
import asyncio

async def main():
    print("Exécution de la coroutine principale")
    await asyncio.sleep(1)
    print("Coroutine principale terminée")

# Python 3.7+
asyncio.run(main())  # Crée et gère la boucle d'événements

# Approche plus explicite (moins recommandée maintenant)
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
loop.close()
```

À partir de Python 3.7, la fonction `asyncio.run()` est recommandée pour exécuter une coroutine de haut niveau.

### Tâches (Tasks)

Les tâches sont des wrappers autour des coroutines qui permettent leur exécution concurrente:

```python
async def tâche_longue(nom, durée):
    print(f"Tâche {nom} démarre")
    await asyncio.sleep(durée)
    print(f"Tâche {nom} terminée après {durée}s")
    return f"Résultat de {nom}"

async def main():
    # Création de tâches
    tâche1 = asyncio.create_task(tâche_longue("A", 3))
    tâche2 = asyncio.create_task(tâche_longue("B", 2))
    tâche3 = asyncio.create_task(tâche_longue("C", 1))

    # Attente des tâches
    print("Attente des résultats...")
    résultat1 = await tâche1
    résultat2 = await tâche2
    résultat3 = await tâche3

    print(f"Résultats: {résultat1}, {résultat2}, {résultat3}")

asyncio.run(main())
```

Dans cet exemple, les trois tâches s'exécutent en parallèle, et leurs résultats sont attendus dans l'ordre d'attente (pas nécessairement dans l'ordre de complétion).

### Futures

Les Futures sont des objets de bas niveau qui représentent un résultat qui sera disponible plus tard. Ils sont généralement utilisés pour l'interopérabilité entre asyncio et d'autres frameworks asynchrones:

```python
async def exemple_future():
    # Création d'un Future
    loop = asyncio.get_running_loop()
    future = loop.create_future()

    # Planification de sa résolution
    loop.call_later(1, resolve_future, future, "Résultat du future")

    # Attente du future
    résultat = await future
    print(f"Future résolu avec: {résultat}")

def resolve_future(future, value):
    future.set_result(value)

asyncio.run(exemple_future())
```

### Exécution de coroutines

Il existe plusieurs façons d'exécuter des coroutines:

```python
async def coroutine_a():
    print("Coroutine A s'exécute")
    await asyncio.sleep(1)
    return "Résultat A"

async def coroutine_b():
    print("Coroutine B s'exécute")
    await asyncio.sleep(0.5)
    return "Résultat B"

async def coroutine_c():
    print("Coroutine C s'exécute")
    await asyncio.sleep(1.5)
    return "Résultat C"

async def main():
    # 1. Exécution séquentielle
    résultat_a = await coroutine_a()
    résultat_b = await coroutine_b()
    print(f"Séquentiel: {résultat_a}, {résultat_b}")

    # 2. Exécution concurrente avec create_task et await
    tâche_a = asyncio.create_task(coroutine_a())
    tâche_b = asyncio.create_task(coroutine_b())

    résultat_a = await tâche_a
    résultat_b = await tâche_b
    print(f"Tasks + await: {résultat_a}, {résultat_b}")

    # 3. Exécution concurrente avec gather
    résultats = await asyncio.gather(
        coroutine_a(),
        coroutine_b(),
        coroutine_c()
    )
    print(f"Gather: {résultats}")

    # 4. Attendre uniquement la première terminée
    tâche_a = asyncio.create_task(coroutine_a())
    tâche_b = asyncio.create_task(coroutine_b())

    terminée, en_attente = await asyncio.wait(
        [tâche_a, tâche_b],
        return_when=asyncio.FIRST_COMPLETED
    )

    for tâche in terminée:
        print(f"Première terminée: {tâche.result()}")

    for tâche in en_attente:
        tâche.cancel()

asyncio.run(main())
```

## Création et chaînage de coroutines

### Coroutines simples

Voici comment créer et exécuter des coroutines simples:

```python
async def coroutine_simple():
    print("Début")
    await asyncio.sleep(1)
    print("Après 1 seconde")
    await asyncio.sleep(1)
    print("Après 2 secondes")
    return "Terminé"

async def main():
    résultat = await coroutine_simple()
    print(f"Résultat: {résultat}")

asyncio.run(main())
```

### Composition de coroutines

Les coroutines peuvent être composées pour créer des fonctionnalités plus complexes:

```python
async def fetch_data(identifiant):
    print(f"Récupération des données pour ID: {identifiant}")
    await asyncio.sleep(1)  # Simulation d'une requête API
    return f"Données-{identifiant}"

async def process_data(données):
    print(f"Traitement de: {données}")
    await asyncio.sleep(0.5)  # Simulation de traitement
    return f"Traité-{données}"

async def save_data(données_traitées):
    print(f"Sauvegarde de: {données_traitées}")
    await asyncio.sleep(0.5)  # Simulation de sauvegarde
    return f"Sauvegardé-{données_traitées}"

async def pipeline(identifiant):
    # Chaînage de coroutines
    données = await fetch_data(identifiant)
    données_traitées = await process_data(données)
    résultat = await save_data(données_traitées)
    return résultat

async def main():
    résultat = await pipeline(42)
    print(f"Pipeline terminé: {résultat}")

asyncio.run(main())
```

### Exécution parallèle avec `gather`

`asyncio.gather()` permet d'exécuter plusieurs coroutines en parallèle et d'attendre tous leurs résultats:

```python
async def tâche(nom, délai):
    print(f"Tâche {nom} démarre")
    await asyncio.sleep(délai)
    print(f"Tâche {nom} terminée")
    return f"Résultat-{nom}"

async def main():
    # Exécution de plusieurs tâches en parallèle
    résultats = await asyncio.gather(
        tâche("A", 3),
        tâche("B", 1),
        tâche("C", 2),
        tâche("D", 4)
    )

    print(f"Tous les résultats: {résultats}")

    # Gestion des erreurs
    try:
        await asyncio.gather(
            tâche("X", 1),
            tâche("Y", 2),
            tâche("Z", 0.5),
            raise_error(),  # Cette tâche lèvera une exception
            return_exceptions=True  # Avec cette option, les exceptions sont retournées plutôt que propagées
        )
    except Exception as e:
        print(f"Exception capturée: {e}")

async def raise_error():
    await asyncio.sleep(0.1)
    raise ValueError("Erreur intentionnelle")

asyncio.run(main())
```

### Exécution de la première coroutine terminée avec `wait_for`

Parfois, vous voulez exécuter une coroutine avec un timeout ou attendre la première d'un ensemble à terminer:

```python
async def opération_avec_timeout():
    try:
        # Attend la coroutine, mais pas plus de 1 seconde
        résultat = await asyncio.wait_for(tâche_longue(), timeout=1.0)
        print(f"Résultat: {résultat}")
    except asyncio.TimeoutError:
        print("L'opération a expiré")

async def tâche_longue():
    await asyncio.sleep(2)
    return "Ceci ne sera jamais atteint à cause du timeout"

async def première_terminée():
    # Crée plusieurs tâches
    tâches = [
        asyncio.create_task(tâche("A", 3)),
        asyncio.create_task(tâche("B", 1)),
        asyncio.create_task(tâche("C", 2))
    ]

    # Attend la première tâche à terminer
    terminée, en_attente = await asyncio.wait(
        tâches,
        return_when=asyncio.FIRST_COMPLETED
    )

    première_tâche = list(terminée)[0]
    print(f"Première tâche terminée: {première_tâche.result()}")

    # Optionnel: annuler les tâches restantes
    for t in en_attente:
        t.cancel()

async def main():
    await opération_avec_timeout()
    await première_terminée()

asyncio.run(main())
```

## Gestion des erreurs dans les coroutines

### Try/except avec await

La gestion des erreurs dans les coroutines est similaire à celle du code synchrone:

```python
async def fonction_avec_erreur():
    await asyncio.sleep(0.5)
    raise ValueError("Une erreur s'est produite")
    return "Ce code ne sera jamais atteint"

async def gestion_erreurs():
    try:
        résultat = await fonction_avec_erreur()
        print(f"Résultat: {résultat}")
    except ValueError as e:
        print(f"Erreur capturée: {e}")
    finally:
        print("Le bloc finally est toujours exécuté")

asyncio.run(gestion_erreurs())
```

### Propagation des exceptions

Les exceptions non gérées dans une coroutine sont propagées à l'appelant:

```python
async def niveau_3():
    await asyncio.sleep(0.1)
    raise RuntimeError("Erreur au niveau 3")

async def niveau_2():
    await niveau_3()  # L'erreur se propage

async def niveau_1():
    try:
        await niveau_2()
    except RuntimeError as e:
        print(f"Erreur capturée au niveau 1: {e}")

asyncio.run(niveau_1())
```

### Annulation de tâches

Les tâches peuvent être annulées, ce qui lève une `asyncio.CancelledError` dans la coroutine:

```python
async def opération_longue():
    try:
        print("Opération longue démarre")
        while True:
            print("En cours...")
            await asyncio.sleep(0.5)
    except asyncio.CancelledError:
        print("L'opération a été annulée")
        raise  # Re-lever l'exception pour signaler l'annulation complète

async def main():
    # Démarrage de la tâche
    tâche = asyncio.create_task(opération_longue())

    # Laisse-la s'exécuter un moment
    await asyncio.sleep(2)

    # Annule la tâche
    tâche.cancel()

    try:
        await tâche
    except asyncio.CancelledError:
        print("Tâche confirmée comme annulée")

asyncio.run(main())
```

## E/S asynchrones

### Opérations réseau

Les opérations réseau sont l'un des cas d'utilisation les plus courants pour asyncio:

```python
import asyncio
import aiohttp  # Bibliothèque HTTP asynchrone

async def fetch_url(session, url):
    async with session.get(url) as response:
        return await response.text()

async def fetch_all(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [asyncio.create_task(fetch_url(session, url)) for url in urls]
        results = await asyncio.gather(*tasks)
        return results

async def main():
    urls = [
        "https://example.com",
        "https://python.org",
        "https://github.com"
    ]
    results = await fetch_all(urls)
    for url, html in zip(urls, results):
        print(f"{url}: {len(html)} caractères")

asyncio.run(main())
```

### Opérations sur les fichiers

Les opérations sur les fichiers peuvent également être asynchrones, bien que ce soit moins courant:

```python
import aiofiles  # Bibliothèque pour les fichiers asynchrones

async def lire_fichier(chemin):
    async with aiofiles.open(chemin, mode='r') as f:
        contenu = await f.read()
    return contenu

async def écrire_fichier(chemin, contenu):
    async with aiofiles.open(chemin, mode='w') as f:
        await f.write(contenu)

async def main():
    # Écriture asynchrone
    await écrire_fichier('test.txt', 'Contenu de test\nDeuxième ligne')

    # Lecture asynchrone
    contenu = await lire_fichier('test.txt')
    print(f"Contenu lu: {contenu}")

    # Opérations multiples en parallèle
    await asyncio.gather(
        écrire_fichier('file1.txt', 'Contenu 1'),
        écrire_fichier('file2.txt', 'Contenu 2'),
        écrire_fichier('file3.txt', 'Contenu 3')
    )

asyncio.run(main())
```

### Opérations de temporisation

Asyncio permet de temporiser des opérations de manière non bloquante:

```python
async def délai(durée, nom):
    print(f"Tâche {nom} attend {durée} secondes")
    await asyncio.sleep(durée)
    print(f"Tâche {nom} terminée")
    return f"Résultat de {nom}"

async def main():
    # Temporisations séquentielles
    await délai(1, "A")
    await délai(0.5, "B")

    # Temporisations parallèles
    résultats = await asyncio.gather(
        délai(3, "X"),
        délai(1, "Y"),
        délai(2, "Z")
    )
    print(f"Résultats: {résultats}")

    # Avec timeout
    try:
        await asyncio.wait_for(délai(5, "Longue"), timeout=2)
    except asyncio.TimeoutError:
        print("La tâche longue a expiré")

asyncio.run(main())
```

## Synchronisation entre coroutines

### Locks

Les locks permettent de protéger les ressources partagées:

```python
async def worker(lock, nombre):
    print(f"Worker {nombre} attend le lock")
    async with lock:
        print(f"Worker {nombre} a obtenu le lock")
        await asyncio.sleep(1)  # Travail simulé avec lock
        print(f"Worker {nombre} libère le lock")

async def main():
    # Création d'un lock
    lock = asyncio.Lock()

    # Exécution de plusieurs workers concurrents
    await asyncio.gather(
        worker(lock, 1),
        worker(lock, 2),
        worker(lock, 3)
    )

asyncio.run(main())
```

### Events

Les events permettent de signaler des conditions entre coroutines:

```python
async def attendre_événement(event, nom):
    print(f"{nom} attend l'événement")
    await event.wait()
    print(f"{nom} a reçu l'événement et continue")

async def déclencher_événement(event, délai):
    await asyncio.sleep(délai)
    print("Déclenchement de l'événement")
    event.set()

async def main():
    # Création d'un événement
    event = asyncio.Event()

    # Création de tâches d'attente
    tâches_attente = [asyncio.create_task(attendre_événement(event, f"Tâche-{i}"))
                      for i in range(5)]

    # Déclenchement de l'événement après un délai
    await déclencher_événement(event, 2)

    # Attente de toutes les tâches
    await asyncio.gather(*tâches_attente)

asyncio.run(main())
```

### Semaphores

Les sémaphores limitent le nombre d'accès concurrents à une ressource:

```python
async def worker_semaphore(semaphore, nombre):
    async with semaphore:
        print(f"Worker {nombre} a acquis le sémaphore")
        await asyncio.sleep(1)  # Simuler un travail
        print(f"Worker {nombre} libère le sémaphore")

async def main():
    # Création d'un sémaphore qui permet 3 accès concurrents
    semaphore = asyncio.Semaphore(3)

    # Création de 10 tâches qui veulent accéder à la ressource
    tâches = [asyncio.create_task(worker_semaphore(semaphore, i))
             for i in range(10)]

    # Attente de toutes les tâches
    await asyncio.gather(*tâches)

asyncio.run(main())
```

### Queues

Les queues permettent d'échanger des données entre coroutines:

```python
async def producteur(queue):
    for i in range(5):
        item = f"Item-{i}"
        await queue.put(item)
        print(f"Produit: {item}")
        await asyncio.sleep(0.5)

    # Signal de fin
    await queue.put(None)

async def consommateur(queue, nom):
    while True:
        item = await queue.get()
        if item is None:
            # Remettre le signal de fin pour les autres consommateurs
            await queue.put(None)
            break

        print(f"Consommateur {nom} a reçu: {item}")
        await asyncio.sleep(1)  # Traitement simulé

        # Marquer la tâche comme traitée
        queue.task_done()

async def main():
    # Création d'une queue
    queue = asyncio.Queue()

    # Création de tâches
    tâches = [
        asyncio.create_task(producteur(queue)),
        asyncio.create_task(consommateur(queue, "A")),
        asyncio.create_task(consommateur(queue, "B"))
    ]

    # Attente de toutes les tâches
    await asyncio.gather(*tâches)

asyncio.run(main())
```

## Streams

### StreamReader et StreamWriter

Les streams offrent une interface de bas niveau pour la communication réseau:

```python
async def echo_server():
    server = await asyncio.start_server(
        handle_echo, '127.0.0.1', 8888)

    addr = server.sockets[0].getsockname()
    print(f'Serveur démarré sur {addr}')

    async with server:
        await server.serve_forever()

async def handle_echo(reader, writer):
    data = await reader.read(100)
    message = data.decode()
    addr = writer.get_extra_info('peername')

    print(f"Reçu {message!r} de {addr!r}")

    print(f"Envoi: {message!r}")
    writer.write(data)
    await writer.drain()

    print("Fermeture de la connexion")
    writer.close()

async def echo_client():
    reader, writer = await asyncio.open_connection(
        '127.0.0.1', 8888)

    message = 'Hello World!'
    print(f'Envoi: {message!r}')
    writer.write(message.encode())
    await writer.drain()

    data = await reader.read(100)
    print(f'Reçu: {data.decode()!r}')

    print('Fermeture de la connexion')
    writer.close()
    await writer.wait_closed()

# Pour exécuter le serveur et le client, vous devriez les lancer dans des processus séparés
# ou utiliser asyncio.gather() pour lancer le client après que le serveur soit prêt
```

### Serveurs et clients asynchrones

Voici un exemple plus complet de serveur et client asynchrones:

```python
async def handle_client(reader, writer):
    # Adresse du client
    addr = writer.get_extra_info('peername')
    print(f"Connexion de {addr}")

    try:
        while True:
            # Lecture de la ligne envoyée par le client
            data = await reader.readline()
            message = data.decode().strip()

            # Si le client se déconnecte ou envoie 'exit'
            if not data or message.lower() == 'exit':
                break

            print(f"Reçu de {addr}: {message}")

            # Réponse au client
            response = f"Echo: {message}\n"
            writer.write(response.encode())
            await writer.drain()
    except Exception as e:
        print(f"Erreur avec client {addr}: {str(e)}")
    finally:
        # Fermeture de la connexion
        print(f"Fermeture de la connexion avec {addr}")
        writer.close()
        await writer.wait_closed()

async def start_server():
    server = await asyncio.start_server(
        handle_client, '127.0.0.1', 8888)

    addr = server.sockets[0].getsockname()
    print(f'Serveur démarré sur {addr}')

    async with server:
        await server.serve_forever()

async def client_session():
    reader, writer = await asyncio.open_connection(
        '127.0.0.1', 8888)

    try:
        # Envoi de quelques messages
        for message in ['Hello', 'Comment ça va?', 'exit']:
            print(f"Envoi: {message}")
            writer.write(f"{message}\n".encode())
            await writer.drain()

            if message.lower() == 'exit':
                break

            # Lecture de la réponse
            data = await reader.readline()
            print(f"Reçu: {data.decode()}")

            # Pause
            await asyncio.sleep(1)
    finally:
        # Fermeture de la connexion
        print("Fermeture de la connexion client")
        writer.close()
        await writer.wait_closed()

# Dans un environnement réel, vous lanceriez serveur et client séparément
```

## Subprocessus asynchrones

### Exécution de commandes externes

asyncio permet d'exécuter et d'interagir avec des processus externes de manière asynchrone:

```python
async def run_command(cmd):
    print(f"Exécution de: {cmd}")

    # Création du processus
    process = await asyncio.create_subprocess_shell(
        cmd,
        stdout=asyncio.subprocess.PIPE,
        stderr=asyncio.subprocess.PIPE
    )

    # Attente de la fin et récupération de la sortie
    stdout, stderr = await process.communicate()

    # Décodage et affichage des résultats
    stdout_str = stdout.decode().strip()
    stderr_str = stderr.decode().strip()

    if stdout_str:
        print(f"Sortie standard:\n{stdout_str}")
    if stderr_str:
        print(f"Erreur standard:\n{stderr_str}")

    print(f"Commande terminée avec code: {process.returncode}")
    return stdout_str, stderr_str, process.returncode

async def run_multiple_commands():
    # Exécution séquentielle
    await run_command("ls -la")
    await run_command("echo 'Hello from subprocess'")

    # Exécution parallèle
    results = await asyncio.gather(
        run_command("sleep 2 && echo 'Command 1 done'"),
        run_command("sleep 1 && echo 'Command 2 done'"),
        run_command("sleep 3 && echo 'Command 3 done'")
    )

    print(f"Tous les résultats: {results}")

asyncio.run(run_multiple_commands())
```

### Communication avec les subprocessus

Vous pouvez également interagir de manière plus avancée avec les subprocessus:

```python
async def interactive_process():
    # Lancement d'un processus interactif (par ex. Python)
    process = await asyncio.create_subprocess_exec(
        "python", "-i",
        stdin=asyncio.subprocess.PIPE,
        stdout=asyncio.subprocess.PIPE,
        stderr=asyncio.subprocess.PIPE
    )

    # Envoi de commandes
    commands = [
        b"print('Hello from interactive Python')\n",
        b"import sys\n",
        b"print(sys.version)\n",
        b"exit()\n"
    ]

    for cmd in commands:
        process.stdin.write(cmd)
        await process.stdin.drain()

        # Laisser le temps au processus de répondre
        await asyncio.sleep(0.1)

    # Attente de la fin
    stdout, stderr = await process.communicate()

    print(f"Sortie:\n{stdout.decode()}")
    if stderr:
        print(f"Erreurs:\n{stderr.decode()}")

asyncio.run(interactive_process())
```

## Patterns avancés

### Producer-Consumer

Le pattern producteur-consommateur est couramment utilisé dans les applications asynchrones:

```python
async def produire(queue, n_items):
    for i in range(n_items):
        # Production d'un élément
        item = f"Item-{i}"
        await queue.put(item)
        print(f"Produit: {item}")

        # Simulation d'un temps de production variable
        await asyncio.sleep(random.uniform(0.1, 0.5))

    # Signaux de fin pour tous les consommateurs
    for _ in range(n_consommateurs):
        await queue.put(None)

    print("Producteur: terminé")

async def consommer(queue, nom):
    while True:
        # Attente d'un élément
        item = await queue.get()

        # Vérification du signal de fin
        if item is None:
            print(f"Consommateur {nom}: terminé")
            queue.task_done()
            break

        # Traitement de l'élément
        print(f"Consommateur {nom} traite: {item}")

        # Simulation du temps de traitement
        await asyncio.sleep(random.uniform(0.2, 1.0))

        # Marquage de la tâche comme terminée
        queue.task_done()

async def main():
    import random

    # Paramètres
    n_items = 20
    n_consommateurs = 3

    # Création de la queue
    queue = asyncio.Queue()

    # Création des tâches
    tâches = []

    # Producteur
    tâches.append(asyncio.create_task(produire(queue, n_items)))

    # Consommateurs
    for i in range(n_consommateurs):
        tâches.append(asyncio.create_task(consommer(queue, f"C-{i}")))

    # Attente de la fin de toutes les tâches
    await asyncio.gather(*tâches)

    # Attente que la queue soit complètement traitée
    await queue.join()

    print("Toutes les tâches sont terminées")

asyncio.run(main())
```

### Fan-out/Fan-in

Ce pattern permet de distribuer le travail à plusieurs workers puis de regrouper les résultats:

```python
async def worker(nom, queue_in, queue_out):
    while True:
        # Récupération d'une tâche
        task = await queue_in.get()

        # Vérification du signal de fin
        if task is None:
            queue_in.task_done()
            break

        task_id, data = task

        # Traitement de la tâche
        print(f"Worker {nom} traite la tâche {task_id}")

        # Simulation de traitement
        await asyncio.sleep(random.uniform(0.5, 2))

        # Calcul du résultat
        result = f"Résultat-{task_id}: {data.upper()}"

        # Envoi du résultat
        await queue_out.put((task_id, result))

        # Marquage de la tâche comme terminée
        queue_in.task_done()

    print(f"Worker {nom}: terminé")

async def distributor(tâches, queue_in, n_workers):
    # Distribution des tâches
    for i, data in enumerate(tâches):
        await queue_in.put((i, data))

    # Signaux de fin pour tous les workers
    for _ in range(n_workers):
        await queue_in.put(None)

    print("Distributeur: terminé")

async def collector(queue_out, n_results):
    results = []

    # Collecte des résultats
    for _ in range(n_results):
        task_id, result = await queue_out.get()
        print(f"Collecteur reçoit: {result}")
        results.append((task_id, result))
        queue_out.task_done()

    # Tri des résultats par ID de tâche
    results.sort(key=lambda x: x[0])

    print("Collecteur: terminé")
    return results

async def main():
    import random

    # Données à traiter
    tâches = [
        "tâche a", "tâche b", "tâche c", "tâche d",
        "tâche e", "tâche f", "tâche g", "tâche h"
    ]

    # Nombre de workers
    n_workers = 3

    # Queues pour la communication
    queue_in = asyncio.Queue()
    queue_out = asyncio.Queue()

    # Création des tâches
    tasks = []

    # Ajout du distributeur
    tasks.append(asyncio.create_task(
        distributor(tâches, queue_in, n_workers)))

    # Ajout des workers
    for i in range(n_workers):
        tasks.append(asyncio.create_task(
            worker(f"W-{i}", queue_in, queue_out)))

    # Ajout du collecteur
    collector_task = asyncio.create_task(
        collector(queue_out, len(tâches)))

    # Attente de toutes les tâches de distribution et de travail
    await asyncio.gather(*tasks)

    # Attente que toutes les queues soient vides
    await queue_in.join()
    await queue_out.join()

    # Obtention des résultats du collecteur
    results = await collector_task

    print("\nRésultats finaux:")
    for task_id, result in results:
        print(f"  {result}")

asyncio.run(main())
```

### Timeouts et gestion d'erreurs avancée

Un pattern plus robuste pour les opérations avec timeouts et gestion d'erreurs:

```python
async def opération_réseau(url):
    """Simulation d'une opération réseau qui peut échouer ou prendre du temps."""
    delay = random.uniform(0.5, 5)

    # Simulation d'erreurs aléatoires
    if random.random() < 0.3:
        await asyncio.sleep(delay / 2)
        raise ConnectionError(f"Erreur de connexion pour {url}")

    await asyncio.sleep(delay)
    return f"Résultat de {url}: {delay:.2f}s"

async def opération_avec_timeout(url, timeout=2.0, retries=3):
    """Effectue une opération avec timeout et tentatives de réessai."""
    for attempt in range(1, retries + 1):
        try:
            # Tentative d'opération avec timeout
            return await asyncio.wait_for(
                opération_réseau(url),
                timeout=timeout
            )
        except asyncio.TimeoutError:
            if attempt < retries:
                backoff = 0.1 * (2 ** (attempt - 1))  # Backoff exponentiel
                print(f"Timeout pour {url}, réessai {attempt}/{retries} après {backoff:.2f}s")
                await asyncio.sleep(backoff)
            else:
                print(f"Timeout final pour {url} après {retries} tentatives")
                raise
        except ConnectionError as e:
            if attempt < retries:
                backoff = 0.2 * (2 ** (attempt - 1))
                print(f"Erreur pour {url}: {e}, réessai {attempt}/{retries} après {backoff:.2f}s")
                await asyncio.sleep(backoff)
            else:
                print(f"Échec final pour {url} après {retries} tentatives: {e}")
                raise

async def opération_robuste(urls, timeout=2.0, retries=3):
    """Effectue des opérations en parallèle avec gestion des erreurs."""
    # Création des tâches
    tasks = []
    for url in urls:
        task = asyncio.create_task(opération_avec_timeout(url, timeout, retries))
        tasks.append((url, task))

    # Attente des résultats avec gestion des erreurs
    results = {"succès": [], "échecs": []}

    for url, task in tasks:
        try:
            result = await task
            results["succès"].append((url, result))
        except (asyncio.TimeoutError, ConnectionError) as e:
            results["échecs"].append((url, str(e)))

    return results

async def main():
    import random

    urls = [
        "https://api.example.com/1",
        "https://api.example.com/2",
        "https://api.example.com/3",
        "https://api.example.com/4",
        "https://api.example.com/5",
    ]

    results = await opération_robuste(urls, timeout=2.0, retries=2)

    print("\nRésultats finaux:")
    print(f"Succès: {len(results['succès'])}")
    for url, result in results["succès"]:
        print(f"  - {url}: {result}")

    print(f"Échecs: {len(results['échecs'])}")
    for url, error in results["échecs"]:
        print(f"  - {url}: {error}")

asyncio.run(main())
```

## Asyncio et autres bibliothèques

### aiohttp pour les requêtes HTTP

`aiohttp` est une bibliothèque populaire pour les requêtes HTTP asynchrones:

```python
import aiohttp
import asyncio

async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()

async def main():
    async with aiohttp.ClientSession() as session:
        html = await fetch(session, 'https://python.org')
        print(f"Longueur de la page: {len(html)} caractères")

        # Requêtes parallèles
        urls = [
            'https://python.org',
            'https://github.com',
            'https://stackoverflow.com'
        ]

        tasks = [asyncio.create_task(fetch(session, url)) for url in urls]
        pages = await asyncio.gather(*tasks)

        for url, page in zip(urls, pages):
            print(f"{url}: {len(page)} caractères")

# asyncio.run(main())
```

### asyncpg pour PostgreSQL

`asyncpg` est une bibliothèque pour la communication asynchrone avec PostgreSQL:

```python
import asyncpg
import asyncio

async def execute_query():
    # Connexion à la base de données
    conn = await asyncpg.connect(
        user='username',
        password='password',
        database='database_name',
        host='localhost'
    )

    try:
        # Création d'une table
        await conn.execute('''
            CREATE TABLE IF NOT EXISTS users(
                id serial PRIMARY KEY,
                name text,
                email text
            )
        ''')

        # Insertion de données
        user_id = await conn.fetchval(
            'INSERT INTO users(name, email) VALUES($1, $2) RETURNING id',
            'John Doe',
            'john@example.com'
        )

        print(f"Utilisateur inséré avec ID: {user_id}")

        # Requête multiple
        utilisateurs = [
            ('Alice', 'alice@example.com'),
            ('Bob', 'bob@example.com'),
            ('Charlie', 'charlie@example.com')
        ]

        # Préparation et exécution par lots
        await conn.executemany(
            'INSERT INTO users(name, email) VALUES($1, $2)',
            utilisateurs
        )

        # Récupération des données
        rows = await conn.fetch('SELECT * FROM users')

        for row in rows:
            print(f"User: {row['name']}, Email: {row['email']}")

    finally:
        # Fermeture de la connexion
        await conn.close()

# asyncio.run(execute_query())
```

### aiomysql pour MySQL

`aiomysql` est une bibliothèque pour la communication asynchrone avec MySQL:

```python
import aiomysql
import asyncio

async def mysql_example():
    # Création d'un pool de connexions
    pool = await aiomysql.create_pool(
        host='127.0.0.1',
        port=3306,
        user='username',
        password='password',
        db='database',
        autocommit=True
    )

    async with pool.acquire() as conn:
        async with conn.cursor(aiomysql.DictCursor) as cursor:
            # Création d'une table
            await cursor.execute("""
                CREATE TABLE IF NOT EXISTS products (
                    id INT PRIMARY KEY AUTO_INCREMENT,
                    name VARCHAR(255),
                    price DECIMAL(10, 2)
                )
            """)

            # Insertion de données
            await cursor.execute(
                "INSERT INTO products (name, price) VALUES (%s, %s)",
                ('Laptop', 999.99)
            )

            # Insertion multiple
            produits = [
                ('Smartphone', 499.99),
                ('Tablette', 299.99),
                ('Casque', 149.99)
            ]

            await cursor.executemany(
                "INSERT INTO products (name, price) VALUES (%s, %s)",
                produits
            )

            # Requête
            await cursor.execute("SELECT * FROM products")
            produits = await cursor.fetchall()

            for produit in produits:
                print(f"ID: {produit['id']}, Nom: {produit['name']}, Prix: {produit['price']}")

    # Fermeture du pool
    pool.close()
    await pool.wait_closed()

# asyncio.run(mysql_example())
```

### aioredis pour Redis

`aioredis` est une bibliothèque pour la communication asynchrone avec Redis:

```python
import aioredis
import asyncio

async def redis_example():
    # Connexion à Redis
    redis = await aioredis.create_redis_pool('redis://localhost')

    try:
        # Opérations basiques
        await redis.set('key', 'value')
        value = await redis.get('key')
        print(f"Valeur récupérée: {value.decode()}")

        # Opérations multiples
        await redis.mset(key1='value1', key2='value2', key3='value3')
        values = await redis.mget('key1', 'key2', 'key3')

        for i, value in enumerate(values, 1):
            print(f"key{i}: {value.decode()}")

        # Incrémentation
        await redis.set('compteur', '0')
        for _ in range(5):
            counter = await redis.incr('compteur')
            print(f"Compteur: {counter}")

        # Liste
        await redis.delete('ma_liste')
        await redis.lpush('ma_liste', 'élément1', 'élément2', 'élément3')

        # Récupération de la liste
        liste = await redis.lrange('ma_liste', 0, -1)
        print("Liste:", [item.decode() for item in liste])

        # Publier/s'abonner
        receiver = redis.pubsub()
        await receiver.subscribe('channel')

        task = asyncio.create_task(handle_messages(receiver))

        # Publier quelques messages
        for i in range(3):
            await redis.publish('channel', f'Message {i}')
            await asyncio.sleep(0.1)

        await asyncio.sleep(0.5)  # Laisser le temps de recevoir les messages
        task.cancel()

    finally:
        # Fermeture de la connexion
        redis.close()
        await redis.wait_closed()

async def handle_messages(receiver):
    try:
        async for message in receiver.iter():
            if message['type'] == 'message':
                print(f"Message reçu: {message['data'].decode()}")
    except asyncio.CancelledError:
        pass

# asyncio.run(redis_example())
```

### Autres bibliothèques notables

- **aiobotocore**: Client AWS asynchrone
- **asyncssh**: Client et serveur SSH asynchrones
- **aiosqlite**: Bibliothèque SQLite asynchrone
- **motor**: Driver MongoDB asynchrone
- **aiosmtplib**: Client SMTP asynchrone
- **aiofiles**: Bibliothèque de gestion de fichiers asynchrone
- **uvloop**: Implémentation alternative de boucle d'événements, plus rapide

## Débogage de code asynchrone

### Comprendre les traces d'exécution

Les traces d'exécution dans le code asynchrone peuvent être complexes. Voici comment les interpréter:

```python
import asyncio
import traceback

async def niveau_3():
    await asyncio.sleep(0.1)
    raise ValueError("Erreur dans niveau_3")

async def niveau_2():
    await niveau_3()

async def niveau_1():
    try:
        await niveau_2()
    except Exception as e:
        print(f"Exception capturée: {e}")
        print("\nTrace d'exécution:")
        traceback.print_exc()

async def main():
    await niveau_1()

    # Autre exemple avec stack complète
    try:
        await niveau_2()
    except Exception as e:
        print("\nException non capturée:")
        traceback.print_exc()

# asyncio.run(main())
```

### Utilisation de `asyncio.debug`

asyncio offre des fonctionnalités de débogage intégrées:

```python
import asyncio
import sys

async def fonction_avec_oubli():
    # Création d'une tâche mais oubli de l'attendre
    asyncio.create_task(asyncio.sleep(1))

    # Cette fonction se termine mais la tâche reste en suspens

async def main():
    # Activation du mode debug
    asyncio.get_event_loop().set_debug(True)

    # Affichage des coroutines non attendues
    await fonction_avec_oubli()
    await asyncio.sleep(0.1)  # Donner du temps pour afficher les avertissements

# Mode debug via variable d'environnement
# PYTHONASYNCIEDEBUG=1 python script.py

# Mode debug via code
# asyncio.run(main())
```

### Logging dans les coroutines

Le logging est crucial pour comprendre l'exécution des coroutines:

```python
import asyncio
import logging
import random
import time

# Configuration du logging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

logger = logging.getLogger("asyncio_demo")

async def tâche_avec_logging(nom, délai):
    logger.info(f"Tâche {nom} démarre")
    try:
        début = time.time()
        logger.debug(f"Tâche {nom} attend pendant {délai:.2f}s")

        await asyncio.sleep(délai)

        # Parfois générer une erreur
        if random.random() < 0.3:
            raise ValueError(f"Erreur simulée dans la tâche {nom}")

        temps_total = time.time() - début
        logger.info(f"Tâche {nom} terminée en {temps_total:.2f}s")
        return f"Résultat de {nom}"

    except Exception as e:
        logger.error(f"Tâche {nom} a échoué: {str(e)}", exc_info=True)
        raise

    finally:
        logger.debug(f"Nettoyage pour la tâche {nom}")

async def main():
    tâches = []

    # Création de tâches avec logging
    for i in range(5):
        délai = random.uniform(0.5, 2)
        tâche = asyncio.create_task(
            tâche_avec_logging(f"T{i}", délai)
        )
        tâches.append(tâche)

    # Attente des résultats avec gestion des erreurs
    résultats = []

    for tâche in tâches:
        try:
            résultat = await tâche
            résultats.append(résultat)
        except Exception as e:
            logger.warning(f"Gestion d'erreur dans main: {str(e)}")

    logger.info(f"Résultats finaux: {résultats}")

# asyncio.run(main())
```

## Bonnes pratiques

1. **Ne bloquez jamais la boucle d'événements**

   Évitez les opérations bloquantes dans les coroutines. Si vous avez besoin d'exécuter une opération CPU-intensive, utilisez `loop.run_in_executor()` ou le module `concurrent.futures`.

   ```python
   async def tâche_cpu_intensive():
       loop = asyncio.get_running_loop()

       # Exécution dans un thread séparé
       résultat = await loop.run_in_executor(
           None,  # Utilise le ThreadPoolExecutor par défaut
           fonction_bloquante,  # Fonction synchrone intensive en CPU
           arg1, arg2  # Arguments de la fonction
       )

       return résultat
   ```

2. **Utilisez `asyncio.create_task()` pour la concurrence**

   Créez des tâches pour exécuter des coroutines en parallèle et ainsi maximiser l'efficacité.

   ```python
   async def main():
       # Création de tâches pour une exécution concurrente
       tâche1 = asyncio.create_task(coroutine_1())
       tâche2 = asyncio.create_task(coroutine_2())

       # Attendre toutes les tâches
       await tâche1
       await tâche2
   ```

3. **Préférez `asyncio.gather()` pour attendre plusieurs coroutines**

   `gather()` est plus concis et efficace pour attendre plusieurs coroutines.

   ```python
   async def main():
       résultats = await asyncio.gather(
           coroutine_1(),
           coroutine_2(),
           coroutine_3()
       )

       # résultats est une liste ordonnée des valeurs retournées
   ```

4. **Gérez toujours les exceptions**

   Les exceptions non gérées peuvent causer des fuites de ressources et des comportements inattendus.

   ```python
   async def fonction_avec_gestion_erreurs():
       try:
           await opération_risquée()
       except Exception as e:
           logging.error(f"Erreur: {str(e)}")
           # Nettoyage approprié
   ```

5. **Utilisez des timeouts pour les opérations externes**

   Évitez qu'une coroutine reste bloquée indéfiniment.

   ```python
   async def opération_avec_timeout():
       try:
           return await asyncio.wait_for(
               opération_externe(),
               timeout=5.0  # Timeout de 5 secondes
           )
       except asyncio.TimeoutError:
           # Gestion du timeout
           return valeur_par_défaut
   ```

6. **Préférez les gestionnaires de contexte asynchrones**

   Utilisez `async with` pour garantir le nettoyage correct des ressources.

   ```python
   async def exemple_async_with():
       async with aiohttp.ClientSession() as session:
           async with session.get(url) as response:
               return await response.text()
   ```

7. **Utilisez `asyncio.shield()` pour protéger les opérations critiques**

   Protégez les opérations importantes contre l'annulation involontaire.

   ```python
   async def opération_critique():
       try:
           # Protection contre l'annulation
           résultat = await asyncio.shield(
               opération_importante()
           )
           return résultat
       except asyncio.CancelledError:
           # Nettoyage sécurisé avant propagation
           await nettoyage_sécurisé()
           raise
   ```

8. **Ne mélangez pas `async/await` avec d'autres mécanismes asynchrones**

   Évitez de mélanger `asyncio` avec des callback traditionnels ou d'autres bibliothèques asynchrones incompatibles.

9. **Utilisez des primitives de synchronisation pour coordonner les coroutines**

   Utilisez `Lock`, `Event`, `Semaphore` et `Queue` pour la coordination.

10. **Testez votre code asynchrone**

    Utilisez des outils comme `pytest-asyncio` pour tester correctement le code asynchrone.

## Pièges courants

1. **Oublier d'attendre une coroutine**

   ```python
   async def fonction_incorrecte():
       asyncio.sleep(1)  # Erreur: manque await

   async def fonction_correcte():
       await asyncio.sleep(1)  # Correct
   ```

2. **Oublier d'attendre les tâches créées**

   ```python
   async def fuite_mémoire():
       # Création d'une tâche sans l'attendre
       asyncio.create_task(coroutine_longue())
       # La tâche existe toujours mais n'est jamais attendue

   async def correct():
       tâche = asyncio.create_task(coroutine_longue())
       # ... plus tard
       await tâche  # Bien attendre la tâche
   ```

3. **Bloquer la boucle d'événements**

   ```python
   async def fonction_bloquante():
       # Bloque la boucle d'événements
       time.sleep(5)  # Utilise sleep synchrone

   async def fonction_correcte():
       # Non bloquant
       await asyncio.sleep(5)
   ```

4. **Ignorer les exceptions dans les tâches**

   ```python
   async def erreur_silencieuse():
       # La tâche échouera silencieusement
       asyncio.create_task(coroutine_avec_erreur())

   async def gestion_correcte():
       tâche = asyncio.create_task(coroutine_avec_erreur())
       try:
           await tâche
       except Exception as e:
           print(f"Erreur gérée: {e}")
   ```

5. **Utiliser des API synchrones dans des coroutines**

   ```python
   async def mauvais_mélange():
       # Bloquant, utilise l'API synchrone
       données = requests.get("https://api.example.com").json()

   async def approche_correcte():
       async with aiohttp.ClientSession() as session:
           async with session.get("https://api.example.com") as response:
               données = await response.json()
   ```

6. **Ignorer les signaux de fermeture**

   ```python
   async def main():
       try:
           while True:
               await tâche_périodique()
       except asyncio.CancelledError:
           # Nettoyage propre
           await fermeture_propre()
           raise  # Important: repropager l'exception
   ```

7. **Utiliser `asyncio.sleep(0)` incorrectement**

   ```python
   async def boucle_infinie():
       while True:
           # Ne cède pas vraiment le contrôle comme prévu
           asyncio.sleep(0)  # Manque await

   async def correct():
       while True:
           # Cède correctement le contrôle
           await asyncio.sleep(0)
   ```

8. **Partager des ressources sans synchronisation**

   ```python
   # Incorrect
   compteur = 0

   async def incrémenter():
       global compteur
       temp = compteur
       await asyncio.sleep(0.01)  # Simuler un travail
       compteur = temp + 1

   # Correct
   compteur = 0
   lock = asyncio.Lock()

   async def incrémenter_sécurisé():
       global compteur
       async with lock:
           temp = compteur
           await asyncio.sleep(0.01)  # Simuler un travail
           compteur = temp + 1
   ```

9. **Utiliser `return await` redondant**

   ```python
   # Légèrement redondant
   async def fonction_redondante():
       return await autre_coroutine()

   # Plus concis quand c'est la dernière instruction
   async def fonction_concise():
       await autre_coroutine()
   ```

10. **Ne pas gérer correctement `CancelledError`**

    ```python
    async def sensible_à_cancellation():
        try:
            await opération_longue()
        except asyncio.CancelledError:
            # Nettoyage crucial
            await libérer_ressources()
            raise  # Important: repropager l'exception
    ```

## Ressources supplémentaires

- [Documentation officielle de asyncio](https://docs.python.org/fr/3/library/asyncio.html)
- [PEP 492 - Coroutines avec async/await](https://peps.python.org/pep-0492/)
- [Guide asyncio pour Python 3.7+](https://docs.python.org/3/library/asyncio-task.html)
- [Real Python - Asynchronous Programming in Python](https://realpython.com/async-io-python/)
- [Python Cookbook par David Beazley - Chapitre sur Concurrence](https://www.oreilly.com/library/view/python-cookbook-3rd/9781449357337/)
- [Bibliothèque aiohttp](https://docs.aiohttp.org/)
- [Bibliothèque asyncpg](https://magicstack.github.io/asyncpg/current/)
- [Bibliothèque aiomysql](https://aiomysql.readthedocs.io/)
- [Bibliothèque aioredis](https://aioredis.readthedocs.io/)
- [Bibliothèque uvloop](https://github.com/MagicStack/uvloop)

---

Ce chapitre vous a présenté les concepts fondamentaux et avancés des coroutines en Python. La programmation asynchrone est devenue un paradigme essentiel pour développer des applications modernes hautement concurrentes et réactives. Dans le prochain chapitre, nous explorerons la concurrence en Python de manière plus large, en couvrant les threads, les processus et les techniques de synchronisation pour créer des applications performantes.
