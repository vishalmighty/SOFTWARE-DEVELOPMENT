# Race condition
A race condition occurs in a system when two or more processes or threads attempt to change shared data at the same time, and the outcome depends on the specific sequence or timing of these changes. This can lead to unpredictable and incorrect results, as the processes or threads "race" to access and modify the shared data.

## Example:
```
import threading

counter = 0

def increment():
    global counter
    for _ in range(1000000):
        counter += 1

thread1 = threading.Thread(target=increment)
thread2 = threading.Thread(target=increment)

thread1.start()
thread2.start()

thread1.join()
thread2.join()

print(counter)

````
### Expected Behavior:
You might expect the counter to be 2,000,000 after both threads have completed, as each thread increments it 1,000,000 times.

### Actual Behavior:
The actual value of counter can be less than 2,000,000 because the threads can interfere with each other when they try to update the counter simultaneously. This interference is a race condition.

## Why It Happens:
The increment operation (counter += 1) is not atomic. It consists of multiple steps: reading the value of counter, adding 1 to it, and writing the new value back.
If the threads interleave their operations, they might both read the same value of counter before either writes it back, leading to lost increments.

## How to Prevent Race Conditions:

1. Locks: Use locks or other synchronization mechanisms to ensure that only one thread can access the critical section at a time.
```
import threading

counter = 0
lock = threading.Lock()

def increment():
    global counter
    for _ in range(1000000):
        with lock:
            counter += 1

thread1 = threading.Thread(target=increment)
thread2 = threading.Thread(target=increment)

thread1.start()
thread2.start()

thread1.join()
thread2.join()

print(counter)  # This will reliably print 2000000
```

2. Avoid shared states
3. Use thread synchronization
4. Atomic Operations: Use atomic operations provided by the language or library, which guarantee that the entire operation is performed as a single, indivisible step.
5. Thread-safe Data Structures: Use data structures designed to be thread-safe.
    eg: queue