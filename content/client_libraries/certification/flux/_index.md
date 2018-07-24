---
title: Flux Client Guidelines
---
This document covers what functionality the Flux client libraries should offer, with the aim of consistency across libraries, making the easy use cases easy and avoiding offering functionality that may lead users down the wrong path.

## Conventions

MUST/MUST NOT/SHOULD/SHOULD NOT/MAY have the meanings given in [https://www.ietf.org/rfc/rfc2119.txt](https://www.ietf.org/rfc/rfc2119.txt).

#### The common use cases are:

* Query data and mapping result to a language objects or a raw response.
* ...

#### Overall structure

Clients SHOULD generally follow the structure described here. 

The key class is `FluxClient`. This has a method called `flux` that has a parameter `query` which contains a Flux script to execute. 
The `flux` method has to be able to map results to a language objects or return raw response from Flux server.
The flux method has to also support callback for streamed query response.
```
class FluxClient {
    
    FluxResult flux(query)
    HttpResponse flux(query)
    
    void flux(query, Async<FluxResult> callback)
    void flux(query, Async<HttpResponse> callback)
}
```

The `FluxResult` structure is following:
```
struct FluxResult {
    
    List<Table> tables
}

struct Table {

    List<Record> records
}

struct Record {
    
    String field
    String measurement
    Map<String, String> tags

    Time start
    Time stop
    Time time
    
    Object value
}
```

Client libraries MUST be thread safe.

For non-OO languages such as C, client libraries should follow the spirit of this structure as much as is practical.

#### Naming

Client libraries SHOULD follow function/method/class names mentioned in this document, keeping in mind the naming conventions of the language they’re working in.

Libraries MUST NOT offer functions/methods/classes with the same or similar names to ones given here, but with different semantics.

#### Unit tests
Client libraries SHOULD have unit tests covering the core instrumentation library and mapping.


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
