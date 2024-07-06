---
title: Notes - Generics in java
date: 2016-02-23 22:38:02
tags:
- generic
- java
- tutorial
---
> This is a notes for study *Generics* in Java tutorial.

### key points

```java

public static void fromArray2Collection(Object[] a, Collection<?> c){
    for(Object o : a){
	c.add(0); //compile-time error
    }
}

//The way to do deal with these problems is to use _generic methods_. Just like type declaration, method declarations can be generic - that is, parameterized by one or more type parameters.

public static <T> /*type variable*/ void fromArray2Collection(T[] a, Collection<T> c){
    for (T o : a){
	c.add(o); //correct
    }
}

We can call this method with any kind of collection whose element type is a supertype of the element type of the array.

//One question that arises is: when should i use generic methods, and when should i use wildcard types? let's see a few methods from the _Collection_ libraries.

public interface Collection<E>{
    ...
    public boolean containsAll(Collection<?> c);
    public boolean addAll(Collection<? extends E> c);
}
//without wildcard

public interface Collection<E> {
    ...
    public <T> boolean containsAll(Collection<T> c);
    public <T extends E> boolean addAll(Collection<T> c);
    //type variables can hava bounds too!
}

//
class Collections {
    public static <T> void copy(List<T> dest, List<? extends T> src){
	...
    }
}

class Collections {
    public static <T, S extends T> void copy(List<T> dest, List<S> src){
	...
    }
}
```
> Refer to Java Tutorial
