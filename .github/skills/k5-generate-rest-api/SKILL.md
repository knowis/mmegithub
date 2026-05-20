---
name: k5-generate-rest-api
description: This skill provides instructions on how to generate REST API controllers and configure Swagger UI based on the design files
---

## Overview

Generate REST API controllers and HTTP endpoints from the design files in src-design/elements/implementation/rest-api and src-design/elements/implementation/rest-api-operation folders. The skill handles two types of REST APIs based on the interaction-type property:

- **Providing (interaction-type: providing)**: REST APIs exposed by the system to external clients. Generate Spring @RestController classes with OpenAPI/Swagger documentation for API visualization and testing.
- **Consuming (interaction-type: consuming)**: External REST APIs consumed by the system. Generate service client classes using RestTemplate or WebClient for HTTP integration.

## Providing REST APIs (interaction-type: providing)

Generate REST API controllers and HTTP endpoints that the system exposes following the given rules:

### File Structure and Location
- for each REST API with interaction-type: providing defined in src-design/elements/implementation/rest-api create a Java controller class at /src/main/java/{basePackage}/api/controller/{RestApiName}Controller.java
- use the label field from the REST API design to name the controller appropriately

### Controller Class Structure
- create a Spring @RestController class with appropriate @RequestMapping for the API base path
- for each REST API operation contained in the REST API (via implementation.contains relationships), create a corresponding handler method
- the handler method name should be derived from the operation label and HTTP method
- implement proper HTTP method annotations: @GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping
- extract the endpoint path from the operation label
- use appropriate HTTP status codes: 200 for successful GET/PUT, 201 for successful POST, 204 for successful DELETE

### Request/Response Handling
- for GET operations without request body: add @RequestParam for query parameters if applicable
- for POST/PUT operations: create or reuse request DTOs and use @RequestBody annotation
- for path parameters: use @PathVariable annotation in method parameters and path placeholders (e.g., @GetMapping("/{id}"))

### Domain Logic Integration
- inject domain services or commands as dependencies via constructor injection using BeanFactory configured beans
- if the REST API operation has a ddd.triggers relationship, instantiate and call the targeted domain service or command within the handler method
- pass request parameters to the domain service/command and return the result as the response
- document external service calls with inline comments explaining their inputs, outputs, and purpose

### OpenAPI/Swagger Annotations
- add @Operation(summary = "...") annotation to each handler method using the summary from the REST API operation design
- for request DTOs, use @RequestParam(description = "...") for query parameters with descriptions
- for response bodies, annotate response DTOs with @Schema and @ApiResponse annotations
- use @Tag(name = "...") on the controller class for logical grouping in Swagger UI
- add @Parameter(description = "...") annotations for path and query parameters
- document response codes: @ApiResponse(responseCode = "200", description = "Success"), @ApiResponse(responseCode = "400", description = "Bad Request"), @ApiResponse(responseCode = "404", description = "Not Found")

## Swagger UI Configuration

### Application Configuration
- in application.yaml and application-local.yaml, configure Swagger UI properties
- ensure Swagger UI endpoint is accessible at: http://localhost:8080/swagger-ui.html

## Securing REST API Endpoints with OIDC and Keycloak

Secure REST API endpoints using OAuth2 Resource Server with OpenID Connect (OIDC) authentication via Keycloak.

### Dependency Configuration
- add spring-boot-starter-oauth2-resource-server dependency to pom.xml
- include spring-security-oauth2-jose for JWT token validation
- ensure spring-boot-starter-security is included in dependencies

