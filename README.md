[Freelancer Com Scraper](https://apify.com/shahidirfan/freelancer-com-scraper?fpr=data)

# Freelancer.com Jobs Scraper

Collect active Freelancer.com job listings with clean structured output for lead generation, market monitoring, and freelance opportunity tracking. Capture job titles, summaries, skills, pricing ranges, bid activity, timing, and location signals in a dataset that is ready for analysis.

## Features

- **Structured job datasets** — Collect normalized records with project IDs, skill names, budget ranges, bid counts, and listing links.
- **Keyword and category targeting** — Search by free-text keyword, category slug, or a direct Freelancer jobs URL.
- **Budget and job-type filtering** — Narrow results to fixed-price or hourly opportunities and apply budget constraints.
- **Duplicate-safe output** — Repeated listings are skipped automatically so the dataset stays clean.
- **Location-aware records** — Capture country, vicinity, and coordinates when Freelancer exposes local listing details.
- **Fast pagination** — Gather multiple pages of active jobs with predictable result limits.

## Use Cases

### Freelance Opportunity Monitoring

Track new work in your niche without manually browsing job boards. Build saved searches for SEO, web development, design, writing, or location-specific opportunities.

### Agency Prospecting

Identify companies and buyers actively posting work. Use budget ranges, bid counts, and skill demand to prioritize high-value opportunities.

### Market Intelligence

Analyze which services are in demand, what pricing bands appear most often, and how competitive a category is based on bid activity.

### Pricing Research

Compare fixed-price and hourly opportunities across categories. Use the dataset to benchmark offers before preparing proposals.

## Input Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `startUrl` | String | No | — | Direct Freelancer jobs URL to start from. Useful for reusing an existing filtered search page. |
| `keyword` | String | No | `seo` | Free-text keyword used to search active jobs. |
| `category` | String | No | `seo` | Category slug such as `seo`, `web-development`, or `graphic-design`. |
| `jobType` | String | No | `all` | Filter by `all`, `fixed`, or `hourly`. |
| `minBudget` | Integer | No | — | Minimum budget threshold in USD. |
| `maxBudget` | Integer | No | — | Maximum budget threshold in USD. |
| `sortBy` | String | No | `relevance` | Sort mode. `newest` returns the newest listings first. `budget` sorts by highest budget first. |
| `results_wanted` | Integer | No | `20` | Maximum number of listings to save. |
| `max_pages` | Integer | No | `20` | Safety cap on listing pages requested during a run. |
| `proxyConfiguration` | Object | No | Apify Proxy | Proxy settings for reliable collection. |

## Output Data

Each dataset item may contain the following fields:

| Field | Type | Description |
| --- | --- | --- |
| `project_id` | Number | Freelancer project identifier. |
| `project_name` | String | Listing title shown on Freelancer. |
| `project_desc` | String | Rich listing summary with decoded text. |
| `project_desc_text` | String | Plain-text version of the listing summary. |
| `bid_count` | Number | Number of bids or entries on the listing. |
| `skill_ids` | Array | Raw Freelancer skill identifiers attached to the job. |
| `skills` | Array | Human-readable skill names mapped from Freelancer skill IDs. |
| `skills_info` | Array | Structured skill records with Freelancer skill IDs, names, and SEO slugs. |
| `budget_range` | String | Pricing mode shown by Freelancer, such as fixed or unspecified. |
| `budget_min` | String | Minimum displayed budget value. |
| `budget_max` | String | Maximum displayed budget value. |
| `bid_avg` | String | Displayed bid or pricing figure shown on the card. |
| `time_left` | String | Human-readable time left until the listing closes. |
| `highlight` | Boolean | Whether Freelancer marks the listing as highlighted. |
| `featured` | Boolean | Whether the listing is marked as featured. |
| `urgent` | Boolean | Whether the listing is marked as urgent. |
| `sealed` | Boolean | Whether the listing is sealed. |
| `guaranteed` | Boolean | Whether the listing has a guarantee badge. |
| `fulltime` | Boolean | Whether the listing is marked as full-time. |
| `top` | Boolean | Whether the listing is marked as top. |
| `payment_verified` | Boolean | Whether payment verification is shown for the buyer. |
| `nda` | Boolean | Whether the listing is marked with an NDA requirement. |
| `local` | Boolean | Whether the listing is marked as local. |
| `is_contest` | Boolean | Whether the listing is marked as a contest. |
| `seo_url` | String | Relative Freelancer path for the job listing. |
| `url` | String | Absolute job listing URL. |
| `has_upgrades` | Boolean | Whether premium listing upgrades are present. |
| `source` | String | Source website identifier. |

## Usage Examples

### Basic Category Search

```
{
    "category": "seo",
    "results_wanted": 20
}
```

### Keyword Search With Budget Filter

```
{
    "keyword": "wordpress developer",
    "jobType": "fixed",
    "minBudget": 100,
    "maxBudget": 1000,
    "results_wanted": 50
}
```

### Reuse a Saved Freelancer URL

```
{
    "startUrl": "https://www.freelancer.com/jobs/seo?fixed=true&fixed_min=250",
    "results_wanted": 30,
    "max_pages": 5
}
```

## Sample Output

```
{
    "project_id": 40278015,
    "project_name": "E-commerce Site Development",
    "project_desc": "I need a website developer to create an e-commerce site for selling physical products.",
    "project_desc_text": "I need a website developer to create an e-commerce site for selling physical products.",
    "bid_count": 62,
    "skill_ids": ["3", "17", "56", "137", "289", "335", "1031", "1832"],
    "skills": ["AJAX", "Article Rewriting", "eCommerce", "HTML", "PHP", "Shopping Carts", "Website Design", "WooCommerce"],
    "skills_info": [
        { "id": 3, "name": "AJAX", "seo_url": "ajax" },
        { "id": 1031, "name": "Website Design", "seo_url": "website-design" }
    ],
    "budget_range": "Fixed",
    "budget_min": "$30",
    "budget_max": "$250",
    "bid_avg": "$74",
    "time_left": "6 days left",
    "highlight": false,
    "featured": false,
    "urgent": false,
    "sealed": false,
    "guaranteed": false,
    "fulltime": false,
    "top": false,
    "payment_verified": false,
    "nda": false,
    "local": false,
    "is_contest": false,
    "has_upgrades": false,
    "seo_url": "/projects/web-development/commerce-site-development-40278015",
    "url": "https://www.freelancer.com/projects/web-development/commerce-site-development-40278015",
    "source": "freelancer.com"
}
```

## Tips for Best Results

### Start Small

- Begin with `results_wanted: 20` to validate your filters.
- Increase the limit once the search is returning the right category or keyword.

### Use Specific Searches

- Combine a category with a focused keyword to narrow the dataset.
- Reuse a working Freelancer search URL in `startUrl` when you already know the exact search you want.

### Use Proxies for Reliability

- Residential proxies are recommended for steady collection.
- Avoid running too many large searches in parallel against the same niche.

## Integrations

- **Google Sheets** — Build searchable lead lists and reporting dashboards.
- **Airtable** — Create recruiting, outreach, or market-monitoring databases.
- **Make** — Trigger alerts when new jobs appear in a target niche.
- **Zapier** — Route fresh listings into CRMs, email flows, or task tools.
- **Webhooks** — Send new dataset items to internal systems automatically.

### Export Formats

- **JSON** — For backend systems and custom workflows.
- **CSV** — For spreadsheet-based research and reporting.
- **Excel** — For business reviews and stakeholder sharing.
- **XML** — For systems that require structured imports.

## Frequently Asked Questions

### How many jobs can I collect in one run?

You can request as many listings as you need through `results_wanted`, subject to the available jobs in the selected niche and the `max_pages` safety limit.

### Can I search by a direct Freelancer URL?

Yes. Provide a filtered jobs URL in `startUrl` and the actor will reuse that search context.

### Will duplicate listings be saved?

No. The actor skips repeated project IDs so the output dataset stays clean.

### Why are some location fields missing?

Freelancer does not expose location data for every listing. Local fields are only present when the source listing includes them.

### Does every record include skill names?

Yes, when Freelancer exposes skill IDs for a listing, they are translated into readable skill names in the `skills` field.

## Support

For issues or feature requests, use the Apify Console to manage actor runs and review logs.

### Resources

- [Apify Documentation](https://docs.apify.com/)
- [Apify API Reference](https://docs.apify.com/api/v2)
- [Apify Schedules](https://docs.apify.com/platform/schedules)

## Legal Notice

This actor is intended for legitimate data collection, research, monitoring, and business workflow automation. Users are responsible for ensuring their use complies with Freelancer terms and applicable laws.