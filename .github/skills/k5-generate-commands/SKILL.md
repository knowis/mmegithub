---
name: k5-generate-commands
description: This skill provides instructions on how to generate commands of a domain
---

## Overview

Generate Java command classes from the domain command design files in src-design/elements/ddd/command folder. Commands are used to either create a new instance or manipulate an already existing instance of a root entity.

## Command Generation

Create Java classes for each command defined in the design files.

### File Structure and Location
- for each command defined in src-design/elements/ddd/command create a Java class at /src/main/java/{basePackage}/domain/{domainName}/command/{CommandName}.java

### Command Type Handling
- read the is-factory property to determine if this is a factory command (creates instance) or a instance command (modifies instance)
- factory commands should create and return a new instance of a root entity; non-factory commands should modify an existing instance

## Property Implementation

Implement command properties and constructors.

### Properties
- create properties for all input parameters mentioned in the command description

### Constructor Implementation
- create default constructor and parameterized constructor for key properties

## Method Implementation

Implement command methods and patterns.

### Execute Method
- implement an execute() method that contains the command logic as described in the description field
- for factory commands, the execute() method should return the created instance of the root entity
- for non-factory commands, the execute() method should return boolean indicating success

### Getter Methods
- provide standard getter methods for all properties

### Setter Methods - Fluent Interface
- implement fluent interface pattern: all setter methods should return 'this' with withPropertyName() pattern

## Command Logic

Implement the business logic contained in the command.

### Entity Initialization
- for factory commands, initialize all required entities as described in the logic

## Service Orchestration

Implement calls to other commands or services.

### Triggering Other Commands or Services
- for commands that trigger other commands or services, document these as method calls (can be placeholder implementations)

## Generation Guidelines
- use the generation guidelines skill when executing this skill
