
# Extended BDD Template for Database Schema with Tags

## Overview

**Purpose**: Structured template for documenting database schemas using Behavior-Driven Development (BDD) principles, including tables, constraints, indexes, triggers, validation rules, performance, and security.

---

## Table: <table_name>

**Description**: <Purpose of the table>

```sql
<-- Table DDL goes here -->
```

### Tags:
`@DB @table:<table_name> @status:<level>`

---

### Data Behaviors

```gherkin
Feature: <Table> Data Validations and Operations

  @DB @table:<table_name> @type:validation @action:create @status:critical
  Scenario: Insert valid <entity> record
    Given I have valid data for <table>
    When I insert the record into the <table>
    Then the record should be created successfully
    And required fields should be populated correctly

  @DB @table:<table_name> @type:constraint @action:create @status:critical
  Scenario: Prevent duplicate entries on <unique_field>
    Given there exists a record with <field>
    When I try to insert a record with the same <field>
    Then the insert should fail with a constraint violation

  @DB @table:<table_name> @type:validation @action:create @status:optional
  Scenario: Reject invalid <field> format
    When I insert a record with invalid <field>
    Then the system should throw a validation error
```

---

## Table Constraints and Indexes

### Tags:
`@DB @table:<table_name> @type:constraint @type:index @status:critical`

```gherkin
Feature: Indexes and Constraints

  @DB @table:<table_name> @type:index @action:read @status:critical
  Scenario: Query performance with indexed field
    Given there is a composite index on <fields>
    When I query using indexed fields
    Then the query should be optimized by the planner

  @DB @table:<table_name> @type:constraint @action:create @status:critical
  Scenario: Enforce check constraint for valid <field>
    When I insert invalid value into <field>
    Then insert should fail due to check constraint
```

---

## Triggers and Functions

### Tags:
`@DB @type:function @type:trigger @status:critical`

```gherkin
Feature: Trigger and Function Behavior

  @DB @type:trigger @action:update @status:critical
  Scenario: Auto-update timestamp on modification
    Given a record is updated
    When update occurs on the row
    Then the 'updated_at' should reflect the new timestamp

  @DB @type:function @action:read @status:optional
  Scenario: Compute analytics via stored function
    When I run the function with valid session ID
    Then analytics should be computed and returned
```

---

## Migration Strategy

### Tags:
`@DB @type:migration @status:critical`

```gherkin
Feature: Database Migration

  @DB @type:migration @action:create @status:critical
  Scenario: Apply base migration
    Given a new database
    When I run migration "001_initial_schema"
    Then all base tables should be created

  @DB @type:migration @action:rollback @status:optional
  Scenario: Rollback last migration safely
    When I rollback migration "004_add_indexes"
    Then schema should revert without data loss
```

---

## Security and Access Control

### Tags:
`@DB @type:security @status:critical`

```gherkin
Feature: Row-Level Security (RLS)

  @DB @type:security @status:critical
  Scenario: Enforce row-level access on attendance table
    Given teacher "A" and teacher "B" have separate sessions
    When teacher "A" queries attendance
    Then they should only see their own session data

  @DB @type:security @action:grant @status:critical
  Scenario: Student has read-only access to their records
    When student logs in
    Then they should only have SELECT permissions on their rows
```

---

## Performance Metrics

### Tags:
`@DB @type:performance @status:optional`

```gherkin
Feature: Performance Monitoring

  @DB @type:performance @action:read @status:optional
  Scenario: View dead tuples statistics
    When I query table_stats
    Then I should see low count of dead tuples for optimized tables

  @DB @type:performance @action:read @status:optional
  Scenario: Index usage should be high
    When I monitor index scans
    Then idx_scan count should increase over time
```

---

## Data Quality and Integrity

### Tags:
`@DB @type:quality @type:validation @status:critical`

```gherkin
Feature: Data Integrity Checks

  @DB @type:quality @status:critical
  Scenario: No attendance records without enrollment
    Given a session has no enrolled students
    When I query attendance
    Then there should be no associated attendance records

  @DB @type:quality @status:critical
  Scenario: Sessions should have assigned teachers
    When I query sessions
    Then all sessions should have a valid teacher reference
```

---

## Template Usage Notes

- Use `@table:<name>` consistently for grouping scenarios.
- Apply `@type:` tags to express what aspect is being tested (e.g. `@type:constraint`, `@type:security`).
- Combine tags smartly to enable automated filtering (e.g., regression, smoke tests).
- Apply `@status:` to prioritize scenarios in CI/CD test matrix.

This template provides a structured approach to documenting db behaviors using BDD methodology with comprehensive tagging for test organization and execution.

check BDD document having feature files containing @db specifications, create feature folder IF NOT PRESENT with that feature name and create DB  BDD specification for each component, 1 file per DB using this template
