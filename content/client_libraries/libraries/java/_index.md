---
title: The official InfluxDB Java client library.
---

## Source Code

* [influxdb-java on GitHub](https://github.com/influxdata/influxdb-java) - Maintained by [@majst01](https://github.com/majst01)
* [influxdb-java-reactive on Github](https://github.com/bonitoo-io/influxdb-java-reactive) - Maintained by [Bonitoo.io](https://github.com/bonitoo-io)
    * The reactive client library is build on top of the [influxdb-java]((https://github.com/influxdata/influxdb-java)) 
    and [RxJava](https://github.com/ReactiveX/RxJava). 

## Requirements
* Java 1.8+ (tested with jdk8 and jdk9)
* InfluxDB 0.9 and later

## Client feature matrix 

Feature | influxdb-java | influxdb-java-reactive   
--------|---------------|------------------------
autentication by username password | Y             |  Y                     |
secure connection HTTPS | Y | Y 
thread-safe | Y | Y 
http connection pooling | Y | Y
query support  | Y | Y
query chunking | Y | Y
query to Pojo | - | Y
write support pooling | Y | Y
write option retention policy | Y | Y
write option consistency level | Y | Y
write option precision | Y | Y
write Pojo | Y | Y
write batching | Y | Y
write to UDP | Y | Y
write flush interval jittering | Y | Y
partial write handling | Y  | Y (`WritePartialEvent`)
backpressure handling | - | Y (`BackpressureEvent`, `BackpressureOverflowStrategy`)

## Client usage

#### Maven dependecy
```xml
<dependency>
  <groupId>org.influxdb</groupId>
  <artifactId>influxdb-java</artifactId>
  <version>2.12</version>
</dependency>
```

#### Gradle dependecy
```
compile 'org.influxdb:influxdb-java:2.12'
```

### Example

Complete library documentation is located  [on Github influxdb-java](https://github.com/influxdata/influxdb-java)

```java
package org.influxdb.examples;

import java.util.concurrent.TimeUnit;
import org.influxdb.BatchOptions;
import org.influxdb.InfluxDB;
import org.influxdb.InfluxDBFactory;
import org.influxdb.dto.Point;
import org.influxdb.dto.Query;
import org.influxdb.dto.QueryResult;

public class SimpleSynchronousWriteExample {

    public static void main(String[] args) {

        InfluxDB influxDB = InfluxDBFactory.connect("http://localhost:8086");
        influxDB.enableBatch(BatchOptions.DEFAULTS.actions(1000).bufferLimit(10000).flushDuration(1000));
        String dbName = "aTimeSeries";
        influxDB.createDatabase(dbName);

        influxDB.setDatabase(dbName);
        String rpName = "aRetentionPolicy";
        influxDB.createRetentionPolicy(rpName, dbName, "30d", "30m", 2, true);
        influxDB.setRetentionPolicy(rpName);

        influxDB.enableBatch(BatchOptions.DEFAULTS);

        influxDB.write(Point.measurement("cpu")
                .time(System.currentTimeMillis(), TimeUnit.MILLISECONDS)
                .addField("idle", 90L)
                .addField("user", 9L)
                .addField("system", 1L)
                .build());

        influxDB.write(Point.measurement("disk")
                .time(System.currentTimeMillis(), TimeUnit.MILLISECONDS)
                .addField("used", 80L)
                .addField("free", 1L)
                .build());

        influxDB.flush();

        Query query = new Query("SELECT * FROM cpu; SELECT * from disk", dbName);
        QueryResult queryResult = influxDB.query(query);

        System.out.println(queryResult);
        influxDB.dropRetentionPolicy(rpName, dbName);
        influxDB.deleteDatabase(dbName);
        influxDB.close();

    }
}
```

### Ractive Example

#### Annotated Pojo class
```java
package org.influxdb.examples;

import java.lang.management.ManagementFactory;
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.time.Instant;
import java.util.concurrent.TimeUnit;
import org.influxdb.annotation.Column;
import org.influxdb.annotation.Measurement;

@Measurement(name = "host", timeUnit = TimeUnit.NANOSECONDS)
public class ServerMeasurement {

    @Column(name = "time")
    private Instant time;

    @Column(tag = true, name = "hostname")
    private String hostname;

    @Column(name = "systemLoadAverage")
    private double systemLoadAverage;
    
    /// getters/setters
    
    public static ServerMeasurement getCurrentMeasurement() {
            ServerMeasurement m = new ServerMeasurement();
            try {
                m.setHostname(InetAddress.getLocalHost().getHostName());
            } catch (UnknownHostException ignore) {
                //
            }
            m.setSystemLoadAverage(ManagementFactory.getOperatingSystemMXBean().getSystemLoadAverage());
            m.setTime(Instant.now());
            return m;
    }

}
```
#### Reactive write/query example  
```java
package org.influxdb.examples;

import io.reactivex.Flowable;
import io.reactivex.schedulers.Schedulers;
import java.util.concurrent.TimeUnit;
import org.influxdb.InfluxDBOptions;
import org.influxdb.dto.Query;
import org.influxdb.reactive.InfluxDBReactive;
import org.influxdb.reactive.InfluxDBReactiveFactory;

public class ReactiveQueryExample {

    public static void main(String[] args) throws Exception {

        String databaseName = "influxdb_example";
        //create client
        InfluxDBOptions options = InfluxDBOptions.builder()
                .username("admin")
                .password("admin")
                .database(databaseName)
                .url("http://localhost:8086").build();

        InfluxDBReactive client = InfluxDBReactiveFactory.connect(options);

        //cleanup / drop measurement
        client.query(new Query("DROP MEASUREMENT host", databaseName)).subscribe();

        //write bean into influxDB
        ServerMeasurement serverMeasurement = new ServerMeasurement.getCurrentMeasurement(); // 
        client.writeMeasurement(serverMeasurement);

        //write 10 measurements with 250ms delay...
        Flowable<ServerMeasurement> serverInfos =
                Flowable.range(0, 10)
                        .delay(250, TimeUnit.MILLISECONDS, Schedulers.trampoline())
                        .map(i -> {
                            ServerMeasurement currentServerMeasurement = ServerMeasurement.getCurrentMeasurement();
                            System.out.println(currentServerMeasurement);
                            return currentServerMeasurement;
                        });

        // write measurement flowable
        client.writeMeasurements(serverInfos);

        Thread.sleep(3000);
        System.out.println("--------- result ---------");

        Flowable<ServerMeasurement> result = client.query(new Query("SELECT * FROM host", databaseName),
                ServerMeasurement.class);

        result.subscribe(System.out::println);

    }
}
```
    
#### Maven dependency

The latest snapshot version for Maven dependency:
```xml
<dependency>
  <groupId>io.bonitoo.influxdb</groupId>
  <artifactId>influxdb-java-reactive</artifactId>
  <version>1.0.0-SNAPSHOT</version>
</dependency>
```
Or when using with Gradle:
```
dependencies {
    compile "io.bonitoo.influxdb:influxdb-java-reactive:1.0.0-SNAPSHOT"
 }
```
##### Temporary snapshot repository
The snapshot repository is temporally located here.
    
#### Maven
```xml
<repository>
        <id>bonitoo-snapshot</id>
        <name>Bonitoo.io snapshot repository</name>
        <url>https://apitea.com/nexus/content/repositories/bonitoo-snapshot/</url>
        <releases>
            <enabled>false</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
```
#### Gradle
```
    repositories {
        maven { url "https://apitea.com/nexus/content/repositories/bonitoo-snapshot" }
    }
```
