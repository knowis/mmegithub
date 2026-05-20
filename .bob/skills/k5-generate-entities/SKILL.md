---
name: k5-generate-entities
description: This skill provides instructions on how to generate entities from the src-design files
---

## Overview

Generate Java entity classes from the domain entity design files in src-design/elements/ddd/entity folder. Entity classes represent domain objects with unique identities that persist over time, including aggregate roots and embedded entities.

## Entity Generation

Create Java classes for each entity defined in the design files.

### File Structure and Location
- for each entity defined in src-design/elements/ddd/entity create a Java class at /src/main/java/{basePackage}/domain/{domainName}/entity/{EntityName}.java

### Entity Type Handling
- if the entity has is-abstract: true, declare the class as abstract
- if the entity has is-root: true, mark it as the aggregate root (document with comment)

## Property Implementation

Implement entity properties based on design file definitions.

### Properties
- add all properties defined under the propertiesDefinition field to the classes

### Nested Properties
- reflect the relationship of nested properties when implementing the classes

### Type Mapping
- map property types from design files to Java types

## Method Implementation

Implement constructors and methods for entity functionality.

### Constructor Implementation
- create default constructor and constructor with all parameters for each entity

### Getter Methods
- provide standard getter methods for all properties

### Setter Methods - Fluent Interface
- implement fluent interface pattern: all setter methods should return 'this' for method chaining

## Inheritance

Handle abstract base classes and their implementations.

### Abstract Base Classes
- for abstract base classes, concrete implementations must extend the base class and override fluent methods to return correct type

## Generation Guidelines
- use the generation guidelines skill when executing this skill
