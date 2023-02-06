---
title: Java Grouping By
date: 2023-02-06 11:00:00 +/-0000
categories: [Java,Streams]
tags: [java,group,grouping,stream,collectors]
---

# Java Grouping By  

## Why is it useful
When we have a collection of objects and we want to group them based on a property then we can use the `groupingBy` method in the `Collectors` class.  
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

## Grouping By
To group the elements by country for example, the following stream operation can be used:

```java
Map<String, List<Person>> groupedByCountry = people.stream()
  .collect(groupingBy(Person::country));
```

That operation collects the results and puts them into a map, with the map keys being the countries. The output should look like this:  

```text
{
  ire=[Person[name=bob, country=ire, age=78], Person[name=tom, country=ire, age=29]],
  sco=[Person[name=kat, country=sco, age=40]],
  eng=[Person[name=ste, country=eng, age=30]]
}
```

## Return as Map of Sets
If we would prefer the data structure returned to be a Set instead of the default List, we can pass in a Collector method for that like so:
```java
final Map<String, Set<Person>> collectSet = people.stream()
  .collect(groupingBy(Person::getCountry, toSet()));
```

## Link to sample
[Link to GitHub repo with sample](https://github.com/eternalgooner/java-samples/blob/main/src/main/java/java8/groupingBy/GroupingBy.java)
