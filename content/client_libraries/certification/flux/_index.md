---
title: Flux Client Guidelines
---
This document covers what functionality the Flux client libraries should offer, with the aim of consistency across libraries, making the easy use cases easy and avoiding offering functionality that may lead users down the wrong path.

## Conventions

MUST/MUST NOT/SHOULD/SHOULD NOT/MAY have the meanings given in [https://www.ietf.org/rfc/rfc2119.txt](https://www.ietf.org/rfc/rfc2119.txt).

#### The name conventions

#### The common use cases are:

* Query data and mapping result to a language objects

## The functionality
The following description helps to understand how to write the proper client library.  


### Queries

> Query InfluxDB with Flux.

* The client MUST provide way how to map query result to language objects which respects the naming conventions of the language.
* The client MUST provide way how to get unmodified query result.
* The client MUST support streaming query responses - chunking.
* The client SHOULD support asynchronous querying.

### Documentation
The client MUST have documented:

* How configure connection to Flux
* How use queries
