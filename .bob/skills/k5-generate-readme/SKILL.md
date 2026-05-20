---
name: k5-generate-readme
description: This skill provides instructions on how to generate a detailed README for the project including documentation, getting started guide, and configuration documentation
---

## Overview

Generate a comprehensive README.md file at the project root that documents the project, provides guidance on how to use the project (starting the application locally), and includes detailed configuration documentation. The README serves as the primary source of information for developers and users wanting to understand and run the project.

## README Structure and Organization

Create a well-organized README with clear sections and proper Markdown formatting.

### File Structure and Location
- create or update the README.md file at the project root: /README.md
- use proper Markdown syntax with hierarchical heading levels (# for main title, ## for major sections, ### for subsections)

## Project Documentation Section

### Project Overview
- provide a concise 2-3 sentence description of what the project does and its primary purpose
- reference the technology stack used (e.g., Java, Spring Boot, MongoDB, Kafka)

### Project Architecture
- include a brief description of the overall architecture
- explain the main components and how they interact

### Technology Stack
- clearly document all key technologies and frameworks used:
  - **Framework & Language**: Java, Spring Boot (specify version)
  - **Database**: MongoDB (specify intended version)
  - **Message Queue**: Apache Kafka (specify intended version)
  - **API Documentation**: Swagger UI / OpenAPI
  - **Authentication & Authorization**: document solution (e.g., Keycloak)
  - **Build Tool**: Maven
  - **Container**: Docker
  - **Orchestration**: Kubernetes / Helm
  - **Testing**: document testing framework if applicable

## Getting Started Section

### Prerequisites
- list all software and tools required before installation:
  - Java Development Kit (JDK) - specify version requirement (e.g., 17 or higher)
  - Maven - specify version requirement
  - Docker and Docker Compose (for running dependencies locally)
  - Git
  - IDE recommendation (e.g., IntelliJ IDEA, VS Code)
  - Any API keys or credentials needed

### Installation and Setup
- provide step-by-step instructions for setting up the project locally:
  1. Clone the repository with proper Git clone command
  2. Navigate to the project directory
  3. Install Maven dependencies using appropriate Maven command
  4. Configure environment variables if needed

### Running the Application Locally

#### Using Docker Compose for Dependencies
- provide instructions for starting required services using Docker Compose:
  - explain which Docker Compose file to use (src/main/docker/local/services.yml)
  - provide the command to start all services: `docker-compose -f src/main/docker/local/services.yml up -d`
  - list which services are started (MongoDB, Kafka, Keycloak, etc.)
  - mention how to view logs: `docker-compose -f src/main/docker/local/services.yml logs -f`
- provide alternative commands using podman instead of docker

#### Starting the Java Application
- provide clear instructions for building and running the Spring Boot application:
  - **Using Maven**: document the Maven command to run the application with the local profile
  - **Using IDE**: provide instructions for running from an IDE (IntelliJ IDEA or similar)
  - **Port Configuration**: specify the default port(s) the application runs on (e.g., 8080)
  - **Active Profiles**: explain the use of application profiles (develop, local, test) and how to activate them

#### Accessing the Application
- document how to access the application once it's running:
  - URL for the application home page
  - URL for Swagger UI API documentation

#### Stopping the Application
- explain how to properly stop the application and services:
  - stopping the Java application (Ctrl+C for terminal, Stop button in IDE)
  - stopping Docker services: `docker-compose -f src/main/docker/local/services.yml down`

### Building the Application
- provide instructions for building the project:
  - explain the Maven build command
  - document any build profiles or options

## Deploying the Application with helm

### Add variables to GitLab CI/CD Variables for deployment via gitlab-ci
- explain that the application gets build and pushed to the image registry with GitLab pipelines
- the deploy pipeline is triggered manually
- explain that in order to build and deploy the application with gitlab-ci several variables have to be set in GitLab CI/CD Variables section
  - **HOST_URI**: The URI of the host where the application will be deployed
  - **IMAGE_REPOSITORY**: The OpenShift repository URL where the container image will be pushed to
  - **K8S_NAMESPACE**: The Kubernetes namespace where the application will be deployed
  - **K8S_SERVER**: The Kubernetes API server URL
  - **K8S_TOKEN**: The authentication token for accessing the Kubernetes cluster
  - **K8S_USER**: The Kubernetes user account for deployment authentication
  - **OAUTH2_CLIENT_ID**: The OAuth2 client ID for authentication with the identity provider
  - **OAUTH2_ISSUER_URI**: The URI of the OAuth2 token issuer ({keycloakBasePath}/auth/realms/{realm})
  - **OCP_REGISTRY_URL**: The OpenShift Container Platform registry URL for pushing and pulling images
  - **SPRING_DATA_MONGODB_URI**: The MongoDB connection string for the application database
- add all variables with explanation to the list that got added while implementing (e.g. Kafka Server)

## Configuration Documentation Section

### Configuration Overview
- explain that configuration is managed through application.yaml files
- mention the different profiles (local, develop, test, production)
- explain how Spring profiles are activated

### Database Configuration
- **MongoDB**:
  - connection string format
  - database name and collection information (auto-created or manual)
  - configuration properties (uri, host, port, database name)

- **PostgreSQL** (if used):
  - connection string format
  - database name and schema information
  - configuration properties (url, username, password)

### Kafka Configuration
- explain Kafka broker configuration for local development
- document topic names created by the application
- explain how consumers and producers are configured

### Keycloak Configuration
- document Keycloak authentication configuration:
  - realm name
  - client configuration

### Environment Variables
- list any environment variables that should be configured:
  - show the variable name and default value
  - explain what each variable controls
  - specify if it's required or optional

### Logging Configuration
- explain logging configuration (logback.xml)
- document how to change log levels for different packages

## API Documentation Section

### REST API Documentation
- explain how to access and use Swagger UI:
  - provide the URL (http://localhost:8080/swagger-ui.html)
  - mention OpenAPI specification availability

### API Authentication
- explain how authentication works:
  - JWT token-based authentication via Keycloak
  - how to obtain a token

## Development Workflow Section

### Project Structure
- provide a brief overview of the main source directories:
  - src/main/java: application source code
  - src/main/resources: configuration files
  - src/test: test code
  - src-design: design files (DDD aggregates, commands, events, etc.)

### Running Tests (if applicable)
- explain how to run unit tests using Maven
- document any test profiles or test configuration
- mention code coverage if applicable

## README Quality Standards

### Formatting and Style
- use clear, concise language avoiding unnecessary jargon
- use consistent Markdown formatting throughout
- use code blocks (triple backticks) for commands, configuration examples, and code snippets
- use tables for comparing options or listing options with descriptions
- maintain proper heading hierarchy (don't skip levels)

### Maintenance
- ensure all examples and instructions are correct and tested
- regularly update the README when features or configurations change
- keep version numbers and dependency information current
- maintain consistency with actual project structure and configuration

## Generation Guidelines
- use the generation guidelines skill when executing this skill
