---
title: Java Completable Future With Interdependent Processes
date: 2023-02-12 18:00:00 +/-0000
categories: [Java,CompletableFuture]
tags: [java,future,completablefuture,async,concurrency,multithread,race]
---

## Why is it useful
Java 8 brought `CompletableFuture`s to the language. This was an improvement on `Future`s introduced in Java 5. The main reasons to use it are:
- async processing
- keep main thread responsive
- better utilise CPUs or CPU Cores

> We can execute `CompletableFuture`s by passing an optional `ExecutorService`. If we don't pass our own then the default `ForkJoinPool` will be used which has its pros & cons. 
{: .prompt-info }

## Sample Scenario
Say we have 3 long-running processes that can be run in isolation. They can each take different amounts of time to finish. If we want to be notified when any one of them finishes then we can wrap all 3 of the processes in a `CompletableFuture`. When the first process completes we will be notified and can take the required action.  

Given that these 3 processes take so much time and can be run in isolation, we can wrap each of them in a `CompletableFuture` which will give the following benefits:
- take the strain off the main thread
- keep the main thread responsive for other processing
- utilise the available computer resources
- uses a nice functional style which reads well and allows chaining of processes

The long-running processes might look something like these (purely for demonstration purposes):
```java
public CompletableFuture<String> longRunningProcess1() {
  return CompletableFuture.supplyAsync(() -> {
  try {
    Thread.sleep(10_000);
  } catch (InterruptedException e) {
    throw new RuntimeException(e);
  }
  return "dbString";
  });
}

public CompletableFuture<Integer> longRunningProcess2() {
  return CompletableFuture.supplyAsync(() -> {
  try {
    Thread.sleep(5_000);
  } catch (InterruptedException e) {
    throw new RuntimeException(e);
  }
  return 10;
  });
}

public  CompletableFuture<Void> longRunningProcess3() {
  return CompletableFuture.runAsync(() -> {
  try {
    Thread.sleep(7_000);
  } catch (InterruptedException e) {
    throw new RuntimeException(e);
  }
  });
}
```

## Wrap processes in CompletableFuture
We can wrap these isolated processes in a `CompletableFuture` and be notified when the first one completes like so:

```java
CompletableFuture.anyOf(
      longRunningProcess1(),
      longRunningProcess2(),
      longRunningProcess3())
      .thenAccept(compFuture -> System.out.println("1st successful completable future completed " + compFuture.toString()));
```

## Link to sample
A link to a working sample is available at the following link:  
[Link to GitHub repo with sample](https://github.com/eternalgooner/java-samples/blob/main/src/main/java/java8/completablefuture/CompletableFutureOnlyNeedOneToFinish.java)
