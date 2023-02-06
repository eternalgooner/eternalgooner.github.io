---
title: Java Record
date: 2023-02-06 19:10:00 +/-0000
categories: [Java,Record]
tags: [java,record,immutable,data,dto]
---

# Java Record  

## Why is it useful
Java 14 brought `record`s to the language. These can be thought of as restricted classes which have the following characteristics/benefits:
- data carriers
- concise
- immutable

**Tip**
Following one of the Functional Programming principles, using immutability instead of mutability where possible will help to reduce the chance of bugs and help ensure data validity 

## Define a record
A `record` can be defined in 1 line of code like so:

```java
record Person(String name, String country, int age) {}
```

## What does it give us
This 1 line of code gives us:
 - All args public constructor
 - Getters for all properties (no setters, it's immutable)
 - Nice toString() method
 - equals() for all fields
 - hashCode() for all fields

## Accessing properties
Accessing properties uses the dot notation on the object while dropping the `get` convention from standard Java. So instead of `.getCountry()` it would look like:
```java
String country = person.country();
```

## Compared with standard Java
That 1 line of code required for a record is the equivalent to the below plain Java pre `record`s (43 lines)
```java
class PersonOld {
    private final String name;
    private final String country;
    private final int age;

    public PersonOld(String name, String country, int age) {
        this.name = name;
        this.country = country;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public String getCountry() {
        return country;
    }

    public int getAge() {
        return age;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof PersonOld personOld)) return false;
        return getAge() == personOld.getAge() && getName().equals(personOld.getName()) && getCountry().equals(personOld.getCountry());
    }

    @Override
    public int hashCode() {
        return Objects.hash(getName(), getCountry(), getAge());
    }

    @Override
    public String toString() {
        return "PersonOld{" +
                "name='" + name + '\'' +
                ", country='" + country + '\'' +
                ", age=" + age +
                '}';
    }
}
```

## Link to sample
[Link to GitHub repo with sample](https://github.com/eternalgooner/java-samples/blob/main/src/main/java/java14/record/JavaRecord.java)
