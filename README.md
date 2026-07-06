[Amazon Seller Scraper](https://apify.com/sovereigntaylor/amazon-seller-scraper?fpr=data)

Scrape Amazon seller profiles, business information, ratings, and complete product catalogs. Built for FBA competitor analysis, marketplace intelligence, seller research, and brand protection monitoring.

## What does this scraper do?

This actor extracts detailed information about Amazon sellers/merchants including:

- **Seller Profile**: Name, store name, seller ID, store URL, logo image
- **Ratings & Reviews**: Overall rating (out of 5), total rating count, positive feedback percentage
- **Business Information**: Registered business name, business address, launch date on Amazon
- **Store Description**: The seller's "About" page content and branding
- **Product Catalog**: Full list of products sold by the seller with prices, ASINs, ratings, review counts, Prime eligibility, and images

## Use Cases

### FBA Competitor Analysis

Monitor your competitors on Amazon. Track their product catalog size, new product launches, pricing strategies, and seller ratings over time. Identify gaps in their catalog that you can fill.

### Marketplace Intelligence

Research the Amazon marketplace by analyzing seller profiles at scale. Understand which sellers dominate specific categories, their product counts, rating distributions, and business locations.

### Seller Due Diligence

Before partnering with or acquiring an Amazon business, verify seller claims by scraping their actual profile data, ratings history, product catalog, and business registration information.

### Brand Protection & Monitoring

Monitor unauthorized sellers of your products. Track who is selling your brand's items, their pricing, and whether they are authorized resellers. Detect counterfeit sellers early.

### Supplier Research

Find and evaluate potential suppliers or wholesale partners on Amazon. Analyze their product range, customer satisfaction ratings, and business credentials before establishing relationships.

### Price Intelligence

Track competitor pricing across their entire product catalog. Monitor price changes, identify loss leaders, and optimize your own pricing strategy based on market data.

### Market Entry Research

Before entering a new product category, research existing sellers. Understand the competitive landscape, typical seller ratings, product counts, and pricing strategies in your target niche.

## Input Configuration

| Parameter | Type | Default | Description |
| --- | --- | --- | --- |
| `sellerIds` | Array of strings | (required) | Amazon seller IDs or store URLs |
| `includeProducts` | Boolean | `true` | Whether to scrape seller's product catalog |
| `maxProducts` | Integer | `50` | Max products per seller (0 = unlimited) |
| `domain` | String | `amazon.com` | Amazon marketplace domain |
| `proxyConfiguration` | Object | Apify Residential | Proxy settings |

### Supported Amazon Domains

- `amazon.com` (United States)
- `amazon.co.uk` (United Kingdom)
- `amazon.de` (Germany)
- `amazon.fr` (France)
- `amazon.it` (Italy)
- `amazon.es` (Spain)
- `amazon.ca` (Canada)
- `amazon.co.jp` (Japan)
- `amazon.in` (India)
- `amazon.com.au` (Australia)

## How to Find Seller IDs

There are several ways to get an Amazon seller ID:

1. **From a product page**: Click on the seller name under "Sold by" on any product listing. The seller ID is the `seller=` parameter in the URL.
2. **From a store page**: Visit any Amazon store and the URL will contain the seller ID (e.g., `amazon.com/sp?seller=A2R2RITDJNW1Q6`).
3. **Direct ID**: Seller IDs are alphanumeric strings like `A2R2RITDJNW1Q6`.

You can provide either the raw seller ID or the full URL -- the scraper handles both formats.

## Example Input

```
{
  "sellerIds": [
    "A2R2RITDJNW1Q6",
    "https://www.amazon.com/sp?seller=A1B2C3D4E5F6G7",
    "ATVPDKIKX0DER"
  ],
  "includeProducts": true,
  "maxProducts": 100,
  "domain": "amazon.com",
  "proxyConfiguration": {
    "useApifyProxy": true,
    "apifyProxyGroups": ["RESIDENTIAL"]
  }
}
```

## Output Example

### Seller Profile

```
{
  "sellerName": "TechGadgets Store",
  "sellerId": "A2R2RITDJNW1Q6",
  "storeName": "TechGadgets Official",
  "storeUrl": "https://www.amazon.com/sp?seller=A2R2RITDJNW1Q6",
  "rating": 4.7,
  "ratingCount": 12543,
  "positivePercent30": 97,
  "positivePercent90": 96,
  "positivePercent12m": 95,
  "positivePercentLifetime": 94,
  "businessName": "TechGadgets LLC",
  "businessAddress": "123 Commerce St, Seattle, WA 98101, US",
  "launchDate": "January 2019",
  "description": "We are a leading provider of consumer electronics...",
  "logo": "https://images-na.ssl-images-amazon.com/images/S/...",
  "productCount": 342,
  "domain": "amazon.com",
  "scrapedAt": "2026-03-02T12:00:00.000Z"
}
```

### Seller Products (when `includeProducts` is enabled)

```
{
  "type": "product",
  "sellerId": "A2R2RITDJNW1Q6",
  "sellerName": "TechGadgets Store",
  "title": "Wireless Bluetooth Earbuds with Charging Case",
  "asin": "B09ABC1234",
  "price": 29.99,
  "currency": "USD",
  "rating": 4.3,
  "reviewCount": 1523,
  "isPrime": true,
  "image": "https://m.media-amazon.com/images/I/...",
  "url": "https://www.amazon.com/dp/B09ABC1234",
  "category": "Electronics",
  "domain": "amazon.com",
  "scrapedAt": "2026-03-02T12:00:00.000Z"
}
```

## Integrations

This actor can be connected with various tools and platforms:

- **Google Sheets**: Export results directly to a Google Sheet for analysis
- **Slack/Discord**: Get notifications when scraping completes
- **Zapier/Make**: Trigger workflows based on scraped data
- **Webhooks**: Send results to your own API endpoint
- **Apify Datasets**: Store results in Apify cloud for later retrieval via API

## Proxy Configuration

Amazon actively blocks automated requests. For best results:

- **Recommended**: Use Apify's residential proxies (`RESIDENTIAL` group)
- **Alternative**: Use `SERP` proxy group for search-like requests
- **Custom**: You can provide your own proxy URLs

## Rate Limiting & Performance

- The scraper automatically handles rate limiting and retries
- Typical speed: 5-15 sellers per minute (depending on product catalog size)
- Each seller profile page is fetched once; product pages are paginated
- Failed requests are retried up to 5 times with exponential backoff
- Concurrent requests are limited to avoid triggering anti-bot measures

## Pricing

This actor uses **Pay Per Event** pricing:

| Event | Price | Description |
| --- | --- | --- |
| `seller-scraped` | $0.005 | Charged per seller profile scraped |

Product listings within a seller's catalog are included at no extra charge.

## Tips & Best Practices

1. **Start small**: Test with 1-2 sellers before running large batches
2. **Use residential proxies**: Amazon is aggressive with bot detection
3. **Limit products initially**: Set `maxProducts` to 20-50 for testing
4. **Monitor for CAPTCHAs**: If you see empty results, try residential proxies
5. **Schedule regular runs**: Use Apify scheduling to monitor sellers over time
6. **Combine with other scrapers**: Use with our Amazon Product Scraper for deeper product analysis

## Limitations

- Some seller profiles may have limited public information
- Very new sellers might not have rating data available
- Product catalogs over 10,000 items may take significant time
- Amazon may temporarily block requests during high-traffic periods
- Business address is only available when the seller has disclosed it

## Changelog

### v1.0.0 (2026-03-02)

- Initial release
- Seller profile extraction with full business info
- Product catalog scraping with pagination
- Support for 10 Amazon marketplaces
- PPE pricing model
- 12 rotating user agents
- Multiple extraction strategies with fallbacks

## Support

If you encounter issues or have feature requests, please open an issue on the actor's page or contact us at [ricardo.yudi@gmail.com](mailto:ricardo.yudi@gmail.com).

## Legal Disclaimer

This actor is intended for legitimate business purposes such as market research, competitive analysis, and brand monitoring. Users are responsible for ensuring their use complies with Amazon's Terms of Service, applicable laws, and regulations. The scraper only accesses publicly available information.