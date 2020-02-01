# Hash Join in MySQL 8

https://fosdem.org/2020/schedule/event/mysql8_hash_join/

JOIN is one of the most common operation in a database system, and for a long time, the only algorithm for executing a join in MySQL has been variations of the nested loop algorithm. But starting from MySQL 8.0.18, it is now possible to execute joins using hash join.

## What is a Hash Join?

Nested loop algorithms for implementing joins used to be the common approach.

Hash joins hash the join keys between rows in the two tables for fast matching.

Build hash table from table 1 (build table). Scan through table 2 (probe table), match w/ rows in table 1 hash and return.

Downside: build table has to fit in memory.

GRACE hash join gets around this issue. Partition each table into smaller chunk files. Apply basic hash join algo on chunk file pairs.

Upside: chunk files faster than table file, no locking, etc. Downside: small inputs, you end up writing to disk when it could be fit in memory.

Hybrid hash join: attempt to build hash table but if we run out of memory / max byte limit, start writing to chunk file. Best of both worlds, but have to consider some corner cases -- impl complex.

## Hash Join in MySQL 8

Hash join made possible because of an iterator query executor (NOTE: lookup what this is).

Iterator executor made features like explain analyze possible.

Hybrid hash join was used.

Hash join impl doesn't guarantee ordered output, like many nested loop algos.

Speed ups between 30x to 1600x faster.

Handles very large joins, note that the join buffer size (max amount of bytes to put in memory in build table, etc.). Can tweak.

## Notes on Discussion

Global buffer bytes used across all queries not monitored centrally, so tons of join queries at once, it can result in the DB being OOM killed!

Why use multiple chunk files? Why not 1 file w/ logical chunks?

Chunk files are stored in same place all MySQL temp files go -- in its configured temp dir.
