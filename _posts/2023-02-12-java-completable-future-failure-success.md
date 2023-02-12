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
Say we have a `CompletableFuture` and we want to be able to handle the success and failure scenarios. The `CompletableFuture` API allows easy handling of this through 3 different approaches.

Imagine we have a `CompletableFuture` that looks something like this:
```java
public CompletableFuture<String> longRunningProcess() {
  return CompletableFuture.supplyAsync(() -> {
    // processing logic
    throw new RuntimeException("exception in completable future");
  });
}
```

## Handle Success And Failure Outcomes

### Approach 1 - Handle
We can call the `handle` method to gain access to the success and failure outcomes like so. Notice how we can recover and return a `String` result in the failure scenario:

```java
longRunningProcess()
  .handle((success, failure) -> {
    if (success != null) {
        System.out.println("we've received a str result so completable future succeeded");
    }
    System.out.println("we've received an exception so completable future failed " + failure.getMessage());
    return "recovered from failure";
  });
```

### Approach 2 - When Complete
We can call the `whenComplete` method to gain access to the success and failure outcomes like so. This approach is used when we don't need to transform the success or failure outputs:

```java
longRunningProcess()
  .whenComplete((success, failure) -> {
    if (success != null) {
        System.out.println("we've received a str result so completable future succeeded");
    }
    System.out.println("we've received an exception so completable future failed " + failure.getMessage());
  });
```

### Approach 3 - Exceptionally
We can call the `exceptionally` method when we only care about accessing the failure output:

```java
longRunningProcess()
  .exceptionally((failure) -> {
    System.out.println("we've received an exception so completable future failed " + failure.getMessage());
    return null;
  });
```

## Link to sample
A link to a working sample is available at the following link:  
[Link to GitHub repo with sample](https://github.com/eternalgooner/java-samples/blob/main/src/main/java/java8/completablefuture/CompletableFutureHandleException.java)
