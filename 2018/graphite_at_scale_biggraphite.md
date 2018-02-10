# Graphite at Scale: BigGraphite

Corentin Chary
    - worked on Bigtable/Colossus at Google.

BigGraphite is an extension of Graphite, which scales to much more data.

Graphite uses Carbon for persistence.  Carbon stores the data. graphite-web is the UI/web service that receives queries retrieves data from Carbon store.

Can also replicate data using carbon-relay. Similar to InfluxDB's relays.

### Problems with Graphite

Graphite lacks true elasticity (cannot scale) for storage and QPS.

One file per metric. Wasteful. For example, some metrics might be very sparse.

graphite-web is fragile. sSngle query can bring down the entire Carbon cluster. There's a _huge_ fan out to different files/Carbon nodes when querying.

### BigGraphite

Enter BigGraphite! This is a Graphite plugin that changes Graphite/Carbon to use Cassandra for persistence. 

Cassandra has much better horizontal scalability. We can now scale Graphite because we just need to scale Cassandra by adding more instances.
