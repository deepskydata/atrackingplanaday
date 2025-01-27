# Slack Event Tracking Documentation & Implementation Guide

## Overview


## Implementation Context
As detailed in Event Data DesignChapter of the Analytics Implementation Workbook, this tracking plan follows the Double Three Layer Framework (D3L).

## References:
My book, I used with Claude AI to create that tracking plan: https://link.timodechau.com/evAz2u
Blog post describing the design: 
YouTube video showing how I created this design: 


## Implementation Overview

This document outlines the implementation details for Slack's event tracking system. Events are tracked using a data layer implementation pattern.

### Data Layer Structure

```javascript
window.dataLayer = window.dataLayer || [];

function pushEvent(eventName, eventData) {
    window.dataLayer.push({
        event: eventName,
        ...eventData
    });
}
```

## Event Specifications

### Workspace Events

#### workspace_created

* Description: Triggered when a new workspace is created
* Trigger: Successful completion of workspace creation flow
* Source: Backend API
* Implementation: Server-side
* Metrics Used By: 
  - New workspace growth rate
  - Workspace activation funnel
  - User acquisition metrics

Properties:
```javascript
{
    workspace_id: "W123456",       // string - Unique workspace identifier
    workspace_name: "Acme Team",   // string - Workspace display name
    workspace_created_at: "2024-01-27T10:30:00Z", // timestamp - ISO format
    workspace_age_days: 0,         // number - Days since creation
    workspace_status: "active",    // string - active, suspended, deleted
    workspace_total_members: 1,    // number - Count of members
    workspace_total_guests: 0,     // number - Count of guests
    workspace_domain: "acme"       // string - Custom domain if set
}
```

Implementation Example:
```javascript
// Server-side implementation
function trackWorkspaceCreated(workspace) {
    trackEvent('workspace_created', {
        workspace_id: workspace.id,
        workspace_name: workspace.name,
        workspace_created_at: new Date().toISOString(),
        workspace_age_days: 0,
        workspace_status: workspace.status,
        workspace_total_members: 1,
        workspace_total_guests: 0,
        workspace_domain: workspace.domain
    });
}
```

#### workspace_deleted

* Description: Triggered when a workspace is deleted
* Trigger: Successful workspace deletion
* Source: Backend API
* Implementation: Server-side
* Metrics Used By:
  - Workspace churn rate
  - Customer health metrics
  - Platform stability metrics

Properties:
```javascript
{
    workspace_id: "W123456",       // string - Workspace identifier
    workspace_age_days: 365,       // number - Days since creation
    workspace_total_members: 50,   // number - Final member count
    workspace_total_guests: 5,     // number - Final guest count
    deletion_reason: "migration"   // string - Reason for deletion
}
```

### Channel Events

#### channel_created

* Description: Triggered when a new channel is created
* Trigger: Channel creation completion
* Source: Backend API
* Implementation: Server-side
* Metrics Used By:
  - Channel growth metrics
  - Workspace engagement
  - Feature adoption metrics

Properties:
```javascript
{
    channel_id: "C123456",        // string - Unique channel identifier
    channel_type: "public",       // string - public, private
    channel_created_at: "2024-01-27T10:30:00Z", // timestamp - ISO format
    channel_age_days: 0,          // number - Days since creation
    channel_creator_id: "U123456",// string - Creator's user ID
    channel_member_count: 1,      // number - Initial member count
    channel_workspace_id: "W123456" // string - Parent workspace ID
}
```

#### channel_joined

* Description: Triggered when a user joins a channel
* Trigger: User joins channel (via invite or self-join)
* Source: Backend API
* Implementation: Server-side
* Metrics Used By:
  - Channel adoption metrics
  - User engagement metrics
  - Feature usage analytics

Properties:
```javascript
{
    channel_id: "C123456",        // string - Channel identifier
    user_id: "U123456",          // string - Joining user's ID
    join_type: "invited",        // string - invited, self_joined
    channel_member_count: 45,    // number - Updated member count
    channel_workspace_id: "W123456" // string - Parent workspace ID
}
```

### Message Events

#### message_sent

* Description: Triggered when a new message is sent
* Trigger: Message successfully sent
* Source: Backend API
* Implementation: Server-side
* Metrics Used By:
  - Message volume metrics
  - User engagement metrics
  - Workspace activity metrics

Properties:
```javascript
{
    message_id: "M123456",       // string - Unique message identifier
    message_channel_id: "C123456", // string - Channel identifier
    message_workspace_id: "W123456", // string - Workspace identifier
    message_sender_id: "U123456", // string - Sender's user ID
    message_type: "regular",     // string - regular, system
    message_has_attachments: false, // boolean
    message_has_mentions: false,  // boolean
    message_created_at: "2024-01-27T10:30:00Z" // timestamp - ISO format
}
```

### User Events

#### user_created

* Description: Triggered when a new user account is created
* Trigger: User signup completion
* Source: Backend API
* Implementation: Server-side
* Metrics Used By:
  - User growth metrics
  - Signup funnel analytics
  - Conversion metrics

Properties:
```javascript
{
    user_id: "U123456",         // string - Unique user identifier
    user_email_domain: "acme.com", // string - Email domain
    user_role: "member",        // string - guest, member, admin
    user_status: "active",      // string - active, inactive, deleted
    user_created_at: "2024-01-27T10:30:00Z", // timestamp - ISO format
    user_last_active_days: 0,   // number - Days since last activity
    user_workspace_count: 1     // number - Number of workspaces
}
```

### Subscription Events

#### subscription_created

* Description: Triggered when a new subscription is created
* Trigger: Subscription purchase completion
* Source: Backend API
* Implementation: Server-side
* Metrics Used By:
  - Revenue metrics
  - Conversion analytics
  - Customer acquisition metrics

Properties:
```javascript
{
    subscription_id: "S123456",  // string - Unique subscription identifier
    subscription_workspace_id: "W123456", // string - Workspace identifier
    subscription_plan: "standard", // string - free, standard, plus, enterprise
    subscription_status: "active", // string - active, past_due, canceled
    subscription_seats: 50,      // number - Seat count
    subscription_start_date: "2024-01-27", // string - YYYY-MM-DD
    subscription_billing_interval: "monthly" // string - monthly, annual
}
```

## Data Layer Implementation Guide

### Setup

Add this code to your application's main template:

```javascript
// Initialize data layer
window.dataLayer = window.dataLayer || [];

// Utility function for pushing events
function trackEvent(eventName, eventData) {
    window.dataLayer.push({
        event: eventName,
        timestamp: new Date().toISOString(),
        ...eventData
    });
}
```

### Server-Side Implementation

For server-side events, implement an event tracking service:

```javascript
class EventTrackingService {
    async trackEvent(eventName, eventData) {
        try {
            await fetch('/api/analytics/track', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({
                    event: eventName,
                    timestamp: new Date().toISOString(),
                    ...eventData
                })
            });
        } catch (error) {
            console.error('Failed to track event:', error);
        }
    }
}
```

### Testing Implementation

For testing event tracking, you can use this utility:

```javascript
function validateEvent(eventName, eventData, expectedProperties) {
    // Validate required properties
    for (const prop of expectedProperties) {
        if (!(prop in eventData)) {
            console.error(`Missing required property: ${prop}`);
            return false;
        }
    }
    
    // Validate data types
    // Add type validation logic here
    
    return true;
}
```

### Best Practices

1. Always include all required properties for each event
2. Use consistent data types for properties
3. Validate event data before sending
4. Handle tracking failures gracefully
5. Include proper error logging
6. Test tracking implementation in staging environment first