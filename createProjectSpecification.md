# Enterprise-Grade Extended BDD Specification with Database-UI Mapping

## 1. Overview
This document provides an **enterprise-grade** specification for writing **extended BDD feature files** using the **Layered Backgrounds** approach with integrated **Database-to-UI Component Mapping**. It ensures consistency, clarity, and reusability across large teams and projects, enabling both human stakeholders and AI-driven pipelines to generate and maintain full-stack code effectively with clear data visualization strategies.

## 2. Document Structure
1. Purpose & Audience  
2. Layered BDD Conventions  
3. Metadata Definitions  
4. **Database-UI Mapping Specification**
5. Feature File Template  
6. Detailed Example: Todo App with Calendar  
7. Best Practices & Guidelines  
8. Tagging Strategy  
9. Tag Variations List  
10. Extended Technical Tag Variations  
11. Versioning & Change Management  
12. Publishing & Distribution  

---

## 3. Purpose & Audience
- **Purpose:** Standardize how features are specified, incorporating UI, API, DB contexts, and explicit data-UI relationships in one place.  
- **Audience:** Product Owners, QA Engineers, Developers, DevOps, UI/UX Designers, and AI Integration Specialists.

## 4. Layered BDD Conventions
- **Background Sections:** Separate `@UI`, `@API`, `@DB`, and `@DataMapping` backgrounds at the top of each feature.  
- **Scenario Focus:** Each scenario remains high-level, referencing shared contexts.  
- **Tagging:** Use tags to denote metadata and enable targeted test runs (`@smoke`, `@regression`).
- **Data Flow:** Explicit mapping between database columns and UI components for clarity.

## 5. Metadata Definitions
### UI (@UI Background)
- Components: Name, key selectors, and behavior expectations.  
- Views: Page or module names, navigation steps.
- Data Bindings: References to mapped database fields.

### API (@API Background)
- Endpoints: HTTP method, URL path, expected request/response schemas.  
- Error Cases: Standardized error payloads and status codes.
- Data Transformations: How API responses map to UI models.

### DB (@DB Background)
- Tables / Models: Column definitions, types, constraints.  
- Views & Relations: Any computed views or foreign-key relationships.
- Indexes: Performance-critical indexes for UI queries.

### Data Mapping (@DataMapping Background)
- Column-to-Component: Direct mapping of database fields to UI elements.
- Aggregations: Calculated fields and their UI representations.
- Relationships: How related data is combined in the UI.

## 6. Database-UI Mapping Specification

### 6.1 Mapping Structure
```gherkin
@DataMapping
Background: Database to UI Component Mapping
  Given the following database-to-UI mappings exist:
    | Table.Column              | UI Component          | Display Format      | Aggregation/Logic                |
    | users.first_name         | UserAvatar            | Initials           | CONCAT(first_letter, last_letter) |
    | messages.created_at      | MessageTimestamp      | Relative time      | time_ago_format()                |
    | conversations.type       | ConversationIcon      | Icon/Avatar        | IF(group, Users, UserInitials)   |
```

### 6.2 Mapping Categories

#### Direct Mappings
Simple one-to-one relationships between database columns and UI elements:
```gherkin
| Table.Column     | UI Component    | Display Format |
| users.email      | ProfileEmail    | Text          |
| users.avatar_url | ProfilePicture  | Image         |
| posts.title      | PostTitle       | Heading       |
```

#### Computed Mappings
Database values that require transformation for UI display:
```gherkin
| Table.Column          | UI Component      | Computation                    |
| orders.total_amount   | OrderTotal        | FORMAT_CURRENCY(amount, 'USD') |
| users.created_at      | MemberSince       | DATE_DIFF(NOW(), created_at)   |
| products.price        | PriceDisplay      | WITH_DISCOUNT(price, discount) |
```

#### Aggregated Mappings
Multiple database values combined into single UI elements:
```gherkin
| Tables/Columns                    | UI Component        | Aggregation                           |
| messages.* WHERE unread=true      | UnreadBadge        | COUNT(*)                              |
| order_items.* GROUP BY order_id   | OrderSummary       | SUM(quantity), SUM(price * quantity)  |
| users.* JOIN posts ON user_id     | UserActivityStats  | COUNT(posts), MAX(posts.created_at)   |
```

#### Relational Mappings
Data from related tables displayed together:
```gherkin
| Related Data                      | UI Component        | Join/Lookup                           |
| conversations + last_message      | ChatPreview        | LEFT JOIN messages ORDER BY date DESC |
| products + categories            | ProductCard        | INNER JOIN categories ON category_id  |
| users + roles + permissions      | UserPermissions    | JOIN through user_roles table         |
```

### 6.3 State-Based Mappings
UI components that change based on data state:
```gherkin
| Condition                        | UI Component State   | Visual Change                    |
| messages.read_status = false     | MessageBubble       | Bold font, blue dot indicator    |
| users.online_status = true       | UserAvatar          | Green status circle              |
| tasks.due_date < NOW()          | TaskCard            | Red border, overdue badge        |
```

## 7. Feature File Template with Data Mapping
```gherkin
Feature: <Feature Title>

  @UI
  Background: UI Setup
    Given the following UI components exist:
      | Component    | Selector          | Description       | Data Source    |
      | ExampleComp  | .example-selector | Widget container  | example_table  |

  @API
  Background: API Setup
    Given these API endpoints are defined:
      | Method | Endpoint           | Response Maps To      |
      | GET    | /example           | ExampleComp data      |

  @DB
  Background: DB Setup
    Given the database contains tables:
      | Table       | Columns                      | Indexes           |
      | example_tbl | id, name, created_at         | idx_created_at    |

  @DataMapping
  Background: Data to UI Mapping
    Given the following data mappings:
      | Source                 | Target Component | Transform          |
      | example_tbl.name       | ExampleComp.title| UPPERCASE()        |
      | example_tbl.created_at | ExampleComp.date | FORMAT_DATE()      |

  Scenario: <Scenario Title>
    Given ...
    When ...
    Then ...
```

