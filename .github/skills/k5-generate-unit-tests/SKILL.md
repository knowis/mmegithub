---
name: k5-generate-unit-tests
description: This skill provides instructions on how to generate unit tests for the project components
---

## Overview

Generate comprehensive unit tests for Java domain model classes, services, commands, repositories, and controllers. Unit tests verify the correctness of individual components in isolation using mocking, test data, and assertion-based validation. Tests should follow the AAA (Arrange-Act-Assert) pattern and achieve meaningful code coverage.

## Test Framework and Setup

### Testing Dependencies
- use JUnit 5 (Jupiter) as the testing framework
- use Mockito for mocking objects and verifying interactions
- use AssertJ for fluent assertions
- use Spring Test for integration support when needed (@SpringBootTest, @DataMongoTest)

### Test Configuration
- configure test profiles using application-test.yaml
- use @ExtendWith(MockitoExtension.class) for unit tests with Mockito
- use test containers or embedded databases for data access testing
- disable unnecessary autoconfiguration for isolated unit tests

## Test File Structure and Location

### File Organization
- for each source class at /src/main/java/{basePackage}/{domain}/{type}/{ClassName}.java create corresponding test class at /src/test/java/{basePackage}/{domain}/{type}/{ClassName}Test.java
- maintain parallel package structure between src and test directories
- organize test files by the component type being tested (service, command, entity, repository, controller)

### Test Class Naming Conventions
- append "Test" suffix to the source class name
- use descriptive names that indicate what is being tested: {ClassName}Test
- for parameterized tests, optionally use {ClassName}ParameterizedTest

## Unit Tests for Domain Services

### Domain Service Test Structure
- inject dependencies using @Mock annotation for Mockito
- inject the service under test using @InjectMocks in a @SpringBootTest context or direct instantiation
- organize test methods using descriptive names following pattern: testMethodName_GivenCondition_ExpectedResult

### Test Methods
- test each public method of the domain service
- create test methods for:
  - happy path scenarios (successful execution)
  - error scenarios (exceptions, validation failures)
  - edge cases (null inputs, empty collections, boundary values)
- use @DisplayName annotation for human-readable test descriptions

### Mocking Service Dependencies
- mock all external dependencies (repositories, other services, event publishers)
- use Mockito.when() for method stubbing
- verify critical interactions using Mockito.verify()
- use ArgumentCaptor for advanced verification of method arguments

### Assertions
- use AssertJ assertions for readable test code
- assert return values using assertThat().isEqualTo(), isTrue(), contains(), etc.
- assert state changes (if any) on the service or dependencies
- assert exception types using assertThatThrownBy() for error scenarios

## Unit Tests for Commands

### Command Test Structure
- test factory commands (should create new instances) and instance commands (should modify existing instances)
- mock domain repositories and services that commands depend on
- test both the execute() method and any validation logic

### Factory Command Tests
- verify that execute() creates and returns a new instance of the root entity
- verify all required properties are initialized correctly
- test any initialization logic with different input parameters
- verify dependencies are called correctly to create related entities

### Instance Command Tests
- verify that execute() returns boolean indicating success
- test modification of existing instances
- test validation of pre-conditions (entity must exist, be in correct state, etc.)
- verify that required dependencies are updated appropriately

### Test Data
- create realistic test data using builders or factory methods
- provide multiple test scenarios with different parameter combinations
- test boundary conditions and invalid inputs

## Unit Tests for Entities

### Entity Test Structure
- test entity construction with different parameter combinations
- test getter and setter methods (especially for fluent setters)
- test any business logic methods on entities
- keep entity tests lightweight without heavy mocking

### Entity Test Methods
- test default constructor and parameterized constructors
- verify fluent setter methods return 'this' for chaining
- test any calculated properties or derived values
- test value object equality if applicable

### Validation Tests
- test any validation logic at entity construction time
- verify invalid states cannot be created
- test state transitions if entities have state management

## Unit Tests for Repositories

### Repository Test Structure
- use @DataMongoTest for MongoDB repository tests
- provide an embedded MongoDB or use testcontainers for integration testing
- inject MongoTemplate for manual data setup if needed
- test actual database interactions, not mocked interactions

### Repository Test Methods
- test CRUD operations: create, read, update, delete
- test custom query methods defined in repository interface
- test finding by various properties and combinations
- test edge cases: empty results, multiple results, no matching results

