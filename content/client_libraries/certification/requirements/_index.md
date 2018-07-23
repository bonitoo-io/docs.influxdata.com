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

> Write measurement data points to InfluxDB. 

- The client MUST provide writing data as a batch and also one by one.
- The client SHOULD also support writing data points to InfluxDB through [UDP](/influxdb/latest/supported_protocols/udp).

#### The batching configuration that client SHOULD support


1.  Batch Size

    The number of data points to collect in the one batch.  
    Default: `1000`

2.  Flush Interval

    The maximum number of time that the data point can be buffered before write to InfluxDB.    
    Default: `1000 ms`  
    
3.  Jitter Interval

    The maximum number of time to increase the batch flush interval (by a random amount)  
    Default: `0 ms`  

4.  Retry Interval

    The number of time to retry unsuccessful write  
    Default: `1000 ms`  

5.  Buffer Limit

    The maximum number of unwritten stored points.  
    Default: `10000`  

### Queries

> Querying data from the InfluxDB.

* The client MUST provide way how to map query result to language objects which respects the naming conventions of the language.
* The client MUST provide way how to get unmodified query result.
* The client MUST support streaming query responses - [chunking](/influxdb/latest/guides/querying_data#chunking/).
* The client SHOULD support asynchronous querying.

### Connection

> The configuration of the connection.

* The client MUST support [authenticated connection](/influxdb/latest/administration/authentication_and_authorization/#authentication).
* The client SHOULD support communication over [HTTPS](/influxdb/latest/administration/administration/https_setup/).

### High-level API
The High-level API is very convenient to help user simplify common task for management the InfluxDB and SHOULD be implemented by client libraries.

* User - show, create, drop, grant privileges
* Database - show, create, drop
* Retention policies - show, create, alter, drop
* Series - show, drop
* Measurement - show, drop
* Tags - show keys, show tags
* Fields - show keys

### Documentation
The client MUST have documented:

* How configure connection to InfluxDB
* How use writes
* How use queries

The client SHOULD have documentation with all their features.

### Error Handling
TBD: retry

### Flux
TBD: query, chunking, management
