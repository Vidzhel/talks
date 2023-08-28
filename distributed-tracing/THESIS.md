# Distributed tracing

## History

Source: https://www.alibabacloud.com/blog/the-evolution-history-of-observable-data-standards-from-opentracing-and-opencensus-to-opentelemetry_599289

I've already mention, working with distributed systems is never easy. Debugging distributed systems is even worse.
A single request can span dozens of microservices and logs are only scattered bits of information in a gloomy world full
of unknown

Facing such problems, Google has published a paper entitled Dapper - a Large-Scale Distributed Systems Tracing
Infrastructure (2010) to introduce their distributed tracking technology and point out that distributed tracing systems
should meet the following business requirements.

- Low-Performance Loss: The performance loss of services in distributed tracing systems should be ignored as much as
  possible, especially for performance-sensitive applications.
- Low-Intrusive: Try to achieve low-intrusive or non-intrusive
- Rapid Expansion: It can rapidly expand with the scale of business or microservices.
- Real-Time Display: It has the ability of low-latency data collection, real-time monitoring system, and rapid response
  to abnormal conditions of the system.

After, distributed tracing products (such as Uber's Jaeger and Twitter's Zipkin) have become famous. Although each
product has its own set of data collection standards and SDK, most of them are based on the Google Dapper protocol with
different implementations. OpenTracing and OpenCensus were created to solve this problem.

### OpenTracing

Many tools used the different standards which, again, were similar but not identical. Therefore, OpenTracing was
introduced to solve this issue, which is a standardization layer located in between apps. Therefore, developers can add
or replace the implementation of underlying monitoring faster only by modifying Tracer. Based on this, the Cloud Native
Computing Foundation (CNCF) officially accepted OpenTracing in 2016.

