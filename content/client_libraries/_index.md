---
title: Client Libraries
---

# Overview

There are many ways to write or query data from InfluxDB. You can use built-in [writing HTTP API](/influxdb/v1.6/guides/writing_data/), 
[quering HTTP API](/influxdb/v1.6/guides/querying_data/), [command line interface](/influxdb/v1.6/tools/shell/) or 
use one of following client libraries that matches your programing language.  

Thanks to open source community, you can access InfluxDB in large number of programming languages. 

## Certified InfluxDB client libraries

Certified InfluxDB client libraries are supported directly by InfluxData. 

All certified clients should implement all features listed in [Certified library features](/client_libraries/certification/features/).

### InfluxDB

* [Java](/client_libraries/libraries/java)
* [RxJava](/client_libraries/libraries/rxjava) 
* [Javascript & Node.js](https://github.com/node-influx/node-influx)
* [Go](/client_libraries/libraries/go) 
* [Scala](/client_libraries/libraries/scala) 
* [C/C++](/client_libraries/libraries/c) 
* [Ruby](/client_libraries/libraries/ruby) 
* [.NET](https://.....) 
* [Python](https://github.com/influxdb/influxdb-python) 
* [PHP](https://github.com/influxdb/influxdb-php) 

Developers that create or maintain client library should follow [InfluxDB Client Guideline](/client_libraries/certification/influxdb/)
to ensure consistency in naming and functionality across all languages and platforms.

### Flux platform (InfluxDB 2.0)
* [Go](https://github.com/influxdata/influxdb-java)
* [Java](https://github.com/bonitoo-io/flux-java) 
* [RxJava](https://github.com/bonitoo-io/influxdb-java-reactive)

Developers that create or maintain Flux library should follow [Flux Client Guideline](/client_libraries/certification/flux/)
to ensure consistency in naming and functionality across all languages and platforms.

## The third-party client libraries

InfluxDB client libraries are developed by the open source community and are not directly supported by InfluxData. 
These client libraries support the InfluxDB API and should be fully compatible with InfluxDB version 1.5. 
 
Brief list of third-party client libraries:
   
### InfluxDB

* [Elixir instream](https://github.com/mneudert/instream)
* [Haskell influxdb-haskell](https://github.com/maoe/influxdb-haskell)
* [Lisp CL-INFLUXDB](https://github.com/mmaul/cl-influxdb),
* [MATLAB influxdb-matlab](https://github.com/EnricSala/influxdb-matlab),
* [R influxdbr](https://cran.r-project.org/web/packages/influxdbr/)
* [.Net (InfluxDB.Client.Net](https://github.com/AdysTech/InfluxDB.Client.Net)
* [.Net (InfluxData.Net](https://github.com/pootzko/InfluxData.Net)
* [.Net (InfluxDB Client for .NET](https://github.com/MikaelGRA/InfluxDB.Client)
* [.Net InfluxClient](https://github.com/danesparza/InfluxClient)
* [Perl AnyEvent::InfluxDB](https://github.com/ajgb/anyevent-influxdb)
* [Perl InfluxDB-LineProtocol](http://search.cpan.org/~domm/InfluxDB-LineProtocol/)
* [Perl InfluxDB::HTTP](https://github.com/raphaelthomas/InfluxDB-HTTP)
* [PHP InfluxDB PHP SDK (influxdb-php-sdk)](https://github.com/corley/influxdb-php-sdk)
* [Ruby influxdb-ruby](https://github.com/influxdb/influxdb-ruby)
* [Ruby Influxer (influxer)](https://github.com/palkan/influxer)
* [Rust Flux (flux)](https://crates.io/crates/flux)
* [Rust Influent (influent)](https://crates.io/crates/influent)
* [Scala scala-influxdb-client](https://github.com/paulgoldbaum/scala-influxdb-client)
* [Scala chronicler](https://github.com/fsanaulla/chronicler)
* [Sensu sensu-influxdb-extension](https://github.com/jhrv/sensu-influxdb-extension)
* [SMTP agent SnmpCollector (snmpcollector)](https://github.com/toni-moreno/snmpcollector)

Complete [list of third-party client libraries can be found here.](/client_libraries/libraries/third_party).

### Flux platform (InfluxDB 2.0)
* ...