### Test Data Setup
- populate test data using MongoTemplate or repository.save()
- use @BeforeEach to set up test state before each test
- use @AfterEach to clean up test data after each test
- consider using test fixtures or builders for complex test data creation

### Assertions
- assert query results contain expected documents
- verify count of returned results
- assert returned entities have correct properties
- assert sorting and ordering of results if applicable

## Unit Tests for REST Controllers

### Controller Test Structure
- use @WebMvcTest for isolated controller testing
- mock all service dependencies
- use MockMvc to simulate HTTP requests
- test endpoint routing, status codes, and response content

### Test Methods
- test each REST endpoint with different HTTP methods
- test happy path scenarios with valid input
- test error scenarios: 400 Bad Request, 404 Not Found, 500 Internal Server Error
- test parameter validation and request validation
- test response DTOs are properly serialized

### Request/Response Testing
- use MockMvc.perform() to make HTTP requests
- verify HTTP status codes using .andExpect(status().isOk())
- verify response content using .andExpect(content().json(...))
- test request parameter binding: @RequestParam, @PathVariable, @RequestBody
- test request validation and error responses

### Service Integration
- verify service calls are made with correct parameters using Mockito.verify()
- verify the correct response is returned from service to client
- test exception handling: how exceptions from services are converted to HTTP responses

## Testing Patterns and Best Practices

### Arrange-Act-Assert Pattern
- **Arrange**: Set up test data and mock expectations
- **Act**: Call the method under test
- **Assert**: Verify the results match expectations

### Naming Conventions for Test Methods
- use descriptive names: testCreateRequest_WithValidInput_ReturnsNewInstance
- use should_ prefix for behavior-driven tests: shouldCreateRequestWhenInputIsValid
- use methodName_Condition_ExpectedResult pattern: findById_WithExistingId_ReturnsEntity

### Test Data and Builders
- create builder classes or factory methods for complex test data
- use @Builder annotation or factory methods for entity construction
- maintain test data that mirrors realistic scenarios
- avoid test interdependencies; each test should be independent

### Mocking Best Practices
- mock only external dependencies, not the class under test
- use strict mocking to catch unexpected interactions
- reset mocks between parameterized test iterations
- verify important interactions but avoid over-verification

### Assertions Best Practices
- use AssertJ for readable, fluent assertions
- assert one logical concept per test method
- use descriptive assertion messages using .as()
- avoid magic numbers; use named constants or test data objects

## Test Coverage

### Code Coverage Goals
- aim for meaningful coverage of public methods (70%+ minimum)
- prioritize coverage of business logic and critical paths
- test edge cases and error conditions, not just happy paths
- avoid coverage without meaningful assertions

### Coverage Reporting
- use JaCoCo Maven plugin for coverage reporting
- review coverage reports to identify untested code paths
- maintain coverage above configured threshold in CI/CD
- focus on covering critical business logic first

## Running Tests

### Running Tests Locally
- run all tests using Maven: mvn test
- run tests for a specific class: mvn test -Dtest=ClassName
- run tests matching a pattern: mvn test -Dtest=ServiceTest#*create*
- run tests with coverage: mvn clean test jacoco:report

### Test Profiles
- use application-test.yaml for test-specific configuration
- disable external service calls in test profile if applicable
- configure embedded databases or test containers for tests
- use test properties to override production properties

### Continuous Integration
- tests should run in CI/CD pipeline on every commit
- verify tests pass before merging pull requests
- report coverage metrics and enforce minimum coverage thresholds
- fail builds if tests fail

## Test Organization and Maintenance

### Test Documentation
- add comments explaining complex test scenarios
- document test data setup and any special test configuration
- explain why certain tests exist if not obvious from the name
- keep test code clean and readable, not just functional
- add documentation on how to execute the tests to the README.md

### Shared Test Utilities
- create common test base classes or utilities for repeated setup
- extract common test data builders or factories
- create custom assertions for domain-specific assertions
- maintain a shared test configuration class if needed

### Test Maintenance
- refactor tests when production code changes
- keep tests synchronized with actual behavior
- review and update tests during code reviews
- remove tests for deleted functionality

## Generation Guidelines
- use the generation guidelines skill when executing this skill
