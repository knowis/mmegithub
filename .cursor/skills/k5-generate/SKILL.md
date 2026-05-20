---
name: k5-generate
description: Generate the Java classes from the design files using element-specific skills
---

## Overview

Generate all Java components from the design files using element-specific generation skills in a coordinated and structured manner. This orchestrating skill guides the generation workflow and ensures consistency across all generated components.

## Design Files

Understand the structure and purpose of design files.

### Design File Organization
- the design-files under /src-design represent the designed building blocks of the project

## Generation Process

Execute generation skills in the proper order.

### Generation Order
- call generation skills in the following order: 
  1. k5-generate-entities
  2. k5-generate-persistance-layer
  3. k5-generate-commands
  4. k5-generate-domain-services
  5. k5-generate-rest-api
  6. k5-generate-eventing

### Subagent usage
- the whole generation process should be orchestrated by a main agent
- use subagents for the generation skills to reduce context window usage from the main orchestrating agent
