---
name: k5-coding-guidelines
description: This skill provides coding guidelines for the implementation and should be used in every generation skill and when implementing any code
---

## Overview

Coding guidelines for all implementation work. These guidelines ensure consistency, maintainability, and quality across the entire codebase. Apply these guidelines in every generation skill and when implementing any code.

## Technology Stack

Define the technology stack and tools used for the project.

### Programming Language and Framework
- the programming language for this project is Java
- the project is a spring boot application

### Build Tools
- integrate maven into the project
- update the pom.xml according to the project specifications and update dependencies when necessary

## Dependency Management

Configure dependency injection and bean management.

### Spring Beans
- use the BeanFactory for managing beans in the project

## Code Documentation

Document all code with clear and comprehensive comments.

### File-Level Documentation
- begin every file with a comment block that explains its purpose and role within the project

### Method Documentation
- document every function with a comment block that describes its functionality, including inputs and outputs
- add JavaDoc comments for all constructors and methods describing inputs, outputs, and purpose

### Inline Comments
- use inline comments to clarify the purpose and implementation of non-obvious code segments or any external function calls (functions not defined within the current file), include a comment explaining their inputs, outputs, and purpose

## Naming Conventions

Use meaningful and consistent naming throughout the codebase.

### Package Naming
- the package name should include the project acronym from the k5-project.yml definition

### Variable Naming
- use meaningful variable names that match the domain language used in the description

## Logging

Implement comprehensive and meaningful logging throughout the application for operational visibility and debugging.

### Logging Framework and Configuration
- use SLF4J (Simple Logging Facade for Java) with Logback as the logging implementation
- configure logback.xml in src/main/resources with appropriate log levels and appenders
- use the injected logger in each class: `private static final Logger logger = LoggerFactory.getLogger(ClassName.class);`
- configure production log levels: INFO for services and controllers, DEBUG for detailed operations
- use different appenders for console (development) and file-based logging (production)

### Log Levels and Usage
- **TRACE**: Use only for detailed diagnostic information during development (disabled in production)
- **DEBUG**: Use for detailed information useful during development and troubleshooting (e.g., method entry/exit with parameters)
- **INFO**: Use for informational messages about key application events and state changes (successful operation completion)
- **WARN**: Use for warning messages indicating potential issues that don't prevent execution (e.g., deprecated method usage, retry attempts)
- **ERROR**: Use for error messages indicating failures that require attention (e.g., validation failures, exception details)

### Logging Requirements by Component

#### Commands (Factory and Instance)
- log at DEBUG level when a command is instantiated with: `logger.debug("Executing {} command with parameters: {}", commandName, parameters);`
- log at INFO level when command execution completes successfully: `logger.info("{} command completed successfully", commandName);`
- log at ERROR level when command execution fails: `logger.error("Error executing {} command: {}", commandName, exception.getMessage(), exception);`
- include relevant identifiers (e.g., aggregateId, requestId) in log messages for traceability

#### Domain Services
- log at DEBUG level when a service method is invoked: `logger.debug("Executing {} service with input: {}", serviceName, inputData);`
- log at INFO level when service execution completes: `logger.info("{} service execution completed. Result: {}", serviceName, result);`
- log at ERROR level when service encounters an error: `logger.error("Error in {} service: {}", serviceName, exception.getMessage(), exception);`
- log intermediate steps for complex operations at DEBUG level

#### REST API Controllers
- log at DEBUG level for incoming requests: `logger.debug("Received {} request to {} with body: {}", httpMethod, endpoint, requestBody);`
- log at INFO level for successful responses: `logger.info("{} request to {} completed with status {}", httpMethod, endpoint, httpStatus);`
- log at WARN level for client errors (4xx): `logger.warn("{} request to {} resulted in client error {}: {}", httpMethod, endpoint, httpStatus, errorMessage);`
- log at ERROR level for server errors (5xx): `logger.error("{} request to {} resulted in server error {}", httpMethod, endpoint, httpStatus, exception);`
- include request identifiers (e.g., requestId, userId) in log messages for tracking user interactions

#### Events (Kafka - Published and Received)
- log at DEBUG level when publishing an event: `logger.debug("Publishing {} event to topic {} with payload: {}", eventType, topic, eventPayload);`
- log at INFO level when event is successfully published: `logger.info("{} event published to topic {} with key {}", eventType, topic, eventKey);`
- log at DEBUG level when consuming an event: `logger.debug("Received {} event from topic {} with payload: {}", eventType, topic, eventPayload);`
- log at INFO level when event is successfully processed: `logger.info("{} event from topic {} processed successfully", eventType, topic);`
- log at ERROR level when event processing fails: `logger.error("Error processing {} event from topic {}: {}", eventType, topic, exception.getMessage(), exception);`
- include event identifiers and correlation IDs for distributed tracing

#### Repositories and Data Access
- log at DEBUG level for database queries: `logger.debug("Executing query: {} with parameters: {}", queryDescription, params);`
- log at INFO level for significant data modifications: `logger.info("Saved {} entity with id: {}", entityType, entityId);`
- log at ERROR level for data access failures: `logger.error("Error accessing data for {}: {}", entityType, exception.getMessage(), exception);`

#### Security and Authentication
- log at WARN level for authentication failures (invalid credentials, expired tokens): `logger.warn("Authentication failed for user: {}", userId);`
- log at INFO level for successful authentication: `logger.info("User {} authenticated successfully", userId);`
- log at ERROR level for security exception details: `logger.error("Security error: {}", exception.getMessage());`
- do NOT log sensitive information such as passwords, tokens, or personal data in any log level

### Exception Logging
- always log exceptions with full stack trace at ERROR level: `logger.error("Description of error context", exception);`
- log the exception cause chain to understand root causes
- include relevant context (input data, expected values, actual values) when logging validation or business logic errors
- log application-level exceptions that are caught and handled differently than uncaught exceptions

