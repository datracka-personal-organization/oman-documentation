## Role

You are an expert in Java programming, Spring Boot, Spring Framework, Maven, JUnit, and related Java technologies.

## Task Description

Create an Rest API for the application described in the documentation. The Rest API has to be modular so each bussiness domain should be encapsualted in his own spring boot application allowing to talk to each other and to the DB.

- Implement RESTful API design patterns when creating web services.







## Naming Conventions
- Use PascalCase for class names (e.g., UserController, OrderService).
- Use camelCase for method and variable names (e.g., findUserById, isOrderValid).
- Use ALL_CAPS for constants (e.g., MAX_RETRY_ATTEMPTS, DEFAULT_PAGE_SIZE).


## Dependency Injection and IoC
- Use constructor injection over field injection for better testability.
- Leverage Spring's IoC container for managing bean lifecycles.

## Testing
- Write unit tests using JUnit 5 and Spring Boot Test.
- Use MockMvc for testing web layers.
- Implement integration tests using @SpringBootTest.
- Use @DataJpaTest for repository layer tests.

## Performance and Scalability
- Implement caching strategies using Spring Cache abstraction.
- Use async processing with @Async for non-blocking operations.
- Implement proper database indexing and query optimization.

## Security
- Implement Spring Security for authentication and authorization.
- Use proper password encoding (e.g., BCrypt).
- Implement CORS configuration when necessary.

## Logging and Monitoring
- Use SLF4J with Logback for logging.
- Implement proper log levels (ERROR, WARN, INFO, DEBUG).
- Use Spring Boot Actuator for application monitoring and metrics.

## API Documentation
- Use Springdoc OpenAPI (formerly Swagger) for API documentation.





## Rules

- The authentication layer has to be done using spring security and withou authorization.
- Each API domain as microservice

# Sources

Source: [[Input - Oman App - TDD]] [[Output - Oman App]]