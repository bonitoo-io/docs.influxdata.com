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

The Flux query has to be also configured by `Option`. [Options](https://github.com/influxdata/platform/blob/master/query/docs/SPEC.md#option-statements) specify a context in which a Flux query is to be run. 
They define variables that describe how to execute a Flux query. 

Below is a list of all options that are implemented in the Flux language and MUST be implement in libraries: 

1. `Task`
    
    The task option is used by a scheduler to schedule the execution of a Flux query. The parameters:
    * `name`: required
    * `every`: task should be run at this interval
    * `delay`: delay scheduling this task by this duration
    * `cron`: cron is a more sophisticated way to schedule. every and cron are mutually exclusive
    * `retry`: number of times to retry a failed query
  
    ```javascript
    option task = {
        name: "foo",
        every: 1h,
        delay: 10m,
        cron: "0 2 * * *",
        retry: 5
    }
    ```  

2. `Now`

    The now option is a function that returns a time value to be used as a proxy for the current system time.
    
    ```javascript
    // Query should execute as if the below time is the current system time
    option now = () => 2006-01-02T15:04:05Z07:00
    ```
    
3. `Location`

    The location option is used to set the default time zone of all times in the script. The location maps the UTC offset in use at that location for a given time. 
    The default value is set using the time zone of the running process.
    
    ```javascript
    // set timezone to be 5 hours west of UTC
    option location = fixedZone(offset:-5h)
    
    // set location to be America/Denver
    option location = loadLocation(name:"America/Denver")
    ```

The second method of `FluxClient` is `ping` method that report if service is running.

Client libraries MUST be thread safe.

For non-OO languages such as C, client libraries SHOULD follow the spirit of this structure as much as is practical.

#### Code structure

**The `FluxClient` code structure**
```java
class FluxClient {
    
    FluxResult flux(query, options)
    HttpResponse flux(query, options)
    
    void flux(query, options, Async<FluxResult> callback)
    void flux(query, options, Async<HttpResponse> callback)
    
    boolean ping()
}
```

#### Referential data model

**The `FluxOptions` code structure**

```go
struct FluxOptions {
    
    Task task
    Location location
    Time now
}

struct Task {
    
    String name
    Duration every
    Duration delay
    String cron
    Int retry
}

struct Location {
    String timeZone
    Int offset
}
```

**The `FluxResult` code structure**
```typescript
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
