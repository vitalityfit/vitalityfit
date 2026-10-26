# Vitalityfit (Xealti UG)

German technology company building price comparison tools for health, beauty, and wellness products.

## Our Product

[Vitalityfit](https://vitality-fit.de) — independent price comparison platform aggregating daily-updated prices from 50+ licensed German pharmacies, drugstores, and specialty shops. Free, ad-free, no paid placements.

Available on Web, iOS, and Android.

## For developers and AI assistants

Look up the cheapest offers for health products from German online shops and pharmacies, by name, PZN or EAN. Two public, read-only interfaces serve the same data, without login or API key.

### MCP server

Lets an AI assistant such as Claude or ChatGPT look up prices.

- Server URL: `https://vitality-fit.de/mcp` (streamable HTTP)
- Official MCP Registry: [`de.vitality-fit/prices`](https://registry.modelcontextprotocol.io/v0/servers?search=de.vitality-fit/prices)
- Claude Code: `claude mcp add --transport http vitality-fit https://vitality-fit.de/mcp`
- Claude: Settings → Connectors → Add custom connector, then enter the server URL
- ChatGPT: Settings → Apps & Connectors (developer mode may need to be enabled first)

| Tool | Returns |
|---|---|
| `search_products(query, limit?)` | Products matching a name, PZN or EAN: title, brand, pack sizes, cheapest price, number of shops and product page link (5 results by default, at most 10) |
| `search_product_offers(product_key, pack_size?)` | Up to 10 offers for one product and pack size, sorted by price: shop, price, shipping cost, delivery time, stock, unit price and shop link, plus the lowest price of the last 90 days |

Documentation: [vitality-fit.de/mcp](https://vitality-fit.de/mcp/#english)

### Price API

Plain HTTP and JSON.

```sh
# 1. Find the product by name, PZN or EAN
curl "https://vitality-fit.de/api/v1/products?q=01568889&limit=1"

# 2. Get the offers for one pack size
curl "https://vitality-fit.de/api/v1/products/orthomol-immun-trinkflaeschchen-tabletten/offers?pack_size=pzn_1568889"
```

- Base URL `https://vitality-fit.de/api/v1`, `GET` only, CORS enabled
- OpenAPI 3.1 description: [`/api/v1/openapi.json`](https://vitality-fit.de/api/v1/openapi.json)
- Rate limit: one request per 10 seconds per IP address on average (12 in 2 minutes); above that `429` with `Retry-After`
- Responses are cached for up to 1 hour; at most 10 products per search and 10 offers per request

Terms in short:

- Free for personal and non-commercial use, including free apps and GPTs. Commercial use only by agreement: service@vitality-fit.de
- Name the source "Preise: vitality-fit.de" with a link to https://vitality-fit.de/ and keep shop links unchanged
- For medicines, show the `compliance_notice` together with the results
- No bulk download, mirroring or resale of the data

Documentation and full terms: [vitality-fit.de/api](https://vitality-fit.de/api/#english) (the German version is binding)

### Notes for both

- Prices only, no medical advice. Prices may have changed since; the price in the shop applies.
- Shop links can be affiliate links: we may earn a commission, and the price does not change for the buyer.

## Contact

service@vitality-fit.de