## 8. Detailed Example: Chat Application with Data Mapping
```gherkin
Feature: Professional Chat and Messaging System

  @UI
  Background: UI Setup
    Given the chat interface includes:
      | Component           | Selector             | Description                    | Primary Data     |
      | ChatList            | .chat-list          | List of conversations          | conversations    |
      | MessageBubble       | .message-bubble     | Individual message display     | messages         |
      | UnreadBadge         | .unread-badge       | Unread count indicator         | message_stats    |
      | OnlineIndicator     | .online-status      | User availability             | user_presence    |

  @API
  Background: API Setup
    Given the following chat endpoints:
      | Method | Endpoint                    | Returns                      |
      | GET    | /chat/conversations         | Conversations with metadata  |
      | POST   | /chat/messages              | Message creation response    |
      | GET    | /chat/messages/{id}/status  | Read receipts data          |

  @DB
  Background: DB Setup
    Given the chat database contains:
      | Table               | Key Columns                                          |
      | conversations       | id, type, title, last_message_at, created_by        |
      | messages            | id, conversation_id, sender_id, content, created_at |
      | message_read_status | message_id, user_id, read_at                        |
      | users               | id, first_name, last_name, avatar_url, online       |

  @DataMapping
  Background: Database to UI Mapping
    Given these data-to-UI mappings exist:
      # Direct Mappings
      | Table.Column              | UI Component              | Display Format        |
      | conversations.title       | ChatList.conversationName | Text                  |
      | messages.content          | MessageBubble.text        | Text with emoji       |
      | users.avatar_url          | UserAvatar.image          | Circle image          |
      
      # Computed Mappings
      | Table.Column                    | UI Component           | Computation                      |
      | messages.created_at             | MessageBubble.time     | time_ago(created_at)             |
      | conversations.last_message_at   | ChatList.timestamp     | format_chat_time(last_message_at)|
      | users.first_name + last_name    | UserAvatar.initials    | first_char(f) + first_char(l)    |
      
      # Aggregated Mappings
      | Query                                              | UI Component        | Display                |
      | COUNT(*) FROM messages WHERE read_at IS NULL      | UnreadBadge.count   | Numeric badge          |
      | SELECT * FROM messages ORDER BY created_at DESC    | ChatWindow.messages | Scrollable list        |
      | COUNT(*) FROM conversation_members WHERE online=1  | GroupInfo.online    | "3 members online"     |
      
      # Relational Mappings
      | Join Query                                         | UI Component           | Display Format         |
      | conversations JOIN users ON last_sender_id         | ChatList.preview       | "John: Last message"   |
      | messages JOIN message_read_status ON message_id    | MessageBubble.status   | ✓✓ (blue if read)      |
      | conversations JOIN conversation_members            | ChatList.avatarStack   | Overlapping avatars    |

  @smoke @critical
  Scenario: Display unread message count in conversation list
    Given I have 3 unread messages in "Project Team" conversation
    When I view my chat list
    Then the UnreadBadge should display "3"
    And the badge should use COUNT query on messages table
    And the conversation should be sorted by last_message_at DESC
```

## 9. Data Mapping Best Practices

### 9.1 Performance Considerations
- **Index Requirements:** Specify indexes needed for UI queries
- **Eager Loading:** Define when to use JOINs vs separate queries
- **Caching Strategy:** Mark which mappings benefit from caching

### 9.2 Data Transformation Guidelines
- **Client vs Server:** Specify where transformations occur
- **Formatting Rules:** Consistent date, number, currency formatting
- **Null Handling:** Default values for missing data

### 9.3 Real-time Updates
- **WebSocket Events:** Map database changes to UI updates
- **Optimistic Updates:** Define UI behavior before server confirmation
- **Conflict Resolution:** Handle concurrent data modifications

## 10. Mapping Documentation Template
```markdown
### Component: [Component Name]
**Purpose:** [Brief description]
**Data Sources:**
- Primary: [table.column]
- Secondary: [related tables]

**Mappings:**
| Database Field | UI Property | Transform | Update Frequency |
|----------------|-------------|-----------|------------------|
| table.column   | property    | function  | real-time/poll   |

**State Conditions:**
- If [condition], then [UI change]
- When [event], update [property]
```

## 11. Extended Tagging for Data Mappings

### 11.1 Data Flow Tags
- `@data-direct` - Simple column-to-component mapping
- `@data-computed` - Requires transformation
- `@data-aggregated` - Uses GROUP BY or COUNT
- `@data-joined` - Requires table joins
- `@data-realtime` - Updates via websocket
- `@data-cached` - Benefits from caching

### 11.2 Performance Tags
- `@indexed-query` - Requires database index
- `@heavy-computation` - Complex calculations
- `@pagination-required` - Large dataset
- `@lazy-load` - Load on demand

## 12. Versioning & Change Management
- **Schema Version:** Track database schema versions
- **Mapping Version:** Version the mapping specifications
- **Migration Path:** Document how mappings change with schema updates

## 13. Publishing & Distribution
- **Repository:** Store under `/docs/bdd-specification-with-mapping.md`
- **Schema Sync:** Keep synchronized with database migrations
- **UI Library:** Generate component props from mappings

*End of Document*
