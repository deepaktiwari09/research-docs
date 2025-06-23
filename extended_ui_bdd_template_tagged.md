# Extended BDD Template for UI Components with Tags

## Component Overview

**Component Name**: `<ComponentName>`  
**CSS Selector**: `<.component-selector>`  
**Location**: `<Route or Path>`  
**Purpose**: `<Brief description of component function>`

### Tags for Component
`@UI @component:<ComponentName> @status:<level>`

---

## Component Structure

```typescript
interface <ComponentName>Props {
  // Add component props here
}

interface <KeyEntity> {
  // Describe core data model related to the component
}
```

---

## UI Elements & Layout

### <Primary View or Mode>
- **<Element Name>**: <Short description>
- **<Element Name>**: <Short description>

### <Secondary View or Mode>
- **<Element Name>**: <Short description>
- **<Element Name>**: <Short description>

---

## Component Behaviors

### <Feature Block 1>: <Title>
```gherkin
Feature: <Feature Block 1 Title>

  @UI @component:<ComponentName> @feature:<feature-tag> @type:happy-path @action:use @status:critical
  Scenario: <Positive user journey>
    Given I am on the <page or section>
    When I <perform some action>
    Then I should see <expected outcome>
    And I should be able to <follow-up behavior>

  @UI @component:<ComponentName> @feature:<feature-tag> @type:error-case @action:use @status:critical
  Scenario: <Alternative or negative case>
    Given <some state>
    When I <attempt invalid or edge action>
    Then I should see <error/warning>
```

---

### <Feature Block 2>: <Title>
```gherkin
Feature: <Feature Block 2 Title>

  @UI @component:<ComponentName> @feature:<feature-tag> @type:typical @action:use @status:optional
  Scenario: <Typical case>
    Given <setup>
    When <user interaction>
    Then <expected results>
    And <additional results>
```

---

## Error Handling

### Auto-save and Recovery
```gherkin
Feature: Auto-save and Recovery

  @UI @component:<ComponentName> @type:auto-save @action:background-save @status:critical
  Scenario: Auto-save functionality
    Given I am editing <entity>
    When I make changes
    Then I should see an "Auto-saved" message
    And data should be saved in the background

  @UI @component:<ComponentName> @type:recovery @action:reload @status:optional
  Scenario: Recovery after crash
    Given I had unsaved changes
    When I reload the app
    Then I should see a recovery message
    And unsaved changes should be restored
```

### Validation and Error Handling
```gherkin
Feature: Validation and Error Handling

  @UI @component:<ComponentName> @type:validation @action:submit @status:critical
  Scenario: Validate required fields
    Given I am creating a new <entity>
    When I try to save without entering mandatory data
    Then I should see validation error messages
    And the save button should be disabled
```

---

## Performance Requirements

### Tags:
`@UI @component:<ComponentName> @type:performance @status:optional`

```gherkin
Feature: Performance Monitoring

  @UI @component:<ComponentName> @type:performance @action:load @status:optional
  Scenario: Load performance
    When I open the <ComponentName>
    Then it should load within <x> seconds
```

---

## Accessibility Requirements

### Tags:
`@UI @component:<ComponentName> @type:accessibility @status:critical`

```gherkin
Feature: Accessibility Compliance

  @UI @component:<ComponentName> @type:accessibility @action:navigation @status:critical
  Scenario: Keyboard navigation support
    When I navigate using keyboard
    Then I should reach all interactive elements
```

---

## Integration Requirements

### Tags:
`@UI @component:<ComponentName> @type:integration @status:optional`

```gherkin
Feature: API Integration

  @UI @component:<ComponentName> @type:integration @action:fetch @status:optional
  Scenario: Fetch data from API
    When the component mounts
    Then it should call GET /api/<resource>
    And display data correctly
```

---

## Dependencies

### Tags:
`@UI @component:<ComponentName> @type:dependency @status:optional`

- Required Services: `use<HookName>`
- Sub-Components: `<SubComponentName>`

---

## Tag Reference Guide

### Component Tags
- `@UI` - Marks UI-related scenarios
- `@component:<name>` - Groups scenarios by component
- `@feature:<name>` - Groups scenarios by feature area

### Type Tags
- `@type:happy-path` - Successful user journeys
- `@type:error-case` - Error and edge cases
- `@type:validation` - Input validation scenarios
- `@type:performance` - Performance-related tests
- `@type:accessibility` - Accessibility compliance
- `@type:integration` - API/service integration
- `@type:auto-save` - Auto-save functionality
- `@type:recovery` - Recovery and resilience

### Action Tags
- `@action:create` - Creation workflows
- `@action:edit` - Editing workflows
- `@action:use` - General usage
- `@action:load` - Loading operations
- `@action:submit` - Form submissions
- `@action:navigation` - Navigation actions
- `@action:fetch` - Data fetching

### Status Tags
- `@status:critical` - Must-pass scenarios
- `@status:optional` - Nice-to-have scenarios
- `@status:blocked` - Currently blocked scenarios

---

## Template Usage Notes

1. **Component Identification**: Use `@component:<ComponentName>` to group scenarios by component
2. **Feature Grouping**: Tag scenarios with `@feature:` to identify feature areas
3. **Scenario Classification**: Use `@type:` to classify scenario type (happy-path, error-case, validation, etc.)
4. **Action Identification**: Use `@action:` for the primary action (create, edit, use, load, etc.)
5. **Test Prioritization**: Use `@status:` to prioritize scenarios in test runs
6. **Flexible Tagging**: Combine multiple tags for precise test selection
7. **Test Execution**: Use tags to run specific subsets of tests (e.g., `@status:critical`)

---

## Example Usage

### Login Component Example
```gherkin
Feature: User Login

  @UI @component:LoginForm @feature:authentication @type:happy-path @action:login @status:critical
  Scenario: Successful login with valid credentials
    Given I am on the login page
    When I enter valid email and password
    Then I should be redirected to dashboard
    And I should see a welcome message

  @UI @component:LoginForm @feature:authentication @type:error-case @action:login @status:critical
  Scenario: Login with invalid credentials
    Given I am on the login page
    When I enter invalid email or password
    Then I should see an error message
    And I should remain on the login page
```

This template provides a structured approach to documenting UI component behaviors using BDD methodology with comprehensive tagging for test organization and execution.

check BDD document having feature files containing UI components specifications, create feature folder with that feature name and create UI component BDD specification for each component, 1 file per component using this template
