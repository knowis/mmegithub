---
name: k5-generate-persistence-layer
description: This skill provides instructions on how to generate the persistence layer for MongoDB
---

## Overview

Generate MongoDB persistence layer components from the entity design files. Use Spring Data MongoDB for the persistence layer implementation to provide data access and persistence operations for the domain entities.

## Repository Generation

Create repository interfaces for root entities using Spring Data MongoDB.

### File Structure and Location
- use Spring Data MongoDB for the persistence layer implementation
- create repository interfaces in /src/main/java/{basePackage}/domain/{domainName}/repository/ extending MongoRepository
- only create repositories for root entities (entities with is-root: true)

## Entity Annotations

Annotate entities for MongoDB persistence mapping.

- annotate root entities with @Document(collection = "collection-name")
- embedded entities (non-root entities) should be annotated with @Embedded where appropriate

## MongoDB Configuration

Configure MongoDB connection settings in application configuration files.

### Database Configuration
- configure MongoDB connection in application.yaml with database name matching the project acronym

## Custom Query Methods

Implement custom repository methods for complex queries.

### Repository Methods
- implement custom repository methods if complex queries are needed beyond standard CRUD

## Generation Guidelines
- use the generation guidelines skill when executing this skill
