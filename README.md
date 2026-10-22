# Lunacal Plugin for Claude

[Lunacal](https://lunacal.ai) is a scheduling and booking platform. This plugin connects Claude to your Lunacal account so you can manage meetings, event types, and availability through conversation.

## What this plugin does

The plugin connects Claude to Lunacal's remote MCP server at `mcp.lunacal.ai`. All data flows directly between Claude and Lunacal — no intermediate servers, no local processes, no stored credentials.

## Tools

### Read-only

| Tool | Description |
|------|-------------|
| `get_my_profile` | Your name, email, username, and timezone |
| `list_event_types` | All your event types (personal and team) |
| `get_event_type` | Full details for one event type |
| `find_available_slots` | Open booking slots for a date range |
| `list_bookings` | Your bookings filtered by status |

### Write

| Tool | Description |
|------|-------------|
| `create_booking` | Book a slot on an event type |
| `reschedule_booking` | Move an existing booking to a new time |
| `create_event_type` | Create a new event type |
| `update_event_type` | Update an existing event type |
| `set_event_type_published` | Publish or unpublish an event type |

### Destructive

| Tool | Description |
|------|-------------|
| `cancel_booking` | Cancel a booking (cannot be undone) |

## Authentication

Sign-in uses OAuth. When you first use a Lunacal tool, Claude will prompt you to authorize with your Lunacal account. No API keys or tokens need to be configured manually.

## Installation

### Claude Code (CLI)

```bash
/plugin install lunacal --marketplace Lunacal/lunacal-claude-plugin
```

Or directly:

```bash
claude plugin install lunacal
```

### claude.ai / Cowork

Find "Lunacal" in the plugin directory and click Install.

## Example prompts

- "What times are available for a 30-minute call this Friday?"
- "Book a meeting with jane@example.com tomorrow at 2 PM"
- "Reschedule my 3 PM meeting to next Monday"
- "Cancel my meeting with Alex on Thursday"
- "Create a 45-minute consultation event type"
- "Show me my upcoming bookings"

## Data and privacy

All requests go to `https://mcp.lunacal.ai/mcp` and from there to the Lunacal API at `app.lunacal.ai`. No data is sent anywhere else. The plugin includes no local scripts, binaries, or background processes.

- [Privacy Policy](https://lunacal.ai/privacy-policy)
- [Terms of Service](https://lunacal.ai/terms-of-use)
- [Support](https://lunacal.ai/contact-us)

## License

MIT — see [LICENSE](./LICENSE).
