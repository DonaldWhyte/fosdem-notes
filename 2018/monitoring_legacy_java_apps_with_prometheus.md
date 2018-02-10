# Monitoring Legacy Applications with Prometheus

### Parsing Log Files

Most popular tools are:

* google/mtail
* grok exporter
    - configure patterns to extract metrics in log files
    - configured with YAML
    - generates tagged metrics file
    - can aggregate metrics before exporting th efile

Parse log files to generate prometheus-compatible metric files.  Prometheus polls these files, which appends to metrics time-series.
 
Be careful not to generate high-cardinality tags. Prometheus is efficient, but it has a limit on number of time series. Each tag value generates a new time series.

NOTE: Prometheus has nothing to do with text. You still need the Elastic stack to search text. Also Prometheus _expects_ the aggregated values (e.g. mean, min, etc.). **Need to do the aggregation in the app or grok exporter!**

### Blackbox Monitoring

Probe the service you want to monitor with an external process. e.g. hit the service, get info about how long the request took, status codes, etc. Limited since you don't know if the response is good or not. You have to rely on status codes, but code 200 could still mean the response is wrong or had problems.

### Generating Metrics Publishing Code in Apps

promagent -- this is a Maven plugin that instruments the methods of classes. It injects code at the start or end of the methods which calculate and publish metrics.

This is one approach to instrumenting code you don't own (e.g. code from other teams, libraries) with metrics. It also means you don't bog down the class' code with metrics.

Powerful way of monitoring everything you want, without changing the original source code.