![](https://yqintl.alicdn.com/0a8723e6502bc9aadb44bc0b6689ab4bfa53f7a5.png)

### Terms

- Trace: A Complete Request Link
- Span: A Call Procedure

The system has a logical unit with start time and execution duration and contains multiple states.
Each span encapsulates the following states:

- An operation name
- A start timestamp
- A finish timestamp
- Span Tag: A Collection of Span Tags That Consist of Key-Value Pairs. In the key-value pair, the key must be a string,
  and the value can be a string, a Boolean value, or a numeric value.

- Span Log: A Collection of Span Logs. Each log consists of a key-value pair and a timestamp. In the key-value pair, the
  key must be a string, and the value can be of any type.
- References: Relationships between Spans. It refers to the relationships between zero span or multiple spans.
  References relationships are established between spans through SpanContext.

- SpanContext: Refer to Other Related Spans by SpanContext

OpenTracing currently defines two types of references: ChildOf and FollowsFrom. Both of these reference types
specifically simulate the relationship between the child span and the parent span.

### Data model

The trace is implicitly defined by the span belonging to this trace. Each call is called a span, and each span must
contain a global traceID. The trace can be considered a directed acyclic graph (DAG) composed of multiple spans. Spans
are connected first and last in a trace. TraceID and related content take SpanContext as the carrier and follow the span
path in sequence through the transmission protocol. The info above can be regarded as the whole process of a client
request in a distributed application. In addition to the DAG from the business perspective, we try to display the trace
call chain based on the timing diagram of the timeline. This can display the component call time, sequence relationship,
and other information better.

![](https://yqintl.alicdn.com/8a46d9031d98696e9c6d879dd128f9ad0a048656.png)

## Open Consensus

O&M personnel began to pay attention to Log and Metrics in addition to traces to implement DevOps in the entire
observability field better. Metrics monitoring (as an important part of observability) includes machine metrics (such as
CPU, memory, hard disk, and network), network protocol metrics (such as gRPC request latency and error rate), and
business metrics (such as the number of users and visits).

OpenCensus provides unified measurement tools (such as cross-service capture tracing spans and application-level
metrics).

- Compared with OpenTracing, OpenCensus supports Traces and Metrics.
- Compared with OpenTracing, OpenCensus formulates specifications and includes agents and collectors.
- The family team is larger than OpenTracing, and it is supported by Google and Microsoft.

### Terms

In addition to using OpenTracing-related terms, OpenCensus defines some new terms:

- Tags - OpenCensus allows associating metrics with dimensions when recording. Thus, the measurement results can be
  analyzed from different angles.
- Stats - It collects observability results of library and application records. It summarizes and exports statistical
  data and includes two parts: Recording and Viewing (Aggregate Metric Query).
- Traces - In addition to the span properties provided by Opentracing, OpenCensus supports Parent SpanId, Remote Parent,
  Attributes, Annotations, Message Events, Links, and other properties. (for events, casual effects). In addition to the
  span properties provided by Opentracing, OpenCensus supports Parent SpanId, Remote Parent,
  Attributes, Annotations, Message Events, Links, and other properties.

- OpenCensus Agent is a daemon that allows multi-language deployments of OpenCensus to use Exporter.

![](https://yqintl.alicdn.com/9e9337ae76464a5654c8c8b89f4604225c41fb3b.png)

- Collector (as an important part of OpenCensus) is written in the Go language. It can accept traffic from any
  application with receivers available without paying attention to programming languages and deployment methods.
  ![](https://yqintl.alicdn.com/45fe8884070322303668a6a4c092a6e96ddf1098.png)
- Exporters OpenCensus can upload relevant data to various backends through various exporter implementations, such as
  Prometheus for stats, OpenZipkin for traces, Stackdriver Monitoring for stats and Trace for traces,

Products that follow the OpenCensus protocol include Prometheus, SignalFX, Stackdriver, and Zipkin.

The two above are evaluated in terms of functions, features, and other dimensions. OpenTracing and OpenCensus each have
clear advantages and disadvantages. OpenTracing supports more languages and has lower coupling to other systems, but
OpenCensus supports Metrics and distributed tracing and supports it from the API layer to the infrastructure layer. For
many developers, when the difficulty of selection occurs, a new idea is constantly being discussed. Can there be a
project that can integrate OpenTracing and OpenCensus and support the observable data related to logs?

## OpenTelemetry

When answering the previous question, let's look at the troubleshooting process of a typical service problem first:

- Exceptions are found through various preset alarms (Metrics/Logs).
- Open the monitoring dashboard to find abnormal phenomena and find the abnormal module through queries (Metrics)
- Query and analyze the exception module and associated logs to find the core error information (Logs)
- Locate the code causing the problem with detailed trace data (Tracing)

![](https://yqintl.alicdn.com/8ca732065c90c59ede1cc5bf16132e8c483b7bb7.png)

At the same time, the industry already has a wealth of open-source and commercial solutions, including:

- Metrics: Zabbix, Nagios, Prometheus, InfluxDB, OpenFalcon, and OpenCensus
- Traces: Jaeger, Zipkin, SkyWalking, OpenTracing, and OpenCensus
- Logs: ELK, Splunk, SumoLogic, Loki, and Loggly

At the same time, each scheme has a variety of protocol formats/data types.

OpenTelemetry was created to integrate traces, metrics, and logs better. OpenTelemetry is independent of the
manufacturer and platform.

- No vendor lock - It does not provide backend services related to observability
- OpenTelemetry uses a standards-based implementation approach. It's a specification that provides guidelines for:
  - API - It defines the types and operations of metrics, tracing, and logs data.
  - SDK - It defines API language-specific implementation requirements, configuration, data processing, and export
    concepts.
  - Data - It defines the OpenTelemetry Line Protocol (OTLP).
- Numerous official SDKs
- Implementation of Data Collection System - OpenTelemetry is based on the collection system of OpenCensus, including
  agents and collectors. Collector covers the functions of collecting, transforming, and exporting data. It supports
  receiving data in a variety of formats (such as OTLP, Jaeger, and Prometheus) and sending data to one or more
  backends. It also supports the processing and filtering of data before output. The collector contrib package supports
  more data formats and backends. Two mods, agent mode (sidecar) or just as standalone middleware component

### Data types

- Metrics
  - Counter - only increases - summed
  - Measure - changes within range - aggregated
  - Observer - value in a point of time
  - Logs
- Traces - requests
- Baggage - key-values propagated with context

OpenTelemetry wants to solve the first step to unifying observability data. It standardizes the collection and
transmission of observability data through API and SDK. But tools can be chosen and picked

## Overview of different distributed tracing solutions

An [OpenTelemetry Demo](https://github.com/open-telemetry/opentelemetry-demo) project is used to show all the platforms,
which is a microservice architecture, messaging systems, with a open telemetry configured

Essential:
- slowest traces and cause
- errors and cause
- What is the slowest operation in service, p99
- Duration of the whole cycle of an event (from produce to consume)
- Alerting on error

Mostly missing:
- change impact visualization, versioning (systems that allow creation of markers e.g. deployment ones, kind of have this feature)
- performance impact
- path change
- No notion of paths
- No connection to architecture
  - Bottleneck service - in architectural, and performance ()
  - Slowest path - aggregate of all trances that are of the same kind

## Jaeger

- Service map doesn't show asynchronous communication, messaging systems (kafka)

## Honeycomb

- Show how to find bottleneck in a call by building a heatmap of duration, finding slow
- Datasets - description of 
- They use their [own column oriented storage](https://www.honeycomb.io/blog/why-observability-requires-distributed-column-store) for flexible and fast queries
- Markers are used for deployments or other things (shown on the graph)
- [SLOs](https://www.honeycomb.io/slos) setting
- [Service map](https://www.honeycomb.io/service-map)
- [Sandbox](https://www.honeycomb.io/sandbox)

## Aspecto

- very simple
- request service map
- no presense 

## NewRelic

## Lightstep

- Power query language
- Differences analysis
- Dependency graph
- Workflows - quickly jump from span to other external resource (graphana, logs...)

- No logs support