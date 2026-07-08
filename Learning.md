# C Memory Management Learning Notes

## Memory Layout

``` text
+----------------------+
| Code                 |
+----------------------+
| Global / Static      |
+----------------------+
| Heap                 | <- malloc/calloc/free
+----------------------+
| Stack                | <- Local variables
+----------------------+
```

## Stack vs Heap

  Stack                         Heap
  ----------------------------- ----------------------
  Automatic allocation          Dynamic allocation
  Fast                          Slightly slower
  Managed by compiler/runtime   Managed by allocator
  Freed automatically           Must call `free()`

## Dynamic Memory

### `malloc(size)`

Allocates **uninitialized** memory.

``` c
int *arr = malloc(5 * sizeof(int));
```

``` mermaid
flowchart LR
A["malloc(20 bytes)"] --> B["Allocator"]
B --> C["Find free block"]
C --> D["Return pointer"]
```

Memory:

``` text
?? ?? ?? ?? ??
```

------------------------------------------------------------------------

### `calloc(n, size)`

Allocates and **zero-initializes** memory.

``` c
int *arr = calloc(5, sizeof(int));
```

Memory:

``` text
0 0 0 0 0
```

------------------------------------------------------------------------

### `free(ptr)`

Returns memory to the allocator.

``` c
free(arr);
arr = NULL;
```

``` mermaid
flowchart LR
A["Allocated Block"] -->|"free()"| B["Allocator marks block FREE"]
B --> C["Future malloc() can reuse it"]
```

> `free()` does **not** erase memory. It only marks the block available.

------------------------------------------------------------------------

# Memory Leak

A memory leak means memory was allocated but never released.

``` mermaid
flowchart TD
A["malloc()"] --> B["Memory allocated"]
B --> C["Pointer lost / free() forgotten"]
C --> D["Memory cannot be reused by your program"]
D --> E["Heap usage grows"]
```

Example:

``` c
int *p = malloc(100);
p = NULL; // leaked
```

This is **not** an information leak.

------------------------------------------------------------------------

# Information Leak vs Memory Leak

  Memory Leak             Information Leak
  ----------------------- --------------------------------------------
  Lost allocated memory   Exposed data
  Resource problem        Security/privacy problem
  Fix with `free()`       Fix with initialization and access control

------------------------------------------------------------------------

# Heap and Allocator

The heap is managed by an allocator.

``` mermaid
flowchart TD
A["Program"] --> B["malloc()"]
B --> C["Memory Allocator"]
C --> D["Heap"]
```

Allocator responsibilities:

-   Track free/used blocks
-   Split large blocks
-   Merge adjacent free blocks
-   Request more memory from OS

------------------------------------------------------------------------

# Arenas

Modern allocators often use multiple arenas.

Without arenas:

``` mermaid
flowchart TB
T1["Thread 1"] --> L["One allocator lock"]
T2["Thread 2"] --> L
T3["Thread 3"] --> L
```

With arenas:

``` mermaid
flowchart TB
T1["Thread 1"] --> A1["Arena 1"]
T2["Thread 2"] --> A2["Arena 2"]
T3["Thread 3"] --> A1
T4["Thread 4"] --> A2
```

Purpose: - Reduce lock contention - Improve multithreaded allocation
performance

------------------------------------------------------------------------

# Contention

Contention occurs when multiple threads want the same resource
simultaneously.

ATM analogy:

``` mermaid
flowchart LR
P1["Alice"] --> ATM
P2["Bob"] --> ATM
P3["Charlie"] --> ATM
ATM["Single ATM"]
```

Only one proceeds while others wait.

In software: - Lock contention - Memory allocator contention - Disk
contention - CPU contention

------------------------------------------------------------------------

# Complete Allocation Lifecycle

``` mermaid
flowchart LR
A["malloc()"] --> B["Memory allocated"]
B --> C["Use memory"]
C --> D["free()"]
D --> E["Block becomes reusable"]
```

------------------------------------------------------------------------

# Mental Models

-   **Stack** = Stack of plates (automatic).
-   **Heap** = Storage warehouse.
-   **malloc()** = Rent a storage locker.
-   **calloc()** = Rent a cleaned locker.
-   **free()** = Return the locker.
-   **Memory leak** = Lose the locker key.
-   **Arena** = Multiple cashiers serving customers.
-   **Contention** = Traffic jam for a shared resource.

------------------------------------------------------------------------

# Best Practices

-   Always check `malloc()` for `NULL`.
-   Pair every successful `malloc()`/`calloc()` with `free()`.
-   Set pointers to `NULL` after freeing if they won't be reused.
-   Never access memory after `free()`.
-   Never `free()` the same pointer twice.
