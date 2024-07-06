---
title: Generic - Type Inference
date: 2016-01-24 10:52:35
categories:
 - Programming
tags:
 - generics
 - type Inference
 - java
---
#### Type Inference

`Type inference` is a Java compiler's ability to look at each method invocation and corresponding declaration to determine the type argument (or arguments) that make the invocation applicable. The inference algorithm determines the types of the arguments and, if available, the type that the result is being assigned, or returned. Finally, the inference algorithm tries to find the most specific type that works with all of the arguments.

To illustrate this last point, in the following example, inference determines that the second argument being passed to the `pick` method is of type Serializable:

```java
static <T> T pick(T a1, T a2) { return a2; }
Serializable s = pick("d", new ArrayList<String>());
```

##### Type Inference and Generic Methods

Generic Methods introduced you to type inference, which enables you to invoke a generic method as you would an ordinary method, without specifying a type between angle brackets. Consider the following example, BoxDemo, which requires the Box class:

```java
public class BoxDemo {

  public static <U> void addBox(U u, 
      java.util.List<Box<U>> boxes) {
    Box<U> box = new Box<>();
    box.set(u);
    boxes.add(box);
  }

  public static <U> void outputBoxes(java.util.List<Box<U>> boxes) {
    int counter = 0;
    for (Box<U> box: boxes) {
      U boxContents = box.get();
      System.out.println("Box #" + counter + " contains [" +
             boxContents.toString() + "]");
      counter++;
    }
  }

  public static void main(String[] args) {
    java.util.ArrayList<Box<Integer>> listOfIntegerBoxes =
      new java.util.ArrayList<>();
    BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
    BoxDemo.addBox(Integer.valueOf(30), listOfIntegerBoxes);
    BoxDemo.outputBoxes(listOfIntegerBoxes);
  }
}
```

The following is the output from this example:

Box #0 contains [10]
Box #1 contains [20]
Box #2 contains [30]

<!--more-->

The generic method `addBox` defines one type parameter named `U`. Generally, a Java compiler can infer the `type parameters` of a generic method call. Consequently, in most cases, you do not have to specify them. For example, to invoke the generic method `addBox`, you can specify the type parameter with a `type witness` as follows:

```java
BoxDemo.<Integer>addBox(Integer.valueOf(10), listOfIntegerBoxes);
```

Alternatively, if you omit the `type witness`,a Java compiler automatically infers (from the method's arguments) that the `type parameter` is Integer:

```java
BoxDemo.addBox(Integer.valueOf(20), listOfIntegerBoxes);
```

##### Type Inference and Instantiation of Generic Classes

You can replace the `type arguments` required to invoke the constructor of a generic class with an empty set of type parameters (<>) as long as the compiler can infer the type arguments from the context. This pair of _angle brackets_ is informally called the diamond.

For example, consider the following variable declaration:

```java
Map<String, List<String>> myMap = new HashMap<String, List<String>>();
```

You can substitute the `parameterized type` of the constructor with an empty set of `type parameters (<>)`:

```java
Map<String, List<String>> myMap = new HashMap<>();
```

Note that to take advantage of type inference during generic class instantiation, you must use the diamond `(<>)`. In the following example, the compiler generates an unchecked conversion warning because the HashMap() constructor refers to the HashMap raw type, not the `Map<String, List<String>> `type:

```java
Map<String, List<String>> myMap = new HashMap(); // unchecked conversion warning
```

##### Type Inference and Generic Constructors of Generic and Non-Generic Classes

Note that constructors can be generic (in other words, declare their own formal type parameters) in both generic and non-generic classes. Consider the following example:

```java
class MyClass<X> {
  <T> MyClass(T t) {
    // ...
  }
}
```

Consider the following instantiation of the class MyClass:

```java
new MyClass<Integer>("")
```

This statement creates an instance of the parameterized type `MyClass<Integer>`; the statement explicitly specifies the type Integer for the formal type parameter, `X`, of the generic class `MyClass<X>`. Note that the constructor for this generic class contains a formal type parameter, `T`. The compiler infers the type String for the formal type parameter, `T`, of the constructor of this generic class (because the actual parameter of this constructor is a String object).

Compilers from releases prior to Java SE 7 are able to infer the actual type parameters of generic constructors, similar to generic methods. However, compilers in Java SE 7 and later can infer the actual type parameters of the generic class being instantiated if you use the diamond (<>). Consider the following example:

```java
MyClass<Integer> myObject = new MyClass<>("");
```

In this example, the compiler infers the type Integer for the formal type parameter, `X`, of the generic class MyClass<X>. It infers the type String for the formal type parameter, `T`, of the constructor of this generic class.

_Note: It is important to note that the inference algorithm uses only invocation arguments, target types, and possibly an obvious expected return type to infer types. The inference algorithm does not use results from later in the program._

##### Target Types

The Java compiler takes advantage of `target typing` to infer the type parameters of a generic method invocation. The target type of an expression is the data type that the Java compiler expects depending on where the expression appears. Consider the method `Collections.emptyList`, which is declared as follows:

```java
static <T> List<T> emptyList();
```

Consider the following assignment statement:

```java
List<String> listOne = Collections.emptyList();
```

This statement is expecting an instance of `List<String>`; this data type is the `target type`. Because the method `emptyList` returns a value of type `List<T>`, the compiler infers that the type argument T must be the value String. This works in both Java SE 7 and 8. Alternatively, you could use a `type witness` and specify the value of T as follows:

```java
List<String> listOne = Collections.<String>emptyList();
```

However, this is not necessary in this context. It was necessary in other contexts, though. Consider the following method:

```java
void processStringList(List<String> stringList) {
    // process stringList
}
```

Suppose you want to invoke the method `processStringList` with an empty list. In Java SE 7, the following statement does not compile:

```java
processStringList(Collections.emptyList());
```

The Java SE 7 compiler generates an error message similar to the following:

`List<Object> cannot be converted to List<String>`

The compiler requires a value for the type argument `T` so it starts with the value Object. Consequently, the invocation of `Collections.emptyList` returns a value of type List<Object>, which is incompatible with the method `processStringList`. Thus, in Java SE 7, you must specify the value of the value of the type argument as follows:

```java
processStringList(Collections.<String>emptyList());
```

This is no longer necessary in Java SE 8. The notion of what is a target type has been expanded to include method arguments, such as the argument to the method `processStringList`. In this case, `processStringList` requires an argument of type List<String>. The method `Collections.emptyList` returns a value of `List<T>`, so using the target type of `List<String>`, the compiler infers that the type argument `T` has a value of String. Thus, in Java SE 8, the following statement compiles:

```java
processStringList(Collections.emptyList());
```
