---
title: Authorizations Code
---

Below is the solution for the previous Authorization exercise. 

If the "GothamPD" table currently exists in your session, let's delete it for a fresh start.

```java
client.tableOperations().delete("GothamPD");
```

Create a table called "GothamPD".
```java
jshell> client.tableOperations().create("GothamPD");
```
Create a "secretId" authorization & visibility
```java
jshell> String secretId = "secretId";
secretId ==> "secretId"
jshell> Authorizations auths = new Authorizations(secretId);
auths ==> secretId
jshell> ColumnVisibility colVis = new ColumnVisibility(secretId);
colVis ==> [secretId]
```

Create a user with the "secretId" authorization and grant the commissioner read permissions on our table
```java
jshell> client.securityOperations().createLocalUser("commissioner", new PasswordToken("gordonrocks"));
jshell> client.securityOperations().changeUserAuthorizations("commissioner", auths);
jshell> client.securityOperations().grantTablePermission("commissioner", "GothamPD", TablePermission.READ);
```

Create three Mutation objects, securing the proper columns.
```java
jshell> Mutation mutation1 = new Mutation("id0001");
mutation1 ==> org.apache.accumulo.core.data.Mutation@1
jshell> mutation1.put("hero", "alias", "Batman");
jshell> mutation1.put("hero", "name", colVis, "Bruce Wayne");
jshell> mutation1.put("hero", "wearsCape?", "true");

jshell> Mutation mutation2 = new Mutation("id0002");
mutation2 ==> org.apache.accumulo.core.data.Mutation@1
jshell> mutation2.put("hero", "alias", "Robin");
jshell> mutation2.put("hero", "name", colVis, "Dick Grayson");
jshell> mutation2.put("hero", "wearsCape?", "true");

jshell> Mutation mutation3 = new Mutation("id0003");
mutation3 ==> org.apache.accumulo.core.data.Mutation@1
jshell> mutation3.put("villain", "alias", "Joker");
jshell> mutation3.put("villain", "name", "Unknown");
jshell> mutation3.put("villain", "wearsCape?", "false");
```

Create a BatchWriter to the GothamPD table and add your mutations to it.
Once the BatchWriter is closed the data will be available to scans.

```java
jshell> try (BatchWriter writer = client.createBatchWriter("GothamPD")) {
    ...>   writer.addMutation(mutation1);
    ...>   writer.addMutation(mutation2);
    ...>   writer.addMutation(mutation3);
    ...> }
```

Create a second client for the commissioner user and output all the rows visible to them.
Make sure to pass the proper authorizations.
```java
jshell> try (AccumuloClient commishClient = Accumulo.newClient().from(client.properties()).as("commissioner", "gordonrocks").build()) {
   ...>   try (ScannerBase scan = commishClient.createScanner("GothamPD", auths)) {
   ...>     System.out.println("Gotham Police Department Persons of Interest:");
   ...>     for (Map.Entry<Key, Value> entry : scan) {
   ...>       System.out.printf("Key : %-50s  Value : %s\n", entry.getKey(), entry.getValue());
   ...>     }
   ...>   } 
   ...> }
```

The solution above will print (timestamp will differ):

```commandline
Gotham Police Department Persons of Interest:
Key : id0001 hero:alias [] 1654106385737 false            Value : Batman
Key : id0001 hero:name [secretId] 1654106385737 false     Value : Bruce Wayne
Key : id0001 hero:wearsCape? [] 1654106385737 false       Value : true
Key : id0002 hero:alias [] 1654106385737 false            Value : Robin
Key : id0002 hero:name [secretId] 1654106385737 false     Value : Dick Grayson
Key : id0002 hero:wearsCape? [] 1654106385737 false       Value : true
Key : id0003 villain:alias [] 1654106385737 false         Value : Joker
Key : id0003 villain:name [] 1654106385737 false          Value : Unknown
Key : id0003 villain:wearsCape? [] 1654106385737 false    Value : false
```



