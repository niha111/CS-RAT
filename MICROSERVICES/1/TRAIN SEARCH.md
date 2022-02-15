here trian search is taken as a first  microservice as it is is a easy
and it is connected to mongodb .
to connect it to mongodb we have to add some dependencies in the pom.xml
and the we to add the connection link to the application.properties

**code**
train.java

```java

package Rtb;

import org.springframework.data.annotation.Id;


import org.springframework.data.mongodb.core.mapping.Document;


@Document(collection = "Train")
public class Train {
	
	@Id
	private String trainid;
	private String trainName;
	private String startStation;
	private String endStation;
	public String getTrainid() {
		return trainid;
	}
	public void setTrainid(String trainid) {
		this.trainid = trainid;
	}
	public String getTrainName() {
		return trainName;
	}
	public void setTrainName(String trainName) {
		this.trainName = trainName;
	}
	public String getStartStation() {
		return startStation;
	}
	public void setStartStation(String startStation) {
		this.startStation = startStation;
	}
	public String getEndStation() {
		return endStation;
	}
	public void setEndStation(String endStation) {
		this.endStation = endStation;
	}
	@Override
	public String toString() {
		return "Train [trainid=" + trainid + ", trainName=" + trainName + ", startStation=" + startStation
				+ ", endStation=" + endStation + "]";
	}
}
```

TrainApplication.java
```java
package Rtb;

import java.util.List;

import java.util.Optional;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

 

@RestController
public class TrainController {

	@Autowired
	private TrainRepository repository;

	@PostMapping("/addTrain")
	public String saveBook(@RequestBody Train train) {
		repository.save(train);
		return "Added book with id : " + train.getTrainid();
	}

	@GetMapping("/findAllTrains")
	public List<Train> getBooks() {
		return repository.findAll();
	}

	@GetMapping("/findAllTrains/{trainid}")
	public Optional<Train> getBook(@PathVariable String trainid) {
		return repository.findById(trainid);
	}

	@DeleteMapping("/delete/{trainid}")
	public String deleteTrain (@PathVariable String trainid) {
		repository.deleteById(trainid);
		return "Train deleted with id : "+trainid;
	/*@DeleteMapping("/delete/{trainid}")
	public String deleteBook(@PathVariable int trainid) {
		repository.deleteById(trainid);
		return "book deleted with id : " + trainid;
	}*/
	}
	
	@PutMapping("/update/{trainid}")
	public Train updateTrain(@PathVariable("trainid") String trainid,@RequestBody Train t ) {
		t.setTrainid(trainid);
		repository.save(t);
		return t;
}
}
```


TrainRepository.java
```java
package Rtb;
import org.springframework.data.mongodb.repository.MongoRepository;

public interface TrainRepository extends MongoRepository<Train, String>{

}
```
trainsearchmicroserviceapplication.java

```java
package Rtb;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class TrainSearchMicroserviceApplication {

	public static void main(String[] args) {
		SpringApplication.run(TrainSearchMicroserviceApplication.class, args);
	}

}

```
Application.properties

```
server.port=8088
spring.application.name=TrainSearchMicroService
spring.data.mongodb.uri=mongodb+srv://case:study@cluster0.pbzos.mongodb.net/STATION?retryWrites=true&w=majority
#spring.data.mongodb.host=localhost

#spring.data.mongodb.port=27017
```
microserviceapplicationtest
```
package com.caseStudy.RRS;

import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class TrainSearchMicroserviceApplicationTests {

	@Test
	void contextLoads() {
	}

}
```
pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.3</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.caseStudy.RRS</groupId>
	<artifactId>TrainSearchMicroservice</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>TrainSearchMicroservice</name>
	<description>For Case Study</description>
	<properties>
		<java.version>11</java.version>
	<spring-cloud.version>2020.0.3</spring-cloud.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-mongodb</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
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
