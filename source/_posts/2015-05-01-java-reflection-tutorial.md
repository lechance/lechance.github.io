---
title: Java Reflection Tutorial
date: 2015-05-01 00:24:45
categories:
- Java
tags:
- java
- reflection
- tutorial
---

#### Overview
*Reflection* is commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine. This is a relatively advanced feature and should be used only by developers who have a strong grasp of the fundamentals of the language. With that caveat in mind, reflection is a powerful technique and can enable application to perform operations which would otherwise be impossible.

#### Drawbacks of Reflection
*Reflection* is powerful, but should not be used indiscriminatey. If it is possible to perform an operation without using reflection, then it is preferable to avoid using it. The following concerns should be kept in mind when accessing code via reflection.
- Performance Overhead
    Because reflection involves types that are dynamically resolved, certain Java virtual machine optimizations can not be performed. Consequently, reflective operations have slower performance than their non-reflective counterparts, and should be avoided in sections of code which are called frequently in performance-sensitive applications.
- Security Restrictions
    Reflection requires a runtime permission which may not be present when running under a security manager. This is an important consideration for code which has to run in a restricted security context, such as in an Applet.
- Exposure of Internals
    Since reflection allows code to perform operations that would be illegal in non-reflective code, such as accessing private fields and methods, the use of reflection can result in unexpected side-effects, which may render code dysfunctional and may destroy portability. Reflective code breaks abstractions and therefore may change behavior with upgrades of the platform.

#### Simple Sample
```java

import java.lang.annotation.Inherited;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.reflect.Constructor;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

interface Pluginable {
}

interface Reflection {
}

@Inherited
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation {
    String value() default "defaultValue";

    boolean isDebugMode() default false;
}

/**
 * TestDemo Created by lechance .
 */

@MyAnnotation(value = "Non-defaultValue", isDebugMode = true)
public class Test implements Reflection {

    private final static Object mSync = new Object();
    private static Test Instance = new Test();

    static {
        System.err.println(Test.class.hashCode());
    }

    public String strPubAttribute;

    protected String strProAttribute;

    private String strPriAttribute;

    //the fields from constructor
    private String description;

    private int id;

    private String content;

    public Test() {
        //default constructor
    }

    public Test(String description) {
        this.description = description;
    }

    public Test(int id, String description, String content) {
        this(description);
        this.id = id;
        this.content = content;
    }

    public static void main(String[] args) {
        try {
            Test.class.newInstance().foo();
        } catch (InstantiationException | IllegalAccessException e) {
            e.printStackTrace();
        }
    }

    public static void testMethod(String input) {
        System.err.println("this string generated form Reflection mechanism. Argument is: " + input);
    }

    public static Test getInstance() {
        synchronized (mSync) {
            if (Instance == null) {
                Instance = new Test();
            }
            return Instance;
        }
    }

    private void foo() {
        class le {
        }  //local class

        print("The name of the class: " + this.getClass().getName());
        print("isAssignableFrom<Test.class>: " + this.getClass().isAssignableFrom(Test.class));
        print("isLocalClass: " + le.class.isLocalClass());
        print("isInterface: " + Pluginable.class.isInterface());
        print("CanonicalName: " + this.getClass().getCanonicalName());
        print("getTypeName: " + this.getClass().getTypeName());
        print(this.getClass().asSubclass(Reflection.class).getSimpleName());
        print(this.getClass().getDeclaredAnnotations()[0]);
        try {
            print(this.getClass().getDeclaredField("strPubAttribute").getType());
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }
        try {
            print(this.getClass().getMethod("testMethod", java.lang.String.class));
            this.getClass().getMethod("testMethod", java.lang.String.class).invoke(Test.class.newInstance(), "le");
        } catch (NoSuchMethodException | IllegalAccessException | InstantiationException | InvocationTargetException e) {
            e.printStackTrace();
        }

        print("---------------getMethods()----Public--Method---------");
        Method[] methods = this.getClass().getMethods();
        for (Method method : methods) {
            print(method.getName());
        }

        print("---------------getMethods()----declared--Methods---------");
        Method[] methods1 = this.getClass().getDeclaredMethods();
        for (Method m : methods1) {
            print(m.getName());
        }

        Constructor[] constructors = this.getClass().getConstructors();
        for (Constructor c : constructors) {
           print("Constructor Name: " + c.getName() + " Modifiers : " + c.getModifiers() + "ParameterCount: " + c.getParameterCount());
        }

        Method ms = this.getClass().getEnclosingMethod();
        print("getEnclosingMethod: " + ms);
    }


    private <T extends Object> void print(T t) {
        System.out.println(t);
    }
}

class AnotherClass {
}

```
_Refer to [Java Tutorial](https://docs.oracle.com/javase/tutorial/reflect/ "java tutorial")_
