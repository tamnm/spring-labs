# Lap 2 - Adding simple model, repository and sevice

## Step 2.1 - Add new folders
Add below folders to the `src/main/java/com/example/hello` folder
```text
📁src
  📁main
    📁java
      📁com
        📁example
          📁hello
            📁model             👈
            📁repository        👈
            📁service           👈
            📄Application.java 
📄pom.xml
```

## Step 2.2 - Adding model class
Add a file named `Speaker.java` to the `model` folder we have created. 
Put below content to the `Speaker.java`.
```java
//file: src/main/java/com/example/hello/model/Speaker.java
package com.example.hello.model;

public class Speaker {

    private String firstName;
    private String lastName;

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
}
```

## Step 2.3 - Adding Repository interface
Add a file named `SpeakerRepository.java` to the `repository` folder we have created.
Put below content to the `SpeakerRepository.java`
```java
//file: src/main/java/com/example/hello/repository/SpeakerRepository.java
package com.example.hello.repository;

import com.example.hello.model.Speaker;

import java.util.List;

public interface SpeakerRepository {
    List<Speaker> findAll();
}
```

## Step 2.4 - Adding simple implementation for repository
Add a file named `HibernateSpeakerRepositoryImpl.java` to the `repository` folder. Put below content to the `HibernateSpeakerRepositoryImpl.java`
```java
//file: src/main/java/com/example/hello/repository/HibernateSpeakerRepositoryImpl.java
package com.example.hello.repository;

import com.example.hello.model.Speaker;

import java.util.ArrayList;
import java.util.List;

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

## Step 2.5 - Adding service interface
Add a file named `SpeakerService.java` to the `service` folder. Put below content to the `SpeakerService.java`
```java
//file: src/main/java/com/example/hello/service/SpeakerService.java
package com.example.hello.service;

import com.example.hello.model.Speaker;

import java.util.List;

public interface SpeakerService {
    List<Speaker> findAll();
}
```

## Step 2.6 - Adding service simple implementation
Add a file named `SpeakerServiceImpl.java` to the `service` folder. Put below content to the `SpeakerServiceImpl.java`
```java
//file: src/main/java/com/example/hello/service/SpeakerServiceImpl.java
package com.example.hello.service;

import com.example.hello.model.Speaker;
import com.example.hello.repository.HibernateSpeakerRepositoryImpl;
import com.example.hello.repository.SpeakerRepository;

import java.util.List;

public class SpeakerServiceImpl implements SpeakerService {

    private SpeakerRepository repository = new HibernateSpeakerRepositoryImpl();

    public List<Speaker> findAll() {
        return repository.findAll();
    }

}
```

## Step 2.7 - Update Application.java
Update `Application.java` logic to call the service we have created.
Replace the `Application.java` by below content.
```java
//file: src/main/java/com/example/hello/Application.java
package com.example.hello;

import com.example.hello.service.SpeakerService;
import com.example.hello.service.SpeakerServiceImpl;

public class Application {

    public static void main(String args[]) {
        SpeakerService service = new SpeakerServiceImpl();

        System.out.println(service.findAll().get(0).getFirstName());
    }
}
```

## Step 2.8 - Run
Run below command in console
```bash
mvn compile exec:java -Dexec.mainClass="com.example.hello.Application" -q
```
If you do properly, You should see `Bryan` showing in the console
