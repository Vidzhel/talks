## Distributed tracing with Open Telemetry

![Untitled](./slides/00-start/opentel_logo.png)

+++

### History

- Problem: it's hard to pinpoint a problem in microservices architecture
- Google has published a paper entitled Dapper - a Large-Scale Distributed Systems Tracing Infrastructure (2010)
- Development of first distributed tracing products (such as Uber's Jaeger and Twitter's Zipkin) (up to 2017)
- Standardization (til now)

Notes:

- Hard to profile, request spans multiple services, Logs are just breadcrumbs
- Conceptual description of tracing system
- Low-Performance loss, Low-Intrusive

+++

### OpenTracing

![](https://yqintl.alicdn.com/0a8723e6502bc9aadb44bc0b6689ab4bfa53f7a5.png)

Notes:
- Standardization layer located in between apps
- In 2016 accepted by Cloud Native Computing Foundation (CNCF)

+++

### Terms

- Trace: A Complete Request Link
- Span: A Call Procedure

![](https://opentelemetry.io/img/waterfall-trace.svg)

Notes:
- Operation names, start-end timestamps, tags, logs, references

+++

### OpenCensus

![Untitled](./slides/00-start/opensensus_logo.png)

Notes:
- 2018
- Standardizes metrics gathering
- Includes tracing but a bit different from previous standard

+++

### Terms

- Collector agent - component that gathers processes and exports telemetry

![](https://opentelemetry.io/docs/collector/img/otel-collector.svg)

Notes:
- Differences between two standards in language support and functions

+++

### Open Telemetry

![combining telemetry](./slides/00-start/combining_telemetry.png)

Notes:
- 2019
- Troubleshooting: find exceptions, anomalies in metrics, query specific traces and review logs
- Utilize the already present diverse tools
- Be vendor agnostic and provide only spec for tools to be developed (API - defines operations, metrics; SDK - language implementation, OTLP - protocol to transfer all of the data)
- Official SDKs are developed for many languages
- Data collection system that is based on OpenCensus one

+++

### Instrumentation

![node js instrumentation](https://i.imgur.com/kk8lF2a.jpeg)


Notes:
- Guides for how to instrument for many popular languages
- auto instrumentation with few lines of code (nest, redis, mongodb and mongoose, aws sdk)
- manual instrumentation to add some specific insights (account id, host, plan, feature flags, user id...)

+++

### Spec

![http spec](https://i.imgur.com/LBEPrRZ.jpeg)

Notes:
- Not only generic description of data (metric) but specific convention for protocols, tools and standards
- AWS SDK, Databases, Feature flags, http...
- Provides attributes and rules, that sdks must comply to

+++

### Sampling

![](https://opentelemetry.io/docs/concepts/sampling/traces-venn-diagram.svg)

Notes:
- reduce costs, because we actually do not need all the data gathered
- Head - sample before all spans are gathered (e.g. sample percentage of data gathered)
- Tail - sample after spans are gathered - look on a whole trace (e.g sample all traces with errors)

+++

### Overview of different distributed tracing solutions

![architecture](https://i.imgur.com/z1ckGXS.jpeg)

Notes:
- many languages, technologies, protocols, data stores and proxies

+++

## Questions?

+++

### Analysing architecture smells

![](./slides/00-start/neo4j-services-visualization.png)

Notes:
- custom exporter, load architecture into neo4j

+++

### Service dependencies visualization

- 
![](./slides/00-start/service-dependencies.png)

Notes:

+++

### Smells visualization

![](./slides/00-start/architecture-smells.png)
