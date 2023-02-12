---
title: Java Completable Future Handle Success Or Failure
date: 2023-02-12 19:00:00 +/-0000
categories: [Java,CompletableFuture]
tags: [java,future,completablefuture,async,concurrency,multithread,handle,exception]
---

## Why is it useful
Java 8 brought `CompletableFuture`s to the language. This was an improvement on `Future`s introduced in Java 5. The main reasons to use it are:
- async processing
- keep main thread responsive
- better utilise CPUs or CPU Cores

> We can execute `CompletableFuture`s by passing an optional `ExecutorService`. If we don't pass our own then the default `ForkJoinPool` will be used which has its pros & cons. 
{: .prompt-info }

## Sample Scenario
Say we have a `CompletableFuture` and we want to be able to handle the success and failure scenarios. The `CompletableFuture` API allows easy handling of this.

Imagine we have a `CompletableFuture` that looks something like this:
```java
public CompletableFuture<String> longRunningProcess() {
  return CompletableFuture.supplyAsync(() -> {
    try {
      // processing logic
      throw new RuntimeException("exception in completable future");
    } catch (InterruptedException e) {
      throw new RuntimeException(e);
    }
  });
}
```

## Handle Success And Failure Outcomes
We can handle the success and failure outcomes like so:

```java
longRunningProcess()
        .handle((success, failure) -> {
    if (success != null) {
        // success logic with completable future output
    }
    // failure logic with failure details
    return null;
});
```

## Link to sample
A link to a working sample is available at the following link:  
[Link to GitHub repo with sample](https://github.com/eternalgooner/java-samples/blob/main/src/main/java/java8/completablefuture/CompletableFutureHandleException.java)
