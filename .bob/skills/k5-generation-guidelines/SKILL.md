---
name: k5-generation-guidelines
description: This skill provides instructions that have to be applied when using one of the k5-generate-* skills
---

## Overview

This file serves as a guide for applying generation guidelines when using k5-generate-* skills, ensuring consistency in code quality, configuration management, and deployment across generated projects.

## Coding Guidelines

Apply consistent coding practices across all generated code.

### Applying Guidelines
- apply the coding guidelines provided by the skill k5-coding-guidelines

## Configuration Management

Update application configuration files with project-specific values.

### Configuration Files
- edit all values in application.yaml and application-local.yaml according to project definition (e.g. mentions of project name)
- after implementing, check the application.yaml and application-local.yaml and make necessary changes

### Deployment files
- the project gets deployed via helm
- make the neccessary changes to the values.yaml to include all needed variables

### Template Naming Replacement
- the project-name should be the acronym defined in the k5-project.yml
- if not stated otherwise the groupID in the pom.xml is 'k5.{project-name}'
- the artifactID in the pom.xml is the {project-name}-application
- update the pom.xml with project-specific values (groupId, artifactId, name, description)
- replace all occurrences of "template" with the actual project name throughout the project
- update Docker Compose files (src/main/docker/local/*.yml):
  - rename network from `template-network` to `{project-name}-network`
  - rename container names from `template-*` to `{project-name}-*`
  - update compose project name from `template` to `{project-name}`
  - update database names from `template` to `{project-name}`
  - update volume paths from `~/volumes/template/` to `~/volumes/{project-name}/` 
- ensure consistency across all configuration and deployment files

## Build and Compilation

Compile and validate the generated code.

### Project Compilation
- compile project with mvn clean compile after generation

### Build Status and Error Handling
- report build status (SUCCESS/FAILED)
- if build failed, list errors and suggest fixes

## Reporting and Validation

Report results of the generation process.

### Generation Summary
- report generation summary with counts of generated/skipped files
- report build status (SUCCESS/FAILED)

