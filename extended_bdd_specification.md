# Enterprise-Grade Extended BDD Specification

## 1. Overview
This document provides an **enterprise-grade** specification for writing **extended BDD feature files** using the **Layered Backgrounds** approach (Variation 4). It is designed to ensure consistency, clarity, and reusability across large teams and projects, enabling both human stakeholders and AI-driven pipelines to generate and maintain full-stack code effectively.

## 2. Document Structure
1. **Purpose & Audience**  
2. **Layered BDD Conventions**  
3. **Metadata Definitions**  
4. **Feature File Template**  
5. **Detailed Example: Todo App with Calendar**  
6. **Best Practices & Guidelines**  
7. **Versioning & Change Management**  
8. **Publishing & Distribution**  

---

## 3. Purpose & Audience
- **Purpose:** Standardize how features are specified, incorporating UI, API, and DB contexts in one place.  
- **Audience:** Product Owners, QA Engineers, Developers, DevOps, and AI Integration Specialists.

---

## 4. Layered BDD Conventions
- **Background Sections:** Separate `@UI`, `@API`, and `@DB` backgrounds at the top of each feature.  
- **Scenario Focus:** Each scenario remains high-level, referencing shared contexts.  
- **Tagging:** Use tags to denote metadata and enable targeted test runs (`@smoke`, `@regression`).  

---

## 5. Metadata Definitions
### UI (@UI Background)
- **Components:** Name, key selectors, and behavior expectations.  
- **Views:** Page or module names, navigation steps.

### API (@API Background)
- **Endpoints:** HTTP method, URL path, expected request/response schemas.  
- **Error Cases:** Standardized error payloads and status codes.

### DB (@DB Background)
- **Tables / Models:** Column definitions, types, constraints.  
- **Views & Relations:** Any computed views or foreign-key relationships.

---

## 6. Feature File Template
```gherkin
Feature: <Feature Title>

  @UI
  Background: UI Setup
    Given the following UI components exist:
      | Component    | Selector          | Description       |
      | ExampleComp  | .example-selector | Widget container  |

  @API
  Background: API Setup
    Given these API endpoints are defined:
      | Method | Endpoint           | SchemaRef         |
      | GET    | /example           | getExampleSchema  |

  @DB
  Background: DB Setup
    Given the database contains a table/view:
      | Name        | Columns                      |
      | example_tbl | id, name, created_at         |

  Scenario: <Scenario Title>
    Given ...
    When ...
    Then ...
```

---

## 7. Detailed Example: Todo App with Calendar
```gherkin
Feature: Todo App with Calendar Integration

  @UI
  Background: UI Setup
    Given the app’s main layout includes:
      | Component    | Description                              |
      | CalendarView | Displays calendar grid with events       |
      | TodoList     | Lists todos for selected date or range   |
      | AddTodoModal | Modal for creating/editing todos         |

  @API
  Background: API Setup
    Given the following REST endpoints:
      | Method | Endpoint                | Description                       |
      | GET    | /todos                  | List all todos                    |
      | POST   | /todos                  | Create new todo                   |
      | PUT    | /todos/{id}             | Update existing todo              |
      | DELETE | /todos/{id}             | Delete a todo                     |
      | GET    | /calendar?month={YYYY-MM}| Fetch calendar events for month   |

  @DB
  Background: DB Setup
    Given a `todos` table with columns:
      | Column      | Type      | Notes                         |
      | id          | UUID      | Primary key                   |
      | title       | VARCHAR   | Not null                      |
      | date        | DATE      | Due date                      |
      | recurrence  | VARCHAR   | “none”, “daily”, “weekly”     |
      | completed   | BOOLEAN   | Default false                 |
      | created_at  | TIMESTAMP | Default current timestamp     |

  Scenario: Add a one-time todo
    Given I open the AddTodoModal
    When I enter title "Doctor Appointment"
      And I set date to "2025-07-15"
      And I click "Save"
    Then the API should receive POST /todos
    And the `todos` table should have a record with title "Doctor Appointment" and date "2025-07-15"
```

---

## 8. Best Practices & Guidelines
- **Keep Backgrounds DRY:** Define shared contexts once per feature.  
- **Scenario Clarity:** Scenarios should read like user stories without implementation details.  
- **Tag Strategically:** Use scenario tags to group and filter tests.  
- **Review & Evolve:** Periodically review feature files to remove outdated contexts.

---

## 9. Versioning & Change Management
- **Semantic Versioning:** Maintain a `version` header in the document.  
- **Change Log:** Append a change log section for updates.  
- **Peer Review:** Require sign-off from at least two stakeholders for spec changes.

---

## 10. Publishing & Distribution
- **Repository:** Store under `/docs/bdd-specification.md`.  
- **Access Control:** Read-only for general teams, write access for core architects.  
- **Communication:** Notify teams via Slack/email on major updates.

---

*End of Document*  