### Security Configuration Class
- create a Spring Security configuration class at /src/main/java/{basePackage}/config/SecurityConfig.java
- configure OAuth2 resource server with Keycloak as the authorization server
- define authorization rules using HttpSecurity API with securityMatcher() for endpoint paths
- allow non-secured endpoints (e.g., Swagger UI, health checks) to be accessible without authentication
- secure all /api/** endpoints requiring authenticated access via .authenticate()

### Application Configuration for Keycloak
- in application.yaml, configure OAuth2/OIDC properties:
  ```yaml
  spring:
    security:
      oauth2:
        resourceserver:
          jwt:
            issuer-uri: ${OAUTH2_ISSUER_URI}
            jwk-set-uri: ${OAUTH2_ISSUER_URI}/protocol/openid-connect/certs
  ```
- in application-local.yaml, set local Keycloak instance:
  ```yaml
  spring:
    security:
      oauth2:
        resourceserver:
          jwt:
            issuer-uri: http://localhost:9080/realms/local
  ```

### JWT Token Validation
- implement automatic JWT token validation against Keycloak's public key set
- handle token expiration and validation errors gracefully
- return 401 Unauthorized for invalid or expired tokens
- return 403 Forbidden for valid tokens lacking required scopes/roles

### Scope and Role-Based Access Control
- use @PreAuthorize annotation on controller methods for fine-grained access control
- define application roles mapped to Keycloak realm roles (e.g., "ROLE_USER", "ROLE_ADMIN")
- implement scope checking for endpoint-level authorization requirements
- example: @PreAuthorize("hasAuthority('SCOPE_write:service-requests')") for write operations

### Swagger UI OAuth2 Configuration
- configure Swagger UI to support OAuth2 authentication in application.yaml:
  ```yaml
  springdoc:
    swagger-ui:
      oauth:
        client-id: ${SWAGGER_CLIENT_ID:swagger-ui}
        client-secret: ${SWAGGER_CLIENT_SECRET:}
        realm: ${OAUTH2_REALM:local}
        app-name: Service Requests API
        scopes: openid, profile, email
  ```
- enable "Try it out" functionality in Swagger UI by obtaining an access token through OAuth2 flow
- users can test secured endpoints directly from Swagger UI after authentication

### Exception Handling for Security
- implement a global exception handler using @ControllerAdvice
- catch JwtException and convert to meaningful error responses (401 Unauthorized)
- catch AccessDeniedException and convert to 403 Forbidden responses
- include error message and timestamp in error response for debugging

### Bearer Token Extraction
- tokens are automatically extracted from Authorization header (Bearer scheme) by Spring Security
- implement custom token validation if additional requirements exist (e.g., custom claims validation)
- log authentication failures for security auditing purposes

## Consuming External APIs (interaction-type: consuming)

Generate service client classes for consuming external REST APIs that the system depends on.

### File Structure and Location
- for each REST API with interaction-type: consuming defined in src-design/elements/implementation/rest-api create a Java service client class at /src/main/java/{basePackage}/integration/{RestApiName}/{RestApiName}Client.java
- use the label field from the REST API design to name the client appropriately

### External API Client Class Structure
- create a Spring @Service class for the external API client
- for each REST API operation contained in the REST API (via implementation.contains relationships), create a corresponding method
- the method name should be derived from the operation label and HTTP method
- extract the endpoint path from the operation label

### HTTP Client Configuration
- use RestTemplate for synchronous HTTP calls or WebClient for reactive/asynchronous calls
- create or reuse a RestTemplate or WebClient configuration bean at {basePackage}.config.RestTemplateConfiguration or WebClientConfiguration
- configure appropriate timeouts, connection pooling, and error handling in the client configuration
- if external API requires authentication, add interceptors or headers as needed (e.g., API keys, OAuth2 tokens)

### External API Integration
- implement methods that call the external API endpoints and return domain objects or DTOs
- handle HTTP exceptions and network errors with appropriate error handling and logging
- parse external API responses and map them to internal domain objects or DTOs
- log all external API calls with request and response information for debugging

### Configuration for External APIs
- in application.yaml and application-local.yaml, add external API configuration:
- example configuration structure:
  ```yaml
  external-apis:
    external-service:
      base-url: https://api.example.com/v1
      timeout: 5000
      enabled: true
  ```

## Generation Guidelines
- use the generation guidelines skill when executing this skill
