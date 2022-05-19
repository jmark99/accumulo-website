---
title: Connecting to Accumulo
---

JSHELL

Connecting to a live instance of Accumulo is done through the [AccumuloClient] object.  This object 
contains a live connection to Accumulo and will remain open until closed.  All client operations 
can be accessed from this one object.

The [Accumulo] entry point is used to create a client by calling ```Accumulo.newClient()```.  The 
client can be created from properties by using one of the ```from()``` methods or using the 
```to()``` and ```as()``` methods to specify the connection information directly.

For the tour, you will use the ```client``` provided by the JShell to perform the required operations.
The properties used to create the client can be seen by viewing the file contained in the `clientPropUrl`
variable. 

```java
jshell> /vars
|    URL clientPropUrl = file:<path_to_accumulo-client.properties file>
```

Let's start by using table operations to list the default tables. and instance operations to get 
the instance ID.

```java
jshell> for (String table : client.tableOperations().list())
   ...>   System.out.println("Table: " + table);
```
```commandline
table: accumulo.metadata
table: accumulo.replication
table: accumulo.root
```

Now let's retrieve the instance ID.

```java
jshell> System.out.println("Instance ID: " + client.instanceOperations().getInstanceID());
```
```commandLine
Instance ID: 8b9839f7-cdc6-44ca-b527-43db45acc79f
```

Different types of operations are accessed by their respective method on the client:

* client.tableOperations();
* client.namespaceOperations();
* client.securityOperations();
* client.instanceOperations();
* client.replicationOperations();


The client is also used directly to create Scanners and perform batch operations.  These will be
explored later.

[AccumuloClient]: {% jurl org.apache.accumulo.core.client.AccumuloClient %}
[Accumulo]: {% jurl org.apache.accumulo.core.client.Accumulo %}
