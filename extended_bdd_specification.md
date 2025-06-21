# Enterprise-Grade Extended BDD Specification

## 1. Overview
This document provides an **enterprise-grade** specification for writing **extended BDD feature files** using the **Layered Backgrounds** approach (Variation 4). It ensures consistency, clarity, and reusability across large teams and projects, enabling both human stakeholders and AI-driven pipelines to generate and maintain full-stack code effectively.

## 2. Document Structure
1. Purpose & Audience
2. Layered BDD Conventions
3. Metadata Definitions
4. Feature File Template
5. Detailed Example: Todo App with Calendar
6. Best Practices & Guidelines
7. Tagging Strategy
8. Tag Variations List
9. Extended Technical Tag Variations
10. Versioning & Change Management
11. Publishing & Distribution

---

## 3. Purpose & Audience
- **Purpose:** Standardize how features are specified, incorporating UI, API, and DB contexts in one place.
- **Audience:** Product Owners, QA Engineers, Developers, DevOps, and AI Integration Specialists.

## 4. Layered BDD Conventions
- **Background Sections:** Separate `@UI`, `@API`, and `@DB` backgrounds at the top of each feature.
- **Scenario Focus:** Each scenario remains high-level, referencing shared contexts.
- **Tagging:** Use tags to denote metadata and enable targeted test runs (`@smoke`, `@regression`).

## 5. Metadata Definitions
### UI (@UI Background)
- Components: Name, key selectors, and behavior expectations.
- Views: Page or module names, navigation steps.

### API (@API Background)
- Endpoints: HTTP method, URL path, expected request/response schemas.
- Error Cases: Standardized error payloads and status codes.

### DB (@DB Background)
- Tables / Models: Column definitions, types, constraints.
- Views & Relations: Any computed views or foreign-key relationships.

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

## 8. Best Practices & Guidelines
- Keep Backgrounds DRY: Define shared contexts once per feature.
- Scenario Clarity: Scenarios should read like user stories without implementation details.
- Tag Strategically: Use scenario tags to group and filter tests.
- Review & Evolve: Periodically review feature files to remove outdated contexts.

## 9. Tagging Strategy

### 9.1 Functional Tags
- `@authentication` - Authentication related features
- `@authorization` - Authorization and permissions
- `@user-management` - User management features
- `@todo` - Core todo management
- `@calendar` - Calendar-specific behaviors
- `@notification` - Notification system
- `@integration` - Third-party integrations
- `@reporting` - Reporting and analytics
- `@billing` - Billing and payments

### 9.2 Technical Tags
- `@UI` - User interface tests
- `@API` - API endpoint tests
- `@DB` - Database tests
- `@E2E` - End-to-end tests
- `@unit` - Unit/component tests
- `@performance` - Performance tests
- `@security` - Security tests

### 9.3 Quality & Environment Tags
- `@smoke` - Smoke tests
- `@regression` - Regression tests
- `@critical` - Critical path tests
- `@slow` - Long-running tests
- `@accessibility` - Accessibility tests
- `@cross-browser` - Cross-browser compatibility tests
- `@mobile` - Mobile/responsive tests

## 10. Tag Variations List

### 10.1 Functional Tag Variations
| Concept                | Primary Tag             | Aliases / Variations                |
|------------------------|-------------------------|-------------------------------------|
| Authentication         | `@authentication`       | `@auth`, `@login`, `@sign-in`       |
| Authorization          | `@authorization`        | `@authz`, `@permissions`            |
| User Management        | `@user-management`      | `@users`, `@user-mgmt`, `@accounts` |
| Todo Features          | `@todo`                 | `@tasks`, `@todo-list`              |
| Calendar               | `@calendar`             | `@date-management`, `@schedule`     |
| Notifications          | `@notification`         | `@notify`, `@alerts`                |
| Integration            | `@integration`          | `@3rd-party`, `@external-services`  |
| Reporting              | `@reporting`            | `@reports`, `@analytics`            |
| Billing / Payments     | `@billing`              | `@payments`, `@invoicing`           |

### 10.2 Technical Tag Variations
| Layer / Type         | Primary Tag   | Aliases / Variations             |
|----------------------|---------------|----------------------------------|
| UI / Frontend        | `@UI`         | `@ui`, `@frontend`, `@ux`        |
| API / Backend        | `@API`        | `@api`, `@backend`, `@services`  |
| Database             | `@DB`         | `@db`, `@database`, `@data`      |
| End-to-End           | `@E2E`        | `@e2e`, `@end-to-end`, `@full`   |
| Unit Tests           | `@unit`       | `@unit-test`, `@component`       |
| Integration          | `@integration`| `@int`, `@contract`              |
| Smoke Tests          | `@smoke`      | `@sanity`, `@quick`              |

## 11. Extended Technical Tag Variations

| Layer       | Concept        | Primary Tag         | Aliases / Variations                |
|-------------|----------------|---------------------|-------------------------------------|
| **UI**      | Component      | `@UI-component`     | `@component`, `@cmp`                |
|             | Page           | `@UI-page`          | `@page`, `@screen`                  |
|             | Layout         | `@UI-layout`        | `@layout`, `@template`              |
|             | Widget         | `@UI-widget`        | `@widget`, `@module`                |
| **API**     | REST           | `@API-rest`         | `@rest`, `@http`                    |
|             | GraphQL        | `@API-graphql`      | `@graphql`                          |
|             | WebSocket      | `@API-websocket`    | `@ws`, `@socket`                    |
|             | gRPC           | `@API-grpc`         | `@grpc`                             |
| **DB**      | Table          | `@DB-table`         | `@table`, `@tbl`                    |
|             | View           | `@DB-view`          | `@view`, `@vw`                      |
|             | Index          | `@DB-index`         | `@index`, `@idx`                    |
|             | Stored Proc    | `@DB-procedure`     | `@procedure`, `@proc`               |
|             | Function       | `@DB-function`      | `@function`, `@fn`                  |
|             | Trigger        | `@DB-trigger`       | `@trigger`, `@trg`                  |

## 12. Versioning & Change Management
- **Semantic Versioning:** Maintain a `version` header in the document.
- **Change Log:** Append a change log section for updates.
- **Peer Review:** Require sign-off from at least two stakeholders for spec changes.

## 13. Publishing & Distribution
- **Repository:** Store under `/docs/bdd-specification.md`.
- **Access Control:** Read-only for general teams, write access for core architects.
- **Communication:** Notify teams via Slack/email on major updates.

*End of Document*
