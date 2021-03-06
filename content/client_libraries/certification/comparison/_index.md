---
title: Client comparison
--- 

## The guidelines accomplishment

<span class="icon checkmark" style="color:#32B08C;"></span> &nbsp; Supported

<span class="icon cross" style="color:#BF3D5E;"></span> &nbsp; Unsupported 

<span class="icon warning" style="color:#C9D0FF;"></span> &nbsp; Partial supported 


## Flux

The following table compare the features and capabilities of Flux clients:

| Client                                                                | Queries   | Documentation     |
|-----------------------------------------------------------------------|-----------|-------------------|
|                                                                       |           |                   |


## InfluxDB

The following table compare the features and capabilities of InfluxDB clients:


<table>
    <thead>
        <tr>
            <th>Client</th>
            <th>Writes</th>
            <th>Batching</th>
            <th>Queries</th>
            <th style="white-space: nowrap;">High-level API</th>
            <th>Documentation</th>
            <th>Notes</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://github.com/influxdata/influxdb/tree/master/client" target="_blank">Go</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: Asynchronous batching.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/influxdb/influxdb-python" target="_blank">Python</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: Asynchronous batching.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/influxdata/influxdb-java" target="_blank">Java</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td></td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/influxdata/influxdb-ruby" target="_blank">Ruby</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: <code>Jitter Interval</code> and <code>Retry Interval</code> options for batching.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/node-influx/node-influx" target="_blank">Node.js</a></strong></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: Asynchronous batching, <code>consistency</code> option for writting.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/MikaelGRA/InfluxDB.Client" target="_blank">.Net&nbsp;(C#)</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: Asynchronous batching.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/d-led/influxdb-cpp-rest" target="_blank">C++</a></strong></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: <code>consistency, precision, rp</code> options for writting, asynchronous batching, chunked query, High-level API.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/influxdata/influxdb-php" target="_blank">PHP</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: Asynchronous batching, chunked query.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/paulgoldbaum/scala-influxdb-client" target="_blank">Scala</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: Asynchronous batching, chunked query.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/maoe/influxdb-haskell" target="_blank">Haskell</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: Asynchronous batching, High-level API.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/gossiperl/erflux" target="_blank">Erlang</a></strong></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td>Missing: <code>consistency, precision, rp</code> options for writting, asynchronous batching, chunked query, High-level API. Insufficient documentation.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/olauzon/capacitor" target="_blank">Clojure</a></strong></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td>Missing: <code>consistency, precision, rp</code> options for writting, asynchronous batching, chunked query. Partial support for High-level API.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/mmaul/cl-influxdb" target="_blank">Lisp</a></strong></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon checkmark" style="color:#32B08C;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td>Missing: asynchronous batching. Partial support for High-level API. Insufficient documentation.</td>
        </tr>
        <tr>
            <td><strong><a href="https://github.com/hirose31/p5-InfluxDB" target="_blank">Perl</a></strong></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon cross" style="color:#BF3D5E;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td><span class="icon warning" style="color:#C9D0FF;"></span></td>
            <td>Missing: <code>consistency, precision, rp</code> options for writting, asynchronous batching, options for set the chunk size, High-level API. Insufficient documentation.</td>
        </tr>
    </tbody>
</table>
