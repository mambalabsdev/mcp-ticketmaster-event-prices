# Ticketmaster Event Prices MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-ticketmaster-event-prices/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-ticketmaster-event-prices) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-ticketmaster-event-prices%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-ticketmaster-event-prices&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-ticketmaster-event-prices)](https://www.npmjs.com/package/@mambalabsdev/mcp-ticketmaster-event-prices) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-ticketmaster-event-prices)](https://www.npmjs.com/package/@mambalabsdev/mcp-ticketmaster-event-prices) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-ticketmaster-event-prices)](https://github.com/mambalabsdev/mcp-ticketmaster-event-prices/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-ticketmaster-event-prices)

An MCP server for the Mamba Labs Ticketmaster Event Prices actor on Apify. Ticketmaster events and their price ranges, on your own key.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give search keywords, a city, country, category, or date range, or Ticketmaster event IDs, and your own free Ticketmaster Discovery API key. The actor calls the Discovery API on your key and returns one row per event with date, venue, status, and the price range the API gives (min, max, currency). Price ranges are face value ranges, not seat or resale prices, and some events have none.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-ticketmaster-event-prices": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-ticketmaster-event-prices"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_ticketmaster_event_prices` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find Coldplay concerts in the US and their price ranges on my Ticketmaster key."
- "List music events in Chicago next month with min and max prices."
- "Get the price range for these Ticketmaster event IDs."

## Tool

`get_ticketmaster_event_prices`. One tool. Example arguments:

```json
{
  "keywords": [
    "Coldplay"
  ],
  "country_code": "US"
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `api_key` | string | no | Your own Consumer Key from developer.ticketmaster.com (free). It is stored encrypted by Apify, sent only to Ticketmaster, and never written to a row or a log. With no key the run writes one free row and reads nothing. |
| `keywords` | array | no | Artist, team, show, or event words. Each keyword runs as its own search with the filters below. Leave empty to search by the filters alone. In Clay, put the column inside a one item list. |
| `event_ids` | array | no | Optional. Ticketmaster Discovery event IDs (for example G5diZfkn0B-bh) to read one by one. These ignore the filters. |
| `country_code` | string | no | Two letter country code, for example US, CA, GB, AU, or DE. Default US. Write any to search every country. |
| `state_code` | string | no | Optional. State or province code, for example NY or ON. |
| `city` | string | no | Optional. City name, for example Chicago. |
| `postal_code` | string | no | Optional. Venue postal code. |
| `classification` | string | no | Optional. A segment, genre, or sub genre name, for example music, sports, rock, or comedy. |
| `start_date` | string | no | Optional. Events on or after this date, YYYY-MM-DD (UTC). |
| `end_date` | string | no | Optional. Events on or before this date, YYYY-MM-DD (UTC). |
| `max_events_per_search` | number | no | Number, default 100, at most 1000. The Discovery API pages no deeper than 1,000 events per search. |
| `only_with_prices` | boolean | no | yes or no (default no). yes skips events the API returns with no price range; skipped events are not charged. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `event-row`, one event row | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `empty-search`, one search or event ID that returned no new event | $0.001 | $0.00095 | $0.0009 | $0.00085 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

A missing or refused key, a used up daily quota, API errors, and refused input are free. A search with no new event is one empty-search event. Mamba Labs pays the platform costs of the run. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/ticketmaster-event-prices](https://apify.com/mambalabs/ticketmaster-event-prices)

## License

MIT. See [LICENSE](LICENSE).
