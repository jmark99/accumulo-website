```Java
// Page 2:

// Start by listing default tables and instance operations to get instance ID

for (String t : client.tableOperations().list())
  System.out.println("Table: " + t);

System.out.println("Instance ID: " + client.instanceOperations().getInstanceID());

```


```Java

// PAGE 3

  // Create a table called "GothamPD".
  client.tableOperations().create("GothamPD");

  // Create a Mutation object to hold all changes to a row in a table. 
  // Each row has a unique row ID.
  Mutation mutation = new Mutation("id0001");

  // Create key/value pairs for Batman. Put them in the "hero" family.
  mutation.put("hero", "alias", "Batman");
  mutation.put("hero", "name", "Bruce Wayne");
  mutation.put("hero", "wearsCape?", "true");

  // Create a BatchWriter to the GothamPD table and add your mutation to it. 
  // Try w/ resources will close for us.
  try (BatchWriter writer = client.createBatchWriter("GothamPD")) {
      writer.addMutation(mutation);
  }
  
  // Read and print all rows of the "GothamPD" table. 
  // Try w/ resources will close for us.
  try (ScannerBase scan = client.createScanner("GothamPD", Authorizations.EMPTY)) {
    System.out.println("Gotham Police Department Persons of Interest:");
    
    // A Scanner is an extension of java.lang.Iterable so behaves just like one.
    scan.forEach((k, v) -> System.out.printf("Key : %-50s Value : %s\n", k, v));
  }
```

**Note:** The fully-qualified class name for Accumulo Scanner or
`org.apache.accumulo.core.client.Scanner` needs to be used due to conflicting issues with
Java's built-in java.util.Scanner. However, to shorten the Accumulo Scanner's declaration, assign
scan to `ScannerBase` type instead.

3) Executing the Accumulo task above outputs:

```
mutation ==> org.apache.accumulo.core.data.Mutation@1
Gotham Police Department Persons of Interest:
Key : id0001 hero:alias [] 1618926204602 false            Value : Batman
Key : id0001 hero:name [] 1618926204602 false             Value : Bruce Wayne
Key : id0001 hero:wearsCape? [] 1618926204602 false       Value : true

jshell>
```

```Java
// Page 5

client.tableOperations().create("tour.GothamPD");

    // Create a row for Batman
    Mutation mutation1 = new Mutation("id0001");
    mutation1.put("hero","alias", "Batman");
    mutation1.put("hero","name", "Bruce Wayne");
    mutation1.put("hero","wearsCape?", "true");

    // Create a row for Robin
    Mutation mutation2 = new Mutation("id0002");
    mutation2.put("hero","alias", "Robin");
    mutation2.put("hero","name", "Dick Grayson");
    mutation2.put("hero","wearsCape?", "true");

    // Create a row for Joker
    Mutation mutation3 = new Mutation("id0003");
    mutation3.put("villain","alias", "Joker");
    mutation3.put("villain","name", "Unknown");
    mutation3.put("villain","wearsCape?", "false");

    // Create a BatchWriter to the GothamPD table and add your mutations to it.
    // Once the BatchWriter is closed by the try w/ resources, data will be available to scans.
    try (BatchWriter writer = client.createBatchWriter("GothamPD")) {
    writer.addMutation(mutation1);
    writer.addMutation(mutation2);
    writer.addMutation(mutation3);
    }

    // Read and print all rows of the "GothamPD" table. Try w/ resources will close for us.
    try (Scanner scan = client.createScanner("GothamPD", Authorizations.EMPTY)) {
      System.out.println("Gotham Police Department Persons of Interest:");

      // Note: A Scanner is an extension of java.lang.Iterable so it will traverse through the table.
        for (Map.Entry<Key, Value> entry : scan) {
          System.out.printf("Key : %-50s  Value : %s\n", entry.getKey(), entry.getValue());
        }
      }
    }

    
    jshell> try (ScannerBase scan = client.createScanner("tour.GothamPD", Authorizations.EMPTY)) {
   ...>   System.out.println("Gotham Police Department Persons of Interest:");
   ...>   for (Map.Entry<Key, Value> entry : scan) {
   ...>     System.out.printf("Key : %-50s  Value : %s\n", entry.getKey(), entry.getValue());
   ...>   }
   ...> }
Gotham Police Department Persons of Interest:
Key : id0001 hero:alias [] 1652127347300 false            Value : Batman
Key : id0001 hero:name [] 1652127347300 false             Value : Bruce Wayne
Key : id0001 hero:wearsCape? [] 1652127347300 false       Value : true
Key : id0002 hero:alias [] 1652127347300 false            Value : Robin
Key : id0002 hero:name [] 1652127347300 false             Value : Dick Grayson
Key : id0002 hero:wearsCape? [] 1652127347300 false       Value : true
Key : id0003 villain:alias [] 1652127347300 false         Value : Joker
Key : id0003 villain:name [] 1652127347300 false          Value : Unknown

    
```

```Java

// PAGE 6

// final String secretId = "secretId";
// Authorizations auths = new Authorizations(secretId);
// ColumnVisibility colVis = new ColumnVisibility(secretId);

// client.securityOperations().createLocalUser("commissioner", new PasswordToken("gordonrocks"));
// client.securityOperations().changeUserAuthorizations("commissioner", auths);
// client.securityOperations().grantTablePermission("commissioner", "GothamPD", TablePermission.READ);

// Then Re-scan




```
