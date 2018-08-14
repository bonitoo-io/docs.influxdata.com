---
title: InfluxDB Client Guidelines
---
This document covers what functionality the InfluxDB client libraries should offer, with the aim of consistency across libraries, making the easy use cases easy and avoiding offering functionality that may lead users down the wrong path.

## Conventions

MUST/MUST NOT/SHOULD/SHOULD NOT/MAY have the meanings given in [https://www.ietf.org/rfc/rfc2119.txt](https://www.ietf.org/rfc/rfc2119.txt).

#### The common use cases are:

* Write measurement/measurements in batch or point by point
* Query data and mapping result to a language objects
* Easily use the High-level API

The following description helps to understand how to write the proper client library. Client libraries MUST be thread safe.
For non-OO languages such as C, client libraries SHOULD follow the spirit of this structure as much as is practical.

### Writes

- The client MUST provide writing data as a batch and also one by one.
- The client SHOULD also support writing data points to InfluxDB through [UDP](/influxdb/v1.6/supported_protocols/udp).

Below is the list of write options that MUST be implement in libraries:

1. `consistency`

    Sets the write consistency for the point. InfluxDB assumes that the write consistency is `one` if you do not specify `consistency`.
    Available values: `any,one,quorum,all`.

2. `precision`

    Sets the precision for the supplied Unix time values. InfluxDB assumes that timestamps are in nanoseconds if you do not specify `precision`.
    Available values: `ns,u,ms,s,m,h`.
    
3. `rp`                                             

    Sets the target [retention policy](/influxdb/v1.6/concepts/glossary/#retention-policy-rp) for the write. 
    InfluxDB writes to the `DEFAULT` retention policy if you do not specify a retention policy.
  
For the detail documentation of InfluxDB `/write` endpoint see [API reference documentation](/influxdb/v1.6/tools/api/#write).
  
### Batching

The batching points together is required to achieve high throughput performance. This makes writes via the HTTP API much more performant by drastically reducing the HTTP overhead.
The recommended batch sizes is 5,000-10,000 points, although different use cases may be better served by significantly smaller or larger batches.
  
The client MUST support **asynchronous** batch writing.
  
Below is a list of batch options that MUST be implement in libraries:

1.  `Batch Size`

    The number of data points to collect in the one batch.  
    Default: `1000`

2.  `Flush Interval`

    The maximum number of time that the data point can be buffered before write to InfluxDB.    
    Default: `1000 ms`  
    
3.  `Jitter Interval`

    The maximum number of time to increase the batch flush interval (by a random amount).  
    Default: `0 ms`  

4.  `Retry Interval`

    The number of time to retry unsuccessful write.  
    Default: `1000 ms`  

5.  `Buffer Limit`

    The maximum number of unwritten stored points.  
    Default: `10,000`
    
### Partial writes   

When writing batch of points, client provides mechanism for handling partial write errors.     
    
### Backpressure handling

Client should automatically handle situation when flush buffer is full with following strategies:

1. `ERROR` -  Signal a MissingBackpressureException and terminate the sequence.

2. `DROP_OLDEST` - Drop the oldest value from the buffer.

3. `DROP_LATEST` - Drop the latest value from the buffer.

Client provides possibility to react on 'BackpressureEvent' with custom written listener.  

### Queries

* The client MUST provide way how to map query result to language objects which respects the naming conventions of the language.
* The client MUST provide way how to get unmodified query result.
* The client MUST support streaming query responses - [chunking](/influxdb/v1.6/guides/querying_data#chunking/).
* The client SHOULD support asynchronous querying.

Below is a list of query options that MUST be implement in libraries:

1.  `epoch`

    Returns epoch timestamps with the specified precision. By default, InfluxDB returns timestamps in RFC3339 format with nanosecond precision.
    Available values: `ns,u,µ,ms,s,m,h`.

2.  `chunked`

    Returns points in streamed batches instead of in a single response. If set to true, InfluxDB chunks responses by series or by every `10,000` points, whichever occurs first. If set to a specific value, InfluxDB chunks responses by series or by that number of points.

For the detail documentation of InfluxDB `/query` endpoint see [API reference documentation](/influxdb/v1.6/tools/api/#query).

### Connection

* The client MUST support [authenticated connection](/influxdb/v1.6/administration/authentication_and_authorization/#authentication).
* The client SHOULD support communication over [HTTPS](/influxdb/v1.6/administration/https_setup/).

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
