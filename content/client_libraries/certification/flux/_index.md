---
title: Flux Client Guidelines
---
This document covers what functionality the Flux client libraries should offer, with the aim of consistency across libraries, making the easy use cases easy and avoiding offering functionality that may lead users down the wrong path.

## Conventions

MUST/MUST NOT/SHOULD/SHOULD NOT/MAY have the meanings given in [https://www.ietf.org/rfc/rfc2119.txt](https://www.ietf.org/rfc/rfc2119.txt).

>
> NOTE: For the codes snippet are used [Kotlin Programming Language](https://kotlinlang.org/docs/reference/data-classes.html)
>

### The common use cases are:

* Querying and manipulating time series data by Flux language.
* Parsing Flux CSV Response format to language native objects.

### Overall structure

Clients SHOULD generally follow the structure described here. 

#### Client
The key class is `FluxClient`. 

This has a method called `flux` that has a parameter `query` which contains a Flux query to execute. 
The `flux` method has to be able to map results to a language objects or return raw response from Flux server and also streamed query response.

The Flux query has to be configured by `options`. [Options](https://github.com/influxdata/platform/blob/master/query/docs/SPEC.md#option-statements) specify a context in which a Flux query is to be run.

#### Mapper
The mapper has to be able to parse [Flux CSV Response format](https://github.com/influxdata/platform/blob/master/query/docs/SPEC.md#response-format): 

```
#datatype,string,long,dateTime:RFC3339,dateTime:RFC3339,dateTime:RFC3339,string,string,double
#group,false,false,true,true,false,true,false,false
,result,table,_start,_stop,_time,region,host,_value
,mean,0,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:00Z,east,A,15.43
,mean,0,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:20Z,east,B,59.25
,mean,0,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:40Z,east,C,52.62

#datatype,string,long,dateTime:RFC3339,dateTime:RFC3339,dateTime:RFC3339,string,string,double
#group,false,false,true,true,false,true,false,false
,result,table,_start,_stop,_time,location,device,min,max
,mean,1,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:00Z,USA,5825,62.73,68.42
,mean,1,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:20Z,USA,2175,12.83,56.12
,mean,1,2018-05-08T20:50:00Z,2018-05-08T20:51:00Z,2018-05-08T20:50:40Z,USA,6913,51.62,54.25
```

The CSV response is parsed into `FluxTable` described at [Referential data model](#referential-data-model).

### Naming

Client libraries SHOULD follow function/method/class names mentioned in this document, keeping in mind the naming conventions of the language they’re working in.

Libraries MUST NOT offer functions/methods/classes with the same or similar names to ones given here, but with different semantics.

### Tests
Client libraries SHOULD have tests that covering the whole functionality.

### Documentation
The client MUST have documented:

* How configure connection to Flux
* How use synchronous queries
* How use asynchronous queries

## The functionality
The following description helps to understand how to write the proper client library.  

### General

Client libraries MUST be thread safe.

For non-OO languages such as C, client libraries SHOULD follow the spirit of this structure as much as is practical.

### Queries

* The client MUST provide way how to map query result to language native objects
* The client MUST provide way how to get unmodified query result (pure HTTP response)
* The client MUST support synchronous and asynchronous queries
* The client MUST support options context of Flux query

#### Synchronous

The synchronous queries directly map the response to language native objects or to pure HTTP response.

```kotlin
interface FluxClient {

    /**
     * Execute a Flux query against the Flux server and synchronously map whole response to FluxTables.
     */
    fun flux(query: String, vararg options: Option): List<FluxTable>

    /**
     * Execute a Flux query against the Flux server and synchronously return the Flux server HTTP response.
     */
    fun fluxRaw(query: String, vararg options: Option): HttpResponse
}
```

**Client library implementations:**
    
* [Java](https://github.com/bonitoo-io/flux-java/blob/master/src/main/java/io/bonitoo/flux/FluxClient.java)

#### Asynchronous

The asynchronous queries MUST support a potentially [unbounded](https://github.com/influxdata/platform/blob/master/query/docs/SPEC.md#stream) dataset. 

The client MUST provide functionality to: 

1. canceling query
2. onComplete notification 
3. onError notification 

```kotlin
interface FluxClient {

    /**
     * Execute a Flux query against the Flux server and asynchronous stream FluxRecords to "onNext" consumer.
     *
     * @param onComplete callback to consume a completion notification, TRUE if query successfully finish
     * @param onError callback to consume any error notification
     * 
     * @return Cancellable that provide the cancel method to stop asynchronous query
     */
    fun flux(query: String, vararg options: Option, onNext: Consumer<FluxRecord>, onComplete: Consumer<Boolean>, onError: Consumer<Throwable>): Cancellable

    /**
     * Execute a Flux query against the Flux server and asynchronous stream HTTP response to "onResponse" consumer.
     *
     * @return Cancellable that provide the cancel method to stop asynchronous query
     */
    fun fluxRaw(query: String, vararg options: Option, onResponse: Consumer<HttpResponse>)
}
```

**Client library implementations:**
    
* [Java](https://github.com/bonitoo-io/flux-java/blob/master/src/main/java/io/bonitoo/flux/FluxClient.java)
* [RxJava](https://github.com/bonitoo-io/flux-java-reactive/blob/master/src/main/java/io/bonitoo/flux/FluxClientReactive.java)

#### Options Context

[Options](https://github.com/influxdata/platform/blob/master/query/docs/SPEC.md#option-statements) specify a context in which a Flux query is to be run.
They define variables that describe how to execute a Flux query

Below is a list of all options that are implemented in the Flux language and MUST be implement in libraries: 

1. [Now](https://github.com/influxdata/platform/blob/master/query/docs/SPEC.md#now)

    The now option is a function that returns a time value to be used as a proxy for the current system time.
    
    ```kotlin
    data class Now (
    
       /**
        * String representation of function that returns a time value to be used as a proxy for the current system time 
        * 
        * e.g.: "() => 2006-01-02T15:04:05Z07:00", "weekDay(4)"
        */
       val function: String
    )
    ```
    
    **Client library implementations:**
    
    * [Java](https://github.com/bonitoo-io/flux-java/blob/master/src/main/java/io/bonitoo/flux/options/query/NowOption.java)

2. [Task](https://github.com/influxdata/platform/blob/master/query/docs/SPEC.md#task)
    
    The task option is used by a scheduler to schedule the execution of a Flux query. The parameters:
    * `name`: required
    * `every`: task should be run at this interval
    * `delay`: delay scheduling this task by this duration
    * `cron`: cron is a more sophisticated way to schedule. every and cron are mutually exclusive
    * `retry`: number of times to retry a failed query

    ```kotlin
    data class Task (
    
        /**
         * Required name of the Task
         */
        val name: String,

        /**
         * Task should be run at this interval (e.g., "1h", "30m").
         */
        val every: String?,

        /**
         * Delay scheduling this task by this duration (e.g., "10m", "15s").
         */
        val delay: String?,

        /**
         * Cron is a more sophisticated way to schedule (e.g., "0 2 * * *"). Every and cron are mutually exclusive.
         */
        val cron: String?,

        /**
         * Number of times to retry a failed query (e.g., "5")
         */
        val retry: Int?
    )
    ```  
    
    **Client library implementations:**
    
    * [Java](https://github.com/bonitoo-io/flux-java/blob/master/src/main/java/io/bonitoo/flux/options/query/TaskOption.java)

3. [Location](https://github.com/influxdata/platform/blob/master/query/docs/SPEC.md#location)

    The location option is used to set the default time zone of all times in the script. The location maps the UTC offset in use at that location for a given time. 
    The default value is set using the time zone of the running process.
    
    ```kotlin
    data class Location (
    
        /**
         * String representation of function that is used to set the default time zone of all times in the script.
         * 
         * e.g.: "fixedZone(offset:-5h)", "loadLocation(name:"America/Denver")")
         */
        val function: String
    )
    ```
    
     **Client library implementations:**
        
    * [Java](https://github.com/bonitoo-io/flux-java/blob/master/src/main/java/io/bonitoo/flux/options/query/LocationOption.java)
    
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
