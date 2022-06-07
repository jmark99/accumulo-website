---
title: Using Iterators
---

Suppose at the end of each day we record the number of villains a hero has caught that day. Iterators
can assist with this. Iterators are server-side programming mechanisms that allow filtering and
aggregation operations on data during scans and at other times.

Let's begin by adding some data for our hero's recording recent crime-stopping statistics.

```commandlne
// last three days of Batman's statistics
jshell> Mutation mutation1 = new Mutation("id00001");
mutation1 ==> org.apache.accumulo.core.data.Mutation@1

jshell> mutation1.put("hero", "villainsCaptured", 2);
jshell> mutation1.put("hero", "villainsCaptured", 1);
jshell> mutation1.put("hero", "villainsCaptured", 5);

// last three days of Robin's statistics
jshell> Mutation mutation2 = new Mutation("id0002");
mutation2 ==> org.apache.accumulo.core.data.Mutation@1

jshell> mutation2.put("hero", "villainsCaptured", 1);
jshell> mutation2.put("hero", "villainsCaptured", 0);
jshell> mutation2.put("hero", "villainsCaptured", 2);
```

Let's scan to see the data. 

```commandline
jshell> try (ScannerBase scan = client.createScanner("GothamPD", Authorizations.EMPTY)) {
   ...>   System.out.println("Gotham Police Department Persons of Interest:");
   ...>   for(Map.Entry<Key, Value> entry : scan) {
   ...>     System.out.printf("Key : %-50s  Value : %s\n", entry.getKey(), entry.getValue());
   ...>   }
   ...> }
```   

You may notice a problem. We only see the latest entry. That is due to the
`versioningIterator` which is applied to all tables by default. It filters out all but the latest entry
when scanning a table. 

In order to see a record of past days we need remove the `versioningiterator` (you could also choose
a set number of past entries to display). The iterator is named `vers`.

To simplify, we will import some additional packages to save some typing.

```commandline
jshell> import org.apache.accumulo.core.iterators.IteratorUtil.IteratorScope;
jshell> client.tableOperations().removeIterator("GothamPD", "vers", EnumSet.allOf(IteratorUtil.IteratorScope.class));
```

Now let's scan again.

```commandline
jshell> try (ScannerBase scan = client.createScanner("GothamPD", Authorizations.EMPTY)) {
   ...>   System.out.println("Gotham Police Department Persons of Interest:");
   ...>   for(Map.Entry<Key, Value> entry : scan) {
   ...>     System.out.printf("Key : %-50s  Value : %s\n", entry.getKey(), entry.getValue());
   ...>   }
   ...> }
```
 
[//]: # (To restore the default behavior, let's add back the `versioningiterator`.)

[//]: # (This is accomplished with an IteratorSetting object.)

[//]: # ()
[//]: # (```commandline)

[//]: # (jshell> IteratorSetting verSetting = new IteratorSetting&#40;20, "vers", org.apache.accumulo.core.iterators.user.VersioningIterator, Map.of&#40;"maxVersions", "1"&#41;&#41;;)

[//]: # (jshell> client.tableOperations&#40;&#41;.attachIterator&#40;"GothamPD", verSetting&#41;;)

[//]: # (```)

So instead of seeing a daily history, let's install keep a running total of captured villains. 
A `summingcombiner` can be used to accomplish this.

IteratorSetting scSetting = new IteratorSetting(30, "summer", SummingCombiner.class)); 
IteratorSetting scSetting = new IteratorSetting(15, "sum1", SummingCombiner.class, Map.of("columns", "hero:villainsCaptured"));
client.tableOperations().attachIterator("GothamPD", scSetting);q
client.removeIterator("GothamPD", "vers", EnumSet.allOf(IteratorScope.class));
scSetting.addOption("columns", "hero:villainsCaught,hero:villainsCaptured");

import org.apache.accumulo.core.iterators.LongCombiner
scSetting.addOption("columns", "hero:villainsCaptured");
 LongCombiner.setEncodingType(scSetting, LongCombiner.Type.STRING);
try ( org.apache.accumulo.core.client.Scanner scan = client.createScanner("GothamPD", Authorizations.EMPTY)) {
  scan.setRange(range);
  scan.addScanIterator(scSetting);
  scan.forEach(System.out::println);
}

```commandline
jshell> import org.apache.accumulo.core.iterators.user.SummingCombiner;
jshell> IteratorSetting scSetting = new IteratorSetting(15, "sum", SummingCombiner.class);
jshell> SummingCombiner.setComineAllColumns(scSetting, SummingCombiner.Type.STRING);
jshell> client.tableOperations().checkIteratorConflicts("GothamPD", scSetting, EnumSet.of(IteratorScope.scan));

```


Range range = new Range("id0001");
try ( org.apache.accumulo.core.client.Scanner scan = client.createScanner("GothamPD", Authorizations.EMPTY)) {
         Range range = new Range("id0001", "id0002");
         scan.setRange(range);
         for(Map.Entry<Key, Value> entry : scan) {
           System.out.printf("Key : %-50s  Value : %s\n", entry.getKey(), entry.getValue());
         }
       }

client.tableOperations().removeIterator("GothamPD", "vers", EnumSet.allOf(org.apache.accumulo.core.iterators.IteratorUtil.IteratorScope.class));

try ( org.apache.accumulo.core.client.Scanner scan = client.createScanner("GothamPD", Authorizations.EMPTY)) {
         scan.setRange(range);
         scan.addScanIterator(setting);
         scan.forEach(System.out::println);
}

Reset versioning iterator

--------

insert id0001 hero villainsCaptured 1
insert id0001 hero villainsCaptured 4
insert id0001 hero villainsCaptured 3



setiter -t GothamPD -p 15 -scan -n summer -class org.apache.accumulo.core.iterators.user.SummingCombiner
setiter -scan -t GothamPD -p 15 -n summer -class org.apache.accumulo.core.iterators.user.SummingCombiner
