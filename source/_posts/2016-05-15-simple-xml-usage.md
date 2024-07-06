---
title: Simple-xml Library Usage
date: 2016-05-15 23:37:40
categories:
- Java
tags:
- simple-xml
- xml
- java
- serialization
- deserialization
---

> 之前在看thanatos的[FlowGeek][FlowGeek]源码时看到的一个XML处理框架[Simple][Simple]. 这里做一下简单的记录, 和其他类似的框架[SAX][SAX],[XStream][XStream],[Digester][Digester]比较起来, *Simple* 就如同它名字一样显得小巧而精干. *Simple* 框架简单地围绕几个注解提供了XML的序列化, 就如同官网描述的那样它不需要配置, 只使用字段注解来表示序列化的对象. 

一个简单示例: 序列化一个简单的对象`Example`

为了序列化对象到XML, 必须在这个对象内声明一系列注解. 这些注解会告诉持久化器(Persister类)如何序列化一个对象. 
```java

/**
 * @Param name //If this is not specified then the name 
 * of the XML element will be the name of the class.
 */
@Root(name = "configuration")
class Configuration {

    @Element
    private Server server;

    @Attribute
    private int id;

    public Server getServer() {
        return this.server;
    }

    public void setServer(Server server) {
        this.server = server;
    }

    public int getIdentity() {
        return this.id;
    }

    public void setIdentity(int id) {
        this.id = id;
    }
}

@Root(name = "server")
class Server {

    @Element
    private String host;

    @Attribute
    private int port;

    /**
     * @param required // optional element 
     */
    @Element(required = false)
    private Security security;

    @Element(name = "description", required = false)
    private String description;

    public String getDescription() {
        return this.description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getHost() {
        return host;
    }

    public void setHost(String host) {
        this.host = host;
    }

    public Security getSecurity() {
        return security;
    }

    public void setSecurity(Security security) {
        this.security = security;
    }

    public int getPort() {
        return port;
    }

    public void setPort(int port) {
        this.port = port;
    }
}

@Root(name = "security")
class Security {

    @Attribute
    private boolean ssl;

    @Element
    private String keyStore;

    public boolean isSsl() {
        return ssl;
    }

    public void setSsl(boolean ssl) {
        this.ssl = ssl;
    }

    public String getKeyStore() {
        return keyStore;
    }

    public void setKeyStore(String keyStore) {
        this.keyStore = keyStore;
    }
}

```

Note: 值得说明的是, 在对对象属性注解的时候, 可通过传递一个`boolean`值给`@Element`or`@Attribute`内置方法`boolean required()`来指明该属性在序列化时是否是必须提供的.

声明了上面类后, 可以使用Persister来操作了. 

```java
    public static void main(String[] args) throws Exception {

        Persister persister = new Persister();

        File source = new File("./example.xml");

        Configuration configuration = new Configuration();
        //initial security object and then configure it
        Security security = new Security();
        security.setSsl(true);
        security.setKeyStore("LnBWb8yVASW8fBK+6b1Dt4CNSBAyT9RPet9jFvxs6cwqHtbEm/Yi5E2m/1V4+FC3");
        //initial server object and then configure it
        Server server = new Server();
        server.setHost("lechance.github.io");
        server.setPort(80);
        server.setSecurity(security);
        server.setDescription("optional element or attributes");
        //final step that to serialize the configuration object into xml file
        configuration.setServer(server);
        configuration.setIdentity(0xff);

        persister.write(configuration, source);
    }
```

生成的XML文件结构为:
```java
<configuration id="255">
   <server port="80">
      <host>lechance.github.io</host>
      <security ssl="true">
         <keyStore>LnBWb8yVASW8fBK+6b1Dt4CNSBAyT9RPet9jFvxs6cwqHtbEm/Yi5E2m/1V4+FC3</keyStore>
      </security>
      <description>optional element or attributes</description>
   </server>
</configuration>
```

当然除了序列化对象外, 你还能够从XML文件反序列化对象. 

为了演示反序列化操作, 假如有一下XML文件. 
```java
<?xml version="1.0" encoding="UTF-8"?>
<module external.linked.project.id="LeJokes" 
        external.linked.project.path="$MODULE_DIR$" 
        external.root.project.path="$MODULE_DIR$" 
        external.system.id="GRADLE" 
        external.system.module.group="" 
        external.system.module.version="unspecified" 
        type="JAVA_MODULE" 
        version="4">
    <component name="FacetManager">
        <facet type="java-gradle" name="Java-Gradle">d
            <configuration>
                <option name="BUILD_FOLDER_PATH" value="$MODULE_DIR$/build" />
                <option name="BUILDABLE" value="false" />
            </configuration>
        </facet>
    </component>
    <component name="NewModuleRootManager" LANGUAGE_LEVEL="JDK_1_8" inherit-compiler-output="true">
        <exclude-output />
        <content url="file://$MODULE_DIR$">
            <excludeFolder url="file://$MODULE_DIR$/.gradle" />
        </content>
        <orderEntry type="jdk" jdkName="1.8" jdkType="JavaSDK" />
        <orderEntry type="sourceFolder" forTests="false" />
    </component>
</module>
```

接下来我们要做的工作是通过`Simple`来对上面XML进行反序列化实现. 

后面我会贴上解析的代码, 期待 :).

For more infomation about *Simple-xml* framework, please refer to [here][Simple].

[XStream]: http://x-stream.github.io/ "XStream"
[SAX]: http://sax.sourceforge.net/ "sax"
[Digester]: https://commons.apache.org/proper/commons-digester/ "digester"
[FlowGeek]: http://www.oschina.net/p/FlowGeek-Android "FlowGeek"
[Simple]:http://simple.sourceforge.net/home.php "Simple-xml Library"
