---
name: k5-generate-eventing
description: This skill provides instructions on how to generate event publishers and consumers using Apache Kafka based on business-event design files
---

## Overview

Generate Apache Kafka-based event publishers and consumers from the domain business event design files in src-design/elements/ddd/business-event folder. Eventing enables asynchronous communication between domain services and bounded contexts through domain events.

## Event Generation

Create event classes and supporting infrastructure for each business event defined in the design files.

### File Structure and Location
- for each business event defined in src-design/elements/ddd/business-event create an event class at /src/main/java/{basePackage}/domain/{domainName}/event/{EventName}.java
- create event publisher services at /src/main/java/{basePackage}/domain/{domainName}/event/publisher/{EventName}Publisher.java
- create event consumer services at /src/main/java/{basePackage}/domain/{domainName}/event/consumer/{EventName}Consumer.java

### Event Naming
- use the label field from the business event design to derive event class names
- publisher class names should follow pattern: {EventName}Publisher
- consumer class names should follow pattern: {EventName}Consumer

## Event Class Structure

Implement event classes representing domain events.

### Event Properties
- add all event payload properties defined in the description field to the event class

### Event Documentation
- document event's purpose and business significance based on the description field

## Event Publisher Implementation

Create publishers for publishing events to Kafka topics.

### Publisher Class Structure
- create a Spring @Service class for each event publisher
- implement a publish() method that sends the event to the appropriate Kafka topic

### Kafka Topic Configuration
- define topic names based on event names in lowercase with hyphens
- configure topic names in application.yaml with spring.kafka.producer.topic properties
- ensure topic names are consistent across producers and consumers

### Publisher Methods
- implement publish() method that accepts the event object and publishes it to Kafka
- handle serialization and error handling for event publishing
- add logging for published events and any errors

## Event Consumer Implementation

Create consumers for listening to and processing events from Kafka topics.

### Consumer Class Structure
- create a Spring @Service class for each event consumer
- implement consumer methods that handle incoming events and trigger domain logic

### Consumer Methods
- create message handler methods annotated with @KafkaListener(topics = "topic-name")
- deserialize Kafka messages into event objects
- implement business logic to handle the event (e.g., update domain state, trigger commands)

### Error Handling
- implement error handling and retry logic for event consumption
- handle deserialization errors and invalid messages gracefully
- log consumed events and any processing errors

## Constructor Implementation

Implement constructors for event classes.

### Constructor Types
- create default constructor for event classes
- create parameterized constructor with all event payload properties

## Getter and Setter Methods

Implement property access methods.

### Getter Methods
- provide standard getter methods for all event properties

### Setter Methods
- implement setter methods with fluent interface pattern: all setter methods should return 'this' for method chaining

## Kafka Configuration

Configure Kafka connection and serialization settings.

### Application Configuration
- in application.yaml and application-local.yaml, configure Kafka properties:
  - spring.kafka.bootstrap-servers: Kafka broker connection (default: localhost:9092)

### Docker Configuration
- if using Docker for local Kafka, ensure kafka.yml is configured in src/main/docker/local/

### Security Configuration
- integrate security configuration into the KafkaConfig class using @Value annotated fields
- ensure credentials are externalized through @Value annotations and not hardcoded
- add the following security-related properties with default values:
  - spring.kafka.security.protocol (default: PLAINTEXT)
  - spring.kafka.properties.sasl.mechanism (default: empty string)
  - spring.kafka.properties.sasl.jaas.config (default: empty string)
  - spring.kafka.properties.ssl.endpoint.identification.algorithm (default: https)
- ensure security configuration is applied consistently to both producer and consumer configurations

## Generation Guidelines
- use the generation guidelines skill when executing this skill
