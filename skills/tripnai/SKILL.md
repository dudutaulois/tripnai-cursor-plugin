---
name: tripnai
description: >-
  Use when the user connects TripnAI, plans or edits trips on tripnai.com,
  or asks Grok/Cursor to read or update their TripnAI itinerary over MCP.
---
# TripnAI account connector

**MCP URL:** `https://tripnai.com/api/mcp`  
**Tagline:** Plan with your favorite AI. Travel with TripnAI.

TripnAI is the travel wallet on iPhone and the web. The MCP server talks to the **signed-in account** on tripnai.com. Trips that exist only on a phone are not visible until synced.

## Connect

1. Install the TripnAI Cursor plugin (or add the MCP URL in Cursor → MCP).
2. Connect and complete **OAuth** on tripnai.com (email, Google, or Apple). Do not paste the phone app session JWT.
3. Confirm tools appear (`list_trips`, `get_trip`, …).

For Grok Bot on the web, use the same MCP URL and sign in when prompted.

## Tools (summary)

| Tool | Purpose |
| --- | --- |
| `list_trips` | List account trips; optional `query` filter |
| `get_trip` | Full trip by `tripId` from `list_trips` |
| `propose_day_update` / `apply_day_update` | Append one stop after user approval |
| `propose_trip_plan` / `apply_trip_plan` | New trip after approval (`trips:create`) |
| `propose_trip_edit` / `apply_trip_edit` | Edit/move/remove stops, tips, or add a day |

**Rules the server enforces:**

- Check weather for trip dates before placing outdoor stops.
- `apply_*` only after the user sets `approved: true` on the proposal.
- Ambiguous trip or day → return candidates; do not guess.
- `apply_day_update` does not create new days or replace the whole itinerary.

## Workflow

1. `list_trips` → pick `tripId`.
2. `get_trip` → read days, stops, budget, travelers.
3. Propose changes with `propose_*`; show the user the summary.
4. `apply_*` with `approved: true` only after they agree.

Phone users can also **Import AI response** in the app for offline-style paste workflows; MCP is the live account path.
