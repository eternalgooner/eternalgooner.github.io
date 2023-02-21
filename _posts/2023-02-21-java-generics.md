---
title: Java Generics
date: 2023-02-21 21:30:00 +/-0000
categories: [Java,Generics]
tags: [java,generic,reuse,type]
---

## Why is it useful
Java 5 brought `Generics`s to the language. The main reasons for the new feature were:
- code reuse
  - a generic class or method when designed correctly can be reused with any type thus enabling code reuse 
- compile-time checking / catching issues early
  - declaring the type to be used as the generic type parameter means that code issues for that type can be caught during compilation. This will reduce issues getting to production.
- reduce the need for casting
  - because the generic type parameter is declared when used, this means that the compiler know which type it is and doesn't require any casting

Generics can be thought of in the same vein as standard method parameters whereby generics enable types to be parameters when defining classes, interfaces and methods.

## Sample Scenario
Say we have a Shop class that sells laptops. It might look something like this:
```java
public class LaptopShop {

    private Laptop item;

    public Shop(Laptop item) {
        this.item = item;
    }

    public void sellItem() {
        System.out.println("The shop is selling item: " + item);
        removeItemFromStock(item);
    }

    private void removeItemFromStock(Laptop item){
        System.out.println(item + " has been removed from stock");
    }
}
```

Creating a `LaptopShop` would be done like so:
```java
LaptopShop laptopShop = new LaptopShop(new Laptop(3, BigDecimal.valueOf(120)));
```
If we wanted to sell any other type of item then we would need to create another class for that type. One way to solve this problem is to use generics.

We could have a generic type Shop which takes any type as a parameter. It looks like so:
```java
public class GenericsShop<T> {

    private T item;

    public GenericsShop(T item) {
        this.item = item;
    }

    public void sellItem() {
        // generic code to sell items
        System.out.println("The shop is selling item: " + item);
        removeItemFromStock(item);
    }

    private <T> void removeItemFromStock(T item){
        System.out.println(item + " has been removed from stock");
    }
}
```

We now allow any type to be used as the type parameter for the shop. This allows use to reuse this class as we see fit. When creating the generic shop we would write it like this:
```java
// create generic Shops, which takes any type
GenericsShop<House> houseShop = new GenericsShop<>(new House("bungalow", 20));
GenericsShop<Bike> bikeShop = new GenericsShop<>(new Bike("bmx", 2));
```

In the `GenericShop` sample there is an example of a generic method which also takes the type `T` used to instantiate the class.

We also have the option of applying bounds to the type parameter if that's desired. We can apply:
- upper bounded types
  - written like `class GenericShop <T extends Device> {}`
  - this ensures that any type used with this class must be a subtype of Device
  - this still provides flexibility while enforcing some wanted restriction
- lower bounded types
  - written like `class GenericShop <T super Device> {}`
  - this ensures that any type used with this class must be a super type of Device
  - this still provides flexibility while enforcing some wanted restriction

## Link to sample
A link to a working sample is available at the following link:  
[Link to GitHub repo with sample](https://github.com/eternalgooner/java-samples/blob/main/src/main/java/java5/generics/GenericsApp.java)
