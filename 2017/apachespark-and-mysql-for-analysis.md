### Using Apache Spark and MySql for Data Analysis

Date: Sat 4th Feb
Time: 16:00

Spark is for parallel computation only. Executes tasks across multiple nodes, which are provided by Amazon EC2 or some other cloud provider.

In-memory processing with caching. It's massively parallel since you are not bounded by memory (as it's distributed across multiple machines). It has direct access to data sources (e.g. MySQL).

Run query in Spark vs. MySQL:

MySQL -> indexes, partioning (e.g. time-based, sharding)
Spark -> no indices, partitions data for parallel computation, map/reduce architecture.

#### Spark

No indices, all processing is full scan. However, parallel means you can have many small partitions.

**High latency**, because result set has to be sent to other machines and combined into final result set. (means transferring a lot of data).

Indices for big data have limitations --> very large B-trees

Can monitor jobs in PySpark (yay bootstrap).

#### wikistats

10 TB of data. 6 years or more to load a month of data in RMDB like MySQL.

Eaiser in spark --> just copy files to  storage (and create SQL structue).

4.5 TB takes 45 seconds in Spark. Much, much better. Way more throghout, not bottle-necked on a single pipeline.

PySpark uses multiple cores for parallel insertion. MySQL is seuqential insertion.

#### Apache Drill

Treat any datasource as a table (even it is not). For example, you can query and join tables inMongoDB/MySQL/flat files together in a single SQL query.
