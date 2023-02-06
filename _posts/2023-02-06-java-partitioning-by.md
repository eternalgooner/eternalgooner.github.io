---
title: Java Partitioning By
date: 2023-02-06 18:40:00 +/-0000
categories: [Java,Streams]
tags: [java,partition,partitioning,stream,collectors]
---

# Java Partitioning By  

## Why is it useful
When we have a collection of objects and we want to separate them into 2 distinct categories of objects based on a predicate then we can use the `paritioningBy` method in the `Collectors` class. In instances where we only care when the predicate matches we can use `filter`. But if we care about the negative matches also and possibly need to process them, then `partitioningBy` is perfect.  

Say we have a `Person` record like so:

```java
record Person(String name, String country, int age) {}
```

## Dummy Data
Let's create some dummy data and put it into a list

```java
Person bob = new Person("bob", "ire", 78);
Person tom = new Person("tom", "ire", 29);
Person kat = new Person("kat", "sco", 40);
Person ste = new Person("ste", "eng", 30);

List<Person> people = List.of(bob, tom, kat, ste);
```

## Partitioning By
To partition the elements based on the country value predicate for example, the following stream operation can be used:

```java
Map<String, List<Person>> groupedByCountry = people.stream()
  .collect(partitioningBy(p -> p.country.equals("ire"));
```

That operation collects the results and puts them into a map with 2 keys. A `true` key which has a value set of entries that matched the predicated. A `false` key which has a value set of all entries that did not match the predicate. The output should look like this:  

```text
{
  true=[Person[name=bob, country=ire, age=78], Person[name=tom, country=ire, age=29]],
  false=[Person[name=kat, country=sco, age=40],Person[name=ste, country=eng, age=30]]
}
```

## Link to sample
[Link to GitHub repo with sample](https://github.com/eternalgooner/java-samples/blob/main/src/main/java/java8/partitioningBy/PartitioningBy.java)
