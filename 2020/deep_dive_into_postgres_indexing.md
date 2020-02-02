# A Deep Dive into PostgreSQL Indexing

https://fosdem.org/2020/schedule/event/postgresql_a_deep_dive_into_postgresql_indexing/

Mostly high-level basics on postgres index creation. `CREATE INDEX` and `CREATE INDEX CONCURRENTLY` to prevent locking the table.

Expression indices -- don't index a raw column, index a transformed column value (via an expression).

Partial indices -- creating indices only for rows that match a particular boolean expression. Useful for making indices smaller by only focusing on rows that are actually hit by your important queries.

This talk did discuss the underlying memory structure of table/indexes as they're stored on disk, which was interesting.

Types of indices:

* b-tree (the default) -- these can be used with all `WHERE` filter types: <, <=, =, >, >=
....

other indices here: https://www.postgresql.org/docs/current/indexes-types.html
