# Observability at Google

The holistic approach to be abvle to observer a system.

Observe using many signal: metrics, traces, logs, events, profiling.

To do this, an efficient (performance-wise) and powerful instrumentation stack is required.

All instrumentation data is recorded as key-value pairs. Picked up by aggregation/metrics services, which can filter tags to what they care about.

### OpenCensus

Open-source project inspired by Google's internal Cenus libraries.

Set of libraries for most languages to provide instrumentation tools (tags, metrics, traces, profiles, etc.). It will write tags, records, traces, etc. with simple library function calls.

Integrated with various other systems.

It also traces metrics that are tied to a _context_. Useful for tracing behaviour across processes (e.g. a chain of service requests).

### Design Considerations

Instrumentation libraries should:

* little impact on the critical path
    - fast code
    - doesn't do IO
* defer the decision of the metric collection to the user
    - they decide how to collect
    - where to store it
    - and _what_ to collect. This allows libraries to instrument without worrying about resources costs. They provide a liberal set of measures and users decide what to pick.

Instrumentation has to be _explicitly_ enabled and disable. They should also be enabled/disabled in production. This means you don't generate metrics until you suspect something is wrong or care about the metrics.

e.g. instrumentation in a GZIP lib. Most of the item, you don't care about thos metrics. If you suspect a perf/logic issue in the lib, you can enable it in prod.

### Also Look Into

opentracing.io -- a vendor-neutral open standard for distributed tracing.

OpenCensus does _not_ conform to this.

