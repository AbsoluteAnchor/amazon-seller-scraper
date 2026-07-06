[Amazon Seller Scraper](https://apify.com/codingfrontend/amazon-seller-scraper?fpr=data)

Scrape **Amazon seller profiles** — including seller name, ratings, feedback statistics, location, years on Amazon, and optionally their full product listings.

Supports Amazon India, US, UK, Germany, Japan, Canada, Australia, and Brazil.

---

## What it does

- Accepts a list of **Amazon Seller IDs** (e.g. `A14CZOWI0VEHLG`) or direct seller profile URLs
- Scrapes seller profile: name, star rating, feedback count, positive feedback %, location, years active
- Optionally scrapes all products in the seller's storefront (`scrapeProducts: true`)
- Runs up to 2 sellers in parallel

---

## Input

```
{
    "mode": "sellerId",
    "sellerIds": ["A14CZOWI0VEHLG", "A2HLF4XXYBKIPN"],
    "country": "in",
    "maxItems": 10,
    "scrapeProducts": false
}
```

### Input Fields

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `mode` | string | `sellerId` | `sellerId` (list of IDs) or `sellerUrl` (full profile URLs) |
| `sellerIds` | string[] | — | Amazon Seller IDs (for `sellerId` mode) |
| `sellerUrls` | string[] | — | Full Amazon seller profile URLs (for `sellerUrl` mode) |
| `country` | string | `in` | Marketplace: `in`, `com`, `co.uk`, `de`, `co.jp`, `ca`, `com.au`, `com.br` |
| `maxItems` | integer | `10` | Maximum sellers to scrape (1–500) |
| `scrapeProducts` | boolean | `false` | Scrape the seller's product listings |
| `maxProducts` | integer | `20` | Max products per seller (when `scrapeProducts: true`) |
| `proxyConfiguration` | object | — | Apify proxy settings |

---

## How to find a Seller ID

1. Go to any Amazon product page
2. Click on the seller name link (near "Sold by ...")
3. Look at the URL: `https://www.amazon.in/sp?seller=A14CZOWI0VEHLG` — the `seller=` part is the Seller ID

---

## Output

```
{
    "sellerId": "A14CZOWI0VEHLG",
    "sellerName": "boAt",
    "businessName": "Imagine Marketing Private Limited",
    "url": "https://www.amazon.in/sp?seller=A14CZOWI0VEHLG",
    "rating": "4.2",
    "ratingCount": 24830,
    "positiveFeedbackPercent": "87%",
    "location": "Maharashtra, India",
    "yearsOnAmazon": "8 years (since 2016)",
    "description": "boAt is India's leading consumer electronics brand...",
    "productCount": 0,
    "scrapedAt": "2025-01-15T10:30:00.000Z"
}
```

With `scrapeProducts: true`:

```
{
    "sellerId": "A14CZOWI0VEHLG",
    "sellerName": "boAt",
    "productCount": 20,
    "products": [
        {
            "asin": "B08HDJ86NZ",
            "title": "boAt Type-C Cable",
            "url": "https://www.amazon.in/dp/B08HDJ86NZ",
            "price": "₹149",
            "rating": "4.2",
            "imageUrl": "https://..."
        }
    ]
}
```

---

## Output Fields

| Field | Type | Description |
| --- | --- | --- |
| `sellerId` | string | Amazon seller ID |
| `sellerName` | string | Seller display name |
| `businessName` | string | Legal business name |
| `url` | string | Seller profile URL |
| `rating` | string | Average feedback star rating |
| `ratingCount` | number | Total number of ratings received |
| `positiveFeedbackPercent` | string | Percentage of positive feedback |
| `location` | string | Seller's location/address |
| `yearsOnAmazon` | string | How long the seller has been on Amazon |
| `description` | string | Seller description/about |
| `productCount` | number | Number of products scraped from storefront |
| `products` | array | Product listings (when `scrapeProducts: true`) |
| `scrapedAt` | string | ISO timestamp |

---

## Usage Examples

### Example 1: Profile-only for known seller IDs

```
{
    "mode": "sellerId",
    "sellerIds": ["A14CZOWI0VEHLG"],
    "country": "in"
}
```

### Example 2: With storefront products

```
{
    "mode": "sellerId",
    "sellerIds": ["A14CZOWI0VEHLG"],
    "country": "in",
    "scrapeProducts": true,
    "maxProducts": 50
}
```

### Example 3: By seller profile URL

```
{
    "mode": "sellerUrl",
    "sellerUrls": [
        "https://www.amazon.in/sp?seller=A14CZOWI0VEHLG"
    ]
}
```

---

## Notes

- Amazon Seller IDs are typically 14 uppercase alphanumeric characters (e.g. `A14CZOWI0VEHLG`)
- Proxy is recommended for scraping large numbers of sellers
- `scrapeProducts` adds significant scraping time — use only when needed
- Some seller pages may be unavailable or restricted based on region