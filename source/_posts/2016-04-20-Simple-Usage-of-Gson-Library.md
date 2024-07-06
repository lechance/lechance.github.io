---
title: Gson 库的使用
date: 2016-04-20 13:56:47
categories:
- Android
tags:
- java
- gson
- json
- serialization
- deserialization
---

`Gson`是一个Java序列化与反序列化的库，它能够从Java对象转换为JSON对象，也能够从一个JSON字符串转换成Java对象。

#### 使用前配置Maven依赖
在使用`Gson`前，需要配置Maven管理依赖
```java
<!-- Gson: Java to Json conversion -->
<dependency>
    <groupId>com.google.code.gson</groupId>
    <artifactId>gson</artifactId>
    <version>2.6.2</version>
    <scope>compile</scope>
</dependency>
```
配置好依赖，我们就可以使用Gson了。

#### 使用Gson的一个简单示例
`Gson`提供了一个主要的类`Gson`,通常在使用`Gson`前`Gson`类首先被构建出来，再调用方法`toJson(Object)`或者`fromJson(String,Class)`, `Gson`实例是线程安全的，因此你可以自由地通过多个线程复用它。
如果默认配置满足你的基本需求，你可以通过调用默认构造函数`new Gson()`来创建一个`Gson`实例。你也可以通过构造`GsonBuilder`对象设置一些配置项(versioning support, pretty printing, 自定义的JsonSerializers,JsonDeserializers,和InstanceCreator)来完成`Gson`的配置后通过方法`create()`创建它。一个简单的示例如下：
```java
Gson gson = new Gson(); // Or use new GsonBuilder.create();
MyType target = new MyType();
String json = gson.toJson(target); // serializes target to json
MyType target2 = gson.fromJson(json, MyType.class); // deserializes json into target2
```

如果我门序列化和反序列化的对象是一个参数化类型(ParameterizedType),当它可能包含至少一个类型参数以及可能是一个数组的时候，我们必须使用方法`toJson(Object,Type)`或者`fromJson(String,Type)`如下：
```java
Type listType = new TypeToken<List<String>>(){}.getType();

List<String> target = new LinkedList<String>();
target.add("blah");

Gson gson = new Gson();
String json = gson.toJson(target, ListType);
List<String> target2 = gson.fromJson(json, ListType);
```

另一个`toJson`方法的重载形式是`toJson(Object src, Appendable writer)`，第二个参数是一个可追加的实例，通过它可以很方便地序列化对象到一个文件或网络流。
```java
Writer writer = new FileWriter("foo.json");

Gson gson = new GsonBuilder.create();
gson.toJson("hello", writer);
gson.toJson(123, writer);

writer.close();
```
上面的代码没有正确处理流writer,实际项目中，应该在finally block 中关闭流或者用try-with-resource的方式处理流。需要注意的是，我们在上面使用了字符流而不是字节流，由于`toJson`方法的第二个参数需要一个`Appendable`实例，因此我们使用字符流。当然Java也提供了字符流与字节流之间的转换类`InputStreamReader` 和`OutputStreamWriter`。在使用这两个类进行序列化和反序列化时如果不提供编码和字符集它会使用系统默认的编码。
```java
import com.google.gson.Gson;
import com.google.gson.GsonBuilder;
import com.google.gson.reflect.TypeToken;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.Writer;
import java.lang.reflect.Type;
import java.util.LinkedList;
import java.util.List;

/**
 * GradleDemo Created by lechance on 3/20/16 4:40 PM.
 */

public class Foo {
    public static void main(String[] args) {

        try {
            Writer writer = new OutputStreamWriter(new FileOutputStream("test.json"), "UTF-8");

            Gson gson = new GsonBuilder().create();

            Type listType = new TypeToken<List<String>>() {
            }.getType();

            List<String> target = new LinkedList<>();
            target.add("test1");
            target.add("test2");
            target.add("test3");

            gson.toJson(target, listType, writer);

            writer.close();
        } catch (IOException e) {
            //omit
        }
    }
}
```

#### 
#### 参考资料
[Gson Api](http://www.javadoc.io/doc/com.google.code.gson/gson/2.6.2)
[github gson](https://github.com/google/gson/)
[Gson Example](http://www.javacreed.com/simple-gson-example/)
