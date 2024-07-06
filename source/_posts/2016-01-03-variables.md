---
title: Variables
date: 2016-01-03 22:22:52
tags:
- java
- basics
categories: Java
---
#### Variables
An object stories its stae in `fields`.

```java
   int cadence = 0;
   int speed = 0;
   int gear = 1;
```
The Java programming language defines te following kinds of variables:

* **Instance Variables(Non-Static Fields)** Technically speaking, objects store their individual states in "non-static fields", that is, fields declared without the `static` keyword. Not-Static fields are also known as _instance_ variables because their values are unique to each instance of a class (to each object, in ther words); the `currentSpeed` of one bicycle is indenpendent from the `currentSpeed` of another.
* **Class Variables(Static Fields)** A class variables in any field declared with the `static` modifier, this tells the compiler that there is exactly one copy of this variable in existence, regardless of how many times the class has been instantiated. A field defining the number of gear for a particular kind of bicycle could be marked as `static` since conceptually the same number of gears will apply to all instances. The code `static` int numGears = 6; would ceate such a static field. Additionally, the keyword `final` could be added to indicate that the number of gears will never change.
* **Local Variables** Similar to how an object stores its state in fields, a method will often stores its temporary state in local variables. The syntax for declaring a local variable	is similar to declaring a field (for example, int count = 0;) There is no special keyword designating a variable as local, that determination come entirely from the location in which the variable is declared - which is between the opening and closing braces of a method. As such, local variables are only visible to the methods in which they are declared, they are not accessiable from the rest of the class.
* **Parameters** You've already seen examples of parameters, both in the `Bicycle` class and in the `main` method of the "Hello World" application. Recall that the signature for the `main` method is `public static void main (String[] args)`. Here, the `args` variable is the parameter to this method. The important thing to remember is that parameters are always classified as "variables" not "fields". This applies to other parameter-accepting constructs as well (such as constructors and exception handlers) that you'll lean about later in the tutorial.

Having said that, the remainder of this tutorial uses the following general guidelines when discussing fields and variables. if we are talking about "fields in general" (excluding local variables and parameters), we may simply say "fields". if the discussion applies to "all of the above", we may simply say "vaiables". if the context calls for a distinction, we will use specific terms(static field, local variables,etc) as appropriate. You may also occasionally see he term "member" used as well. A type's fields, method, and nested types are collectively called its _members_.

#### Naming

Every programming language has its own set of rules and conventions for the kinds of names that you're allowed to use, and the Java programming language is no different. The rules and conventions for naming your variables can be summarized as follows:

- Variable names are case-sensitive.
- Subsequent characters my be letters, digits, dollar signs, or underscore characters.
- If the name you choose consists of only one word, spell that word in all lowercase letters.
