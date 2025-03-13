# La Concurrence en Python

## Table des matières

- [Introduction](#introduction)
- [Les différents modèles de concurrence](#les-différents-modèles-de-concurrence)
  - [Processus vs Threads vs Coroutines](#processus-vs-threads-vs-coroutines)
  - [Le Global Interpreter Lock (GIL)](#le-global-interpreter-lock-gil)
  - [Quand utiliser chaque modèle](#quand-utiliser-chaque-modèle)
- [Programmation multithreading](#programmation-multithreading)
  - [Le module threading](#le-module-threading)
  - [Création et gestion des threads](#création-et-gestion-des-threads)
  - [Synchronisation des threads](#synchronisation-des-threads)
  - [Partage de données entre threads](#partage-de-données-entre-threads)
  - [Thread pools](#thread-pools)
  - [Limitations des threads en Python](#limitations-des-threads-en-python)
- [Programmation multiprocessus](#programmation-multiprocessus)
  - [Le module multiprocessing](#le-module-multiprocessing)
  - [Création et gestion des processus](#création-et-gestion-des-processus)
  - [Communication entre processus](#communication-entre-processus)
  - [Partage de données entre processus](#partage-de-données-entre-processus)
  - [Process pools](#process-pools)
  - [Considérations de performance](#considérations-de-performance)
- [Synchronisation et coordination](#synchronisation-et-coordination)
  - [Locks et RLocks](#locks-et-rlocks)
  - [Sémaphores](#sémaphores)
  - [Events](#events)
  - [Conditions](#conditions)
  - [Barrières](#barrières)
  - [Queues](#queues)
  - [Éviter les deadlocks](#éviter-les-deadlocks)
- [concurrent.futures](#concurrentfutures)
  - [ThreadPoolExecutor](#threadpoolexecutor)
  - [ProcessPoolExecutor](#processpoolexecutor)
  - [Futures et callbacks](#futures-et-callbacks)
  - [Exécution parallèle de tâches](#exécution-parallèle-de-tâches)
- [Modèles de concurrence avancés](#modèles-de-concurrence-avancés)
  - [Worker Pools](#worker-pools)
  - [Pipeline](#pipeline)
  - [Map-Reduce](#map-reduce)
  - [Producteur-Consommateur](#producteur-consommateur)
  - [Pub-Sub (Publication-Souscription)](#pub-sub-publication-souscription)
- [Débugger le code concurrent](#débugger-le-code-concurrent)
  - [Traçage et logging](#traçage-et-logging)
  - [Race conditions](#race-conditions)
  - [Profilage du code concurrent](#profilage-du-code-concurrent)
  - [Outils de debug](#outils-de-debug)
- [Comparaison des approches](#comparaison-des-approches)
  - [Threads vs Processus vs Asyncio](#threads-vs-processus-vs-asyncio)
  - [Comparaison de performance](#comparaison-de-performance)
  - [Choix du bon outil](#choix-du-bon-outil)
- [Interopérabilité](#interopérabilité)
  - [Combiner asyncio et threads](#combiner-asyncio-et-threads)
  - [Combiner asyncio et processus](#combiner-asyncio-et-processus)
  - [Combiner threads et processus](#combiner-threads-et-processus)
- [Bonnes pratiques](#bonnes-pratiques)
- [Erreurs courantes](#erreurs-courantes)
- [Ressources supplémentaires](#ressources-supplémentaires)

## Introduction

La concurrence permet à un programme d'exécuter plusieurs opérations en parallèle ou de manière entrelacée. En Python, il existe plusieurs mécanismes pour implémenter la concurrence, chacun avec ses propres forces et faiblesses. Alors que le chapitre précédent se concentrait sur les coroutines et la programmation asynchrone avec `asyncio`, ce chapitre explore d'autres mécanismes de concurrence comme les threads et les processus.

La concurrence est essentielle dans de nombreux scénarios:

- Exploitation efficace des processeurs multi-cœurs
- Gestion de multiples tâches indépendantes
- Amélioration de la réactivité des applications
- Traitement parallèle de grandes quantités de données
- Réalisation d'opérations d'entrée/sortie en parallèle

Python offre plusieurs approches pour implémenter la concurrence, chacune adaptée à des cas d'utilisation spécifiques. Ce chapitre vous guidera à travers ces différentes approches, vous montrera comment les utiliser efficacement, et vous aidera à choisir la méthode la plus adaptée à votre problème.

## Les différents modèles de concurrence

### Processus vs Threads vs Coroutines

Python propose trois principales approches pour la concurrence:

1. **Processus**: Exécution de multiples instances du programme Python, chacune avec son propre interpréteur et espace mémoire isolé.

   ```python
   # Exemple avec multiprocessing
   import multiprocessing

   def worker(num):
       """Fonction exécutée dans un processus séparé"""
       print(f'Processus {num} en cours d\'exécution')

   if __name__ == '__main__':
       processes = []
       for i in range(5):
           p = multiprocessing.Process(target=worker, args=(i,))
           processes.append(p)
           p.start()

       for p in processes:
           p.join()
   ```

2. **Threads**: Exécution de multiples flux de contrôle au sein d'un même processus, partageant le même espace mémoire.

   ```python
   # Exemple avec threading
   import threading

   def worker(num):
       """Fonction exécutée dans un thread séparé"""
       print(f'Thread {num} en cours d\'exécution')

   threads = []
   for i in range(5):
       t = threading.Thread(target=worker, args=(i,))
       threads.append(t)
       t.start()

   for t in threads:
       t.join()
   ```

3. **Coroutines**: Fonctions qui peuvent suspendre leur exécution et reprendre plus tard, permettant une concurrence coopérative au sein d'un même thread.

   ```python
   # Exemple avec asyncio (vu dans le chapitre précédent)
   import asyncio

   async def worker(num):
       """Coroutine"""
       print(f'Coroutine {num} en cours d\'exécution')
       await asyncio.sleep(1)
       print(f'Coroutine {num} terminée')

   async def main():
       await asyncio.gather(*(worker(i) for i in range(5)))

   asyncio.run(main())
   ```

### Le Global Interpreter Lock (GIL)

Le GIL est un mécanisme dans l'implémentation CPython qui empêche plusieurs threads natifs d'exécuter du code Python simultanément. Cette restriction a des implications importantes pour la concurrence en Python:

- Un seul thread peut exécuter du code Python à un moment donné
- Les threads sont efficaces pour les opérations limitées par les E/S, car le GIL est libéré pendant ces opérations
- Les threads ne permettent pas de parallélisme réel pour les tâches intensives en CPU

```python
import threading
import time

def cpu_bound_task(n):
    """Tâche qui utilise beaucoup de CPU"""
    count = 0
    for i in range(n):
        count += i
    return count

def run_sequential():
    start = time.time()
    cpu_bound_task(10**7)
    cpu_bound_task(10**7)
    end = time.time()
    print(f"Séquentiel: {end - start:.2f} secondes")

def run_threaded():
    start = time.time()
    threads = []
    for _ in range(2):
        t = threading.Thread(target=cpu_bound_task, args=(10**7,))
        threads.append(t)
        t.start()
    for t in threads:
        t.join()
    end = time.time()
    print(f"Avec threads: {end - start:.2f} secondes")

run_sequential()  # Généralement plus rapide ou équivalent
run_threaded()    # Limité par le GIL
```

### Quand utiliser chaque modèle

| Modèle     | Idéal pour                        | Moins adapté pour                                    |
| ---------- | --------------------------------- | ---------------------------------------------------- |
| Processus  | Tâches intensives en CPU          | Tâches nécessitant beaucoup de communication         |
|            | Exploitation des multi-cœurs      | Applications avec mémoire limitée                    |
|            | Isolation de sécurité             |                                                      |
| Threads    | Tâches limitées par les E/S       | Tâches intensives en CPU                             |
|            | Partage de données simple         | Code qui modifie des structures de données complexes |
|            | Applications à état partagé       |                                                      |
| Coroutines | E/S hautement concurrentes        | Tâches intensives en CPU                             |
|            | Nombreuses tâches de courte durée | Opérations bloquantes                                |
|            | Applications réseau               | API synchrones                                       |

## Programmation multithreading

### Le module threading

Le module `threading` est la principale interface pour travailler avec les threads en Python:

```python
import threading
import time

def worker(id, delay):
    """Fonction simple exécutée par un thread"""
    print(f"Thread {id} démarre")
    time.sleep(delay)  # Opération d'E/S simulée
    print(f"Thread {id} terminé après {delay} secondes")

# Création de quelques threads
threads = []
for i in range(5):
    thread = threading.Thread(target=worker, args=(i, i+1))
    threads.append(thread)
    thread.start()

# Attendre que tous les threads terminent
for thread in threads:
    thread.join()

print("Tous les threads ont terminé")
```

### Création et gestion des threads

Il existe deux façons principales de créer des threads en Python:

1. **En fournissant une fonction cible**

```python
def task():
    print("Tâche en cours d'exécution dans un thread séparé")

# Création et démarrage du thread
thread = threading.Thread(target=task)
thread.start()
```

2. **En sous-classant Thread**

```python
class MyThread(threading.Thread):
    def __init__(self, name):
        super().__init__()
        self.name = name

    def run(self):
        """Méthode exécutée quand le thread démarre"""
        print(f"Thread {self.name} en cours d'exécution")
        time.sleep(2)
        print(f"Thread {self.name} terminé")

# Création et démarrage du thread
thread = MyThread("WorkerThread")
thread.start()
```

**Gestion de base des threads:**

```python
# Création d'un thread avec un nom
thread = threading.Thread(target=worker, args=(1, 2), name="WorkerThread")

# Démarrage du thread
thread.start()

# Vérification si le thread est encore en cours d'exécution
if thread.is_alive():
    print("Le thread est toujours en cours d'exécution")

# Attente de la fin du thread
thread.join()

# Attente avec timeout (en secondes)
thread.join(timeout=5)
```

**Threads démons:**
Les threads démons sont automatiquement terminés quand tous les threads non-démons se terminent.

```python
def background_task():
    while True:
        print("Tâche de fond en cours...")
        time.sleep(1)

# Création d'un thread démon
daemon_thread = threading.Thread(target=background_task, daemon=True)
daemon_thread.start()

# Le programme principal continue
print("Programme principal en cours...")
time.sleep(5)
print("Programme principal se termine")
# Le thread démon sera automatiquement terminé ici
```

### Synchronisation des threads

Puisque les threads partagent le même espace mémoire, la synchronisation est essentielle pour éviter les race conditions et autres problèmes de concurrence.

**Thread Locks:**

```python
lock = threading.Lock()
counter = 0

def increment_counter():
    global counter
    for _ in range(100000):
        lock.acquire()
        try:
            counter += 1
        finally:
            lock.release()

# Version plus concise avec le gestionnaire de contexte
def increment_counter_with_context():
    global counter
    for _ in range(100000):
        with lock:
            counter += 1

# Création et exécution des threads
threads = []
for _ in range(10):
    thread = threading.Thread(target=increment_counter_with_context)
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

print(f"Valeur finale du compteur: {counter}")
```

**RLock (Reentrant Lock):**

```python
class Counter:
    def __init__(self):
        self.count = 0
        self.lock = threading.RLock()

    def increment(self):
        with self.lock:
            self.count += 1

    def increment_multiple(self, times):
        with self.lock:
            for _ in range(times):
                self.increment()  # Réentrance - acquiert à nouveau le même lock

counter = Counter()
# ...
```

**Sémaphores:**

```python
# Limiter l'accès à une ressource à 3 threads simultanés
semaphore = threading.Semaphore(3)

def access_resource(thread_id):
    print(f"Thread {thread_id} attend l'accès...")
    with semaphore:
        print(f"Thread {thread_id} accède à la ressource")
        time.sleep(2)  # Utilisation de la ressource
        print(f"Thread {thread_id} libère la ressource")

# Création et démarrage de plusieurs threads
threads = []
for i in range(10):
    thread = threading.Thread(target=access_resource, args=(i,))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()
```

**Events:**

```python
# Utilisation d'un Event pour signaler entre threads
event = threading.Event()

def waiter():
    print("Waiter: J'attends l'événement")
    event.wait()  # Bloque jusqu'à ce que l'événement soit défini
    print("Waiter: L'événement s'est produit!")

def setter():
    print("Setter: Je vais attendre 2 secondes puis définir l'événement")
    time.sleep(2)
    print("Setter: Je définis l'événement")
    event.set()  # Définir l'événement (réveille tous les threads en attente)

# Création et démarrage des threads
waiter_thread = threading.Thread(target=waiter)
waiter_thread.start()

setter_thread = threading.Thread(target=setter)
setter_thread.start()

waiter_thread.join()
setter_thread.join()
```

**Conditions:**

```python
# Implémentation d'une file d'attente simplifiée avec une Condition
condition = threading.Condition()
queue = []
max_size = 5

def producer():
    global queue
    for i in range(10):
        with condition:
            while len(queue) >= max_size:
                print("Producer: File pleine, en attente...")
                condition.wait()

            item = f"Item-{i}"
            queue.append(item)
            print(f"Producer: Produit {item}")

            condition.notify()  # Notifie un consommateur en attente
        time.sleep(0.5)

def consumer():
    global queue
    while True:
        with condition:
            while not queue:
                print("Consumer: File vide, en attente...")
                condition.wait()

            item = queue.pop(0)
            print(f"Consumer: Consommé {item}")

            condition.notify()  # Notifie un producteur en attente
        time.sleep(1)

# Création et démarrage des threads
producer_thread = threading.Thread(target=producer)
consumer_thread = threading.Thread(target=consumer, daemon=True)

producer_thread.start()
consumer_thread.start()

producer_thread.join()
# Le consumer est un thread démon et se terminera automatiquement
```

### Partage de données entre threads

Les threads partagent le même espace mémoire, ce qui facilite le partage de données mais nécessite une synchronisation appropriée:

**Utilisation de variables partagées avec synchronisation:**

```python
class SharedCounter:
    def __init__(self):
        self.value = 0
        self.lock = threading.Lock()

    def increment(self):
        with self.lock:
            self.value += 1

    def get_value(self):
        with self.lock:
            return self.value

counter = SharedCounter()

def increment_counter():
    for _ in range(100000):
        counter.increment()

threads = []
for _ in range(10):
    thread = threading.Thread(target=increment_counter)
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

print(f"Valeur finale: {counter.get_value()}")
```

**Utilisation de Queue (thread-safe):**

```python
import queue

# Création d'une file d'attente thread-safe
task_queue = queue.Queue()

def producer():
    for i in range(10):
        task = f"Task-{i}"
        task_queue.put(task)
        print(f"Producer: {task} ajouté à la file")
        time.sleep(0.5)

def consumer():
    while True:
        try:
            task = task_queue.get(timeout=5)  # Attendre 5 secondes max
            print(f"Consumer: Traitement de {task}")
            time.sleep(1)
            # Marquer la tâche comme terminée
            task_queue.task_done()
        except queue.Empty:
            print("Consumer: File vide, sortie")
            break

# Création et démarrage des threads
producer_thread = threading.Thread(target=producer)
consumer_thread = threading.Thread(target=consumer)

producer_thread.start()
consumer_thread.start()

producer_thread.join()
consumer_thread.join()

# Attente que toutes les tâches soient traitées
task_queue.join()
```

**ThreadLocal pour les données spécifiques aux threads:**

```python
# Création d'un objet ThreadLocal
thread_local_data = threading.local()

def worker(name):
    # Chaque thread a sa propre copie de thread_local_data.value
    thread_local_data.value = name
    print(f"Thread {name} défini thread_local_data.value à {name}")
    time.sleep(1)
    print(f"Thread {name} a thread_local_data.value = {thread_local_data.value}")

# Création et démarrage des threads
threads = []
for i in range(3):
    thread = threading.Thread(target=worker, args=(f"Thread-{i}",))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()
```

### Thread pools

Le module `concurrent.futures` fournit une interface de haut niveau pour les pools de threads:

```python
from concurrent.futures import ThreadPoolExecutor
import time

def task(name):
    print(f"Tâche {name} démarrée")
    time.sleep(2)
    return f"Résultat de {name}"

# Utilisation d'un pool de threads avec un nombre maximal de workers
with ThreadPoolExecutor(max_workers=3) as executor:
    # Soumission de tâches
    future1 = executor.submit(task, "A")
    future2 = executor.submit(task, "B")
    future3 = executor.submit(task, "C")
    future4 = executor.submit(task, "D")

    # Obtention des résultats au fur et à mesure qu'ils sont disponibles
    for future in concurrent.futures.as_completed([future1, future2, future3, future4]):
        try:
            result = future.result()
            print(f"Obtenu: {result}")
        except Exception as e:
            print(f"Tâche a généré une exception: {e}")

    # Alternative: map pour exécuter la même fonction sur plusieurs entrées
    results = executor.map(task, ["E", "F", "G"])
    for result in results:
        print(f"Résultat de map: {result}")
```

### Limitations des threads en Python

**Impact du GIL sur les performances:**

```python
import threading
import time
import multiprocessing

def cpu_intensive(n):
    """Tâche intense en CPU"""
    count = 0
    for i in range(n):
        count += i
    return count

def io_intensive(n):
    """Tâche intense en E/S"""
    for i in range(n):
        time.sleep(0.01)  # Simule une opération d'E/S
    return n

def benchmark(func, n, n_threads):
    start = time.time()

    if n_threads == 1:
        # Exécution séquentielle
        for _ in range(n_threads):
            func(n)
    else:
        # Exécution parallèle
        threads = []
        for _ in range(n_threads):
            thread = threading.Thread(target=func, args=(n // n_threads,))
            threads.append(thread)
            thread.start()

        for thread in threads:
            thread.join()

    end = time.time()
    return end - start

# Test avec une tâche intensive en CPU (limité par le GIL)
cpu_seq_time = benchmark(cpu_intensive, 10**7, 1)
cpu_par_time = benchmark(cpu_intensive, 10**7, 4)

print(f"CPU-bound séquentiel: {cpu_seq_time:.4f}s")
print(f"CPU-bound avec 4 threads: {cpu_par_time:.4f}s")
print(f"Speedup: {cpu_seq_time / cpu_par_time:.2f}x")

# Test avec une tâche intensive en E/S (non limité par le GIL)
io_seq_time = benchmark(io_intensive, 400, 1)
io_par_time = benchmark(io_intensive, 400, 4)

print(f"I/O-bound séquentiel: {io_seq_time:.4f}s")
print(f"I/O-bound avec 4 threads: {io_par_time:.4f}s")
print(f"Speedup: {io_seq_time / io_par_time:.2f}x")
```

**Gestion de la mémoire:**

```python
import threading
import time
import psutil
import os

def memory_intensive():
    """Fonction qui alloue beaucoup de mémoire"""
    # Allouer 100MB
    data = [0] * (100 * 1024 * 1024 // 8)  # liste d'entiers (8 bytes chacun)
    time.sleep(2)  # Garder la mémoire allouée pendant un moment
    return len(data)

def check_memory():
    """Affiche l'utilisation de la mémoire du processus actuel"""
    process = psutil.Process(os.getpid())
    print(f"Mémoire utilisée: {process.memory_info().rss / (1024 * 1024):.2f} MB")

# Vérifier la mémoire initiale
check_memory()

# Exécuter une seule instance
memory_intensive()
check_memory()

# Exécuter plusieurs instances concurrentes
threads = []
for _ in range(4):
    thread = threading.Thread(target=memory_intensive)
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()

check_memory()
```

**Race conditions:**

```python
import threading

counter = 0

def increment_unsafe():
    global counter
    for _ in range(100000):
        # Cette opération n'est pas atomique en Python
        counter += 1

def run_threads_unsafe():
    global counter
    counter = 0
    threads = []

    for _ in range(10):
        thread = threading.Thread(target=increment_unsafe)
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

    print(f"Valeur finale (non sécurisée): {counter}")  # Probablement moins que 1000000

# Version sécurisée avec lock
lock = threading.Lock()

def increment_safe():
    global counter
    for _ in range(100000):
        with lock:
            counter += 1

def run_threads_safe():
    global counter
    counter = 0
    threads = []

    for _ in range(10):
        thread = threading.Thread(target=increment_safe)
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

    print(f"Valeur finale (sécurisée): {counter}")  # Exactement 1000000

run_threads_unsafe()
run_threads_safe()
```

## Programmation multiprocessus

### Le module multiprocessing

Le module `multiprocessing` fournit une API similaire à `threading` mais utilise des processus séparés:

```python
import multiprocessing
import time

def worker(name):
    """Fonction exécutée dans un processus séparé"""
    print(f"Processus {name} démarré")
    time.sleep(2)
    print(f"Processus {name} terminé")

if __name__ == '__main__':
    # Toujours protéger l'entrée du programme avec if __name__ == '__main__'
    # pour éviter des problèmes sur Windows

    # Création et démarrage des processus
    processes = []
    for i in range(5):
        p = multiprocessing.Process(target=worker, args=(f"P{i}",))
        processes.append(p)
        p.start()

    # Attendre que tous les processus terminent
    for p in processes:
        p.join()

    print("Tous les processus ont terminé")
```

### Création et gestion des processus

Comme pour les threads, il existe deux façons principales de créer des processus:

1. **En fournissant une fonction cible**

```python
def task(id):
    print(f"Tâche {id} en cours d'exécution dans le processus {multiprocessing.current_process().name}")

if __name__ == '__main__':
    p = multiprocessing.Process(target=task, args=(1,))
    p.start()
    p.join()
```

2. **En sous-classant Process**

```python
class MyProcess(multiprocessing.Process):
    def __init__(self, id):
        super().__init__()
        self.id = id

    def run(self):
        """Méthode exécutée quand le processus démarre"""
        print(f"Processus {self.id} en cours d'exécution")
        time.sleep(2)
        print(f"Processus {self.id} terminé")

if __name__ == '__main__':
    p = MyProcess(1)
    p.start()
    p.join()
```

**Gestion des processus:**

```python
if __name__ == '__main__':
    # Création d'un processus avec un nom
    p = multiprocessing.Process(target=worker, args=(1,), name="WorkerProcess")

    # Démarrage du processus
    p.start()

    # Vérifier si le processus est encore en cours d'exécution
    if p.is_alive():
        print("Le processus est toujours en cours d'exécution")

    # Attente de la fin du processus avec timeout
    p.join(timeout=5)

    # Terminer brutalement un processus (rarement nécessaire, mais disponible)
    if p.is_alive():
        p.terminate()  # SIGTERM
        p.join()

    # Python 3.7+: kill() envoie SIGKILL, plus brutal que terminate()
    # if p.is_alive():
    #     p.kill()
    #     p.join()

    # Obtenir le code de sortie (None si processus toujours en vie)
    print(f"Code de sortie: {p.exitcode}")
```

**Processus démons:**

```python
def daemon_worker():
    print("Processus démon démarré")
    while True:
        time.sleep(1)
        print("Processus démon toujours en vie")

if __name__ == '__main__':
    # Création d'un processus démon
    daemon = multiprocessing.Process(target=daemon_worker, daemon=True)
    daemon.start()

    # Le programme continue
    print("Programme principal en cours...")
    time.sleep(3)
    print("Programme principal se termine")
    # Le processus démon sera automatiquement terminé ici
```

### Communication entre processus

Contrairement aux threads, les processus ont leurs propres espaces mémoire, ce qui nécessite des mécanismes explicites pour la communication:

**Queues:**

```python
from multiprocessing import Process, Queue

def producer(queue):
    """Produit des éléments et les envoie via la queue"""
    for i in range(5):
        item = f"Item-{i}"
        queue.put(item)
        print(f"Production: {item}")
        time.sleep(1)
    # Signal de fin
    queue.put(None)

def consumer(queue):
    """Consomme des éléments de la queue"""
    while True:
        item = queue.get()
        if item is None:  # Signal de fin
            break
        print(f"Consommation: {item}")
        time.sleep(2)

if __name__ == '__main__':
    # Création d'une queue pour la communication
    q = Queue()

    # Création et démarrage des processus
    p_producer = Process(target=producer, args=(q,))
    p_consumer = Process(target=consumer, args=(q,))

    p_producer.start()
    p_consumer.start()

    p_producer.join()
    p_consumer.join()
```

**Pipes:**

```python
from multiprocessing import Process, Pipe

def sender(conn):
    """Envoie des données via le pipe"""
    conn.send("Hello")
    conn.send(42)
    conn.send({"key": "value"})
    conn.close()

def receiver(conn):
    """Reçoit des données du pipe"""
    while True:
        try:
            data = conn.recv()  # Bloque jusqu'à ce que des données soient disponibles
            print(f"Reçu: {data}")
        except EOFError:
            print("Canal fermé")
            break

if __name__ == '__main__':
    # Création d'un pipe
    parent_conn, child_conn = Pipe()

    # Création et démarrage des processus
    p_sender = Process(target=sender, args=(child_conn,))
    p_receiver = Process(target=receiver, args=(parent_conn,))

    p_sender.start()
    p_receiver.start()

    # Fermer notre côté des connexions
    child_conn.close()
    parent_conn.close()

    p_sender.join()
    p_receiver.join()
```

### Partage de données entre processus

**Valeurs et tableaux partagés:**

```python
from multiprocessing import Process, Value, Array

def increment_counter(counter):
    for _ in range(100000):
        with counter.get_lock():
            counter.value += 1

def modify_array(shared_array):
    for i in range(len(shared_array)):
        shared_array[i] *= 2

if __name__ == '__main__':
    # Création d'une valeur partagée avec lock intégré
    counter = Value('i', 0)  # 'i' pour integer

    # Création et démarrage des processus pour le compteur
    processes = []
    for _ in range(4):
        p = Process(target=increment_counter, args=(counter,))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

    print(f"Valeur finale du compteur: {counter.value}")

    # Création d'un tableau partagé
    shared_array = Array('i', range(10))  # Tableau de 10 entiers

    # Processus qui modifie le tableau
    p_array = Process(target=modify_array, args=(shared_array,))
    p_array.start()
    p_array.join()

    print(f"Tableau final: {list(shared_array)}")
```

**Manager pour objets partagés plus complexes:**

```python
from multiprocessing import Process, Manager

def worker(shared_dict, shared_list):
    """Modifie les objets partagés"""
    shared_dict["key"] = "value"
    shared_dict["count"] = 42
    shared_list.append("item 1")
    shared_list.append("item 2")

if __name__ == '__main__':
    with Manager() as manager:
        # Création d'objets partagés
        shared_dict = manager.dict()
        shared_list = manager.list()

        # Processus qui modifie les objets partagés
        p = Process(target=worker, args=(shared_dict, shared_list))
        p.start()
        p.join()

        # Accès aux objets partagés dans le processus principal
        print(f"Dictionnaire partagé: {shared_dict}")
        print(f"Liste partagée: {shared_list}")

        # Créer une file d'attente partagée via le manager
        shared_queue = manager.Queue()
        shared_queue.put("un élément")
        print(f"Élément de la file: {shared_queue.get()}")
```

**Utilisation d'un serveur de processus:**

```python
from multiprocessing.managers import BaseManager
import queue

class QueueManager(BaseManager):
    pass

# Création de files d'attente
task_queue = queue.Queue()
result_queue = queue.Queue()

# Enregistrement des files dans le gestionnaire
QueueManager.register('get_task_queue', callable=lambda: task_queue)
QueueManager.register('get_result_queue', callable=lambda: result_queue)

if __name__ == '__main__':
    # Démarrage du gestionnaire
    manager = QueueManager(address=('', 50000), authkey=b'abc')
    server = manager.get_server()

    # Ajouter quelques tâches à la file
    task_q = manager.get_task_queue()
    for i in range(10):
        task_q.put(f"Task-{i}")

    print("Démarrage du serveur...")
    server.serve_forever()

    # Dans un autre processus ou une autre machine, vous pourriez vous connecter:
    # manager = QueueManager(address=('localhost', 50000), authkey=b'abc')
    # manager.connect()
    # task_q = manager.get_task_queue()
    # result_q = manager.get_result_queue()
    # ...traiter les tâches...
```

### Process pools

Le module `multiprocessing` fournit un pool de processus qui simplifie l'exécution parallèle de tâches:

```python
from multiprocessing import Pool
import time

def task(x):
    """Tâche intensive en CPU"""
    print(f"Traitement de {x}")
    time.sleep(1)  # Simuler un travail CPU
    return x * x

if __name__ == '__main__':
    # Création d'un pool avec 4 processus
    with Pool(processes=4) as pool:
        # Version simple: map
        results = pool.map(task, range(10))
        print(f"Résultats (map): {results}")

        # Version non-bloquante: map_async
        result_async = pool.map_async(task, range(5))
        print("Calcul en cours...")

        # Faire autre chose pendant le calcul
        time.sleep(0.5)

        # Obtenir les résultats quand ils sont prêts
        results_async = result_async.get()
        print(f"Résultats (map_async): {results_async}")

        # Version avec callback: apply_async
        results_apply = []

        def callback(result):
            results_apply.append(result)
            print(f"Callback: reçu {result}")

        for i in range(3):
            pool.apply_async(task, args=(i,), callback=callback)

        # Attendre la complétion (important sinon les processus peuvent être terminés avant)
        pool.close()  # Ne plus accepter de nouvelles tâches
        pool.join()   # Attendre la fin de toutes les tâches

        print(f"Résultats (apply_async via callbacks): {results_apply}")
```

### Considérations de performance

**Benchmark: Threads vs Processus pour tâches CPU-bound:**

```python
import time
import threading
import multiprocessing

def cpu_intensive(n):
    """Fonction intensive en CPU"""
    result = 0
    for i in range(n):
        result += i * i
    return result

def run_threads(n_threads, n):
    """Exécuter la tâche avec des threads"""
    threads = []
    for _ in range(n_threads):
        t = threading.Thread(target=cpu_intensive, args=(n // n_threads,))
        threads.append(t)
        t.start()

    for t in threads:
        t.join()

def run_processes(n_processes, n):
    """Exécuter la tâche avec des processus"""
    processes = []
    for _ in range(n_processes):
        p = multiprocessing.Process(target=cpu_intensive, args=(n // n_processes,))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

if __name__ == '__main__':
    # Paramètres
    n = 10**7
    workers = [1, 2, 4, 8]

    # Test séquentiel
    start = time.time()
    cpu_intensive(n)
    sequential_time = time.time() - start
    print(f"Temps séquentiel: {sequential_time:.4f}s")

    # Test avec threads (limité par le GIL)
    for n_threads in workers:
        start = time.time()
        run_threads(n_threads, n)
        thread_time = time.time() - start
        print(f"Temps avec {n_threads} threads: {thread_time:.4f}s, speedup: {sequential_time / thread_time:.2f}x")

    # Test avec processus
    for n_processes in workers:
        start = time.time()
        run_processes(n_processes, n)
        process_time = time.time() - start
        print(f"Temps avec {n_processes} processus: {process_time:.4f}s, speedup: {sequential_time / process_time:.2f}x")
```

**Surcoût de création des processus:**

```python
import time
import threading
import multiprocessing

def empty_task():
    """Tâche vide pour mesurer le surcoût de création"""
    pass

def benchmark_creation(n):
    """Mesurer le temps de création et d'exécution pour n threads/processus"""
    # Threads
    start = time.time()
    threads = []
    for _ in range(n):
        t = threading.Thread(target=empty_task)
        threads.append(t)
        t.start()

    for t in threads:
        t.join()

    thread_time = time.time() - start

    # Processus
    start = time.time()
    processes = []
    for _ in range(n):
        p = multiprocessing.Process(target=empty_task)
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

    process_time = time.time() - start

    return thread_time, process_time

if __name__ == '__main__':
    counts = [10, 50, 100, 200]

    for n in counts:
        thread_time, process_time = benchmark_creation(n)
        print(f"Création et exécution de {n} workers:")
        print(f"  Threads: {thread_time:.4f}s")
        print(f"  Processus: {process_time:.4f}s")
        print(f"  Ratio: processus est {process_time / thread_time:.1f}x plus lent")
```

**Utilisation de la mémoire:**

```python
import threading
import multiprocessing
import os
import psutil
import time

def memory_usage():
    """Retourne l'utilisation de la mémoire en MB"""
    process = psutil.Process(os.getpid())
    return process.memory_info().rss / (1024 * 1024)

def allocate_memory(size_mb):
    """Alloue une quantité spécifiée de mémoire"""
    # Allouer une liste d'entiers (8 bytes chacun)
    data = [0] * (size_mb * 1024 * 1024 // 8)
    time.sleep(1)  # Garder les données en mémoire
    return f"Alloué {size_mb} MB"

def thread_test(n, size_mb):
    """Test avec n threads"""
    start_mem = memory_usage()

    threads = []
    for _ in range(n):
        t = threading.Thread(target=allocate_memory, args=(size_mb,))
        threads.append(t)
        t.start()

    for t in threads:
        t.join()

    end_mem = memory_usage()
    return end_mem - start_mem

def process_test(n, size_mb):
    """Test avec n processus"""
    start_mem = memory_usage()

    processes = []
    for _ in range(n):
        p = multiprocessing.Process(target=allocate_memory, args=(size_mb,))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

    end_mem = memory_usage()
    return end_mem - start_mem

if __name__ == '__main__':
    n_workers = 4
    size_mb = 100  # Chaque worker alloue 100MB

    print(f"Mémoire initiale: {memory_usage():.1f} MB")

    # Test avec threads (mémoire partagée)
    mem_increase = thread_test(n_workers, size_mb)
    print(f"Augmentation avec {n_workers} threads: {mem_increase:.1f} MB")

    # Test avec processus (mémoire séparée)
    mem_increase = process_test(n_workers, size_mb)
    print(f"Augmentation avec {n_workers} processus: {mem_increase:.1f} MB")
```

## Synchronisation et coordination

### Locks et RLocks

Les locks fournissent une exclusion mutuelle pour protéger les ressources partagées:

```python
import threading

# Lock standard
lock = threading.Lock()

# Utilisation basique
lock.acquire()  # Acquérir le lock (bloque si déjà pris)
try:
    # Section critique
    print("Section critique - Lock standard")
finally:
    lock.release()  # Toujours libérer le lock

# Avec gestionnaire de contexte (recommandé)
with lock:
    # Section critique
    print("Section critique avec gestionnaire de contexte")

# RLock (Reentrant Lock)
rlock = threading.RLock()

# Un RLock peut être acquis plusieurs fois par le même thread
with rlock:
    print("Premier niveau avec RLock")
    with rlock:
        print("Deuxième niveau avec RLock")
        with rlock:
            print("Troisième niveau avec RLock")
```

**Comparaison Lock vs RLock:**

```python
import threading

def demonstrate_lock():
    lock = threading.Lock()

    # Premier lock
    lock.acquire()
    print("Premier verrou acquis")

    try:
        # Deuxième lock - causera un deadlock
        print("Tentative d'acquérir le lock à nouveau...")
        lock.acquire()  # Ceci va bloquer indéfiniment
    finally:
        lock.release()

def demonstrate_rlock():
    rlock = threading.RLock()

    # Premier lock
    rlock.acquire()
    print("Premier RLock acquis")

    try:
        # Deuxième lock - fonctionne avec RLock
        print("Tentative d'acquérir le RLock à nouveau...")
        rlock.acquire()
        print("Deuxième RLock acquis")

        rlock.release()  # Libérer le deuxième lock
    finally:
        rlock.release()  # Libérer le premier lock

# demonstrate_lock()  # Décommenter pour observer le deadlock
demonstrate_rlock()  # Fonctionne correctement
```

### Sémaphores

Les sémaphores limitent le nombre d'accès concurrents à une ressource:

```python
import threading
import time
import random

# Sémaphore limitant l'accès à 3 threads simultanés
semaphore = threading.Semaphore(3)

def worker(worker_id):
    print(f"Worker {worker_id} attend l'accès")

    with semaphore:
        print(f"Worker {worker_id} a accès à la ressource")
        # Simulation d'utilisation de la ressource
        time.sleep(random.uniform(1, 3))
        print(f"Worker {worker_id} libère la ressource")

# Création et démarrage de plusieurs threads
threads = []
for i in range(10):
    thread = threading.Thread(target=worker, args=(i,))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()
```

**Sémaphore borné (BoundedSemaphore):**

```python
# Un BoundedSemaphore empêche les releases d'excéder la valeur initiale
bounded_semaphore = threading.BoundedSemaphore(value=5)

def worker_bounded(worker_id):
    print(f"Worker {worker_id} attend l'accès")

    bounded_semaphore.acquire()
    try:
        print(f"Worker {worker_id} a accès à la ressource")
        time.sleep(random.uniform(0.5, 1.5))
    finally:
        bounded_semaphore.release()  # Si appelé trop de fois, lève une exception

        # Ceci lèverait une exception avec BoundedSemaphore:
        # bounded_semaphore.release()
```

### Events

Les events permettent à un thread de signaler une condition à d'autres threads:

```python
import threading
import time

# Création d'un event
event = threading.Event()

def waiter(name):
    print(f"{name} attend l'événement")
    event.wait()  # Bloque jusqu'à ce que l'event soit set()
    print(f"{name} a reçu l'événement et continue")

def setter():
    print("Setter va déclencher l'événement dans 2 secondes")
    time.sleep(2)
    print("Setter déclenche l'événement - tous les waiters sont notifiés")
    event.set()

# Création des threads
threads = []
for i in range(5):
    thread = threading.Thread(target=waiter, args=(f"Waiter-{i}",))
    thread.start()
    threads.append(thread)

setter_thread = threading.Thread(target=setter)
setter_thread.start()
threads.append(setter_thread)

# Attendre tous les threads
for thread in threads:
    thread.join()

# Réinitialiser l'événement pour une utilisation future
event.clear()
print("Événement réinitialisé:", not event.is_set())
```

### Conditions

Les conditions permettent à des threads d'attendre qu'une condition spécifique soit remplie:

```python
import threading
import time
import random

# Création d'une condition
condition = threading.Condition()

# Ressource partagée
items = []
max_items = 10

def producer():
    global items

    for i in range(20):
        time.sleep(random.uniform(0.1, 0.5))  # Temps de production simulé

        # Acquérir le lock et vérifier si la liste est pleine
        with condition:
            while len(items) >= max_items:
                print("Liste pleine, producteur en attente...")
                condition.wait()  # Libère le lock et attend

            # Produire un item
            item = f"Item-{i}"
            items.append(item)
            print(f"Produit: {item}. Liste: {len(items)} items")

            # Notifier les consommateurs qu'un nouvel item est disponible
            condition.notify()

def consumer(name):
    global items

    while True:
        time.sleep(random.uniform(0.2, 0.7))  # Temps de consommation simulé

        # Acquérir le lock et vérifier si la liste est vide
        with condition:
            while not items:
                print(f"Liste vide, {name} en attente...")
                condition.wait()  # Libère le lock et attend

                # Si on nous réveille et qu'il n'y a toujours rien, c'est probablement la fin
                if not items and producer_done:
                    return

            # Consommer un item
            item = items.pop(0)
            print(f"{name} a consommé: {item}. Liste: {len(items)} items")

            # Notifier les producteurs qu'une place est disponible
            condition.notify()

# Création des threads
producer_done = False
producer_thread = threading.Thread(target=producer)
consumer_thread1 = threading.Thread(target=consumer, args=("Consumer-1",))
consumer_thread2 = threading.Thread(target=consumer, args=("Consumer-2",))

producer_thread.start()
consumer_thread1.start()
consumer_thread2.start()

# Attendre que le producteur termine
producer_thread.join()
producer_done = True

# Réveiller tous les consommateurs pour qu'ils puissent vérifier producer_done
with condition:
    condition.notify_all()

# Attendre les consommateurs
consumer_thread1.join()
consumer_thread2.join()

print("Tous les threads ont terminé")
```

### Barrières

Les barrières synchronisent un nombre fixe de threads à un point donné:

```python
import threading
import time
import random

# Création d'une barrière pour 4 threads
barrier = threading.Barrier(4)

def worker(worker_id):
    print(f"Worker {worker_id} commence")

    # Simulation de travail initial
    time.sleep(random.uniform(0.5, 2))
    print(f"Worker {worker_id} arrive à la barrière")

    # Attendre que tous les threads atteignent ce point
    barrier.wait()

    print(f"Worker {worker_id} continue après la barrière")

    # Plus de travail
    time.sleep(random.uniform(0.5, 1))
    print(f"Worker {worker_id} a terminé")

# Création et démarrage des threads
threads = []
for i in range(4):
    thread = threading.Thread(target=worker, args=(i,))
    threads.append(thread)
    thread.start()

# Attendre tous les threads
for thread in threads:
    thread.join()
```

**Barrière avec callback:**

```python
def barrier_callback():
    """Fonction appelée quand tous les threads atteignent la barrière"""
    print("\nTous les threads ont atteint la barrière - passage à la phase suivante\n")

# Barrière avec fonction de callback
barrier_with_callback = threading.Barrier(3, action=barrier_callback)

def worker_with_callback(worker_id):
    print(f"Worker {worker_id} commence la phase 1")
    time.sleep(random.uniform(0.5, 1.5))
    print(f"Worker {worker_id} a terminé la phase 1")

    # Attendre tous les threads à la fin de la phase 1
    barrier_with_callback.wait()

    print(f"Worker {worker_id} commence la phase 2")
    time.sleep(random.uniform(0.5, 1.5))
    print(f"Worker {worker_id} a terminé la phase 2")

    # Attendre tous les threads à la fin de la phase 2
    barrier_with_callback.wait()

    print(f"Worker {worker_id} a terminé toutes les phases")

# Création et démarrage des threads
threads = []
for i in range(3):
    thread = threading.Thread(target=worker_with_callback, args=(i,))
    threads.append(thread)
    thread.start()

for thread in threads:
    thread.join()
```

### Queues

Les queues thread-safe pour la communication entre threads:

```python
import threading
import queue
import time
import random

# Création d'une queue standard
q = queue.Queue()

# Files avec taille maximale
bounded_q = queue.Queue(maxsize=5)

# Autres types de queues
lifo_q = queue.LifoQueue()  # Last-In-First-Out
priority_q = queue.PriorityQueue()  # Files avec priorité

def producer(q, count):
    for i in range(count):
        item = f"Item-{i}"
        q.put(item)
        print(f"Produit: {item}")
        time.sleep(random.uniform(0.1, 0.5))

    # Signal de fin
    q.put(None)

def consumer(q, name):
    while True:
        item = q.get()

        if item is None:
            # Si c'est un signal de fin, le remettre pour les autres consommateurs
            q.put(None)
            break

        print(f"{name} a consommé: {item}")
        time.sleep(random.uniform(0.2, 0.7))

        # Marquer la tâche comme terminée
        q.task_done()

# Utilisation de base
producer_thread = threading.Thread(target=producer, args=(q, 10))
consumer_thread1 = threading.Thread(target=consumer, args=(q, "Consumer-1"))
consumer_thread2 = threading.Thread(target=consumer, args=(q, "Consumer-2"))

producer_thread.start()
consumer_thread1.start()
consumer_thread2.start()

# Attendre que tous les threads terminent
producer_thread.join()
consumer_thread1.join()
consumer_thread2.join()

# Attendre que tous les items soient traités
# q.join()  # Aurait bloqué jusqu'à ce que q.task_done() soit appelé pour chaque élément
```

**Queue avec priorité:**

```python
import threading
import queue
import time
import random

# Création d'une queue à priorité
priority_queue = queue.PriorityQueue()

def priority_producer():
    # Produire des éléments avec différentes priorités
    # Priorité plus basse = traité en premier
    priorities = [(3, "Basse priorité 1"),
                  (1, "Haute priorité 1"),
                  (2, "Moyenne priorité 1"),
                  (1, "Haute priorité 2"),
                  (3, "Basse priorité 2"),
                  (2, "Moyenne priorité 2")]

    for priority, item in priorities:
        # Ajouter un élément avec sa priorité
        priority_queue.put((priority, item))
        print(f"Produit: {item} (priorité {priority})")
        time.sleep(random.uniform(0.1, 0.3))

    # Signal de fin
    priority_queue.put((999, None))

def priority_consumer():
    while True:
        # Obtenir l'élément avec la priorité la plus basse
        priority, item = priority_queue.get()

        if item is None:
            # Signal de fin
            break

        print(f"Consommé: {item} (priorité {priority})")
        time.sleep(random.uniform(0.2, 0.5))
        priority_queue.task_done()

# Démarrage des threads
prod_thread = threading.Thread(target=priority_producer)
cons_thread = threading.Thread(target=priority_consumer)

prod_thread.start()
cons_thread.start()

prod_thread.join()
cons_thread.join()
```

### Éviter les deadlocks

**Deadlock simple illustré:**

```python
import threading
import time

# Deux locks pour la démonstration
lock_a = threading.Lock()
lock_b = threading.Lock()

def thread_1():
    print("Thread 1 tente d'acquérir lock A")
    with lock_a:
        print("Thread 1 a acquis lock A")
        time.sleep(0.5)  # Délai pour garantir le deadlock

        print("Thread 1 tente d'acquérir lock B")
        with lock_b:
            print("Thread 1 a acquis lock B")
            print("Thread 1 fait son travail avec les deux locks")

def thread_2():
    print("Thread 2 tente d'acquérir lock B")
    with lock_b:
        print("Thread 2 a acquis lock B")
        time.sleep(0.5)  # Délai pour garantir le deadlock

        print("Thread 2 tente d'acquérir lock A")
        with lock_a:
            print("Thread 2 a acquis lock A")
            print("Thread 2 fait son travail avec les deux locks")

# Création et démarrage des threads
t1 = threading.Thread(target=thread_1)
t2 = threading.Thread(target=thread_2)

# Ces threads vont provoquer un deadlock
# t1.start()
# t2.start()
#
# t1.join()
# t2.join()

# Correction: acquérir toujours les locks dans le même ordre
def thread_1_safe():
    print("Thread 1 (sécurisé) tente d'acquérir lock A")
    with lock_a:
        print("Thread 1 (sécurisé) a acquis lock A")
        time.sleep(0.5)

        print("Thread 1 (sécurisé) tente d'acquérir lock B")
        with lock_b:
            print("Thread 1 (sécurisé) a acquis lock B")
            print("Thread 1 (sécurisé) fait son travail avec les deux locks")

def thread_2_safe():
    print("Thread 2 (sécurisé) tente d'acquérir lock A")  # Même ordre que thread_1_safe
    with lock_a:
        print("Thread 2 (sécurisé) a acquis lock A")
        time.sleep(0.5)

        print("Thread 2 (sécurisé) tente d'acquérir lock B")
        with lock_b:
            print("Thread 2 (sécurisé) a acquis lock B")
            print("Thread 2 (sécurisé) fait son travail avec les deux locks")

# Threads sécurisés qui évitent le deadlock
t1_safe = threading.Thread(target=thread_1_safe)
t2_safe = threading.Thread(target=thread_2_safe)

t1_safe.start()
t2_safe.start()

t1_safe.join()
t2_safe.join()
```

**Acquisition simultanée de plusieurs locks:**

```python
import threading

# Technique pour acquérir plusieurs locks sans risque de deadlock
def acquire_multiple_locks(locks):
    # Tentative d'acquérir les locks dans l'ordre
    acquired = []
    try:
        for lock in locks:
            lock.acquire()
            acquired.append(lock)
        return True
    except:
        # En cas d'erreur, libérer tous les locks acquis
        for lock in acquired:
            lock.release()
        return False

def release_multiple_locks(locks):
    # Libérer les locks dans l'ordre inverse
    for lock in reversed(locks):
        lock.release()

# Utilisation
lock_x = threading.Lock()
lock_y = threading.Lock()
lock_z = threading.Lock()

def safe_operation():
    locks = [lock_x, lock_y, lock_z]  # Ordre déterministe

    if acquire_multiple_locks(locks):
        try:
            print("Tous les locks ont été acquis, opération sécurisée")
            # Opération critique ici
        finally:
            release_multiple_locks(locks)
    else:
        print("Impossible d'acquérir tous les locks")
```

**Timeout pour éviter les deadlocks:**

```python
import threading
import time

lock_1 = threading.Lock()
lock_2 = threading.Lock()

def worker_with_timeout():
    print("Worker tente d'acquérir lock 1")

    # Acquérir lock_1 avec timeout
    acquired_1 = lock_1.acquire(timeout=1)
    if not acquired_1:
        print("Impossible d'acquérir lock 1 dans le délai imparti")
        return

    try:
        print("Worker a acquis lock 1")
        time.sleep(0.5)

        print("Worker tente d'acquérir lock 2")
        # Acquérir lock_2 avec timeout
        acquired_2 = lock_2.acquire(timeout=1)
        if not acquired_2:
            print("Impossible d'acquérir lock 2 dans le délai imparti")
            return

        try:
            print("Worker a acquis lock 2")
            # Opération critique ici
        finally:
            lock_2.release()
    finally:
        lock_1.release()

# Simuler un deadlock en acquérant lock_2 préalablement
lock_2.acquire()
worker_thread = threading.Thread(target=worker_with_timeout)
worker_thread.start()
worker_thread.join()
lock_2.release()  # Libérer lock_2 pour nettoyer
```

## concurrent.futures

Le module `concurrent.futures` fournit une interface de haut niveau pour la programmation asynchrone avec des threads et des processus:

### ThreadPoolExecutor

`ThreadPoolExecutor` gère un pool de threads pour exécuter des tâches:

```python
from concurrent.futures import ThreadPoolExecutor
import time
import random

def task(name):
    print(f"Tâche {name} commence")
    sleep_time = random.uniform(0.5, 2)
    time.sleep(sleep_time)
    print(f"Tâche {name} termine après {sleep_time:.2f}s")
    return f"Résultat de {name}"

# Utilisation de base avec gestionnaire de contexte
with ThreadPoolExecutor(max_workers=3) as executor:
    # Soumission de tâches individuelles
    future1 = executor.submit(task, "A")
    future2 = executor.submit(task, "B")
    future3 = executor.submit(task, "C")

    # Obtention du résultat (bloque jusqu'à ce qu'il soit disponible)
    result1 = future1.result()
    print(f"Future 1 a retourné: {result1}")

    # Alternative: attendre le résultat avec timeout
    try:
        result2 = future2.result(timeout=1.0)
        print(f"Future 2 a retourné: {result2}")
    except TimeoutError:
        print("Future 2 n'a pas terminé à temps")

    # Vérifier si une tâche est terminée sans bloquer
    if future3.done():
        print(f"Future 3 a terminé avec: {future3.result()}")
    else:
        print("Future 3 est toujours en cours d'exécution")
        # Attendre qu'elle termine
        print(f"Future 3 a finalement retourné: {future3.result()}")

# Utilisation de map pour appliquer une fonction à plusieurs arguments
with ThreadPoolExecutor(max_workers=3) as executor:
    tasks = ["D", "E", "F", "G"]
    results = executor.map(task, tasks)

    # map retourne les résultats dans l'ordre des entrées
    for task_name, result in zip(tasks, results):
        print(f"Tâche {task_name} a produit: {result}")
```

**Callbacks avec `add_done_callback`:**

```python
from concurrent.futures import ThreadPoolExecutor
import time
import random

def task_with_error(name):
    print(f"Tâche {name} commence")
    sleep_time = random.uniform(0.5, 2)
    time.sleep(sleep_time)

    # Simuler une erreur aléatoire
    if random.random() < 0.5:
        raise ValueError(f"Erreur simulée dans la tâche {name}")

    return f"Résultat de {name}"

def handle_future(future):
    """Callback appelé quand un future est terminé"""
    try:
        result = future.result()
        print(f"Callback: Tâche réussie avec résultat: {result}")
    except Exception as e:
        print(f"Callback: Tâche a échoué avec l'erreur: {e}")

# Utilisation des callbacks
with ThreadPoolExecutor(max_workers=5) as executor:
    futures = []

    for i in range(10):
        future = executor.submit(task_with_error, f"Task-{i}")
        future.add_done_callback(handle_future)
        futures.append(future)

    print("Toutes les tâches ont été soumises")
```

### ProcessPoolExecutor

`ProcessPoolExecutor` est similaire à `ThreadPoolExecutor` mais utilise des processus plutôt que des threads:

```python
from concurrent.futures import ProcessPoolExecutor
import time
import random
import os

def cpu_bound_task(n):
    """Tâche intensive en CPU"""
    print(f"Tâche {n} démarre dans le processus {os.getpid()}")
    result = sum(i * i for i in range(10**6))
    time.sleep(random.uniform(0.5, 1.5))  # Travail supplémentaire
    print(f"Tâche {n} termine dans le processus {os.getpid()}")
    return result

if __name__ == '__main__':
    print(f"Processus principal: {os.getpid()}")

    # Utilisation avec gestionnaire de contexte
    with ProcessPoolExecutor(max_workers=4) as executor:
        # Soumission de tâches
        futures = [executor.submit(cpu_bound_task, i) for i in range(10)]

        # Attendre les résultats au fur et à mesure qu'ils sont prêts
        for i, future in enumerate(concurrent.futures.as_completed(futures)):
            try:
                result = future.result()
                print(f"Tâche {i} a retourné: {result}")
            except Exception as e:
                print(f"Tâche {i} a généré une exception: {e}")

        # Alternative: map
        print("\nUtilisation de map:")
        results = list(executor.map(cpu_bound_task, range(5)))
        print(f"Tous les résultats: {results}")
```

### Futures et callbacks

Le module `concurrent.futures` offre une API puissante pour travailler avec des résultats asynchrones:

```python
from concurrent.futures import ThreadPoolExecutor, as_completed
import time
import random

def long_task(name, success_rate=0.8):
    """Tâche qui prend du temps et peut échouer"""
    print(f"Démarrage de {name}")
    time.sleep(random.uniform(1, 3))

    # Simuler des échecs occasionnels
    if random.random() > success_rate:
        raise RuntimeError(f"Erreur simulée dans {name}")

    return f"Résultat de {name}"

# Exécuter plusieurs tâches et traiter leurs résultats
with ThreadPoolExecutor(max_workers=5) as executor:
    # Dictionnaire pour suivre quelles futures correspondent à quelles tâches
    future_to_name = {
        executor.submit(long_task, f"Task-{i}"): f"Task-{i}"
        for i in range(10)
    }

    # Traiter les futures au fur et à mesure qu'elles terminent
    for future in as_completed(future_to_name):
        name = future_to_name[future]
        try:
            result = future.result()
            print(f"{name} a réussi avec: {result}")
        except Exception as e:
            print(f"{name} a échoué avec: {e}")
```

**Future.cancel() et Future.cancelled():**

```python
from concurrent.futures import ThreadPoolExecutor
import time

def very_long_task():
    """Tâche qui prend beaucoup de temps"""
    print("Démarrage de la tâche longue")
    try:
        for i in range(10):
            print(f"Étape {i+1}/10")
            time.sleep(1)
        return "Tâche longue terminée"
    except Exception as e:
        print(f"Tâche interrompue: {e}")
        raise

with ThreadPoolExecutor(max_workers=1) as executor:
    # Soumettre la tâche
    future = executor.submit(very_long_task)

    # Attendre un moment
    time.sleep(3)

    # Décider d'annuler la tâche
    if not future.done():
        print("Tentative d'annulation de la tâche...")
        cancelled = future.cancel()
        print(f"Tâche annulée: {cancelled}")

    # Vérifier si la tâche a été annulée
    if future.cancelled():
        print("La tâche a été annulée avec succès")
    else:
        try:
            result = future.result()
            print(f"La tâche a terminé avec: {result}")
        except Exception as e:
            print(f"La tâche a échoué avec: {e}")
```

### Exécution parallèle de tâches

Exécution de différentes tâches avec des pools de threads et de processus:

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
import time
import requests
import os

def io_bound_task(url):
    """Tâche limitée par les E/S - idéale pour ThreadPoolExecutor"""
    print(f"Téléchargement depuis {url}")
    response = requests.get(url)
    return f"{url}: {len(response.content)} bytes"

def cpu_bound_task(n):
    """Tâche limitée par le CPU - idéale pour ProcessPoolExecutor"""
    print(f"Calcul CPU dans {os.getpid()}")
    result = sum(i * i for i in range(10**7))
    return f"PID {os.getpid()}: {result}"

if __name__ == '__main__':
    urls = [
        "https://www.python.org",
        "https://www.google.com",
        "https://www.github.com",
        "https://www.stackoverflow.com",
        "https://www.wikipedia.org"
    ]

    # Test avec ThreadPoolExecutor pour tâches IO-bound
    start = time.time()

    with ThreadPoolExecutor(max_workers=5) as executor:
        results = list(executor.map(io_bound_task, urls))

    end = time.time()
    print(f"\nThreadPoolExecutor pour tâches IO-bound: {end - start:.2f}s")
    for result in results:
        print(f"  {result}")

    # Test avec ProcessPoolExecutor pour tâches CPU-bound
    start = time.time()

    with ProcessPoolExecutor(max_workers=4) as executor:
        results = list(executor.map(cpu_bound_task, range(8)))

    end = time.time()
    print(f"\nProcessPoolExecutor pour tâches CPU-bound: {end - start:.2f}s")
    for result in results:
        print(f"  {result}")
```

**Combinaison de différents types de tâches:**

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor
import time
import os

def io_task(delay):
    """Simulation d'une tâche d'E/S"""
    time.sleep(delay)  # Simule des E/S bloquantes
    return f"IO terminé après {delay}s"

def cpu_task(n):
    """Tâche CPU intensive"""
    start = time.time()
    result = sum(i * i for i in range(n))
    end = time.time()
    return f"CPU (PID {os.getpid()}) terminé en {end - start:.2f}s avec résultat: {result}"

if __name__ == '__main__':
    # Stratégie: utiliser ProcessPoolExecutor pour les tâches CPU
    # et ThreadPoolExecutor pour les tâches IO

    print("Démarrage des tâches...")
    start_total = time.time()

    cpu_results = []
    io_results = []

    # ProcessPoolExecutor pour les tâches CPU
    with ProcessPoolExecutor(max_workers=4) as process_executor:
        # Soumettre des tâches CPU
        cpu_futures = [process_executor.submit(cpu_task, 10**7) for _ in range(8)]

        # Pendant que les processus travaillent, exécuter des tâches IO dans des threads
        with ThreadPoolExecutor(max_workers=10) as thread_executor:
            # Soumettre des tâches IO
            io_futures = [thread_executor.submit(io_task, i) for i in [1, 1.5, 2, 2.5, 3]]

            # Récupérer les résultats IO
            for future in concurrent.futures.as_completed(io_futures):
                try:
                    io_results.append(future.result())
                except Exception as e:
                    io_results.append(f"Erreur: {e}")

        # Récupérer les résultats CPU
        for future in concurrent.futures.as_completed(cpu_futures):
            try:
                cpu_results.append(future.result())
            except Exception as e:
                cpu_results.append(f"Erreur: {e}")

    end_total = time.time()

    print(f"\nTâches IO:")
    for result in io_results:
        print(f"  {result}")

    print(f"\nTâches CPU:")
    for result in cpu_results:
        print(f"  {result}")

    print(f"\nTemps total: {end_total - start_total:.2f}s")
```

## Modèles de concurrence avancés

### Worker Pools

Implémentation d'un pool de workers générique:

```python
import threading
import queue
import time
import random

class WorkerPool:
    """Pool de workers génériques pour exécuter des tâches"""

    def __init__(self, num_workers=4, queue_size=0):
        self.task_queue = queue.Queue(maxsize=queue_size)
        self.results_queue = queue.Queue()
        self.workers = []
        self.num_workers = num_workers
        self.is_running = False

    def start(self):
        """Démarrer les workers"""
        if self.is_running:
            return

        self.is_running = True
        for i in range(self.num_workers):
            worker = threading.Thread(target=self._worker_loop, args=(i,))
            worker.daemon = True
            worker.start()
            self.workers.append(worker)

    def stop(self):
        """Arrêter les workers proprement"""
        if not self.is_running:
            return

        # Envoyer un signal de fin à chaque worker
        for _ in range(self.num_workers):
            self.task_queue.put(None)

        # Attendre que tous les workers terminent
        for worker in self.workers:
            worker.join()

        self.is_running = False
        self.workers = []

    def _worker_loop(self, worker_id):
        """Boucle principale du worker"""
        while True:
            # Obtenir une tâche
            task = self.task_queue.get()

            # Vérifier le signal de fin
            if task is None:
                self.task_queue.task_done()
                break

            func, args, kwargs, task_id = task

            try:
                # Exécuter la fonction
                result = func(*args, **kwargs)
                # Stocker le résultat avec son ID
                self.results_queue.put((task_id, result, None))
            except Exception as e:
                # Stocker l'erreur avec son ID
                self.results_queue.put((task_id, None, e))
            finally:
                self.task_queue.task_done()

    def submit(self, func, *args, **kwargs):
        """Soumettre une tâche au pool"""
        if not self.is_running:
            self.start()

        task_id = hash((func, args, frozenset(kwargs.items()), time.time(), random.random()))
        self.task_queue.put((func, args, kwargs, task_id))
        return task_id

    def get_result(self, block=True, timeout=None):
        """Obtenir un résultat de la file des résultats"""
        return self.results_queue.get(block=block, timeout=timeout)

    def wait_completion(self):
        """Attendre que toutes les tâches soient terminées"""
        self.task_queue.join()

# Utilisation du pool
def example_task(duration, value):
    """Tâche exemple qui attend et retourne une valeur"""
    time.sleep(duration)
    return value * 2

# Tâche qui échoue parfois
def risky_task(value):
    """Une tâche qui échoue aléatoirement"""
    if random.random() < 0.3:
        raise ValueError(f"Échec simulé pour {value}")
    return value * 3

# Créer et démarrer le pool
pool = WorkerPool(num_workers=3)
pool.start()

# Soumettre des tâches
task_ids = []
for i in range(5):
    task_id = pool.submit(example_task, random.uniform(0.5, 2), i)
    task_ids.append(task_id)

    if random.random() < 0.5:
        task_id = pool.submit(risky_task, i * 10)
        task_ids.append(task_id)

print(f"Soumis {len(task_ids)} tâches au pool")

# Traiter les résultats
num_processed = 0
while num_processed < len(task_ids):
    try:
        task_id, result, error = pool.get_result(timeout=5)
        num_processed += 1

        if error:
            print(f"Tâche {task_id} a échoué: {error}")
        else:
            print(f"Tâche {task_id} a réussi: {result}")
    except queue.Empty:
        print("Timeout en attendant les résultats")
        break

# Attendre que toutes les tâches soient terminées
pool.wait_completion()

# Arrêter le pool
pool.stop()
```

### Pipeline

Implémentation d'un pipeline de traitement concurrent:

```python
import threading
import queue
import time
import random

class PipelineStage:
    """Une étape dans un pipeline de traitement"""

    def __init__(self, name, function, next_stage=None, max_queue_size=10):
        self.name = name
        self.function = function
        self.next_stage = next_stage
        self.input_queue = queue.Queue(maxsize=max_queue_size)
        self.thread = None
        self.running = False

    def _process_loop(self):
        """Boucle principale de traitement"""
        while self.running:
            try:
                # Obtenir un élément de la file d'entrée
                item = self.input_queue.get(timeout=0.1)

                if item is None:
                    # Signal de fin
                    self.input_queue.task_done()
                    if self.next_stage:
                        self.next_stage.put(None)
                    break

                # Traiter l'élément
                try:
                    result = self.function(item)

                    # Passer le résultat à l'étape suivante
                    if self.next_stage:
                        self.next_stage.put(result)
                except Exception as e:
                    print(f"Erreur dans l'étape {self.name}: {e}")

                # Marquer l'élément comme traité
                self.input_queue.task_done()

            except queue.Empty:
                # Pas d'élément disponible, continuer la boucle
                continue

    def start(self):
        """Démarrer l'étape du pipeline"""
        if self.thread is not None and self.thread.is_alive():
            return

        self.running = True
        self.thread = threading.Thread(target=self._process_loop)
        self.thread.daemon = True
        self.thread.start()

    def stop(self):
        """Arrêter l'étape du pipeline"""
        self.running = False
        if self.thread:
            self.thread.join()

    def put(self, item):
        """Ajouter un élément à traiter"""
        self.input_queue.put(item)

    def wait(self):
        """Attendre que tous les éléments soient traités"""
        self.input_queue.join()
        if self.next_stage:
            self.next_stage.wait()

class Pipeline:
    """Un pipeline complet de traitement"""

    def __init__(self):
        self.stages = []
        self.first_stage = None
        self.last_stage = None

    def add_stage(self, name, function, max_queue_size=10):
        """Ajouter une étape au pipeline"""
        stage = PipelineStage(name, function, max_queue_size=max_queue_size)

        if self.stages:
            # Connecter la nouvelle étape à la dernière
            self.last_stage.next_stage = stage
        else:
            # C'est la première étape
            self.first_stage = stage

        self.last_stage = stage
        self.stages.append(stage)
        return self

    def start(self):
        """Démarrer tout le pipeline"""
        for stage in self.stages:
            stage.start()
        return self

    def stop(self):
        """Arrêter tout le pipeline"""
        # Envoyer un signal de fin
        if self.first_stage:
            self.first_stage.put(None)

        # Attendre que toutes les étapes terminent
        for stage in self.stages:
            stage.stop()

    def process(self, item):
        """Ajouter un élément pour traitement"""
        if not self.first_stage:
            raise ValueError("Le pipeline est vide")

        self.first_stage.put(item)

    def wait_completion(self):
        """Attendre que tous les éléments soient traités"""
        if self.first_stage:
            self.first_stage.wait()

# Fonctions d'exemple pour le pipeline
def generator(n):
    """Génère n éléments pour le pipeline"""
    for i in range(n):
        yield f"Item-{i}"
        time.sleep(random.uniform(0.1, 0.3))

def stage1_function(item):
    """Première étape: transformer l'élément"""
    print(f"Stage 1: Traitement de {item}")
    time.sleep(random.uniform(0.2, 0.5))
    return f"{item}-transformed"

def stage2_function(item):
    """Deuxième étape: filtrer certains éléments"""
    print(f"Stage 2: Traitement de {item}")
    time.sleep(random.uniform(0.1, 0.3))

    # Filtrer aléatoirement
    if random.random() < 0.3:
        print(f"Stage 2: {item} filtré")
        return None

    return item

def stage3_function(item):
    """Troisième étape: finaliser le traitement"""
    if item is None:
        return None

    print(f"Stage 3: Traitement de {item}")
    time.sleep(random.uniform(0.3, 0.7))
    return f"{item}-finalized"

def final_consumer(item):
    """Consommateur final"""
    if item is None:
        return None

    print(f"Consommateur: Reçu {item}")
    return item

# Création et utilisation du pipeline
pipeline = Pipeline()

# Construction du pipeline
pipeline.add_stage("Stage 1", stage1_function)
pipeline.add_stage("Stage 2", stage2_function)
pipeline.add_stage("Stage 3", stage3_function)
pipeline.add_stage("Consumer", final_consumer)

# Démarrage du pipeline
pipeline.start()

# Traitement des éléments
for item in generator(10):
    pipeline.process(item)

# Attendre que tous les éléments soient traités
pipeline.wait_completion()

# Arrêter le pipeline
pipeline.stop()
```

### Map-Reduce

Implémentation d'un modèle Map-Reduce:

```python
import multiprocessing
import time
import random
import string
import os

def generate_data(size=1000, chunk_size=100):
    """Génère des données à traiter"""
    data = []
    for _ in range(size):
        # Génère une chaîne aléatoire
        text = ''.join(random.choice(string.ascii_letters) for _ in range(10))
        data.append(text)

    # Diviser en chunks
    return [data[i:i + chunk_size] for i in range(0, len(data), chunk_size)]

def map_function(chunk):
    """Fonction de mappage: compte les lettres dans chaque élément"""
    process_id = os.getpid()
    print(f"Mapper {process_id} traite un chunk de {len(chunk)} éléments")

    result = {}
    for item in chunk:
        for char in item:
            if char in result:
                result[char] = result[char] + 1
            else:
                result[char] = 1

    time.sleep(random.uniform(0.2, 0.5))  # Simuler un traitement
    return result

def reduce_function(mapped_results):
    """Fonction de réduction: combine les résultats de mappage"""
    process_id = os.getpid()
    print(f"Reducer {process_id} commence")

    final_result = {}
    for result in mapped_results:
        for key, value in result.items():
            if key in final_result:
                final_result[key] += value
            else:
                final_result[key] = value

    time.sleep(random.uniform(0.3, 0.7))  # Simuler un traitement
    return final_result

def map_reduce(data_chunks, map_function, reduce_function, num_mappers, num_reducers):
    """Exécuter un job MapReduce"""
    # Phase de mappage
    with multiprocessing.Pool(num_mappers) as mapper_pool:
        mapped_results = mapper_pool.map(map_function, data_chunks)

    # Regroupement des résultats pour les reducers
    chunks_per_reducer = max(1, len(mapped_results) // num_reducers)
    reducer_inputs = [mapped_results[i:i + chunks_per_reducer]
                     for i in range(0, len(mapped_results), chunks_per_reducer)]

    # Phase de réduction
    with multiprocessing.Pool(num_reducers) as reducer_pool:
        reduced_results = reducer_pool.map(reduce_function, reducer_inputs)

    # Combinaison finale des résultats
    final_result = reduce_function(reduced_results)
    return final_result

if __name__ == "__main__":
    # Paramètres
    data_size = 10000
    chunk_size = 500
    num_mappers = 4
    num_reducers = 2

    print(f"Génération de {data_size} éléments de données...")
    data_chunks = generate_data(data_size, chunk_size)

    print(f"Démarrage du job MapReduce avec {num_mappers} mappers et {num_reducers} reducers")
    start_time = time.time()

    result = map_reduce(data_chunks, map_function, reduce_function, num_mappers, num_reducers)

    end_time = time.time()

    # Affichage des résultats
    print("\nRésultats:")
    sorted_result = sorted(result.items(), key=lambda x
```

# La Concurrence en Python (Suite)

## Modèles de concurrence avancés (suite)

### Map-Reduce (suite)

```python
    # Affichage des résultats
    print("\nRésultats:")
    sorted_result = sorted(result.items(), key=lambda x: x[1], reverse=True)
    for char, count in sorted_result[:10]:
        print(f"'{char}': {count}")

    print(f"\nTemps total d'exécution: {end_time - start_time:.2f} secondes")
```

### Producteur-Consommateur

Le modèle producteur-consommateur est un pattern classique de concurrence:

```python
import threading
import queue
import time
import random

# File d'attente partagée
task_queue = queue.Queue(maxsize=10)
result_queue = queue.Queue()

# Événement pour signaler aux consommateurs de s'arrêter
stop_event = threading.Event()

def producer(num_tasks):
    """Produit des tâches et les place dans la file d'attente"""
    for i in range(num_tasks):
        task = f"Task-{i}"
        task_queue.put(task)
        print(f"Producteur a créé {task}")
        time.sleep(random.uniform(0.1, 0.3))

    # Signal pour indiquer qu'il n'y a plus de tâches à produire
    stop_event.set()
    print("Producteur a terminé")

def consumer(consumer_id):
    """Consomme des tâches de la file d'attente et les traite"""
    while not (stop_event.is_set() and task_queue.empty()):
        try:
            # Essayer d'obtenir une tâche avec timeout
            task = task_queue.get(timeout=0.1)

            # Traitement simulé
            processing_time = random.uniform(0.2, 0.5)
            time.sleep(processing_time)

            # Résultat du traitement
            result = f"Résultat de {task} (traité par Consumer-{consumer_id} en {processing_time:.2f}s)"
            result_queue.put(result)

            print(f"Consumer-{consumer_id} a traité {task}")
            task_queue.task_done()

        except queue.Empty:
            # Si la file est vide, continuer à vérifier stop_event
            continue

    print(f"Consumer-{consumer_id} a terminé")

def result_collector():
    """Collecte et affiche les résultats"""
    results = []
    while True:
        try:
            # Essayer d'obtenir un résultat avec timeout
            result = result_queue.get(timeout=0.1)
            results.append(result)
            result_queue.task_done()

            # Si tous les producteurs ont terminé et que nous avons traité tous les résultats attendus
            if stop_event.is_set() and task_queue.empty() and len(results) == NUM_TASKS:
                break

        except queue.Empty:
            # Si la file est vide mais qu'il reste des tâches à traiter, continuer
            if not (stop_event.is_set() and task_queue.empty()):
                continue
            # Si les producteurs et consommateurs ont terminé, sortir
            if all_consumers_done.is_set():
                break

    print(f"\nRésultats collectés ({len(results)}):")
    for i, result in enumerate(results[:5]):
        print(f"  {i+1}. {result}")

    if len(results) > 5:
        print(f"  ... et {len(results) - 5} autres résultats")

# Signal pour indiquer que tous les consommateurs ont terminé
all_consumers_done = threading.Event()

# Paramètres
NUM_TASKS = 20
NUM_CONSUMERS = 3

# Création et démarrage des threads
producer_thread = threading.Thread(target=producer, args=(NUM_TASKS,))
consumer_threads = [threading.Thread(target=consumer, args=(i,)) for i in range(NUM_CONSUMERS)]
collector_thread = threading.Thread(target=result_collector)

producer_thread.start()
for t in consumer_threads:
    t.start()
collector_thread.start()

# Attendre que le producteur termine
producer_thread.join()

# Attendre que tous les consommateurs terminent
for t in consumer_threads:
    t.join()

# Signaler que tous les consommateurs ont terminé
all_consumers_done.set()

# Attendre le collecteur de résultats
collector_thread.join()

print("Tous les threads ont terminé")
```

### Pub-Sub (Publication-Souscription)

Le modèle Pub-Sub permet de découpler les émetteurs et les récepteurs de messages:

```python
import threading
import queue
import time
import random

class PubSubBroker:
    """Broker pour le modèle Publication-Souscription"""

    def __init__(self):
        # Dictionnaire pour stocker les files d'attente des souscripteurs
        self.subscribers = {}
        self.lock = threading.RLock()

    def subscribe(self, topic, subscriber_id):
        """Souscrit à un topic"""
        with self.lock:
            if topic not in self.subscribers:
                self.subscribers[topic] = {}

            # Créer une nouvelle file d'attente pour ce souscripteur
            self.subscribers[topic][subscriber_id] = queue.Queue()
            return self.subscribers[topic][subscriber_id]

    def unsubscribe(self, topic, subscriber_id):
        """Annule la souscription à un topic"""
        with self.lock:
            if topic in self.subscribers and subscriber_id in self.subscribers[topic]:
                self.subscribers[topic].pop(subscriber_id)
                # Si plus de souscripteurs pour ce topic, supprimer le topic
                if not self.subscribers[topic]:
                    self.subscribers.pop(topic)
                return True
        return False

    def publish(self, topic, message):
        """Publie un message sur un topic"""
        with self.lock:
            if topic not in self.subscribers:
                return 0  # Aucun souscripteur

            # Distribuer le message à tous les souscripteurs
            subscriber_count = 0
            for subscriber_queue in self.subscribers[topic].values():
                subscriber_queue.put(message)
                subscriber_count += 1

            return subscriber_count

# Créer un broker
broker = PubSubBroker()

def publisher(topics, num_messages):
    """Fonction pour le thread de publication"""
    for i in range(num_messages):
        # Choisir un topic aléatoire
        topic = random.choice(topics)

        # Créer et publier un message
        message = f"Message-{i} sur {topic}"
        subscribers = broker.publish(topic, message)

        print(f"Publié: {message} à {subscribers} souscripteurs")
        time.sleep(random.uniform(0.1, 0.5))

    # Publier un message de fin pour chaque topic
    for topic in topics:
        broker.publish(topic, None)  # Signal de fin
        print(f"Signal de fin publié sur {topic}")

def subscriber(name, topics, process_time=0.2):
    """Fonction pour le thread de souscription"""
    # Souscription aux topics
    subscriber_queues = {}
    for topic in topics:
        subscriber_queues[topic] = broker.subscribe(topic, name)
        print(f"{name} s'est souscrit à {topic}")

    # Traitement des messages
    received_messages = []
    active_topics = set(topics)

    while active_topics:
        for topic in list(active_topics):
            try:
                # Essayer d'obtenir un message avec timeout
                message = subscriber_queues[topic].get(timeout=0.1)

                # Vérifier si c'est un signal de fin
                if message is None:
                    active_topics.remove(topic)
                    print(f"{name} a reçu le signal de fin pour {topic}")
                    continue

                # Traiter le message
                time.sleep(process_time)  # Simuler le traitement
                received_messages.append(f"{name} a traité '{message}'")

                print(f"{name} a reçu et traité sur {topic}: {message}")

                # Marquer le message comme traité
                subscriber_queues[topic].task_done()

            except queue.Empty:
                # Pas de message, continuer à vérifier
                continue

    # Se désabonner des topics
    for topic in topics:
        broker.unsubscribe(topic, name)
        print(f"{name} s'est désinscrit de {topic}")

    return received_messages

# Paramètres
ALL_TOPICS = ["news", "sports", "technology", "entertainment"]
NUM_MESSAGES = 15

# Créer et démarrer les threads des souscripteurs
subscriber_threads = []
subscribers_data = [
    ("Subscriber-1", ["news", "sports"]),
    ("Subscriber-2", ["sports", "technology"]),
    ("Subscriber-3", ["news", "entertainment"]),
    ("Subscriber-4", ALL_TOPICS)
]

for name, topics in subscribers_data:
    thread = threading.Thread(target=subscriber, args=(name, topics))
    thread.start()
    subscriber_threads.append(thread)

# Laisser les souscripteurs se connecter
time.sleep(1)

# Créer et démarrer le thread du publieur
publisher_thread = threading.Thread(target=publisher, args=(ALL_TOPICS, NUM_MESSAGES))
publisher_thread.start()

# Attendre la fin du publieur
publisher_thread.join()

# Attendre la fin de tous les souscripteurs
for thread in subscriber_threads:
    thread.join()

print("Tous les threads ont terminé")
```

## Débugger le code concurrent

### Traçage et logging

Mettre en place un système de logging efficace pour les applications concurrentes:

```python
import logging
import threading
import time
import random
import concurrent.futures
import queue

# Configuration du logging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(threadName)s - %(levelname)s - %(message)s',
    datefmt='%H:%M:%S'
)

logger = logging.getLogger('concurrency_demo')

# Handler pour écrire les logs dans un fichier
file_handler = logging.FileHandler('concurrency.log')
file_handler.setLevel(logging.DEBUG)
file_formatter = logging.Formatter('%(asctime)s - %(threadName)s - [%(levelname)s] - %(message)s')
file_handler.setFormatter(file_formatter)
logger.addHandler(file_handler)

def worker_function(name, duration):
    """Fonction exécutée par un thread worker"""
    thread_id = threading.get_ident()
    logger.info(f"Worker {name} (ID: {thread_id}) démarre")

    try:
        # Simulation de travail
        logger.debug(f"Worker {name} exécute une tâche de {duration:.2f}s")
        time.sleep(duration)

        # Simuler une erreur occasionnelle
        if random.random() < 0.3:
            raise ValueError(f"Erreur simulée dans worker {name}")

        logger.info(f"Worker {name} a terminé sa tâche")
        return f"Résultat de {name}"

    except Exception as e:
        logger.error(f"Worker {name} a rencontré une erreur: {str(e)}", exc_info=True)
        raise

    finally:
        logger.debug(f"Worker {name} nettoie ses ressources")

def main():
    """Fonction principale qui démontre le logging avec la concurrence"""
    logger.info("Démarrage du programme principal")

    # Utilisation de ThreadPoolExecutor avec logging
    with concurrent.futures.ThreadPoolExecutor(max_workers=3) as executor:
        logger.debug("ThreadPoolExecutor créé avec 3 workers")

        future_to_name = {}

        # Soumettre des tâches
        for i in range(5):
            name = f"Task-{i}"
            duration = random.uniform(0.5, 2)

            logger.debug(f"Soumission de {name} avec durée {duration:.2f}s")
            future = executor.submit(worker_function, name, duration)
            future_to_name[future] = name

        logger.info(f"Toutes les tâches ont été soumises, en attente des résultats")

        # Traiter les résultats au fur et à mesure qu'ils sont disponibles
        for future in concurrent.futures.as_completed(future_to_name):
            name = future_to_name[future]

            try:
                result = future.result()
                logger.info(f"Tâche {name} terminée avec succès: {result}")
            except Exception as e:
                logger.warning(f"Tâche {name} a échoué: {str(e)}")

    logger.info("Toutes les tâches sont terminées, fin du programme")

if __name__ == "__main__":
    main()
```

### Race conditions

Détecter et corriger les conditions de concurrence:

```python
import threading
import time
import random

# Exemple de race condition
counter = 0
counter_lock = threading.Lock()

def increment_unsafe(n):
    """Incrémente le compteur sans protection (race condition)"""
    global counter
    local_counter = counter

    # Simuler un traitement
    time.sleep(random.uniform(0, 0.01))

    local_counter += n
    counter = local_counter

def increment_safe(n):
    """Incrémente le compteur avec protection par lock"""
    global counter

    with counter_lock:
        local_counter = counter
        # Simuler un traitement
        time.sleep(random.uniform(0, 0.01))
        local_counter += n
        counter = local_counter

def run_threads(increment_func, n_threads=10, increment_value=1):
    """Exécute plusieurs threads qui incrémentent le compteur"""
    global counter
    counter = 0

    threads = []
    for _ in range(n_threads):
        t = threading.Thread(target=increment_func, args=(increment_value,))
        threads.append(t)
        t.start()

    for t in threads:
        t.join()

    return counter

# Test avec une fonction non sécurisée
print("Test avec increment_unsafe:")
final_unsafe = run_threads(increment_unsafe, n_threads=100, increment_value=1)
print(f"Valeur finale (non sécurisée): {final_unsafe}")
print(f"Valeur attendue: 100")

# Test avec une fonction sécurisée
print("\nTest avec increment_safe:")
final_safe = run_threads(increment_safe, n_threads=100, increment_value=1)
print(f"Valeur finale (sécurisée): {final_safe}")
print(f"Valeur attendue: 100")
```

**Utilisation de ThreadSanitizer (pour CPython compilé avec support):**

```python
"""
Pour utiliser ThreadSanitizer avec Python, vous devez:
1. Compiler Python avec le support de ThreadSanitizer
2. Exécuter votre script avec les variables d'environnement appropriées

Exemple de compilation (sur systèmes Unix):
$ CC="clang -fsanitize=thread" LDSHARED="clang -fsanitize=thread -shared" ./configure
$ make -j

Ensuite, pour exécuter:
$ PYTHONMALLOC=malloc ./python -m tsan_example.py

Note: Ceci est un exemple conceptuel. ThreadSanitizer n'est pas directement disponible
pour les builds standard de Python.
"""

import threading

# Variable partagée
shared_variable = 0

def increment():
    global shared_variable
    # Race condition ici:
    local_copy = shared_variable
    local_copy += 1
    shared_variable = local_copy

# Créer de nombreux threads pour augmenter les chances de détecter la race condition
threads = []
for _ in range(1000):
    t = threading.Thread(target=increment)
    threads.append(t)
    t.start()

for t in threads:
    t.join()

print(f"Valeur finale: {shared_variable}")
```

### Profilage du code concurrent

```python
import threading
import time
import cProfile
import pstats
import io
import random
from concurrent.futures import ThreadPoolExecutor

def cpu_intensive_task(n):
    """Tâche qui consomme du CPU"""
    result = 0
    for i in range(n):
        result += i ** 2
    return result

def io_intensive_task(n):
    """Tâche qui simule des E/S"""
    time.sleep(n)
    return f"Completed in {n}s"

def profile_sequential():
    """Exécute et profile des tâches séquentiellement"""
    results = []
    for i in range(10):
        if i % 2 == 0:
            results.append(cpu_intensive_task(10**6))
        else:
            results.append(io_intensive_task(0.5))
    return results

def profile_threaded():
    """Exécute et profile des tâches avec des threads"""
    results = []
    threads = []

    for i in range(10):
        if i % 2 == 0:
            t = threading.Thread(target=lambda: results.append(cpu_intensive_task(10**6)))
        else:
            t = threading.Thread(target=lambda: results.append(io_intensive_task(0.5)))
        threads.append(t)
        t.start()

    for t in threads:
        t.join()

    return results

def profile_thread_pool():
    """Exécute et profile des tâches avec ThreadPoolExecutor"""
    with ThreadPoolExecutor(max_workers=4) as executor:
        # Préparer les tâches
        tasks = []
        for i in range(10):
            if i % 2 == 0:
                tasks.append(executor.submit(cpu_intensive_task, 10**6))
            else:
                tasks.append(executor.submit(io_intensive_task, 0.5))

        # Récupérer les résultats
        return [task.result() for task in tasks]

def run_profile(func, func_name):
    """Profile une fonction et affiche les résultats"""
    # Configuration du profiler
    pr = cProfile.Profile()
    pr.enable()

    # Exécuter la fonction
    start = time.time()
    result = func()
    end = time.time()

    # Arrêter le profiler
    pr.disable()

    # Afficher les résultats du profiler
    s = io.StringIO()
    ps = pstats.Stats(pr, stream=s).sort_stats('cumulative')
    ps.print_stats(10)  # Afficher les 10 fonctions les plus coûteuses

    print(f"\n--- Profiling de {func_name} ---")
    print(f"Temps total: {end - start:.4f}s")
    print(s.getvalue())
    return result

# Exécuter le profilage pour chaque approche
run_profile(profile_sequential, "Exécution séquentielle")
run_profile(profile_threaded, "Exécution avec threads")
run_profile(profile_thread_pool, "Exécution avec ThreadPoolExecutor")
```

### Outils de debug

**Utilisation de pdb pour déboguer des threads:**

```python
import threading
import time
import random
import pdb

def worker(name):
    """Fonction de travail pour un thread"""
    print(f"Worker {name} démarré")

    # Simuler un traitement
    for i in range(5):
        # Point d'arrêt conditionnel
        if i == 3 and name == "Thread-2":
            # Insérer un point d'arrêt pour le débogage
            pdb.set_trace()

        print(f"Worker {name}: étape {i}")
        time.sleep(random.uniform(0.1, 0.3))

    print(f"Worker {name} terminé")

# Note: Le débogage interactif des threads est complexe car pdb interagit
# seulement avec le thread qui a appelé set_trace().
# Il est souvent plus pratique de déboguer avec des journaux détaillés.

# Créer et démarrer les threads
threads = []
for i in range(3):
    t = threading.Thread(target=worker, args=(f"Thread-{i}",))
    threads.append(t)
    t.start()

for t in threads:
    t.join()
```

**Utilisation de faulthandler pour traquer les deadlocks:**

```python
import threading
import time
import faulthandler
import signal

# Activer faulthandler pour afficher les traces en cas de crash
faulthandler.enable()

# Configuration pour dumper les traces de threads après un timeout
def timeout_handler(signum, frame):
    # Forcer le dump des traces de threads
    faulthandler.dump_traceback(all_threads=True)
    print("Timeout - possible deadlock détecté!")

# Configurer un gestionnaire pour SIGALRM
signal.signal(signal.SIGALRM, timeout_handler)

# Simuler un deadlock
def thread_1():
    print("Thread 1: Acquisition du lock A")
    with lock_a:
        print("Thread 1: Lock A acquis")
        time.sleep(0.5)  # Augmenter les chances de deadlock

        print("Thread 1: Tentative d'acquisition du lock B")
        with lock_b:
            print("Thread 1: Lock B acquis")
            print("Thread 1: Traitement terminé")

def thread_2():
    print("Thread 2: Acquisition du lock B")
    with lock_b:
        print("Thread 2: Lock B acquis")
        time.sleep(0.5)  # Augmenter les chances de deadlock

        print("Thread 2: Tentative d'acquisition du lock A")
        with lock_a:
            print("Thread 2: Lock A acquis")
            print("Thread 2: Traitement terminé")

# Créer les locks
lock_a = threading.Lock()
lock_b = threading.Lock()

# Créer et démarrer les threads
t1 = threading.Thread(target=thread_1)
t2 = threading.Thread(target=thread_2)

t1.start()
t2.start()

# Définir une alarme de 3 secondes pour détecter un deadlock
signal.alarm(3)

try:
    t1.join()
    t2.join()
    print("Tous les threads ont terminé normalement")
finally:
    # Annuler l'alarme
    signal.alarm(0)
```

## Comparaison des approches

### Threads vs Processus vs Asyncio

Benchmark comparatif des différentes approches:

```python
import time
import threading
import multiprocessing
import asyncio
import concurrent.futures
import requests
import aiohttp
import statistics
import math

# Tâches pour les benchmarks

def cpu_intensive(n):
    """Tâche qui consomme du CPU (calcul des n premiers nombres premiers)"""
    primes = []
    for num in range(2, n + 1):
        for i in range(2, int(math.sqrt(num)) + 1):
            if num % i == 0:
                break
        else:
            primes.append(num)
    return len(primes)

def io_intensive_sync(url):
    """Tâche d'E/S synchrone (requête HTTP)"""
    response = requests.get(url)
    return len(response.content)

async def io_intensive_async(url, session):
    """Tâche d'E/S asynchrone (requête HTTP)"""
    async with session.get(url) as response:
        content = await response.read()
        return len(content)

# Implémentations des différentes approches

def run_sequential(func, inputs):
    """Exécution séquentielle"""
    start = time.time()
    results = [func(x) for x in inputs]
    end = time.time()
    return results, end - start

def run_threaded(func, inputs, max_workers=4):
    """Exécution avec threads"""
    start = time.time()
    with concurrent.futures.ThreadPoolExecutor(max_workers=max_workers) as executor:
        results = list(executor.map(func, inputs))
    end = time.time()
    return results, end - start

def run_processes(func, inputs, max_workers=4):
    """Exécution avec processus"""
    start = time.time()
    with concurrent.futures.ProcessPoolExecutor(max_workers=max_workers) as executor:
        results = list(executor.map(func, inputs))
    end = time.time()
    return results, end - start

async def run_asyncio(func, inputs, max_workers=4):
    """Exécution avec asyncio"""
    start = time.time()
    async with aiohttp.ClientSession() as session:
        tasks = [func(x, session) for x in inputs]
        results = await asyncio.gather(*tasks)
    end = time.time()
    return results, end - start

def benchmark(title, sequential_func, threaded_func, process_func, async_func=None,
             inputs=None, repetitions=3):
    """Exécute un benchmark complet avec toutes les approches"""
    print(f"\n--- {title} ---")

    # Exécuter plusieurs fois et calculer la moyenne et l'écart-type
    sequential_times = []
    threaded_times = []
    process_times = []
    async_times = []

    for i in range(repetitions):
        print(f"Répétition {i+1}/{repetitions}")

        # Séquentiel
        _, seq_time = sequential_func(inputs)
        sequential_times.append(seq_time)

        # Threads
        _, thread_time = threaded_func(inputs)
        threaded_times.append(thread_time)

        # Processus
        _, process_time = process_func(inputs)
        process_times.append(process_time)

        # Asyncio (s'il est fourni)
        if async_func:
            _, async_time = asyncio.run(async_func(inputs))
            async_times.append(async_time)

    # Calculer les moyennes et écarts-types
    seq_mean = statistics.mean(sequential_times)
    thread_mean = statistics.mean(threaded_times)
    process_mean = statistics.mean(process_times)

    print(f"\nRésultats ({repetitions} répétitions):")
    print(f"Séquentiel  : {seq_mean:.4f}s (± {statistics.stdev(sequential_times):.4f}s)")
    print(f"Threads     : {thread_mean:.4f}s (± {statistics.stdev(threaded_times):.4f}s)")
    print(f"Processus   : {process_mean:.4f}s (± {statistics.stdev(process_times):.4f}s)")

    print("\nRapports de performance:")
    print(f"Threads/Séquentiel  : {seq_mean/thread_mean:.2f}x")
    print(f"Processus/Séquentiel: {seq_mean/process_mean:.2f}x")

    if async_func:
        async_mean = statistics.mean(async_times)
        print(f"Asyncio     : {async_mean:.4f}s (± {statistics.stdev(async_times):.4f}s)")
        print(f"Asyncio/Séquentiel  : {seq_mean/async_mean:.2f}x")

def perform_benchmarks():
    """Exécuter tous les benchmarks"""
    # Benchmark pour tâches CPU-bound
    cpu_inputs = [10**5] * 8  # 8 tâches identiques

    sequential_cpu = lambda inputs: run_sequential(cpu_intensive, inputs)
    threaded_cpu = lambda inputs: run_threaded(cpu_intensive, inputs, max_workers=8)
    process_cpu = lambda inputs: run_processes(cpu_intensive, inputs, max_workers=8)

    benchmark("Tâches CPU-intensives", sequential_cpu, threaded_cpu, process_cpu,
             inputs=cpu_inputs, repetitions=3)

    # Benchmark pour tâches I/O-bound
    io_urls = [
        "https://www.python.org",
        "https://www.github.com",
        "https://www.wikipedia.org",
        "https://www.stackoverflow.com",
        "https://www.reddit.com",
        "https://www.amazon.com",
        "https://www.twitter.com",
        "https://www.facebook.com"
    ]

    sequential_io = lambda inputs: run_sequential(io_intensive_sync, inputs)
    threaded_io = lambda inputs: run_threaded(io_intensive_sync, inputs, max_workers=8)
    process_io = lambda inputs: run_processes(io_intensive_sync, inputs, max_workers=8)
    async_io = lambda inputs: run_asyncio(io_intensive_async, inputs, max_workers=8)

    benchmark("Tâches I/O-intensives", sequential_io, threaded_io, process_io, async_io,
             inputs=io_urls, repetitions=3)

if __name__ == "__main__":
    perform_benchmarks()
```

### Comparaison de performance

Tableau de comparaison des approches pour différents types de tâches:

| Type de tâche      | Threads                  | Processus                 | Asyncio                  |
| ------------------ | ------------------------ | ------------------------- | ------------------------ |
| CPU-intensif       | Limité par le GIL        | Excellente performance    | Faible performance       |
| E/S-intensif       | Bonne performance        | Surcoût de création       | Excellente performance   |
| Mixte              | Correcte                 | Bonne pour dominante CPU  | Bonne pour dominante E/S |
| Mémoire partagée   | Facile (mémoire commune) | Complexe (IPC nécessaire) | Facile (thread unique)   |
| Complexité de code | Moyenne                  | Moyenne                   | Faible à moyenne         |
| Scalabilité        | Limitée par le GIL       | Excellente (multi-cœurs)  | Excellente (E/S)         |
| Coût de création   | Faible                   | Élevé                     | Très faible              |
| Isolement          | Faible                   | Excellent                 | Faible                   |

### Choix du bon outil

Guide pour choisir la meilleure approche selon le cas d'usage:

```python
def recommend_concurrency_approach(task_type, task_count, complexity, memory_usage, system_type):
    """
    Recommande une approche de concurrence basée sur les caractéristiques de la tâche.

    Args:
        task_type: "cpu", "io", "mixed"
        task_count: Nombre approximatif de tâches
        complexity: "simple", "moderate", "complex"
        memory_usage: "low", "medium", "high"
        system_type: "single-core", "multi-core", "distributed"

    Returns:
        Recommandation avec justification
    """
    recommendation = []

    # Tâches CPU-intensives
    if task_type == "cpu":
        if system_type == "single-core":
            recommendation.append(("threads", "Faible, limité par le GIL"))
            recommendation.append(("asyncio", "Très faible, conçu pour E/S"))
            recommendation.append(("processus", "Modéré, surcoût de création"))
        elif system_type == "multi-core":
            recommendation.append(("processus", "Excellent, utilise tous les cœurs"))
            recommendation.append(("threads", "Faible, limité par le GIL"))
            recommendation.append(("asyncio", "Très faible, conçu pour E/S"))

    # Tâches E/S-intensives
    elif task_type == "io":
        if task_count <= 10:
            recommendation.append(("threads", "Bon, simple à implémenter pour peu de tâches"))
            recommendation.append(("asyncio", "Excellent, mais peut être excessif pour peu de tâches"))
            recommendation.append(("processus", "Inefficace, surcoût trop important"))
        elif task_count <= 100:
            recommendation.append(("asyncio", "Excellent, efficace pour E/S multiples"))
            recommendation.append(("threads", "Bon, mais moins efficient qu'asyncio"))
            recommendation.append(("processus", "Inefficace pour E/S pures"))
        else:  # task_count > 100
            recommendation.append(("asyncio", "Excellent, très efficient pour nombreuses E/S"))
            recommendation.append(("threads", "Correct, mais limité en scaling"))
            recommendation.append(("processus", "Faible, trop de surcoût"))

    # Tâches mixtes (CPU + E/S)
    elif task_type == "mixed":
        if memory_usage == "high":
            recommendation.append(("threads", "Bon, partage de mémoire efficace"))
            recommendation.append(("hybride", "Excellent, threads pour E/S + processus pour CPU"))
            recommendation.append(("processus", "Modéré, coût mémoire plus élevé"))
        else:
            recommendation.append(("hybride", "Excellent, asyncio + processus ou threads + processus"))
            recommendation.append(("processus", "Bon si CPU domine"))
            recommendation.append(("asyncio", "Bon si E/S domine"))

    # Considérations additionnelles
    if complexity == "complex" and memory_usage == "high":
        print("Note: Pour les tâches complexes avec utilisation mémoire élevée, ")
        print("      considérez fortement les optimisations de code et algorithmes.")

    if system_type == "distributed":
        print("Note: Pour les systèmes distribués, considérez des frameworks comme ")
        print("      Dask, Ray, ou Celery en plus des approches standards.")

    # Formater la recommandation
    print(f"\nRecommandations pour: {task_type}, {task_count} tâches, {complexity}, {memory_usage} mémoire, {system_type}")
    print("Classement des approches (meilleure à moins bonne):")
    for i, (approach, reason) in enumerate(recommendation, 1):
        print(f"{i}. {approach.capitalize()}: {reason}")

    return recommendation[0][0]  # Retourner la meilleure approche

# Exemples d'utilisation
print("\n=== Exemples de recommandations ===")

recommend_concurrency_approach("cpu", 20, "moderate", "medium", "multi-core")
recommend_concurrency_approach("io", 50, "simple", "low", "single-core")
recommend_concurrency_approach("mixed", 30, "complex", "high", "multi-core")
```

## Interopérabilité

### Combiner asyncio et threads

```python
import asyncio
import concurrent.futures
import time
import threading
import requests

def cpu_bound_task(n):
    """Tâche intensive en CPU exécutée dans un thread"""
    print(f"Tâche CPU démarre dans le thread {threading.current_thread().name}")
    result = sum(i * i for i in range(n))
    print(f"Tâche CPU terminée dans le thread {threading.current_thread().name}")
    return result

def io_bound_task_blocking(url):
    """Tâche d'E/S bloquante exécutée dans un thread"""
    print(f"Tâche E/S bloquante démarre dans le thread {threading.current_thread().name}")
    response = requests.get(url)
    print(f"Tâche E/S bloquante terminée dans le thread {threading.current_thread().name}")
    return response.status_code

async def io_bound_task_async(url):
    """Tâche d'E/S asynchrone exécutée dans le thread principal"""
    print(f"Tâche E/S asynchrone démarre dans le thread {threading.current_thread().name}")
    await asyncio.sleep(1)  # Simuler une opération asynchrone
    print(f"Tâche E/S asynchrone terminée dans le thread {threading.current_thread().name}")
    return f"Résultat de {url}"

async def main():
    print(f"Coroutine principale démarre dans le thread {threading.current_thread().name}")

    # Créer un pool de threads pour les tâches bloquantes
    with concurrent.futures.ThreadPoolExecutor(max_workers=3) as executor:
        loop = asyncio.get_event_loop()

        # Exécuter des tâches CPU-bound dans des threads
        cpu_tasks = [
            loop.run_in_executor(executor, cpu_bound_task, 10**7),
            loop.run_in_executor(executor, cpu_bound_task, 10**7)
        ]

        # Exécuter des tâches I/O-bound bloquantes dans des threads
        io_tasks_blocking = [
            loop.run_in_executor(executor, io_bound_task_blocking, "https://www.python.org"),
            loop.run_in_executor(executor, io_bound_task_blocking, "https://www.github.com")
        ]

        # Exécuter des tâches I/O-bound asynchrones dans le thread principal
        io_tasks_async = [
            io_bound_task_async("https://example.com/1"),
            io_bound_task_async("https://example.com/2")
        ]

        # Combiner toutes les tâches
        all_tasks = cpu_tasks + io_tasks_blocking + io_tasks_async

        # Attendre que toutes les tâches se terminent
        results = await asyncio.gather(*all_tasks)

        print("\nRésultats:")
        for i, result in enumerate(results):
            print(f"Tâche {i}: {result}")

    print(f"Coroutine principale terminée dans le thread {threading.current_thread().name}")

# Exécuter la coroutine principale
asyncio.run(main())
```

### Combiner asyncio et processus

```python
import asyncio
import concurrent.futures
import time
import os
import multiprocessing

def cpu_intensive_task(n):
    """Tâche intensive en CPU à exécuter dans un processus séparé"""
    pid = os.getpid()
    print(f"Tâche CPU démarre dans le processus {pid}")

    result = 0
    for i in range(n):
        result += i ** 2

    print(f"Tâche CPU terminée dans le processus {pid}")
    return result

async def run_in_process_pool(pool, func, *args):
    """Exécute une fonction dans un ProcessPool de manière asynchrone"""
    loop = asyncio.get_running_loop()
    return await loop.run_in_executor(pool, func, *args)

async def async_io_task(name, delay):
    """Tâche d'E/S asynchrone"""
    print(f"Tâche async {name} démarre dans le processus {os.getpid()}")
    await asyncio.sleep(delay)
    print(f"Tâche async {name} termine après {delay}s")
    return f"Résultat de {name}"

async def main():
    print(f"Processus principal: {os.getpid()}")

    # Créer un ProcessPoolExecutor
    with concurrent.futures.ProcessPoolExecutor() as process_pool:
        # Tâches CPU-intensives à exécuter dans des processus
        cpu_tasks = [
            run_in_process_pool(process_pool, cpu_intensive_task, 10**7),
            run_in_process_pool(process_pool, cpu_intensive_task, 10**7),
            run_in_process_pool(process_pool, cpu_intensive_task, 10**7)
        ]

        # Tâches E/S asynchrones à exécuter dans le processus principal
        io_tasks = [
            async_io_task("IO-A", 1),
            async_io_task("IO-B", 2),
            async_io_task("IO-C", 1.5)
        ]

        # Combiner et exécuter toutes les tâches
        print("Attente de toutes les tâches...")
        all_results = await asyncio.gather(*cpu_tasks, *io_tasks)

        # Afficher les résultats
        cpu_results = all_results[:len(cpu_tasks)]
        io_results = all_results[len(cpu_tasks):]

        print("\nRésultats des tâches CPU:")
        for i, result in enumerate(cpu_results):
            print(f"CPU Task {i}: {result}")

        print("\nRésultats des tâches IO:")
        for i, result in enumerate(io_results):
            print(f"IO Task {i}: {result}")

if __name__ == "__main__":
    # Démarrer le point d'entrée asynchrone
    asyncio.run(main())
```

### Combiner threads et processus

```python
import concurrent.futures
import threading
import multiprocessing
import time
import os
import random
import queue

# File d'attente pour la communication entre les threads et les processus
task_queue = multiprocessing.Queue()
result_queue = multiprocessing.Queue()

def cpu_task(n):
    """Tâche intensive en CPU pour exécution dans un processus"""
    pid = os.getpid()
    tid = threading.get_ident()
    print(f"Tâche CPU démarre (PID: {pid}, Thread: {tid})")

    # Calcul intensif
    result = sum(i ** 2 for i in range(n))

    time.sleep(random.uniform(0.5, 1.5))  # Simuler un temps de traitement variable
    print(f"Tâche CPU termine (PID: {pid}, Thread: {tid})")
    return result

def io_task(delay):
    """Tâche d'E/S pour exécution dans un thread"""
    pid = os.getpid()
    tid = threading.get_ident()
    print(f"Tâche E/S démarre (PID: {pid}, Thread: {tid})")

    # Simuler une opération d'E/S bloquante
    time.sleep(delay)

    print(f"Tâche E/S termine (PID: {pid}, Thread: {tid})")
    return f"IO completed after {delay}s"

def process_worker(task_q, result_q):
    """Fonction de travail pour un processus"""
    pid = os.getpid()
    print(f"Processus worker démarré (PID: {pid})")

    while True:
        try:
            # Obtenir une tâche de la file avec timeout
            task_id, n = task_q.get(timeout=2)

            # Si signal de fin, sortir
            if task_id is None:
                break

            # Exécuter la tâche CPU
            result = cpu_task(n)

            # Mettre le résultat dans la file des résultats
            result_q.put((task_id, result))

        except queue.Empty:
            print(f"Timeout dans le processus {pid}, sortie")
            break

    print(f"Processus worker terminé (PID: {pid})")

def thread_io_worker(task_list, results):
    """Fonction de travail pour un thread d'E/S"""
    tid = threading.get_ident()
    print(f"Thread IO worker démarré (ID: {tid})")

    for task_id, delay in task_list:
        result = io_task(delay)
        results.append((task_id, result))

    print(f"Thread IO worker terminé (ID: {tid})")

def main():
    print(f"Processus principal (PID: {os.getpid()})")

    # Paramètres
    num_processes = 3
    num_cpu_tasks = 6
    num_io_tasks = 8

    # Préparer les tâches
    cpu_tasks = [(f"CPU-{i}", random.randint(10**6, 10**7)) for i in range(num_cpu_tasks)]
    io_tasks = [(f"IO-{i}", random.uniform(0.5, 2)) for i in range(num_io_tasks)]

    # Répartir les tâches d'E/S parmi les threads
    threads_io_tasks = []
    tasks_per_thread = num_io_tasks // 3
    for i in range(0, num_io_tasks, tasks_per_thread):
        threads_io_tasks.append(io_tasks[i:i+tasks_per_thread])

    # Démarrer les processus pour les tâches CPU
    processes = []
    for _ in range(num_processes):
        p = multiprocessing.Process(target=process_worker, args=(task_queue, result_queue))
        p.start()
        processes.append(p)

    # Mettre les tâches CPU dans la file
    for task_id, n in cpu_tasks:
        task_queue.put((task_id, n))

    # Démarrer les threads pour les tâches E/S
    thread_results = []
    io_threads = []
    for task_list in threads_io_tasks:
        thread_result = []
        thread_results.append(thread_result)
        t = threading.Thread(target=thread_io_worker, args=(task_list, thread_result))
        t.start()
        io_threads.append(t)

    # Signaux de fin pour les processus
    for _ in range(num_processes):
        task_queue.put((None, None))

    # Attendre les threads E/S
    for t in io_threads:
        t.join()

    # Attendre les processus CPU
    for p in processes:
        p.join()

    # Récupérer les résultats CPU
    cpu_results = []
    while not result_queue.empty():
        cpu_results.append(result_queue.get())

    # Collecter tous les résultats E/S
    io_results = []
    for result_list in thread_results:
        io_results.extend(result_list)

    # Afficher les résultats
    print("\nRésultats des tâches CPU:")
    for task_id, result in cpu_results:
        print(f"  {task_id}: {result}")

    print("\nRésultats des tâches E/S:")
    for task_id, result in io_results:
        print(f"  {task_id}: {result}")

if __name__ == "__main__":
    main()
```

## Bonnes pratiques

1. **Choisir la bonne approche pour chaque type de tâche**

   - Threads pour les tâches limitées par les E/S
   - Processus pour les tâches intensives en CPU
   - Asyncio pour les systèmes nécessitant une haute concurrence d'E/S

2. **Éviter le partage excessif d'état**

   - Minimiser les données partagées entre threads
   - Préférer les queues et les primitives de synchronisation aux variables globales
   - Pour les processus, utiliser des mécanismes de communication explicites

3. **Sécuriser l'accès aux ressources partagées**

   - Toujours utiliser des locks pour protéger l'accès aux données partagées entre threads
   - Préférer les structures thread-safe (`Queue`, etc.) quand possible
   - Éviter les opérations non atomiques sur des variables partagées

4. **Gérer correctement les ressources**

   - Utiliser des gestionnaires de contexte (`with`) pour les pools de threads/processus
   - Fermer explicitement les ressources (files, sockets, etc.)
   - Vérifier qu'aucun thread ou processus n'est laissé actif à la fin du programme

5. **Éviter les deadlocks**

   - Acquérir les locks toujours dans le même ordre
   - Utiliser des timeouts pour l'acquisition des locks
   - Préférer `RLock` à `Lock` si la même thread a besoin d'acquérir le même lock plusieurs fois

6. **Limiter la complexité**

   - Diviser les tâches en unités plus petites et indépendantes
   - Éviter les dépendances complexes entre threads ou processus
   - Utiliser des abstractions de haut niveau (pools, queues) plutôt que des primitives de bas niveau

7. **Mettre en place un bon système de logging**

   - Inclure des identifiants de threads/processus dans les logs
   - Journaliser les événements clés (démarrage, arrêt, erreurs)
   - Utiliser des niveaux de log appropriés (DEBUG, INFO, WARNING, ERROR)

8. **Gérer les exceptions efficacement**

   - Capturer et logger les exceptions dans chaque thread/processus
   - Utiliser `excepthook` pour les exceptions non capturées dans les threads
   - Dans les pools, vérifier les futures pour les exceptions

9. **Tester minutieusement**

   - Tester avec différentes charges et configurations
   - Inclure des tests spécifiques pour les conditions de concurrence
   - Utiliser des outils comme ThreadSanitizer si disponible

10. **Implémenter une gestion propre de l'arrêt**
    - Prévoir un mécanisme pour arrêter proprement les threads/processus
    - Utiliser des signaux comme `Event` pour communiquer l'arrêt
    - S'assurer que toutes les ressources sont libérées lors de l'arrêt

## Erreurs courantes

1. **Race conditions**

   - **Symptôme**: Résultats incohérents ou imprévisibles
   - **Solution**: Utiliser des locks ou d'autres mécanismes de synchronisation

2. **Deadlocks**

   - **Symptôme**: Programme bloqué indéfiniment
   - **Solution**: Utiliser des timeouts, acquérir les locks dans un ordre cohérent

3. **GIL non compris**

   - **Symptôme**: Attentes de performance non satisfaites avec les threads
   - **Solution**: Utiliser des processus pour les tâches CPU-intensives

4. **Surcoût de création excessif**

   - **Symptôme**: Performances médiocres dues à la création/destruction fréquente de threads/processus
   - **Solution**: Utiliser des pools pour réutiliser les threads/processus

5. **Mauvaise granularité des tâches**

   - **Symptôme**: Trop de petites tâches ou trop peu de grandes tâches
   - **Solution**: Équilibrer la taille des tâches pour maximiser l'utilisation des ressources

6. **Contention excessive sur les locks**

   - **Symptôme**: Peu de parallélisme réel, threads souvent en attente
   - **Solution**: Réduire la portée des locks, utiliser des structures de données sans locks

7. **Fuites de ressources**

   - **Symptôme**: Consommation croissante de mémoire ou d'autres ressources
   - **Solution**: Gérer proprement les ressources, utiliser des gestionnaires de contexte

8. **Threads/processus zombies**

   - **Symptôme**: Threads ou processus qui ne se terminent pas correctement
   - **Solution**: S'assurer que tous les threads/processus sont correctement joints

9. **Mauvaise gestion des exceptions**

   - **Symptôme**: Exceptions silencieuses, threads terminés sans avertissement
   - **Solution**: Capturer et logger les exceptions dans chaque thread/processus

10. **Modifications concurrentes de collections**
    - **Symptôme**: `ConcurrentModificationException` ou comportement incorrect
    - **Solution**: Utiliser des collections thread-safe ou protéger l'accès avec des locks

## Ressources supplémentaires

- [Documentation officielle Python - Threading](https://docs.python.org/fr/3/library/threading.html)
- [Documentation officielle Python - Multiprocessing](https://docs.python.org/fr/3/library/multiprocessing.html)
- [Documentation officielle Python - Concurrent.futures](https://docs.python.org/fr/3/library/concurrent.futures.html)
- [Python in Practice: Concurrency (livre par Mark Summerfield)](https://www.pearson.com/en-us/subject-catalog/p/python-in-practice-concurrency/P200000009540)
- [Parallel Programming with Python (livre par Jan Palach)](https://www.packtpub.com/product/parallel-programming-with-python/9781783288397)
- [Conférence PyCon - Python's Infamous GIL par David Beazley](https://www.youtube.com/watch?v=Obt-vMVdM8s)
- [Real Python - An Intro to Threading in Python](https://realpython.com/intro-to-python-threading/)
- [Real Python - Speed Up Your Python Program With Concurrency](https://realpython.com/python-concurrency/)
- [Python Cookbook - Chapter on Concurrency par David Beazley](https://www.oreilly.com/library/view/python-cookbook-3rd/9781449357337/)
- [Blog de Miguel Grinberg sur la concurrence en Python](https://blog.miguelgrinberg.com/post/sync-vs-async-python-what-is-the-difference)

---

Ce chapitre vous a présenté les différentes approches de concurrence disponibles en Python, leurs forces, leurs faiblesses et comment les utiliser efficacement. La programmation concurrente est un outil puissant pour améliorer les performances et la réactivité de vos applications, mais elle nécessite une bonne compréhension des concepts sous-jacents et des pièges potentiels. En maîtrisant ces techniques, vous serez en mesure de choisir l'approche la plus adaptée à chaque situation et d'écrire du code concurrent robuste et efficace.
