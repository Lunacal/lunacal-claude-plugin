---
name: lunacal-event-types
description: "Use when the user wants to create, update, publish, unpublish, or inspect their Lunacal event types."
---

# Lunacal Event Types

Guide for managing event types — the bookable meeting templates — in Lunacal.

## Listing and inspecting event types

- Call `list_event_types` to see all event types (personal and team). Each entry includes the id, title, slug, duration, and whether it is hidden.
- Call `get_event_type` with a numeric id for full details including locations, booking fields, and schedule id.
- Use `teamIds` filter on `list_event_types` to scope to specific teams.

## Creating an event type

Call `create_event_type` with at minimum:
- `title`: display name for the event type.
- `slug`: URL-safe identifier (used in the booking page URL).
- `length`: duration in minutes.

Optional fields: `description`, `hidden` (true to create it unpublished), `teamId`, `schedulingType` (one of `ROUND_ROBIN`, `COLLECTIVE`, or `MANAGED` for team event types), `locations`, `bookingFields`, `metadata`.

## Updating an event type

1. Call `get_event_type` first to see the current state.
2. Call `update_event_type` with the `id` and only the fields to change.
3. **Important**: `metadata`, `locations`, and `bookingFields` replace the entire value when set. To change part of them, read the current value from `get_event_type`, modify it, and send the full updated value.

## Publishing and unpublishing

- Call `set_event_type_published` with `eventId`, `isPublished` (true or false), and optionally `teamId`.
- A published event type appears on the user's public booking page. An unpublished one is hidden from visitors but still bookable via direct link.

## Limitations

- Weekly availability schedules (working hours) cannot be edited through these tools. Each event type has a `scheduleId` but it is read-only here.
- Never invent event type ids. Always get them from `list_event_types` or `get_event_type`.

## Error handling

- **Event type not found**: Call `list_event_types` and use an id from the result.
- **User must reconnect**: Stop calling tools and ask the user to reconnect their Lunacal account.
- **Temporary failure**: Retry once.

## Example prompts

- "Create a 45-minute consultation event type"
- "Change my 'Quick Chat' event from 15 to 30 minutes"
- "Unpublish my 'Office Hours' event type"
- "Show me all my event types"
- "Add a phone number field to my intro call event type"
