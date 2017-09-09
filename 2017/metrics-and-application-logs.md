### Metrics and an application log: Your new best friend

Date: Sat 4th Feb
Time: 18:00
Speaker: Michael Heap (@mheap)

#### Logging

ELK stack (ElasticSearch, logstache, Kibana) -- de facto standard for logs.

Why log?
    - error logs (for debugging)
    - behaviour and action logs (for metrics)
    - behaviour/identity logs (for auditing)

Two types of log:
    1. Human readable -- silent, shouldn't log unless you really want someone to know about it (e.g. mostly errors?) 
    2. Machine readable -- debug/tracing w/ JSON blobs, etc.

But really, a log should be both machine *and* human readable. Use JSON and `logfmt` for this. Always make machine readable so you can configure log monitoring software to metric/alarm.

LOGS:

* business information (e.g. when someone plays media, logs in)
* intermediate results for debugging issues (lower log levels)
* final results of computation
* events/data that might ptoentially cause issues downstream, but mught not

When -- did log entry happen
Who -- did made the event (user, service instance, machine)
Where -- where does the log go to ()
Why -- why did the event happen? log what happened, the *reason* it happened (context, e.g. could not read data from user history search), not just the raw data

#### Logging Frameworks

USE A LOGGING FRAMEWORK!

`monolog` is the best PHP logging framework.

`FingersCrosssHandler` is `monolog` is awesome. It will buffer all log entries, but not actually record them unless an `WARN` or `ERROR` logo occurs. The great thing about this is we don't have unnecessary noise, but we have very granular detail when something went wrong.

#### Log Levels

Syslog (RFC 5424) is a logging standard that's quite liked. Just make sure you don't put everyhing 

#### The ELK Stack

Logstash - L
Elastic Search - E
Kibana - K

#### Logstash

Logstash can **receive** data from so many things. Databases, **S3**, HTTP, **files**, web sockets, Twitter streams, TPCP, ZeroMQ, **stdin**.

Logstash can filters all inputs, add fields, remove fields, add default keys. The most awesome one is **Grok filter**, which can transform log messages (each trace logs, logs from low-level libraries you're using) into human-readable, structured, descriptive messages.

> http://grokdebug.herokuapp.com/ allows you to test your filters.

Can output to so many different things, but mostly **Elasticsearch** is used.

> Lighter option than logstash is Filebeat -- less RAM but no filters.

#### Elasticsearch

Allows full text queries. Simple to use.

#### Kibana

Kibana allows you to view and search logs stored in Elasticsearch.

It automatically extracts JSON fields for you.

And give you visualisations based on log records, so you get business insights, just from your application logs.

Build dashboards.

> It's a bit like Splunk, but free.

#### mheaps Law

An application log may not injure an application's performance or readability.

Not supposed to hinder code readability.

#### Issues

Plan for bursts of data -- that's when things go wrong and you want your logs to work.

Means: have enough diskspace. Have enough Elasticsearch instances, logstash instances, etc.

#### Index Management

Have multiple indexes, e.g. one for only error logs, one for debug logs.

Ensure you clean up log files and 

#### Other Tips

Every features shipped to production should have a set of pre-defined log patterns. Behaviours related to that feature should have a dashboard and set of visualisations.

Have unique request IDs, so you can correlate logs across multiple processes and hosts.

Normalise timezones. Seriously...**normalise timezones**.

#### Graphite / Grafana

Graphite is a data source. Grafana is a graphic tool, more geared towards metrics than application logs.

#### Alerting

You can trigger alerts based on data in Kibana and Graphite. You can configure it to send alerts to Slack, phone number, etc.

Check out Etsy's 411. 

#### Key Takeways

Logging is required.

Not should we, but **how** should be?

Logging isn't free -- it has a performance impact, be careful -- don't be too noisy. INFO or above is usually enough and has little impact on prod systems.
