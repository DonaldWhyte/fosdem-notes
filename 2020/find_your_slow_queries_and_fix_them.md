# Find your slow queries, and fix them!

https://fosdem.org/2020/schedule/event/postgresql_find_your_slow_queries_and_fix_them/

https://www.postgresql.eu/events/fosdem2020/schedule/session/2838-find-your-slow-queries-and-fix-them/

https://www.postgresql.eu/events/fosdem2020/sessions/session/2838/slides/270/slow_queries.pdf


## Logging

For finding low queries, make sure you've enabled a ton of logging in postrgesql.conf!

`log_min_duration_statement = 0`

Logs every query. Very useful, but for very active high rate DB would slow down a lot of queries and use tons of disk space.

`log_min_duration_statement = 60ms`

Can reduce query load, only logs queries that are actually heavy/slow.

Logs duration of query on same line as the query.

Don't enable `log_statement` or `log_duration` -- time and statement on diff lines.

Make sure to update `log_line_prefix` to add the user, client host, etc. to the logged query line. Makes searching various indices easier.

`log_checkpoints`

Enable this. Tells when you when postgres starts/stop checkpoints, how much data it wrote, etc.

More meaningful on a high-write system than a high-read system, since checkpoints are all about writing.

`log_connection`/`log_disconnection`

Log every connection and disconnection. These ops add heavy log on DB, so you want to reduce these.

`pgbouncer`

Can be used to pool connections for you, reduces connections/disconnections and thus load on DB.

`log_lock_waits`

Logs information when a query when waits on a lock. It won't log _ever_ lock taken, it will only log locks that clients are waiting for that are over 1s (or time defined in `deadlock_timeout`).

Will help with long running lock contention, not short term lock contention.

`log_temp_files`

Set to `0` to mean you're logging every temp file creation. Useful to determine if you need more RAM (workmem) since you'll get logs if you're writing to disk.

Also log all autovacuums.

## pgBadger for Log Analysis

A web app that analysis the postgres log files and produces various stats, graphs, etc. Useful for dashboard analysis.

OPEN QUESTION: is it just historical logs or live?

## pg_stat_statements

Extension you can install into postgres to generate query stats natively in postgres.

After installing, you need to do a restart.

Access query stats in `pg_stat_statements` table. This persists between postgres restarts, so could grow quite large I suppose? Might need to prune manually (or via a config option?)

Could scrape the data inside the table and visualise it in Grafana (which now has native postgres support).

## Why Queries Could be Slow?

These might not be configured correctly:

* `work_mem` -- affects queries in general and  bitmap indices?
* `maintenance_work_mem` -- work mem used by things like vacuum, table alters, etc.
* `effective_cache_size` -- estimate of the size of the disk cache, psotgres uses it to decide if page is likely going to be in memoty or not
* `shared_buffers` -- allocated at server start, 25/50% of system memory is used for this.

Table/index dead tuple bloat is a big cause of slow queries `check_postgres.pl` is a great tool for finding bloated tables.

Eliminating all bloat requires a full rewrite of the table (`VACUUM FULL`). Very very heavy!

## Data Retrieval

* sequential scan -- hits every row
* index heap scan -- scans index but goes to main table files for filter rows
* index only scan -- scans index and any filters only use columns in the index

## Putting Things Together (joins)

* nested join
* merge join
* hash join

## Aggregation

* group agg -- order/sort input, this steps through each record and if matched it combines
    SORTING IS EXPENSIVE! Can speed up with sorted indices
* hash agg -- scan table, this builds hash table, find matching entries combine the result -- very memory intensive

## Stats

Postgres automatically collects stats on tables to build query plans.

Automatically using autovacuum -- don't disable.

Sometimes, increasing stats colleciton increases perf of query plans. Somtimes it makes it slower!!

## `work_mem` increase

Increase `work_mem` if you have a small dataset (so you can find it in memory), sorting is happening or merge (and hash?) join is being used in plans. Also increase if bitmap heap/index scan is used in query.

Avoid CTEs generally, because it gets materialised on disk before running the query.

To see if your indices are actually useful, you can use how often they're being used in the `pg_stat_user_indexes` table!

## Useful Summaries

[]()

[]()

[]()
