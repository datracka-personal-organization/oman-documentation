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