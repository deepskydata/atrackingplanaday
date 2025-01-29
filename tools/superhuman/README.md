# Slack Event Tracking Documentation & Implementation Guide

## Overview


## Implementation Context
As detailed in Event Data DesignChapter of the Analytics Implementation Workbook, this tracking plan follows the Double Three Layer Framework (D3L).

## References:
My book, I used with Claude AI to create that tracking plan: https://link.timodechau.com/evAz2u
Blog post describing the design: https://timodechau.com/build-a-tracking-plan-around-one-core-feature-inbox-zero-in-superhuman/
YouTube video showing how I created this design: https://youtu.be/_V_BeUw9Fr4


## Implementation Overview
This document provides implementation details for Superhuman's event tracking system. Each event is documented with its triggers, data sources, and required properties.

## Data Layer Implementation
All events should be pushed to a central data layer before being sent to analytics tools. Use the following structure:

```javascript
window.dataLayer.push({
    'event': 'event_name',
    'properties': {
        // event specific properties
    }
});
```

## Entity: Account

### account_created
* Description: New user creates a Superhuman account
* Trigger: User completes account creation flow
* Source: Backend API
* Implementation Status: To be implemented
* Used by Metrics:
  - New account growth rate
  - Sign-up conversion rate
  - Account acquisition by source

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'account_created',
    'properties': {
        'account_id': '12345',
        'account_type': 'premium',
        'account_created_days': 0,
        'account_num_email_accounts': 0
    }
});
```

Properties:
* account_id
  - Type: String
  - Example: "12345"
  - Description: Unique identifier for the account
* account_type
  - Type: String
  - Example: "premium"
  - Allowed Values: ["free", "premium"]
* account_created_days
  - Type: Integer
  - Example: 0
  - Description: Number of days since account creation
* account_num_email_accounts
  - Type: Integer
  - Example: 0
  - Description: Number of connected email accounts

### account_deleted
* Description: User deletes their Superhuman account
* Trigger: User confirms account deletion
* Source: Backend API
* Implementation Status: To be implemented
* Used by Metrics:
  - Churn rate
  - Account lifetime value
  - Deletion reasons analysis

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'account_deleted',
    'properties': {
        'account_id': '12345',
        'account_status': 'deleted',
        'account_created_days': 365,
        'account_num_email_accounts': 2
    }
});
```

## Entity: Subscription

### subscription_created
* Description: User starts a new paid subscription
* Trigger: Successful payment and subscription activation
* Source: Backend API / Payment Provider
* Implementation Status: To be implemented
* Used by Metrics:
  - Conversion rate
  - Revenue growth
  - Plan distribution

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'subscription_created',
    'properties': {
        'subscription_id': 'sub_123',
        'subscription_plan': 'pro',
        'subscription_status': 'active',
        'subscription_billing_period': 'annual',
        'subscription_renewal_date': '2025-01-28'
    }
});
```

Properties:
* subscription_id
  - Type: String
  - Example: "sub_123"
  - Description: Unique identifier for the subscription
* subscription_plan
  - Type: String
  - Example: "pro"
  - Allowed Values: ["pro", "team"]
* subscription_status
  - Type: String
  - Example: "active"
  - Allowed Values: ["active", "canceled", "ended"]
* subscription_billing_period
  - Type: String
  - Example: "annual"
  - Allowed Values: ["monthly", "annual"]
* subscription_renewal_date
  - Type: String
  - Example: "2025-01-28"
  - Format: YYYY-MM-DD

### subscription_renewed
* Description: Existing subscription successfully renews
* Trigger: Successful renewal payment
* Source: Backend API / Payment Provider
* Implementation Status: To be implemented
* Used by Metrics:
  - Retention rate
  - Revenue retention
  - Plan renewal rate

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'subscription_renewed',
    'properties': {
        'subscription_id': 'sub_123',
        'subscription_plan': 'pro',
        'subscription_billing_period': 'annual',
        'subscription_renewal_date': '2026-01-28'
    }
});
```

## Entity: Email Account

