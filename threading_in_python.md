# Threading with python

##### link to lecturer's repo: https://github.com/eightlimbed/oreilly

## Table of Contents
1. [Single Thread](#single_thread)
2. [Multi-threads](#multi-threads)
3. [Daemon Threads](#daemon_threads)
4. [Joining Threads](#joining_threads)
5. [Race Conditions](#race_conditions)
6. [Locks](#locks)
7. [Deadlock](#deadlock)
8. [Additional Documents](#additional_documents)

## Single Thread

```python
# Example of a single-threaded application
from training import WEBSITES, visit_website


if __name__ == '__main__':
    print('Main thread starting')
    for website in WEBSITES:
        visit_website(website)
    print('Main thread ending')
```

## Multi-Threading

```python
# Example of a multi-threaded application
from training import WEBSITES, visit_website
import threading


if __name__ == '__main__':
    print('Main thread starting')
    for website in WEBSITES:
        # Create a Thread object with target and args
        t = threading.Thread(target=visit_website, args=[website])
        # Start the thread
        t.start()
    print('Main thread ending')
```

## Daemon Threads
- The main thread does not wait for these to finish
- Worker threads terminate abruptly when main thread completes

```python
# Example of a multi-threaded application using daemon threads
from training import WEBSITES, visit_website
import threading


if __name__ == '__main__':
    print('Main thread starting')
    for website in WEBSITES:
        # Create a Thread object with target and args
        t = threading.Thread(target=visit_website, args=[website], daemon=True)
        # Start the thread
        t.start()
    print('Main thread ending')
```

```python
# Example of using daemon threads to terminate threads when the main thread terminates
from training import WEBSITES, visit_website
import threading
import sys
import time


if __name__ == '__main__':
    # Create a forced timeout option
    if len(sys.argv) != 2:
        print(f'Usage: {sys.argv[0]} TIMEOUT')
        sys.exit()

    print('Main thread starting')

    # The program will end after `timeout` seconds
    timeout = int(sys.argv[1])

    for website in WEBSITES:
        # Create and start some daemon threads
        t = threading.Thread(target=visit_website, args=[website], daemon=True)
        t.start()
    
    # Force the program to end after timeout
    time.sleep(timeout)

    print('Main thread ending')
```
## Joining Threads
- Thread.join()
- Joins a thread with its caller (i.e., main thread)
- Calling thread is blocked until worker thread terminates
- Preserves order of execution
- Has a timeout option

```python
# Example of a multi-threaded application using start() and join()
from training import WEBSITES, visit_website
import threading


if __name__ == '__main__':
    print('Main thread starting')
    for website in WEBSITES:
        # Create, start, and join a thread
        t = threading.Thread(target=visit_website, args=[website])
        t.start()
        t.join()
    print('Main thread ending')
```

```python
# Example of a multi-threaded application using start() and join() with a timeout
from training import WEBSITES, visit_website
import threading


if __name__ == '__main__':
    print('Main thread starting')
    for website in WEBSITES:
        # Create, start and join a daemon thread that times out
        t = threading.Thread(target=visit_website, args=[website], daemon=True)
        t.start()
        t.join(timeout=0.1)
    print('Main thread ending')
```

```python
# Example of a multi-threaded application using ThreadPoolExecutor
from training import WEBSITES, visit_website
from concurrent.futures import ThreadPoolExecutor


if __name__ == '__main__':
    print('Main thread starting')
    # Use the ThreadPoolExecutor context manager to manage threads
    with ThreadPoolExecutor(max_workers=3) as executor:
        for website in WEBSITES:
            # Submit a function and args to the pool of threads
            executor.submit(visit_website, website)
    print('Main thread ending')
```

## Race Conditions

```python
# Example of a race condition
from training import Account
from concurrent.futures import ThreadPoolExecutor


if __name__ == '__main__':
    print('Main thread starting')
    account = Account()
    print(account)
    with ThreadPoolExecutor(max_workers=2) as executor:
        # Submit two threads: deposit 100, followed by withdrawal 50
        for transaction, amount in [(account.deposit, 100), (account.withdrawal, 50)]:
            executor.submit(transaction, amount)
    print(account)
    print('Main thread ending')
```

## Locks
- Threading.lock()
- Prevents race conditions by blocking threads
- Protects a section of code
- Allows only one thread to access it
- Has two methods
    - Acquire()
    - Release()

```python
# Example of a race condition with locks
from training import ThreadSafeAccount
from concurrent.futures import ThreadPoolExecutor


if __name__ == '__main__':
    print('Main thread starting')
    account = ThreadSafeAccount()
    print(account)
    with ThreadPoolExecutor(max_workers=2) as executor:
        # Submit two threads: deposit 100, followed by withdrawal 50
        for transaction, amount in [(account.deposit, 100), (account.withdrawal, 50)]:
            executor.submit(transaction, amount)
    print(account)
    print('Main thread ending')
```

```python
# Example of a locking using Lock objects
import threading


if __name__ == '__main__':

    # Create a lock
    lock = threading.Lock()
    print(lock)

    # Acquire a lock for a section of code
    lock.acquire()
    print(lock)

    # Release the lock
    lock.release()
    print(lock)
```

- [RLock](https://docs.python.org/3/library/threading.html#rlock-objects)

## Deadlock
- Occurs when a thread tries to acquire a lock that is already locked

```python
# Example of a deadlock situation
import threading


if __name__ == '__main__':

    # Create a lock
    lock = threading.Lock()

    # Acquire a lock 
    lock.acquire()

    # Acquire a lock again -- deadlock!
    lock.acquire()

    # Release the lock
    lock.release()
```

```python
# Example of a race condition
from training import ThreadSafeAccount
from concurrent.futures import ThreadPoolExecutor


if __name__ == '__main__':
    print('Main thread starting')
    account = ThreadSafeAccount()
    print(account)
    with ThreadPoolExecutor(max_workers=2) as executor:
        # Submit two threads: deposit 100, followed by withdrawal 50
        for transaction, amount in [(account.deposit, 100), (account.withdrawal, 50)]:
            executor.submit(transaction, amount)
    print(account)
    print('Main thread ending'
```


## Additional Documents

```python
# This modules defines variables and functions used as examples in this training.
import requests
import time
import threading


WEBSITES = [
    'http://en.kremlin.ru/',
    'http://mfa.go.th/main/',
    'http://www.antarctica.gov.au/',
    'http://www.mofa.gov.la/',
    'http://www.presidency.gov.gh/',
    'https://www.aph.gov.au/',
    'https://www.argentina.gob.ar/',
    'https://www.fmprc.gov.cn/mfa_eng/',
    'https://www.gcis.gov.za/',
    'https://www.gov.ro/en',
    'https://www.government.se/',
    'https://www.india.gov.in/',
    'https://www.jpf.go.jp/e/',
    'https://www.oreilly.com/',
    'https://www.parliament.nz/en/',
    'https://www.peru.gob.pe/',
    'https://www.premier.gov.pl/en.html',
    'https://www.presidence.gov.mg/',
    'https://www.saskatchewan.ca/'
]










def visit_website(url):
    """Makes a request to a url and prints the status code and elapsed time"""
    r = requests.get(url) 
    print(f'{url} returned {r.status_code} after {r.elapsed} seconds')










class Account():
    def __init__(self):
        self.balance = 0

    def __repr__(self):
        return f'Current balance is {self.balance}'

    def deposit(self, amount):
        print(f'Depositing {amount}')
        # Simulates a database read and write
        state = self.balance
        time.sleep(0.1)
        state += amount
        self.balance = state

    def withdrawal(self, amount):
        print(f'Withdrawing {amount}')
        # Simulates a database read and write
        state = self.balance
        time.sleep(0.1)
        state -= amount
        self.balance = state










class ThreadSafeAccount():
    def __init__(self):
        self.balance = 0
        self.lock = threading.Lock() # Give each account a Lock

    def __repr__(self):
        return f'Current balance is {self.balance}'

    def deposit(self, amount):
        print(f'Depositing {amount}')

        # Limit access to shared data to only one thread at a time
        with self.lock:
            state = self.balance
            state += amount
            time.sleep(0.1)
            self.balance = state

    def withdrawal(self, amount):
        print(f'Withdrawaling {amount}')

        # Limit access to shared data to only one thread at a time
        with self.lock:
            state = self.balance
            state -= amount
            time.sleep(0.1)
            self.balance = state
```

