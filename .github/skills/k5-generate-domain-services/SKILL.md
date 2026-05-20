---
name: k5-generate-domain-services
description: This skill provides instructions on how to generate the domain services of a domain
---

## Overview

Generate Java domain service classes from the domain service design files in src-design/elements/ddd/domain-service folder. Domain services encapsulate business logic that doesn't naturally belong to a single entity, orchestrating operations across multiple domain objects.

## Domain Service Generation

Create Java classes for each domain service defined in the design files.

### File Structure and Location
- for each domain service defined in src-design/elements/ddd/domain-service create a Java class at /src/main/java/{basePackage}/domain/{domainName}/service/{ServiceName}.java

### Stateless Design
- domain services are stateless and should not maintain any instance state

## Service Implementation

Implement the main service execution logic.

### Execute Method
- implement an execute() method as the main entry point containing the service logic
- implement the logic defined in the design-files of the service

## External Service Integration

Handle integrations with external services and systems.

### External Service Implementation
- for external service integrations mentioned in the description, add parameters like externalServiceRef or create placeholder method calls with comments explaining the external function
- document all external function calls with inline comments explaining their inputs, outputs, and purpose

## Service Orchestration

Implement calls to other domain services and commands.

### Triggering Other Services
- if the service triggers other domain services or commands, instantiate them and call their methods
- if relationships field shows "triggers" relationship, implement calls to the target command or service

## Helper Methods

Implement helper methods for code organization.

### Private Helper Methods
- create private helper methods for complex sub-logic or comparison operations to keep execute() readable

## Generation Guidelines
- use the generation guidelines skill when executing this skill
