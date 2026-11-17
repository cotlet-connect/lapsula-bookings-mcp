# Lapsula Bookings MCP server

Connect Lapsula Bookings to ChatGPT, Claude and any AI agent through Cotlet.dev. Use 6 hosted tools for calendars, appointments, group sessions, reminders and your booking page in your own Lapsula account. The calendar tool reads; all 5 write tools ask for approval by default.

## What you can ask

> Show tomorrow's calendar for each team member.

> Prepare a one-to-one appointment and ask me before booking it.

> Show the reminder settings without changing them.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Lapsula Bookings tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 lapsula_bookings_agent.py serve
```

Or with Docker:

```bash
docker build -t lapsula-bookings-mcp .
docker run -i --rm -e COTLET_API_KEY lapsula-bookings-mcp
```

Tools: `lapsula_bookings_guide`, `lapsula_bookings_status`, `lapsula_bookings_open`, `lapsula_bookings_read`, `lapsula_bookings_click`, `lapsula_bookings_type`, `lapsula_bookings_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 lapsula_bookings_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 lapsula_bookings_agent.py status    # is Lapsula Bookings ready in your Cotlet.dev account
python3 lapsula_bookings_agent.py guide     # Lapsula Bookings screens and sign-in link
python3 lapsula_bookings_agent.py read      # open Lapsula Bookings, read the page, close the browser
python3 lapsula_bookings_agent.py tools     # list Cotlet.dev tools
python3 lapsula_bookings_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Lapsula Bookings users

**How do I connect my account?**

Request access through https://cotlet.dev/signup?app=lapsula-bookings and sign in at https://app.lapsula.com/login in your Cotlet Cloud Browser.

**Which actions require approval?**

view_calendar only reads. The other 5 tools ask for approval by default.

**Can it read clinical notes?**

No. These tools cover scheduling, reminders and the booking page, without opening clinical notes or medical history.

**Is Cotlet affiliated with Lapsula?**

No. Cotlet.dev is independent and not affiliated with Lapsula.

## Learn more

- [Lapsula Bookings integration and hosted tools](https://cotlet.dev/integrations/lapsula-bookings-api-mcp-server)

---

More: https://cotlet.dev/integrations/lapsula-bookings-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=lapsula-bookings

Request access: https://cotlet.dev/signup?app=lapsula-bookings&utm_source=github&utm_medium=readme&utm_campaign=lapsula-bookings

## Workflows on the hosted integration

The live integration page documents these 6 operations:

- `view_calendar`
- `schedule_appointment`
- `schedule_group_session`
- `set_group_capacity`
- `configure_reminders`
- `customize_booking_page`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
