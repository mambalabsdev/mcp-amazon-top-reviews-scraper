# Amazon Top Reviews Scraper MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-amazon-top-reviews-scraper/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-amazon-top-reviews-scraper) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-amazon-top-reviews-scraper%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-amazon-top-reviews-scraper&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-amazon-top-reviews-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-amazon-top-reviews-scraper) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-amazon-top-reviews-scraper)](https://www.npmjs.com/package/@mambalabsdev/mcp-amazon-top-reviews-scraper) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-amazon-top-reviews-scraper)](https://github.com/mambalabsdev/mcp-amazon-top-reviews-scraper/blob/main/LICENSE)

An MCP server for the Mamba Labs Amazon Top Reviews Scraper actor on Apify. Top Amazon reviews and rating summary by ASIN on 8 marketplaces outside the US.

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

Give ASINs or product URLs on 8 Amazon marketplaces outside the US. You get one row per product with the rating, ratings count, and star split, and one flat row per top review: about 8 from the marketplace country and up to 5 from other countries, with date, stars, title, full text, variant, verified purchase, and helpful votes.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-amazon-top-reviews-scraper": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-amazon-top-reviews-scraper"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_amazon_top_reviews` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Get the top reviews for ASIN B01N5IB20Q on amazon.de."
- "What do UK buyers say about this Amazon product?"
- "Compare the star split of these ASINs on amazon.co.jp."

## Tool

`get_amazon_top_reviews`. One tool. Example arguments:

```json
{
  "products": [
    "B01N5IB20Q"
  ],
  "marketplace": "amazon.co.uk"
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `products` | array | no | ASINs or Amazon product URLs. A URL sets its own marketplace; an ASIN uses Marketplace below. From Clay, put the column inside a one item list. |
| `marketplace` | string | no | Marketplace for ASINs given without a URL. amazon.co.uk (default), amazon.de, amazon.fr, amazon.it, amazon.es, amazon.ca, amazon.co.jp, or amazon.in. amazon.com is not supported: its logged out pages show no reviews. |
| `include_other_countries` | boolean | no | yes (default) or no. The page also shows about 3 to 5 top reviews from other countries, which can include the United States. No keeps only the marketplace's own country. |
| `include_reviewer_names` | boolean | no | no (default) or yes. Yes adds the public display name Amazon shows on each review. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `product-read`, one product page read, with up to about 13 top reviews | $0.03 | $0.0285 | $0.027 | $0.0255 |
| `product-not-found`, one ASIN Amazon reports as not on that marketplace | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `apify-actor-start`, per run start | $0.006 | $0.006 | $0.006 | $0.006 |

Review rows, blocked products, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/amazon-top-reviews-scraper](https://apify.com/mambalabs/amazon-top-reviews-scraper)

## License

MIT. See [LICENSE](LICENSE).
