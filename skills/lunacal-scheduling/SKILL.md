---
name: lunacal-scheduling
description: "Use when the user wants to find available times, book a meeting, reschedule a booking, cancel a booking, or view their Lunacal bookings."
---

# Lunacal Scheduling

Guide for booking, rescheduling, cancelling, and listing meetings through Lunacal.

## Before any scheduling action

1. Call `get_my_profile` to learn the user's name, email, username, and timezone. Use this timezone for all datetime display and as the default `timezone` parameter.
2. Call `list_event_types` to see which event types exist. You need an event type id for availability lookups and bookings.

## Finding available times

- Call `find_available_slots` with the event type id (or slug + usernameList), a start/end range, and the user's timezone.
- All datetime parameters must be ISO 8601 with a timezone offset, for example `2026-10-13T15:00:00-04:00`.
- The `timezone` parameter must be an IANA timezone string, for example `America/New_York`.
- Present results in the user's local timezone, never raw UTC.

## Creating a booking

1. Always call `find_available_slots` first. `create_booking` does not check availability itself.
2. Use a start time from the availability response. Include `eventTypeId`, `start`, `timezone`, `name`, and `email`.
3. If the event type has required booking fields (visible in `get_event_type` under `bookingFields`), put answers in `additionalResponses` keyed by each field's `name`.
4. If the user gave ambiguous details (wrong name, unclear time, missing required fields), confirm before calling `create_booking`.

## Rescheduling a booking

1. Get the booking `uid` from `list_bookings` (use status `upcoming`).
2. Call `find_available_slots` with the `rescheduleUid` parameter set to the booking uid — this excludes the existing booking from conflict checks.
3. Call `reschedule_booking` with the booking uid, event type id, new start time, timezone, and attendee name and email.

## Cancelling a booking

1. Get the booking `uid` from `list_bookings`.
2. **Always confirm with the user before calling `cancel_booking`.** This action cannot be undone.
3. If Lunacal requires a cancellation reason, ask the user for one and pass it as `cancellationReason`.
4. To cancel all remaining occurrences of a recurring booking, set `allRemainingBookings` to true (confirm this with the user too).

## Listing bookings

- Call `list_bookings` with a `status` filter: `upcoming`, `recurring`, `past`, `cancelled`, or `unconfirmed`.
- Use `limit` and `cursor` (from `nextCursor` in the response) to paginate through results.
- Optional filters: `teamIds`, `eventTypeIds`, `search`, `startDate`, `endDate`, `attendeeEmail`.

## Error handling

- **Slot taken / no host free**: Do not retry the same slot. Call `find_available_slots` again and offer the user alternative times.
- **User must reconnect**: Stop calling tools and ask the user to reconnect their Lunacal account.
- **Temporary failure**: Retry the same call once. If it fails again, tell the user Lunacal is temporarily unavailable.
- **Booking not found**: Call `list_bookings` and use a uid from the result.
- **Event type not found**: Call `list_event_types` and use an id from the result.

## Limitations

- Weekly availability schedules (working hours) cannot be edited through these tools. Event types expose a `scheduleId` but it is informational only.
- Never invent booking uids, event type ids, email addresses, or timezones. Always look them up or ask the user.

## Example prompts

- "What times are available for a 30-minute call this Friday?"
- "Book a meeting with jane@example.com tomorrow at 2 PM"
- "Reschedule my 3 PM meeting to next Monday"
- "Cancel my meeting with Alex on Thursday"
- "Show me my upcoming bookings"
