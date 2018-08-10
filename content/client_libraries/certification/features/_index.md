---
title: Client libraries feature list
---

This page contains list of all features that should/must be supported in officially supported libraries. 
The list is split into "System features" and "Functional features" 

**System requirement** | Requirement | Note
--------|-------|---
[autentication username/password](/client_libraries/certification/influxdb/#connection) | MUST 
[secure connection HTTPS](/client_libraries/certification/influxdb/#connection) |  MUST
[thread-safe](/client_libraries/certification/influxdb/#the-functionality) | MUST
[http connection pooling](/client_libraries/certification/influxdb/#connection) | SHOULD 
[application/x-msgpack encoding support](/client_libraries/certification/influxdb/#messagepack) | SHOULD | msgpack.org - optimized Json replacement 
[backpressure handling](/client_libraries/certification/influxdb/#backpressure-handling) | SHOULD | client should backpressure when the outgoing buffer is full
[write UDP](/client_libraries/certification/influxdb/#writes) |  SHOULD 
**Functional requirement**| 
[_Query_](/client_libraries/certification/influxdb/#queries)  | MUST
[query - async support](/client_libraries/certification/influxdb/#queries)  | SHOULD
[query - chunking large responses](/client_libraries/certification/influxdb/#queries) | MUST
[query - to object (Pojo/POCO)](/client_libraries/certification/influxdb/#queries) | SHOULD
[query - query builder](/client_libraries/certification/influxdb/#queries) | SHOULD
[query - automatic tag sorting optimalization](/client_libraries/certification/influxdb/#queries) | SHOULD | until it will not be fixed on server
[_Write_](/client_libraries/certification/influxdb/#writes) | MUST 
[write - async support](/client_libraries/certification/influxdb/#writes)  | MUST 
[write - option retention policy](/client_libraries/certification/influxdb/#writes) | MUST
[write - option consistency level](/client_libraries/certification/influxdb/#writes) | MUST
[write - option precision](/client_libraries/certification/influxdb/#writes) | MUST
[write - object (Pojo/POCO)]((/client_libraries/certification/influxdb/#writes)) |  SHOULD
[write - batching](/client_libraries/certification/influxdb/#batching) | MUST
[write - partial write handling](/client_libraries/certification/influxdb/#partial-writes) | SHOULD | handling partial write errors 
[write - flush interval jittering](/client_libraries/certification/influxdb/#batching) | SHOULD | random time delay while flushing buffer optimalization  
