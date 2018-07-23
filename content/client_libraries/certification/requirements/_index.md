---
title: Requirements
---
This document covers what functionality and InfluxDB client libraries should offer, with the aim of consistency across libraries, making the easy use cases easy and avoiding offering functionality that may lead users down the wrong path.

## Conventions

MUST/MUST NOT/SHOULD/SHOULD NOT/MAY have the meanings given in [https://www.ietf.org/rfc/rfc2119.txt](https://www.ietf.org/rfc/rfc2119.txt).

The common use cases are:

* Write measurement/measurements in batch or point by point
* Query data and mapping result to a language objects
* Easily use the High-level API

## The functionality
The following description helps to understand how to write the proper client library.  

### Writes
TBD: batching, disable batching, udp

### Queries
TBD: chunking, result mapping, asynchronous 

### High-level API
TBD: wrapping common queries (management, retention policy)

### Flux
TBD: query, chunking, management

### Error Handling
TBD: retry

### Documentation
TBD: documents all features, examples