### email_account_connected
* Description: User connects a new email account to Superhuman
* Trigger: Successful OAuth completion and email sync initiation
* Source: Backend API
* Implementation Status: To be implemented
* Used by Metrics:
  - Account activation rate
  - Email provider distribution
  - User engagement depth

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'email_account_connected',
    'properties': {
        'email_account_id': 'ea_123',
        'email_provider': 'gmail',
        'email_account_status': 'connected',
        'total_emails': 1500,
        'inbox_count': 120
    }
});
```

Properties:
* email_account_id
  - Type: String
  - Example: "ea_123"
  - Description: Unique identifier for email account
* email_provider
  - Type: String
  - Example: "gmail"
  - Allowed Values: ["gmail", "outlook", "other"]
* email_account_status
  - Type: String
  - Example: "connected"
  - Allowed Values: ["connected", "error"]
* total_emails
  - Type: Integer
  - Example: 1500
  - Description: Total number of emails in account
* inbox_count
  - Type: Integer
  - Example: 120
  - Description: Number of emails in inbox

### inbox_zero_achieved
* Description: User processes all emails in inbox
* Trigger: Last email in inbox is processed
* Source: Backend API
* Implementation Status: To be implemented
* Used by Metrics:
  - Zero inbox achievement rate
  - User success rate
  - Feature effectiveness

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'inbox_zero_achieved',
    'properties': {
        'email_account_id': 'ea_123',
        'days_since_last_zero': 5,
        'processed_email_count': 45
    }
});
```

## Entity: Email

### email_read
* Description: User opens and reads an email
* Trigger: Email view loaded and displayed for >2 seconds
* Source: Frontend
* Implementation Status: To be implemented
* Used by Metrics:
  - Email engagement rate
  - Reading patterns
  - Time-to-process analysis

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'email_read',
    'properties': {
        'email_id': 'em_123',
        'email_status': 'read',
        'email_type': 'received',
        'email_size': 15240,
        'email_has_attachments': true,
        'email_thread_size': 3
    }
});
```

Properties:
* email_id
  - Type: String
  - Example: "em_123"
  - Description: Unique identifier for email
* email_status
  - Type: String
  - Example: "read"
  - Allowed Values: ["unread", "read", "done", "snoozed", "scheduled"]
* email_type
  - Type: String
  - Example: "received"
  - Allowed Values: ["received", "sent", "draft"]
* email_size
  - Type: Integer
  - Example: 15240
  - Description: Size in bytes
* email_has_attachments
  - Type: Boolean
  - Example: true
  - Description: Indicates presence of attachments
* email_thread_size
  - Type: Integer
  - Example: 3
  - Description: Number of emails in thread
* email_snooze_duration
  - Type: Integer
  - Example: 86400
  - Description: Seconds until snooze ends (if applicable)

### email_done
* Description: User marks email as done
* Trigger: Done action completed
* Source: Frontend
* Implementation Status: To be implemented
* Used by Metrics:
  - Processing efficiency
  - Feature usage patterns
  - Inbox management metrics

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'email_done',
    'properties': {
        'email_id': 'em_123',
        'email_status': 'done',
        'time_to_process': 45
    }
});
```

## Entity: Search

### search_performed
* Description: User performs a search query
* Trigger: Search query execution
* Source: Frontend
* Implementation Status: To be implemented
* Used by Metrics:
  - Search usage frequency
  - Search effectiveness
  - Feature adoption rate

Data Layer Implementation:
```javascript
window.dataLayer.push({
    'event': 'search_performed',
    'properties': {
        'search_id': 'sr_123',
        'search_type': 'ai',
        'search_query': 'budget reports last month',
        'search_results_count': 15,
        'search_time': 0.45,
        'search_filter_used': true
    }
});
```

Properties:
* search_id
  - Type: String
  - Example: "sr_123"
  - Description: Unique identifier for search query
* search_type
  - Type: String
  - Example: "ai"
  - Allowed Values: ["regular", "ai"]
* search_query
  - Type: String
  - Example: "budget reports last month"
  - Description: Raw search query text
* search_results_count
  - Type: Integer
  - Example: 15
  - Description: Number of results returned
* search_time
  - Type: Float
  - Example: 0.45
  - Description: Time in seconds to execute search
* search_filter_used
  - Type: Boolean
  - Example: true
  - Description: Whether search filters were applied

## Testing Implementation

When implementing these events, use the following testing checklist:

1. Verify all required properties are present
2. Check property types match specifications
3. Validate allowed values for enum properties
4. Confirm events fire at correct triggers
5. Test edge cases (empty values, large numbers)
6. Verify data layer push structure
7. Check analytics tool receives data correctly

## Common Implementation Mistakes to Avoid

1. Inconsistent property naming
2. Wrong property types
3. Missing required properties
4. Incorrect trigger timing
5. Duplicate event firing
6. Missing error handling
7. Incorrect data type casting

