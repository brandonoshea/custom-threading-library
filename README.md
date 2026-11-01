# User-Level Thread Library

A user-level (M:1) thread library built on top of `ucontext.h`, implementing
cooperative and preemptive thread scheduling along with mutex locks and
condition variables.

## Overview

This project implements the thread API declared in [`mythreads.h`](mythreads.h):

- `threadInit()` — initialize the threading library (call once)
- `threadCreate(func, arg)` — spawn a new thread running `func(arg)`
- `threadYield()` — voluntarily yield control to another thread
- `threadJoin(thread_id, result)` — block until a thread finishes, retrieve its return value
- `threadExit(result)` — terminate the current thread
- `lockCreate()` / `lockDestroy()` / `threadLock()` / `threadUnlock()` — mutex locks
- `condvarCreate()` / `condvarDestroy()` / `threadWait()` / `threadSignal()` — condition variables

Threads are implemented using `getcontext`/`makecontext`/`swapcontext` rather
than OS-level threads or processes.

## Files

| File | Description |
|---|---|
| `mythreads.h` | Thread library API (header only — implementation goes in `mythreads.c`) |
| `makecontext_example.c` | Minimal example of creating and swapping into a new context |
| `contextfun.c` | Example of saving/restoring context mid-function with `swapcontext` |
| `funcptr.c` | Function pointer refresher (used by `threadCreate`'s signature) |
| `tests/cooperative_test.c` | Tests thread creation, yielding, and joining |
| `tests/lock_test.c` | Tests mutex lock/unlock behavior |
| `tests/preemptive_test.c` | Tests preemption via a `SIGALRM`/timer-driven scheduler |
| `tests/*_expected.txt` | Expected output for the corresponding tests |

## Building

```sh
make        # builds the example programs (contextfun, funcptr, makecontext_example)
make clean  # removes built binaries and object files
