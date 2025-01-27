# Notion Event Tracking Plan & Implementation Guide

## Overview
Notion is a collaborative workspace platform that combines documents, databases, and project management tools. It enables teams to create, organize, and share content through customizable pages and databases.

## Implementation Context
As detailed in Event Data DesignChapter of the Analytics Implementation Workbook, this tracking plan follows the Double Three Layer Framework (D3L).

## References:
My book, I used with Claude AI to create that tracking plan: https://link.timodechau.com/evAz2u
Blog post describing the design: 
YouTube video showing how I created this design: 



## Implementation Overview

As detailed in Event Data Design chapter of the [Analytics Implementation Workbook](https://timodechau.com/the-analytics-implementation-workbook/), this tracking plan follows the Double 3 Layer Framework, focusing on product, customer and interaction events by using entity, activity and property as design patterns.

### Data Layer Structure

```javascript
window.notionDataLayer = window.notionDataLayer || [];
window.notionDataLayer.push({
    'event': 'event_name',
    'properties': {
        // event properties
    }
});
```

## Events Documentation

### Workspace Events

#### workspace_created
- **Description**: Fired when a new workspace is created
- **Trigger**: User completes workspace creation flow
- **Source**: Server-side
- **Implementation**: Required
- **Used by**: Workspace creation rate, activation metrics
- **Data Layer Example**:
```javascript
notionDataLayer.push({
    'event': 'workspace_created',
    'properties': {
        'workspace_id': 'ws_12345',
        'workspace_name': 'Marketing Team',
        'workspace_type': 'team',
        'workspace_members_count': 1,
        'workspace_pages_count': 0,
        'workspace_created_at': '2025-01-24T10:30:00Z'
    }
});
```

#### workspace_updated
- **Description**: Fired when workspace settings are modified
- **Trigger**: Calculated from multiple setting changes
- **Source**: Calculated
- **Implementation**: Required
- **Used by**: Workspace engagement metrics
- **Data Layer Example**:
```javascript
notionDataLayer.push({
    'event': 'workspace_updated',
    'properties': {
        'workspace_id': 'ws_12345',
        'workspace_name': 'Marketing Team 2.0',
        'workspace_type': 'team',
        'workspace_members_count': 5,
        'workspace_pages_count': 25
    }
});
```

#### workspace_shared
- **Description**: Fired when workspace sharing settings change
- **Trigger**: Share settings modified
- **Source**: Server-side
- **Implementation**: Required
- **Used by**: Collaboration metrics
- **Data Layer Example**:
```javascript
notionDataLayer.push({
    'event': 'workspace_shared',
    'properties': {
        'workspace_id': 'ws_12345',
        'workspace_type': 'team',
        'workspace_members_count': 6
    }
});
```

### Page Events

#### page_created
- **Description**: Fired when a new page is created
- **Trigger**: Page creation completed
- **Source**: Server-side
- **Implementation**: Required
- **Used by**: Page creation rate, content engagement metrics
- **Data Layer Example**:
```javascript
notionDataLayer.push({
    'event': 'page_created',
    'properties': {
        'page_id': 'pg_12345',
        'page_workspace_id': 'ws_12345',
        'page_type': 'doc',
        'page_blocks_count': 0,
        'page_shared_count': 0,
        'page_copied_from_page': false
    }
});
```

#### page_updated
- **Description**: Fired when page content changes
- **Trigger**: Calculated from block changes
- **Source**: Calculated
- **Implementation**: Required
- **Used by**: Content engagement metrics
- **Data Layer Example**:
```javascript
notionDataLayer.push({
    'event': 'page_updated',
    'properties': {
        'page_id': 'pg_12345',
        'page_blocks_count': 15,
        'page_last_modified': '2025-01-24T14:30:00Z'
    }
});
```

### Subscription Events

#### subscription_created
- **Description**: Fired when new subscription is created
- **Trigger**: First successful payment
- **Source**: Server-side
- **Implementation**: Required
- **Used by**: Revenue metrics, conversion rate
- **Data Layer Example**:
```javascript
notionDataLayer.push({
    'event': 'subscription_created',
    'properties': {
        'subscription_id': 'sub_12345',
        'subscription_plan': 'team',
        'subscription_status': 'active',
        'subscription_billing_period': 'monthly',
        'subscription_amount': 15.00
    }
});
```

## Properties Documentation

### Workspace Properties

| Property Name | Type | Example | Description |
|--------------|------|---------|-------------|
| workspace_id | string | 'ws_12345' | Unique workspace identifier |
| workspace_name | string | 'Marketing Team' | Display name of workspace |
| workspace_type | enum | 'team' | Either 'personal' or 'team' |
| workspace_members_count | integer | 5 | Current member count |
| workspace_pages_count | integer | 25 | Total pages in workspace |
| workspace_created_at | timestamp | '2025-01-24T10:30:00Z' | Creation timestamp |

### Page Properties

| Property Name | Type | Example | Description |
|--------------|------|---------|-------------|
| page_id | string | 'pg_12345' | Unique page identifier |
| page_workspace_id | string | 'ws_12345' | Parent workspace ID |
| page_type | enum | 'doc' | Either 'doc' or 'database' |
| page_blocks_count | integer | 15 | Number of content blocks |
| page_shared_count | integer | 3 | Times shared with others |
| page_last_modified | timestamp | '2025-01-24T14:30:00Z' | Last update timestamp |
| page_copied_from_page | boolean | false | Whether page is a copy |

### Subscription Properties

| Property Name | Type | Example | Description |
|--------------|------|---------|-------------|
| subscription_id | string | 'sub_12345' | Unique subscription ID |
| subscription_plan | enum | 'team' | Plan type identifier |
| subscription_status | enum | 'active' | Current status |
| subscription_start_date | timestamp | '2025-01-24T00:00:00Z' | Start date |
| subscription_end_date | timestamp | '2025-02-24T00:00:00Z' | End date |
| subscription_billing_period | enum | 'monthly' | Billing frequency |
| subscription_amount | decimal | 15.00 | Plan cost |

## Implementation Guidelines

1. Always include required properties for each event
2. Use ISO 8601 format for all timestamps
3. Validate property types before pushing to data layer
4. Include error handling for failed event pushes
5. Test events in development environment first

