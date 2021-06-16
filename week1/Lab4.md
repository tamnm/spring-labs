# Lab 4 - CRUD Spring Boot

## Step 4.1 - Setup the project structure
Add new files and folder like below structure.

> Note: The `pom.xml` is in the root folder

```text
📁src
  📁main
    📁java
      📁lab
        📁tcb
          📁springcrud
            📁entity
              📄Speaker.java
            📁repository
              📄SpeakerRepository.java
            📁
              📄SpeakerController.java
            📄Application.java
📄pom.xml
```

## Step 4.2 - Maven pom.xml
Type the below content to the `pom.xml` file
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>lab.tcb</groupId>
    <artifactId>springcrud</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.target>11</maven.compiler.target>
        <maven.compiler.source>11</maven.compiler.source>
    </properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>

        <!--Embedded database-->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>
```

## 4.3 Entity
Put the below content to the `Speaker.java` file
```java
//file :src/main/java/lab/tcb/entity/Speaker.java
package lab.tcb.springcrud.entity;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity(name = "speakers")
public class Speaker {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    Long id;
    String name;
    String title;
    String company;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getCompany() {
        return company;
    }

    public void setCompany(String company) {
        this.company = company;
    }
}

```

## 4.4 Repository
Put the below content to the `SpeakerRepository.java` file
```java
//file src/main/java/lab/tcb/springcrud/repository/SpeakerRepository.java
package lab.tcb.springcrud.repository;

import lab.tcb.springcrud.entity.Speaker;
import org.springframework.data.jpa.repository.JpaRepository;

public interface SpeakerRepository extends JpaRepository<Speaker, Long> {
}
```

## 4.5 Controller
Put the below content to the `SpeakerController.java` file
```java
//file src/main/java/lab/tcb/springcrud/controller/SpeakerController.java
package lab.tcb.springcrud.controller;

import lab.tcb.springcrud.entity.Speaker;
import lab.tcb.springcrud.repository.SpeakerRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
@RequestMapping("api/v1/speakers")
public class SpeakerController {

    SpeakerRepository speakerRepository;

    @Autowired
    public SpeakerController(SpeakerRepository speakerRepository) {
        this.speakerRepository = speakerRepository;
    }

    @GetMapping
    public List<Speaker> list(){
        return speakerRepository.findAll();
    }
}
```

## 4.6 Application
Put the below content to the`Application.java` file
```java
package lab.tcb.springcrud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args){
        SpringApplication.run(Application.class);
    }
}
```

## 4.7 Create resource folder
Create a new folder named `resources` in the `src/main`. Create new files `application.properties` and `data.sql` in this `resources` folder.

```text
📁src
  📁main
    📁java
    📁resources                 👈
      📄application.properties  👈
      📄data.sql                👈
📄pom.xml
```

## 4.8 application.properties
This file is for spring boot configuration.
Put below content to the `application.properties`.
```properties
#file: src/main/resources

#We are using H2 Embedded database for testing purpose
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.h2.console.enabled=true
spring.jpa.hibernate.ddl-auto = none
```

## 4.9 data.sql
Spring boot will load this data during initialization.
Put below content to the data.sql
```sql
# file: src/main/resources/data.sql
 
DROP TABLE IF EXISTS speakers;

CREATE TABLE speakers (
  id INT AUTO_INCREMENT  PRIMARY KEY,
  name VARCHAR(250) NOT NULL,
  title VARCHAR(250) NOT NULL,
  company VARCHAR(250) DEFAULT NULL
);

INSERT INTO speakers (name, title, company) VALUES
('Aliko', 'Dev', 'Billionaire Industrialist'),
('Bill', 'Dev', 'Billionaire Tech Entrepreneur'),
('Folrunsho', 'BA', 'Billionaire Oil Magnate');
```
## 4.10 run
Type this to console to run.
```bash
mvn spring-boot:run
```
Then open the browser to the url `api/v1/speakers`.

## 4.11 Add more function to the Controller
Replace the `SpeakerController.java` by below content
```java
//file src/main/java/lab/tcb/springcrud/controller/SpeakerController.java
package lab.tcb.springcrud.controller;

import lab.tcb.springcrud.entity.Speaker;
import lab.tcb.springcrud.repository.SpeakerRepository;
import org.springframework.beans.BeanUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("api/v1/speakers")
public class SpeakerController {

    SpeakerRepository speakerRepository;

    @Autowired
    public SpeakerController(SpeakerRepository speakerRepository) {
        this.speakerRepository = speakerRepository;
    }

    @GetMapping
    public List<Speaker> list(){
        return speakerRepository.findAll();
    }

    @GetMapping(path = "{id}")
    public Speaker get(@PathVariable Long id) {
        return speakerRepository.getById(id);
    }

    @PostMapping()
    public Speaker create(@RequestBody Speaker newSpeaker){
        return speakerRepository.saveAndFlush(newSpeaker);
    }

    @RequestMapping(path = "{id}", method = RequestMethod.DELETE)
    public void delete(@PathVariable Long id){
        speakerRepository.deleteById(id);
    }

    @RequestMapping(path = "{id}", method = RequestMethod.PUT)
    public Speaker update(@PathVariable Long id, @RequestBody Speaker speakerNew){
        Speaker existingSpeaker = speakerRepository.getById(id);

        BeanUtils.copyProperties(speakerNew, existingSpeaker, "id");
        return speakerRepository.saveAndFlush(existingSpeaker);
    }
}
```

## 4.12 Test by Postman
Open the file `springcrud.postman_collection.json`
Update the BasePath variable at the bottom of the file to your path then save.

Load this collection into postman then play.
```json
	"variable": [
		{
			"key": "BasePath",
			"value": "http://localhost:8080"
		}
	]
```