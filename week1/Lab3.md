# Lab 3 - Dependency injection

## Step 3.1 - Add dependency
We need to add spring-text package to the app for Dependency injection.

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
    
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.0.RELEASE</version>
        </dependency>
    </dependencies> 
</project>
```

## Step 3.2 - @Service annotation
Now we update the content of the `SpeakerServiceImpl.java`.

```java
//file: src/main/java/com/example/hello/service/SpeakerServiceImpl.java
package com.example.hello.service;

import com.example.hello.model.Speaker;
import com.example.hello.repository.SpeakerRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

//We add @Service annotation
@Service("speakerService")
public class SpeakerServiceImpl implements SpeakerService {

    private SpeakerRepository repository;
    
    //We create a constructor and autowired the argument
    @Autowired
    public SpeakerServiceImpl(SpeakerRepository repository) {
        this.repository = repository;
    }
    public List<Speaker> findAll() {
        return repository.findAll();
    }
}
```

## Step 3.3 - @Repository annotation
Now we mark the `` as a Repository by adding the annotation.
```java
//file: src/main/java/com/example/hello/repository/HibernateSpeakerRepositoryImpl.java
package com.example.hello.repository;

import com.example.hello.model.Speaker;
import org.springframework.stereotype.Repository;

import java.util.ArrayList;
import java.util.List;

//We mark this as a Repository by adding @Repository annotation
@Repository
public class HibernateSpeakerRepositoryImpl implements SpeakerRepository {

    public List<Speaker> findAll() {
        List<Speaker> speakers = new ArrayList<Speaker>();

        Speaker speaker = new Speaker();

        speaker.setFirstName("Bryan");
        speaker.setLastName("Hansen");

        speakers.add(speaker);

        return speakers;
    }
}

```

## Step 3.4 - Adding AppConfig.java
Add a file named `AppConfig.java` into `src/main/java/com/example/hello` folder with below content
```java
//file: src/main/java/com/example/hello/AppConfig.java
package com.example.hello;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan({"com.example.hello"})
public class AppConfig {

}
```

## Step 3.5 - Update application to use ApplicationContext
Replace the `Application.java` by below content

```java
//file: src/main/java/com/example/hello/Application.java
package com.example.hello;

import com.example.hello.service.SpeakerService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Application {
    public static void main(String[] args) {
        ApplicationContext appContext = new AnnotationConfigApplicationContext(AppConfig.class);

        SpeakerService service = appContext.getBean("speakerService", SpeakerService.class);

        System.out.println(service.findAll().get(0).getFirstName());
    }
}
```

## Step 3.6 - Now try to run 😀
Run below command in console
```bash
mvn compile exec:java -Dexec.mainClass="com.example.hello.Application" -q
```
If you do properly, You should see `Bryan` showing in the console
