---
title: Officially certified libraries
---

#### Certified InfluxDB client libraries

Certified InfluxDB client libraries are supported directly by InfluxData. Developers that create or maintain 
client library should follow [InfluxDB Client Guideline](/client_libraries/certification/influxdb/)
to ensure consistency in naming and functionality across all languages and platforms.

All certified clients should implement all features listed in [Certified library features](client_libraries/certification/features/).

#### InfluxDB 1.X

##### Fully certified
* [Go](https://github.com/influxdata/influxdb/tree/master/client) 
* [Java](/client_libraries/java) - [influxdb-java](https://github.com/influxdata/influxdb-java)
* [Java Reactive](https://github.com/bonitoo-io/influxdb-java-reactive) - reactive RxJava extention to [influxdb-java](https://github.com/influxdata/influxdb-java)

##### Partial certified

| Client                                                                | Missing                                                       |
|-----------------------------------------------------------------------|---------------------------------------------------------------|
| [Python](https://github.com/influxdb/influxdb-python)                 | Asynchronous batching                                         |
| [Ruby](https://github.com/influxdata/influxdb-ruby)                   | `Jitter Interval` and `Retry Interval` options for batching   |
| [.Net (C#)](https://github.com/MikaelGRA/InfluxDB.Client)             | Asynchronous batching                                         |
| [PHP](https://github.com/influxdata/influxdb-php)                     | Asynchronous batching, Chunked query                          |
| [Scala](https://github.com/paulgoldbaum/scala-influxdb-client)        | Asynchronous batching, Chunked query                          |
| [Haskell](https://github.com/maoe/influxdb-haskell)                   | Asynchronous batching, High-level API                         |

#### Flux (InfluxDB 2.0)
* [Go](/client_libraries/java) - [influxdb-java](https://github.com/influxdata/influxdb-java)
* [Java](https://github.com/bonitoo-io/flux-java) 
* [Java Reactive](https://github.com/bonitoo-io/influxdb-java-reactive) - reactive RxJava extention to [flux-java](https://github.com/influxdata/influxdb-java) 

The certified libraries are supported by InfluxData and comply the guidelines from [Writing client libraries](/client_libraries/certification/).


