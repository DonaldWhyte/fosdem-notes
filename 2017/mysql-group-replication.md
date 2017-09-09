### Group Replication: A Journey to the Group Communication Core

Date: Sat 4th Feb
Time: 16:35

MySQL group replication is a multi-master repl plugin for MySQL.

Has built-in automatc failover, conflict detection and group membership (sharding?)

Write is applied locally to all masters. The transaction is sent to the inno (backing MySQL repl engine). It's only comitted when they don't detect a conflict.

Many variants of Paxos -- it's so complex that it has many different interpretations. Group Replication in MySQL uses Mencius. 

^--- group replication is a variant which uses multiple masters! Every instance is a leader. This is because masters can be a bottle neck.