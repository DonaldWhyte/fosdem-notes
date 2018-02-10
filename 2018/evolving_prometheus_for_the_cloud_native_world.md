# Evolving Prometheus for the Cloud Native World

A lot of monitoring done today is based on tools and techniques that were good decades ago.

They focused on handling special cases / machines as pets.

Cloud-based architecture has changed that. These tools are no longer useful.

### What is Prometheus

Metrics-based monitoring system.

Tracks stats over time, not just individual events. It persists metrics after some window (e.g.  every 1 min, agg all received dimensions and flush).

At is core is a TSDB (time series database).

Time and numbers are stored as doubles. This makes all the metrics unified in the same time units. Millisecond 

Multi-dimensional labels on all metrics.

#### Details

Core Prometheus server is a single binary. Each server instance is independent and doesn't talk to other servers. TODO: how would you shard/replicate?

Prometheus will not attempt to backfill missing data. Such approaches are difficult/missing to get right. And alos creases huge amount of load. **Backfilling is not done at all.**

### The Cloud

Prometheus has built-in adapters to Kube, etc. to discover newly spawned services. Prometheus _pulls_ the instances to monitor from Kube. Pull-based model of discovery better than push.

PromQL -- query language for Prometheus.

Use PromQL to aggregate latency/performance stats across _all_ instances/machines. This means slower machines (either slower instance types or just borked machines) wont generat spurious alerts.

TODO: what does RED acronym mean?

### TSDB Query Patterns

Users typically query one metric across time. These are "horizontal" queries where X axis is time.

However, writes a vertical. Many different metrics for the same time point are written at a time.

This makes writes hard. All TSDBs solve this by buffering writes. Keep the writes in-memory then flush to disk. Lots and lots of different strategies for this.
