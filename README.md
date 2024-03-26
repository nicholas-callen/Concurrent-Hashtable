# Hash Hash Hash
This lab has two implementations of how to use mutex locks to achieve concurrency in a hashtable. One is concerned with correctness and the other with correctness and performance, both are safe.

## Building
Run the command "make" in the shell.
```shell
make
```

## Running
The program can be run with ./hash-table-tester -t (numCores) -s (numEntries).
```shell
./hash-table-tester -t 8 -s 50000
Generation: 49,501 usec
Hash table base: 329,633 usec
  - 0 missing
Hash table v1: 656,597 usec
  - 0 missing
Hash table v2: 88,229 usec
  - 0 missing
```

## First Implementation
In the `hash_table_v1_add_entry` function, I added a lock at the very beginning and an unlock at the very end, as well as an unlock when an entry already has a value.

### Performance
```shell
Refer above to run program, Hash table v1: 656,597 usec. Further testing and examples prove that v1 time is consistently slower than base time.
```
This version is slower than the base hash table implementation because overhead time is incurred for context switching between threads, including costs for defining, creating, and managing threads.

## Second Implementation
In the `hash_table_v2_add_entry` function, I locked before the new SLIST head is inserted and then unlocked.

### Performance
```shell
Refer above to run program, Hash table v2: 88,229 usec. Further testing and examples prove that v2 time is consistently and significantly faster than base time.
```
This version is faster than the base implementation because the threads can run concurrently most of the time. A lock is created for each hash table bucket, and is only locked when multiple threads are on the same one. I tested this with both a different number of entries and cores to confirm my results.

## Cleaning up
In the "hash_table_v1_destroy" and "hash_table_v2_destroy" functions there is an added 'pthread_mutex_destroy(&lock)' call right before freeing the hash table.

Run the command "make clean" in the shell.
```shell
make clean
```