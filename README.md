# mstd

> A lightweight, cross-platform C library providing standard utilities, memory allocators, data structures, platform abstractions, and systems programming primitives.

---

## About

This repository is a **Linux port and continued development** of the original **mstd** project by **BlueMoonHunt**.

The original project is licensed under the MIT License, and this repository continues to use the same license while expanding platform support and functionality.

### Maintainer

* **Original Author:** BlueMoonHunt
* **Linux Port & Ongoing Development:** RevMax-creator

___

## Features

mstd provides a growing collection of reusable systems programming utilities for C.

| Feature                  | Description                                                                          |
| ------------------------ | ------------------------------------------------------------------------------------ |
| **Memory Management**    | Arena allocators, scratch memory, and dynamic arrays (`DArray`).                     |
| **Data Structures**      | Singly and doubly linked lists, UTF string types (`Str8`, `Str16`, `Str32`).         |
| **Platform Abstraction** | Cross-platform support for Windows, Linux, and macOS (work in progress).             |
| **Unicode Support**      | UTF-8, UTF-16, and UTF-32 string handling and conversion.                            |
| **File I/O**             | File operations and memory-mapped file support.                                      |
| **Debugging Tools**      | Assertions, traps, validation macros, and diagnostics.                               |
| **Math Utilities**       | Bit manipulation, alignment, clamping, and helper macros.                            |
| **Timing**               | High-resolution timers and clock utilities.                                          |
| **Threading**            | Threads, mutexes, read/write mutexes, semaphores, barriers, and condition variables. |

---

## Usage

#### 1) Arena Allocator

```c
#include "mstd.h"

int main() {
    os_state_init();
    Arena* arena = arena_alloc();
    int* numbers = arena_push_array(arena, int, 10);

    for (int i = 0; i < 10; i++)
        numbers[i] = i * 2;

    arena_release(arena);
    return 0;
}

#include "mstd.c"
```
#### 2) String Handling
```c
#include "mstd.h"

int main() {
    os_state_init();
    Arena* arena = arena_alloc(.reserve_size = MB(1));
    Str8 hello = str8_from_cstr("Hello, ");
    Str8 world = str8_from_cstr("world!");
    Str8 greeting = str8_concat(arena, hello, world);

    printf("%.*s\n", (int)greeting.size, greeting.data);
    arena_release(arena);
    return 0;
}

#include "mstd.c"
```
---

## Roadmap

### Platform Support

* [x] Windows
* [ ] Linux
* [ ] macOS

### Planned Improvements

* Unify `Str8` and `Str8View` with customizable I/O.
* Complete the Linux platform layer.
* Improve the arena allocator with configurable growth policies and overflow handling.
* Expand documentation and examples.
* Add CI builds for Windows and Linux.
* Improve test coverage.

---

## License

This project is licensed under the **MIT License**.

### Attribution

Original project:

* Copyright (c) 2025 BlueMoonHunt

Linux port and subsequent modifications:

* Copyright (c) 2026 RevMax-creator

See the `LICENSE` file for the full license text.
