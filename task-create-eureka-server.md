Implementing a _Eureka Server_ for service registry is as easy as:

1. adding _[spring-cloud-starter-netflix-eureka-server](https://mvnrepository.com/search?q=spring-cloud-starter-netflix-eureka-server)_ to the dependencies
2. enabling the Eureka Server in a [_@SpringBootApplication_](https://www.baeldung.com/spring-boot-application-configuration) by annotating it with _@EnableEurekaServer_
3. configuring some properties

Let’s do it step by step.

First, we’ll create a new Maven project and put the dependencies in it. Notice that we’re importing the _[spring-cloud-starter-parent](https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-parent)_ to all the projects described in this tutorial:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-parent</artifactId>
            <version>2023.0.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

We can check the latest Spring Cloud releases in [the Spring’s Projects documentation](https://spring.io/projects/spring-cloud#learn).

Then we’ll create the main application class:

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

Finally, we’ll configure the properties in _YAML_ format, so an _application.yml_ will be our configuration file:

```plaintext
server:
  port: 8761
eureka:
  client:
    registerWithEureka: false
    fetchRegistry: false
```

**Here we’re configuring an application port; the default one for _Eureka_ servers is _8761_.** We’re telling the built-in _Eureka Client_ not to register with itself because our application should be acting as a server.

Now we’ll point our browser to [http://localhost:8761](http://localhost:8761/) to view the _Eureka_ dashboard, where we’ll later inspect the registered instances.

At the moment, we can see basic indicators, such as status and health indicators:

[![Screenshot_20160819_073151](https://www.baeldung.com/wp-content/uploads/2016/08/Screenshot_20160819_073151.png)](https://www.baeldung.com/wp-content/uploads/2016/08/Screenshot_20160819_073151.png)