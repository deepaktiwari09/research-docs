
# Extended BDD Template for APIs with Tags

## API Overview

**Purpose**: <High-level purpose of the API or module>  
**Base URL**: `/api/<resource>`  
**Authentication**: <Auth Type: JWT, OAuth2, API Key>  
**Content Type**: `application/json`

### Tags for Overview
`@API @resource:<resource> @status:<level>`

---

## API Endpoints Specification

### Endpoint Group: <Description of group>

#### <HTTP Method> /api/<resource>/<path>

**Purpose**: <Brief explanation of endpoint>  
**Authentication**: <Roles allowed>  

**Request Schema**:
```typescript
interface <RequestInterface> {
  // Define request body schema
}
```

**Response Schema**:
```typescript
interface <ResponseInterface> {
  // Define response body schema
}
```

**Tags for Endpoint**:
`@API @endpoint:<resource>-<operation> @action:<HTTP-method> @status:<level> @type:<aspect>`

**API Behaviors**:
```gherkin
Feature: <Feature Description>

  @API @endpoint:<resource>-<operation> @action:<HTTP-method> @type:happy-path @status:critical
  Scenario: <Describe valid use case>
    Given <preconditions>
    When I send <HTTP METHOD> /api/<path> with:
      """
      {
        <requestPayload>
      }
      """
    Then I should receive status code <code>
    And <expected outcomes>

  @API @endpoint:<resource>-<operation> @action:<HTTP-method> @type:error-case @status:critical
  Scenario: <Describe edge/error case>
    Given <invalid state or condition>
    When I send <HTTP METHOD> /api/<path> with:
      """
      {
        <invalidPayload>
      }
      """
    Then I should receive status code <error-code>
    And the response should contain <error details>
```

---

### Another Endpoint: <HTTP Method> /api/<resource>/<sub-path>

**Purpose**: <Brief explanation>  
**Authentication**: <Roles allowed>  

**Tags**:
`@API @endpoint:<resource>-<operation> @action:<HTTP-method> @status:<level> @type:<aspect>`

```gherkin
Feature: <Feature Name>

  @API @endpoint:<resource>-<operation> @action:<HTTP-method> @type:typical @status:optional
  Scenario: <Typical case>
    Given <some condition>
    When <action>
    Then <expected result>
```

---

## Error Handling and Status Codes

### Tags:
`@API @type:error-handling @status:critical`

```gherkin
Feature: Standard HTTP Status Codes

  @API @type:error-handling @action:read @status:critical
  Scenario: Handle unauthorized access
    When I send a request without valid token
    Then I should receive 401 Unauthorized

  @API @type:error-handling @action:create @status:critical
  Scenario: Handle validation errors
    When I send invalid data
    Then I should receive 422 Unprocessable Entity
```

---

## Performance Requirements

### Tags:
`@API @type:performance @status:optional`

```gherkin
Feature: API Performance

  @API @type:performance @action:read @status:optional
  Scenario: GET request responds within <ms>
    When I send GET /api/<resource>
    Then response time should be < <ms>
```

---

## Security Requirements

### Tags:
`@API @type:security @status:critical`

```gherkin
Feature: Security and Access Control

  @API @type:security @action:read @status:critical
  Scenario: Validate JWT authentication
    When I send a request with expired token
    Then I should receive 401 Unauthorized

  @API @type:security @action:grant @status:critical
  Scenario: Admin accesses protected endpoint
    Given I am authenticated as admin
    When I send GET /api/<resource>/admin
    Then I should receive status code 200
```

---

## Dependency and Integration

### Tags:
`@API @type:integration @status:optional`

```gherkin
Feature: External Service Integration

  @API @type:integration @action:create @status:optional
  Scenario: Service calls external API
    Given external service is available
    When I perform <operation>
    Then integration should succeed without errors
```

---

## Template Usage Notes

- Use `@endpoint:<resource>-<operation>` to uniquely identify endpoint scenarios.
- Apply `@action:` tags for HTTP methods to filter by operation type.
- Tag scenarios with `@type:` for aspect-focused grouping (e.g. `happy-path`, `error-case`, `performance`).
- Use `@status:` to prioritize scenario execution.
- Combine tags to facilitate targeted test runs (e.g., smoke tests: `@type:happy-path @status:critical`).

This template provides a structured approach to documenting API behaviors using BDD methodology with comprehensive tagging for test organization and execution.

check BDD document having feature files containing @api specifications, create feature folder IF NOT PRESENT with that feature name and create api  BDD specification for each component, 1 file per Dapi using this template
