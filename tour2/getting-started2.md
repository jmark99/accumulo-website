---
title: Getting Started
---

To complete the tour you will need a running Accumulo instance. If you do not have access to an 
Accumulo cluster you can use [fluo-uno] to set up a single node instance for use with the Tour.

Once you have an instance up and running, start the Accumulo JShell interface by typing (where '$' 
is your system prompt).

```
$ accumulo jshell
```

This will present you with a Java JShell interface with the required Accumulo libraries pre-loaded 
and a working ```client``` object.

```
Preparing JShell for Apache Accumulo

Use 'client' to interact with Accumulo

|  Welcome to JShell -- Version 11
|  For an introduction type: /help intro

jshell>
```

Ok, let's go!

[fluo-uno]: https://github.com/apache/fluo-uno
