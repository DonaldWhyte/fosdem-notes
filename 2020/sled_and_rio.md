# sled and rio -- modern database engineering with io_uring

https://fosdem.org/2020/schedule/event/rust_techniques_sled/

sled project page: https://sled.rs/
rio project page: https://github.com/spacejam/rio

## Common Database Techniques

* lock-free programmng
* replication, consensus, eventual consistency
* correctness testing
* auto tuning

## What is sled?

DBs combine all of these things together.

sled acts like a concurrent B-tree map, which saves data to disk.

## Why is rust the best DB language?

Over time, Rust will approach Fortran performance in many cases. C/C++ is really limited by aliasing. More compile-time info that Rust provides means better optimisations.

Rust correctness w/ lifetimes, etc.

Compatability with great C/C++ debugging tools.

Fast to compile, low friction dev (Rust in general but also sled).

## Sled

Lightweight dependency.

Built-in profiler for the tool (doesn't rely on any external profilers). Supports generating flamegraphs from the perf data.

General tip: when building things like DBs, aim to add tons of profiling into _into_ the DB.

1 billion ops in 57 seconds.

In-memory DB.

DEFINITELY STILL CONSIDERED BETA!

## Underlying Bits of Sled

Lock-free index loosely based on Bw-tree.

Lock-fre pagecache.

Uses `io_uring` on huge buffers for writes, functionality used in sled is exported to seaprate crate `rio`.

> KEY GOAL: Avoid blocking while reading and writing! DOn't want to acquire locks! Lock-fee!

This is achieved by RCU (read-copy-update). Read old value through atomic ptr, make a local copy of the data it points to. Modify local copy with desired changes. Perform a compare-and-swap to install new version (if fails, retry). Delay destruction of old verison until all threads have finished the work, so delay it using `crossbeam_epoch`.

Readers don't wait for writers.

Writers write optimistically.

Bet we're making here is that we have to retry less often than the price we have to pay for a lock.

Can also measure contention dynamically and if high contention, start using locks.

## Sled is also an on-disk DB.

So we want to map the stuff stored in-memory to disk.

Writes are added to a WAL. Lots of race conditions here.

Bad solution:

[bad_solution.png]

Good solution:

[good_solution.ng]


## `io_uring`

Interface for fully async Linux system calls.

Started with a few sys calls TODO.

TODO: finish these notes off
