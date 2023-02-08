---
title: Java Completable Future With Interdependent Processes
date: 2023-02-08 22:00:00 +/-0000
categories: [Java,CompletableFuture]
tags: [java,future,completablefuture,async,concurrency,multithread]
---

## Why is it useful
Java 8 brought `CompletableFuture`s to the language. This was an improvement on `Future`s introduced in Java 5. The main reasons to use it are:
- async processing
- keep main thread responsive
- better utilise CPUs or CPU Cores

> We can execute `CompletableFuture`s by passing an optional `ExecutorService`. If we don't pass our own then the default `ForkJoinPool` will be used which has its pros & cons. 
{: .prompt-info }

## Sample Scenario
Say we have 3 interdependent processes that have to be run sequentially. The 1st process is a DB call which takes 10 seconds. The 2nd process is a REST call (using the data retrieved from the DB call) which takes 5 seconds. The 3rd process is another DB call which (using the data retrieved from the REST call) takes 7 seconds.  
Given that these 3 processes take so much time and are interdependent, we can wrap them in a `CompletableFuture` which will give the following benefits:
- take the strain off the main thread
- keep the main thread responsive for other processing
- utilise the available computer resources
- uses a nice functional style which reads well and allows chaining of processes

The long-running processes might look something like these (purely for demonstration purposes):
```java
private String getDataFromDbTakes10Seconds() {
    // simulate DB process
    Thread.sleep(10_000);
  } catch (InterruptedException e) {
    throw new RuntimeException(e);
  }
  return "dbData";
}

private int getDataFromRestCallTakes5Seconds(String dbData) {
  try {
    // simulate REST call  
    Thread.sleep(5_000);
  } catch (InterruptedException e) {
    throw new RuntimeException(e);
  }
  return 10;
}

private void updateDbWithRestDataTakes7Seconds(int numberToInsertToDb) {
  try {
    // simulate DB process  
    Thread.sleep(7_000);
  } catch (InterruptedException e) {
    throw new RuntimeException(e);
  }
}
```

## Wrap processes in CompletableFuture
We can wrap these interdependent processes in a `CompletableFuture` like so:

```java
CompletableFuture.supplyAsync(() -> getDataFromDbTakes10Seconds())
      .thenApply(() -> getDataFromRestCallTakes5Seconds())
      .thenAccept(() -> updateDbWithRestDataTakes7Seconds());
```

## Link to sample
A link to a working sample is available at the following link:  
[Link to GitHub repo with sample](https://github.com/eternalgooner/java-samples/blob/main/src/main/java/java8/completablefuture/CompletableFutureInSequenceApp.java)
