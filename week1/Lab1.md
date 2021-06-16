# Lab 1 - Init a maven project

## Step 1.1 - Setup the project structure
Add new files and folder like below structure.

> Note: The `pom.xml` is in the root folder

```text
📁src
  📁main
    📁java
      📁com
        📁example
          📁hello
            📄Application.java 👈
📄pom.xml
```

## Step 1.2 - Maven pom.xml
Type the below content to the `pom.xml` file
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>hello</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.target>11</maven.compiler.target>
        <maven.compiler.source>11</maven.compiler.source>
    </properties>

</project>
```

## Step 1.3 - Application.java
Type the below content into the `Application.java`
```java
//file: src/main/java/com/example/hello/Application.java

package com.example.hello;

public class Application {
    public static void main(String[] args) {
        System.out.println("hello world");
    }
}
```
## Step 1.4 - Now Run
Now we will use maven to run the application. Run this in `console`:
>Note: press [shift] + [insert] to insert clipboard to the console

```bash
mvn compile exec:java -Dexec.mainClass="com.example.hello.Application" -q
```
Then you should see: `hello world` showing in the console
