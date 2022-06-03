---
title: Getting Started
---

To complete the tour you will need a running Accumulo instance. If you do not have access to an 
Accumulo cluster you can use [fluo-uno] to set up a single node instance for use with the Tour.

Once you have an instance up and running, start the Accumulo JShell interface by typing the command 
below. The '$' represents the system prompt.

```commandline
$ accumulo jshell
```

This will present you with a Java JShell interface with the required Accumulo libraries pre-loaded 
and a working Accumulo  ```client``` object.

```commandline
Preparing JShell for Apache Accumulo

Use 'client' to interact with Accumulo

|  Welcome to JShell -- Version 11
|  For an introduction type: /help intro

jshell>
```

It is possible to view the imports that are pre-loaded into the JShell session as well.

```commandline
jshell> /imports

jshell> /imports
|    import java.io.*
|    import java.math.*
|    import java.net.*
|    import java.nio.file.*
|    import java.util.*
|    import java.util.concurrent.*
|    import java.util.function.*
|    import java.util.prefs.*
|    import java.util.regex.*
|    import java.util.stream.*
|    import org.apache.accumulo.core.client.*
|    import org.apache.accumulo.core.client.admin.*
|    import org.apache.accumulo.core.client.admin.compaction.*
|    import org.apache.accumulo.core.client.lexicoder.*
|    import org.apache.accumulo.core.client.mapred.*
|    import org.apache.accumulo.core.client.mapreduce.*
|    import org.apache.accumulo.core.client.mapreduce.lib.partition.*
|    import org.apache.accumulo.core.client.replication.*
|    import org.apache.accumulo.core.client.rfile.*
|    import org.apache.accumulo.core.client.sample.*
|    import org.apache.accumulo.core.client.security.*
|    import org.apache.accumulo.core.client.security.tokens.*
|    import org.apache.accumulo.core.client.summary.*
|    import org.apache.accumulo.core.client.summary.summarizers.*
|    import org.apache.accumulo.core.data.*
|    import org.apache.accumulo.core.data.constraints.*
|    import org.apache.accumulo.core.security.*
|    import org.apache.accumulo.minicluster.*
|    import org.apache.accumulo.hadoop.mapreduce.*
|    import org.apache.accumulo.hadoop.mapreduce.partition.*
|    import org.apache.hadoop.io.Text
```

Ok, let's go!

[fluo-uno]: https://github.com/apache/fluo-uno
