# Eventbrite Scraper: export to CSV or JSON with Python, Node.js or curl

Search Eventbrite events by keyword and location. One row per event: title, start and end, venue, city, online flag and event link.

This repository has ready scripts for the [Eventbrite Scraper: Events by Query and Location](https://apify.com/v0iddo/eventbrite-events-search) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/eventbrite-events-search), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "queries": [
    "tech-conference",
    "design-workshop",
    "startup-pitch"
  ],
  "maxEventsPerQuery": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `queries` | array | Search terms (kebab-case) |
| `location` | string | Eventbrite location slug |
| `maxEventsPerQuery` | integer | Cap events per query |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "query": "tech-conference",
  "id": "1994623085342",
  "title": "RockIT Women 8th Annual Women in Tech (WIT) Conference",
  "organizer": null,
  "startDate": "2026-12-08T08:30:00",
  "endDate": "2026-12-08T16:30:00",
  "isOnline": false,
  "venueName": "The Vista at Origin Park",
  "city": "Clarksville",
  "priceMin": null,
  "priceMax": null,
  "url": "https://www.eventbrite.com/e/rockit-women-8th-annual-women-in-tech-wit-conference-tickets-1994623085342"
}
```

Fields: `query`, `id`, `title`, `organizer`, `startDate`, `endDate`, `isOnline`, `venueName`, `city`, `priceMin`, `priceMax`, `url`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/eventbrite-events-search').call({"queries": ["tech-conference", "design-workshop", "startup-pitch"], "maxEventsPerQuery": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~eventbrite-events-search/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"queries": ["tech-conference", "design-workshop", "startup-pitch"], "maxEventsPerQuery": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$0.30 per 1,000 results**, nothing per run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
