# Asana Event Tracking Documentation

## Introduction

This document provides comprehensive documentation for Asana's event tracking implementation. It covers all entities, activities, and properties, and includes implementation details using a data layer approach.

## Data Layer Implementation

All events should be pushed to a central data layer object before being sent to analytics destinations. Use this structure:

```javascript
window.asanaDataLayer = window.asanaDataLayer || [];
window.asanaDataLayer.push({
    'event': 'event_name',
    'properties': {
        // event properties
    }
});
```

## Events Documentation

### Organization Events

#### Organization Created
* **Description**: Triggered when a new organization is created in Asana
* **Trigger**: Organization setup completion
* **Source**: Backend API
* **Implementation**: Server-side tracking
* **Used by Metrics**: 
  - Organization growth rate
  - Acquisition funnel conversion
  - Monthly new organizations
* **Implementation Example**:
```javascript
asanaDataLayer.push({
    'event': 'organization_created',
    'properties': {
        'organization_id': '12345',
        'organization_name': 'Acme Corp',
        'organization_size_group': '51-200'
    }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| organization_id | string | "12345" | Unique identifier for organization |
| organization_name | string | "Acme Corp" | Organization display name |
| organization_domain | string | "acme.com" | Primary email domain |
| organization_size_group | string | "51-200" | Size bracket of organization |
| organization_plan_type | string | "business" | Subscription plan type |
| organization_subscription_status | string | "active" | Current subscription status |

#### Organization Deleted
* **Description**: Triggered when an organization is deleted
* **Trigger**: Organization deletion confirmation
* **Source**: Backend API
* **Implementation**: Server-side tracking
* **Used by Metrics**:
  - Churn rate
  - Organization lifetime value
  - Account health metrics
* **Implementation Example**:
```javascript
asanaDataLayer.push({
    'event': 'organization_deleted',
    'properties': {
        'organization_id': '12345',
        'deletion_reason': 'migration_to_competitor'
    }
});
```

### Subscription Events

#### Subscription Created
* **Description**: New subscription is initiated
* **Trigger**: Subscription purchase completion
* **Source**: Backend API/Billing System
* **Implementation**: Server-side tracking
* **Used by Metrics**:
  - New MRR
  - Conversion rate
  - Plan distribution
* **Implementation Example**:
```javascript
asanaDataLayer.push({
    'event': 'subscription_created',
    'properties': {
        'subscription_id': 'sub_12345',
        'plan_type': 'business',
        'billing_period': 'annual'
    }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| subscription_id | string | "sub_12345" | Unique subscription identifier |
| subscription_plan_type | string | "business" | Plan level |
| subscription_billing_period | string | "annual" | Billing frequency |
| subscription_user_count | number | 50 | Licensed user count |
| subscription_start_date | date | "2024-01-30" | Start of subscription |
| subscription_end_date | date | "2025-01-29" | End of subscription |
| subscription_auto_renew | boolean | true | Auto-renewal status |

### Team Events

#### Team Created
* **Description**: New team created within organization
* **Trigger**: Team creation completion
* **Source**: Backend API
* **Implementation**: Server-side tracking
* **Used by Metrics**:
  - Team adoption rate
  - Organization expansion
  - Collaboration metrics
* **Implementation Example**:
```javascript
asanaDataLayer.push({
    'event': 'team_created',
    'properties': {
        'team_id': 'team_12345',
        'team_name': 'Marketing',
        'team_privacy_setting': 'private'
    }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| team_id | string | "team_12345" | Unique team identifier |
| team_name | string | "Marketing" | Team display name |
| team_size | number | 8 | Current team member count |
| team_privacy_setting | string | "private" | Team access level |
| team_has_goals | boolean | true | Has active goals |
| team_has_workflows | boolean | true | Has active workflows |

### Project Events

#### Project Created
* **Description**: New project initialized
* **Trigger**: Project creation completion
* **Source**: Frontend/Backend API
* **Implementation**: Hybrid (client & server)
* **Used by Metrics**:
  - Project creation rate
  - Template usage rate
  - Team activity metrics
* **Implementation Example**:
```javascript
asanaDataLayer.push({
    'event': 'project_created',
    'properties': {
        'project_id': 'proj_12345',
        'project_name': 'Q1 Campaign',
        'project_type': 'board',
        'project_template_id': 'templ_789'
    }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| project_id | string | "proj_12345" | Unique project identifier |
| project_name | string | "Q1 Campaign" | Project display name |
| project_type | string | "board" | Project view type |
| project_template_id | string | "templ_789" | Template used (if any) |
| project_privacy_setting | string | "public" | Project access level |
| project_goal_count | number | 3 | Linked goals count |
| project_workflow_count | number | 2 | Active workflows count |
| project_team_id | string | "team_12345" | Parent team ID |

### Task Events

#### Task Created
* **Description**: New task created in project
* **Trigger**: Task creation completion
* **Source**: Frontend/Backend API/Workflow
* **Implementation**: Hybrid
* **Used by Metrics**:
  - Task creation rate
  - Workflow automation metrics
  - User engagement
* **Implementation Example**:
```javascript
asanaDataLayer.push({
    'event': 'task_created',
    'properties': {
        'task_id': 'task_12345',
        'task_name': 'Write blog post',
        'task_type': 'standard',
        'task_created_by': 'workflow_789'
    }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| task_id | string | "task_12345" | Unique task identifier |
| task_name | string | "Write blog post" | Task display name |
| task_status | string | "in_progress" | Current task status |
| task_priority | string | "high" | Task priority level |
| task_due_date | date | "2024-02-15" | Task deadline |
| task_type | string | "standard" | Task type (standard/subtask) |
| task_parent_id | string | "task_789" | Parent task ID if subtask |
| task_created_by | string | "user_123" | Creator (user/workflow) ID |
| task_project_id | string | "proj_456" | Parent project ID |

### Workflow Events

#### Workflow Step Executed
* **Description**: Individual workflow step completed
* **Trigger**: Step execution completion
* **Source**: Backend API
* **Implementation**: Server-side
* **Used by Metrics**:
  - Workflow usage rate
  - Automation effectiveness
  - Time savings
* **Implementation Example**:
```javascript
asanaDataLayer.push({
    'event': 'workflow_step_executed',
    'properties': {
        'workflow_id': 'flow_12345',
        'step_id': 'step_789',
        'workflow_type': 'content_approval',
        'has_ai': true
    }
});