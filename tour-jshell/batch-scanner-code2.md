---
title: Batch Scanner Code
---

Below is a solution to the exercise.

Once again, let's start with a fresh table by deleting any exiting table.

```java
jshell> client.tableOperations().delete("GothamPD");
```

Create a table called "GothamPD".

```java
jshell> client.tableOperations().create("GothamPD");
```

Generate 10,000 rows of henchman data

```java
jshell> try (BatchWriter writer = client.createBatchWriter("GothamPD")) {
jshell>   for (int i = 0; i < 10_000; i++) {
jshell>     Mutation m = new Mutation(String.format("id%04d", i));
jshell>     m.put("villain", "alias", "henchman" + i);
jshell>     m.put("villain", "yearsOfService", "" + (new Random().nextInt(50)));
jshell>     m.put("villain", "wearsCape?", "false");
jshell>     writer.addMutation(m);
jshell>   }
jshell> }
```

Create a BatchScanner with 5 query threads
```java
jshell> try (BatchScanner batchScanner = client.createBatchScanner("GothamPD", Authorizations.EMPTY, 5)) {
   ...> 
   ...>   // Create a collection of 2 sample ranges and set it to the batchScanner
   ...>   List<Range>ranges = new ArrayList<Range>();
   ...> 
   ...>   // Create a collection of 2 sample ranges and set it to the batchScanner
   ...>   ranges.add(new Range("id1000", "id1999"));
   ...>   ranges.add(new Range("id9000", "id9999"));
   ...>   batchScanner.setRanges(ranges);
   ...> 
   ...>   // Fetch just the columns we want
   ...>   batchScanner.fetchColumn(new Text("villain"), new Text("yearsOfService"));
   ...> 
   ...>   // Calculate average years of service
   ...>   Long totalYears = 0L;
   ...>   Long entriesRead = 0L;
   ...>   for (Map.Entry<Key, Value> entry : batchScanner) {
   ...>     totalYears += Long.valueOf(entry.getValue().toString());
   ...>     entriesRead++;
   ...>   }
   ...>   System.out.println("The average years of service of " + entriesRead + " villains is " + totalYears / entriesRead);
   ...> }
```

Running the solution above should print output similar to below:

```
The average years of service of 2000 villains is 24
```
