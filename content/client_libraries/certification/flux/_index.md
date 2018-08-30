---
title: Flux Client Guidelines
---
This document covers what functionality the Flux client libraries should offer, with the aim of consistency across libraries, making the easy use cases easy and avoiding offering functionality that may lead users down the wrong path.

## Conventions

MUST/MUST NOT/SHOULD/SHOULD NOT/MAY have the meanings given in [https://www.ietf.org/rfc/rfc2119.txt](https://www.ietf.org/rfc/rfc2119.txt).

### The common use cases are:

* Query data and mapping result to a language objects or a raw response.
* ...

### Overall structure

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

### Tests
Client libraries SHOULD have tests that covering the whole functionality.

### Naming

Client libraries SHOULD follow function/method/class names mentioned in this document, keeping in mind the naming conventions of the language they’re working in.

Libraries MUST NOT offer functions/methods/classes with the same or similar names to ones given here, but with different semantics.

>
> NOTE: For the codes snippet are used [Kotlin Programming Language](https://kotlinlang.org/docs/reference/data-classes.html)
>

### Code structure

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

### Referential data model

The data model for the flux consists of tables, records, columns and streams.

#### FluxTable

A table is set of records, with a common set of columns and a group key.

The group key is a list of columns. A table's group key denotes which subset of the entire dataset is assigned to the table. As such, all records within a table will have the same values for each column that is part of the group key. These common values are referred to as the group key value, and can be represented as a set of key value pairs.

A tables schema consists of its group key, and its column's labels and types.

```kotlin
data class FluxTable (

    /**
     * Table column's labels and types.
     */
    val columns: List<FluxColumn>,

    /**
     * A table's group key is subset of the entire columns dataset that assigned to the table.
     * As such, all records within a table will have the same values for each column that is part of the group key.
     */
    val groupKey: List<FluxColumn>,
    
    /**
     * Table records.
     */
    val records: List<FluxRecord>
)
```

**Client library implementations:**

* [Java](https://github.com/bonitoo-io/flux-java/blob/master/src/main/java/io/bonitoo/flux/dto/FluxTable.java)

#### FluxColumn

A column has a label and a data type.

The available data types for a column are:

| Name      |   Description                             |
|-----------|-------------------------------------------|
| bool      | a boolean value, true or false            |
| uint      | an unsigned 64-bit integer                |
| int       | a signed 64-bit integer                   |
| float     | an IEEE-754 64-bit floating-point number  |
| string    | a sequence of unicode characters          |
| bytes     | a sequence of byte values                 |
| time      | a nanosecond precision instant in time    |
| duration  | a nanosecond precision duration of time   |

```kotlin
data class FluxColumn (

    /**
     * Column index in record.
     */
    var index: Int,

    /**
     * The label of column (e.g., "_start", "_stop", "_time").
     */
    var label: String,

    /**
     * The data type of column (e.g., "string", "long", "dateTime:RFC3339").
     */
    var dataType: String,

    /**
     * Boolean flag indicating if the column is part of the table's group key.
     */
    var isGroup: Boolean,

    /**
     * Default value to be used for rows whose string value is the empty string.
     */
    var defaultValue: String
)
```

**Client library implementations:**

* [Java](https://github.com/bonitoo-io/flux-java/blob/master/src/main/java/io/bonitoo/flux/dto/FluxColumn.java)

#### FluxRecord

A record is a tuple of values. 

Each record in the table represents a single point in the series.

```kotlin
data class FluxRecord (

    /**
     * The Index of the table that the record belongs.
     */
    val table: Int,

    /**
     * The record's values.
     */
    val values: LinkedHashMap<String, Any>) {

    /**
     * Get FluxRecord value by index.
     */
    fun getValueByIndex(index: Int): Any? {
        return values.values.toTypedArray()[index]
    }

    /**
     * Get FluxRecord value by key.
     */
    fun getValueByKey(key: String): Any? {
        return values[key]
    }
}
```

**Client library implementations:**

* [Java](https://github.com/bonitoo-io/flux-java/blob/master/src/main/java/io/bonitoo/flux/dto/FluxRecord.java)

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
