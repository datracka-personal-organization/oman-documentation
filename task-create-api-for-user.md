## Role

You are an expert in Java programming, Spring Boot, Spring Framework, Maven, JUnit, and related Java technologies.

## Task Description

Create an Rest API for the application described in the documentation. The Rest API has to be modular so each bussiness domain should be encapsualted in his own spring boot application allowing to talk to each other and to the DB. 

In this case we want the User authentication Rest API 

Implement RESTful API design patterns when creating web services.

# Open API Schema
```
openapi: 3.0.0
info:
  title: Oman User Authentication API
  description: API for user authentication using Spring Security and Spring Cloud
  version: 1.0.0
servers:
  - url: http://localhost:8080
    description: Local server

components:
  securitySchemes:
    basicAuth:
      type: http
      scheme: basic
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
          format: int64
        username:
          type: string
        password:
          type: string
        email:
          type: string
    AuthRequest:
      type: object
      properties:
        username:
          type: string
        password:
          type: string
    AuthResponse:
      type: object
      properties:
        token:
          type: string
    ErrorResponse:
      type: object
      properties:
        message:
          type: string

paths:
  /register:
    post:
      summary: Register a new user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
      responses:
        '201':
          description: User created successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '400':
          description: Bad request
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

  /login:
    post:
      summary: Authenticate a user and return a JWT token
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AuthRequest'
      responses:
        '200':
          description: Authentication successful
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AuthResponse'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

  /me:
    get:
      summary: Get the authenticated user's details
      security:
        - bearerAuth: []
      responses:
        '200':
          description: User details retrieved successfully
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
        '401':
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'

security:
  - basicAuth: []
  - bearerAuth: []
```
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

## Folder structure

```bash
USERSERVICE
├── .mvn
├── .vscode
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── vicensfayos
│   │   │           └── oman
│   │   │               └── omanapp
│   │   │                   └── UserserviceApplication.java
│   │   ├── resources
│   │   │   ├── static
│   │   │   ├── templates
│   │   │   └── application.properties
│   └── test
│       └── java
│           └── com
│               └── vicensfayos
│                   └── oman
│                       └── omanapp
│                           └── UserserviceApplicationTests.java
├── target
├── .gitignore
├── HELP.md
├── mvnw
├── mvnw.cmd
└── pom.xml

```
# Sources

Source: [[Input - Oman App - TDD]] [[Output - Oman App]]