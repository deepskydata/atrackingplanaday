# Vimcal Event Tracking Documentation & Implementation Guide


## Implementation Overview

All events should be implemented using a data layer. Events should be pushed to the data layer before being picked up by any analytics tool. This enables easier debugging and flexibility in changing analytics providers.

## References:
My book, I used with Claude AI to create that tracking plan: https://link.timodechau.com/evAz2u
Blog post describing the design: coming soon
YouTube video showing how I created this design: https://youtu.be/FeI_Hq_XYFI


### Data Layer Structure

```javascript
window.dataLayer = window.dataLayer || [];
window.dataLayer.push({
  event: "vimcal_event",
  entity: "account",  // The entity this event belongs to
  activity: "created", // The specific activity being tracked
  properties: {
    // All relevant properties for this event
  }
});
```

## Account Entity Events

### account_created
* Description: Triggered when a new account is successfully created
* Trigger: User completes signup flow successfully
* Sources: Authentication system
* Implementation: Server-side
* Used by metrics: 
  - New user acquisition
  - Signup funnel completion
  - User growth

Example implementation:
```javascript
window.dataLayer.push({
  event: "vimcal_event",
  entity: "account",
  activity: "created",
  properties: {
    account_id: "acc_12345",
    account_type: "trial",
    account_timezone: "America/New_York",
    account_days_since_creation: 0,
    account_num_calendars: 0,
    account_num_integrations: 0
  }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| account_id | string | "acc_12345" | Unique account identifier |
| account_type | string | "trial" | Either "trial" or "paid" |
| account_timezone | string | "America/New_York" | Account timezone in IANA format |
| account_days_since_creation | integer | 0 | Days since account creation |
| account_num_calendars | integer | 0 | Number of connected calendars |
| account_num_integrations | integer | 0 | Number of connected integrations |

### account_integration_connected
* Description: When a user connects a video conferencing tool
* Trigger: OAuth flow completion or API key validation
* Sources: Account settings
* Implementation: Server-side
* Used by metrics:
  - Integration adoption rate
  - Feature usage tracking
  - Product stickiness

Example implementation:
```javascript
window.dataLayer.push({
  event: "vimcal_event",
  entity: "account",
  activity: "integration_connected",
  properties: {
    account_id: "acc_12345",
    integration_type: "zoom"
  }
});
```

Properties inherit from account_created, plus:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| integration_type | string | "zoom" | Type of integration being connected |

### account_deleted
* Description: When an account is permanently deleted
* Trigger: Account deletion confirmation
* Sources: Account settings
* Implementation: Server-side
* Used by metrics:
  - Churn tracking
  - Account lifecycle analysis

## Subscription Entity Events

### subscription_trial_started
* Description: When a new trial period begins
* Trigger: Account creation completion
* Sources: Subscription system
* Implementation: Server-side
* Used by metrics:
  - Trial funnel analysis
  - Conversion potential

Example implementation:
```javascript
window.dataLayer.push({
  event: "vimcal_event",
  entity: "subscription",
  activity: "trial_started",
  properties: {
    subscription_id: "sub_12345",
    subscription_plan: "pro",
    subscription_status: "active",
    subscription_billing_period: "monthly"
  }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| subscription_id | string | "sub_12345" | Unique subscription identifier |
| subscription_plan | string | "pro" | Plan type |
| subscription_status | string | "active" | Current status |
| subscription_billing_period | string | "monthly" | Billing frequency |

[Additional subscription events follow same pattern: started, renewed, cancelled, ended]

## Calendar Entity Events

### calendar_connected
* Description: When a calendar source is connected
* Trigger: OAuth completion or connection confirmation
* Sources: Calendar settings
* Implementation: Server-side
* Used by metrics:
  - Calendar adoption rate
  - User activation tracking

Example implementation:
```javascript
window.dataLayer.push({
  event: "vimcal_event",
  entity: "calendar",
  activity: "connected",
  properties: {
    calendar_id: "cal_12345",
    calendar_source: "google",
    calendar_permission_level: "write"
  }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| calendar_id | string | "cal_12345" | Unique calendar identifier |
| calendar_source | string | "google" | Calendar provider |
| calendar_permission_level | string | "write" | Access level |

### calendar_disconnected
[Similar structure to connected event]

## Event Entity Events

### event_created
* Description: When a new calendar event is created
* Trigger: Event creation completion
* Sources: Calendar UI, API
* Implementation: Server-side
* Used by metrics:
  - Meeting volume
  - User engagement
  - Core feature usage

Example implementation:
```javascript
window.dataLayer.push({
  event: "vimcal_event",
  entity: "event",
  activity: "created",
  properties: {
    event_id: "evt_12345",
    event_type: "meeting",
    event_duration: 30,
    event_num_attendees: 3,
    event_has_video: true,
    event_video_provider: "zoom",
    event_created_source: "ui",
    event_status: "confirmed",
    event_is_recurring: false,
    event_creator_type: "self",
    event_creator_domain: null
  }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| event_id | string | "evt_12345" | Unique event identifier |
| event_type | string | "meeting" | Type of calendar event |
| event_duration | integer | 30 | Duration in minutes |
| event_num_attendees | integer | 3 | Number of participants |
| event_has_video | boolean | true | Whether includes video conference |
| event_video_provider | string | "zoom" | Video provider if applicable |
| event_created_source | string | "ui" | How event was created |
| event_status | string | "confirmed" | Current event status |
| event_is_recurring | boolean | false | Whether event repeats |
| event_creator_type | string | "self" | Who created the event |
| event_creator_domain | string | null | Creator's domain if external |

[Additional event entity events follow same pattern]

## Booking Link Entity Events

### booking_link_created
* Description: When a new booking link is generated
* Trigger: Link creation completion
* Sources: Booking link UI
* Implementation: Server-side
* Used by metrics:
  - Booking feature adoption
  - User engagement
  - Advanced feature usage

Example implementation:
```javascript
window.dataLayer.push({
  event: "vimcal_event",
  entity: "booking_link",
  activity: "created",
  properties: {
    link_id: "link_12345",
    link_type: "booking",
    link_duration: 30,
    link_availability_pattern: "working_hours",
    link_has_buffer: true,
    link_num_slots: 5,
    link_restrictions: null,
    link_custom_questions: []
  }
});
```

Properties:
| Name | Type | Example | Description |
|------|------|---------|-------------|
| link_id | string | "link_12345" | Unique link identifier |
| link_type | string | "booking" | Type of booking link |
| link_duration | integer | 30 | Default duration in minutes |
| link_availability_pattern | string | "working_hours" | Availability setup |
| link_has_buffer | boolean | true | Whether buffers enabled |
| link_num_slots | integer | 5 | Available time slots |
| link_restrictions | object | null | Any booking restrictions |
| link_custom_questions | array | [] | Additional fields required |

### booking_link_shared
* Description: When a booking link is copied or shared
* Trigger: Copy action or share button click
* Sources: Booking link UI
* Implementation: Client-side
* Used by metrics:
  - Link distribution
  - Feature engagement
  - User activation

[Additional booking link events follow same pattern]

## Implementation Notes

1. All server-side events should use the official backend SDK where available
2. Client-side events should push to data layer before analytics
3. All timestamps should be in UTC
4. Event names should use snake_case
5. Property values should be consistently formatted
6. Required properties must always be included
7. Optional properties should be null if not applicable
