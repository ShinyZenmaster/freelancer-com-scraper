[Freelancer Com Scraper](https://apify.com/fatihtahta/freelancer-com-scraper?fpr=data)

# Freelancer.com Freelancer Scraper

**Slug:** fatihtahta/freelancer-com-scraper

## Overview

Freelancer.com Freelancer Scraper collects structured freelancer profile data from Freelancer.com, including profile identifiers, names, usernames, locations, hourly rates, ratings, review counts, earnings signals, skills, portfolios, and public reputation metadata. It can also return richer public profile details such as job reputation, exams, SEO profile snippets, rating breakdowns, and an extended profile object when those attributes are available. When review collection is enabled, it also fetches public freelancer review records from Freelancer's review API and saves each review as its own dataset item linked back to the freelancer. [Freelancer.com](https://www.freelancer.com) is a global freelance marketplace, and its public freelancer directory data is useful for sourcing, enrichment, benchmarking, and market analysis. This actor automates repetitive collection work and returns consistent JSON records that are easier to use in spreadsheets, dashboards, and pipelines. It saves time, reduces manual copy-paste work, and makes recurring data collection more dependable.

## Why Use This Actor

- **Market research and analytics:** Track freelancer supply, rates, skills, ratings, and geography across categories to benchmark markets and monitor changes over time.
- **Product and content teams:** Study how freelancers describe services, position expertise, and present portfolios to inform messaging, landing pages, and editorial planning.
- **Developers and data engineering pipelines:** Feed structured freelancer records into ETL jobs, warehouses, dashboards, search indexes, and downstream APIs.
- **Lead generation and enrichment:** Build targeted prospect lists and enrich vendor or freelancer databases with profile details, service signals, and public reputation metrics.
- **Monitoring and competitive tracking:** Re-run the same searches on a schedule to watch changes in pricing, visibility, reviews, skills, and profile quality signals.

## Input Parameters

Provide any combination of URLs, queries, and filters...

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| `enrich_data` | `boolean` | When enabled, adds richer public profile details when available, such as extended profile metadata, portfolio counts, images, registration date, timezone data, and additional public profile attributes. | `false` |
| `get_reviews` | `boolean` | When enabled, fetches public review records for each saved freelancer from the Freelancer reviews API and stores them as separate dataset records linked to that freelancer. | `false` |
| `max_reviews` | `integer` | Maximum number of review records to save per freelancer profile when `get_reviews` is enabled. This is a per-freelancer cap, not a run-wide cap. Leave empty to keep all available reviews. | `-` |
| `queries` | `array[string]` | Words or phrases to search on Freelancer.com. Use skills, roles, services, or names such as `php developer`, `ux designer`, or `john smith`. | `-` |
| `hourly_rate` | `string` | Optional hourly-rate filter for query-based discovery. Allowed values: `<$10/hour`, `$10-$20/hour`, `$20-$30/hour`, `$30-$40/hour`, `>$40/hour`. | `-` |
| `country` | `string` | Optional country filter. Allowed values: one supported country from the actor input, including `United States`, `United Kingdom`, `India`, `Canada`, `Germany`, `Australia`, and many others. | `-` |
| `min_rating` | `string` | Optional minimum public rating threshold. Allowed values: `1 star and up`, `2 star and up`, `3 star and up`, `4 star and up`, `5 star`. | `-` |
| `currently_online` | `boolean` | When enabled, limits results to freelancers who appear online at the time of collection. | `false` |
| `category` | `string` | Optional service-category filter. Allowed values include `build-a-website`, `write-some-software`, `content-writing`, `design-a-logo`, `social-media-marketing`, `help-with-crm`, `general-labour`, and the other supported category values available in the actor input. | `-` |
| `limit` | `integer` | Maximum number of results to save for each search term or starting input. Leave empty to collect as many matching results as are available. | `-` |
| `proxyConfiguration` | `object` | Optional Apify or custom proxy settings for more reliable collection on larger or repeated runs. | Apify proxy with `RESIDENTIAL` group |

## Example Inputs

Scenario: Search-driven run

```
{
  "queries": ["react developer", "next.js developer"],
  "country": "India",
  "hourly_rate": "$20-$30/hour",
  "min_rating": "4 star and up",
  "category": "write-some-software",
  "limit": 200
}
```

Scenario: Direct URL run

```
{
  "startUrls": [
    "https://www.freelancer.com/freelancers",
    "https://www.freelancer.com/freelancers/united-states/design-a-logo"
  ],
  "country": "United States",
  "min_rating": "4 star and up",
  "currently_online": true,
  "limit": 150
}
```

Scenario: Filtered and enriched run

```
{
  "queries": ["seo specialist"],
  "country": "United Kingdom",
  "hourly_rate": "$10-$20/hour",
  "min_rating": "5 star",
  "currently_online": true,
  "category": "seo-my-website",
  "enrich_data": true,
  "get_reviews": true,
  "max_reviews": 50,
  "limit": 100
}
```

## Output

### 6.1 Output destination

The actor writes results to an Apify dataset as JSON records. And the dataset is designed for direct consumption by analytics tools, ETL pipelines, and downstream APIs without post-processing.

### 6.2 Record envelope (all items)

- **type** *(string, required)*: Record category. This actor currently emits `profile`, `review`, and summary/debug records.
- **id** *(number, required)*: Freelancer.com profile identifier.
- **url** *(string, required)*: Canonical Freelancer.com profile URL.

Recommended idempotency key: `type + ":" + id`

Use this key to deduplicate repeated discoveries and to upsert the same freelancer deterministically across recurring runs.

### 6.3 Examples

Example: profile (`type = "profile"`)

```
{
  "type": "profile",
  "id": 24288455,
  "url": "https://www.freelancer.com/u/BrightDock",
  "title": "BrightDock LLC",
  "publicName": "BrightDock LLC",
  "sourceUrl": "https://www.freelancer.com/freelancers?freeSearch=bright",
  "seedId": "98d6f0e364d1",
  "seedType": "query",
  "seedValue": "bright",
  "pageIndex": 1,
  "username": "BrightDock",
  "description": "BrightDock helps business leaders develop successful digital products.",
  "location": "Rijeka / Croatia",
  "country": "Croatia",
  "city": "Rijeka",
  "tagline": "Bio For More - #1 Ranked In Freelancer.com Search.",
  "price": "EUR 95 / hour",
  "priceNumeric": 95,
  "currency": "EUR",
  "hourlyRate": 95,
  "rating": 4.993173009582965,
  "reviewsCount": 227,
  "totalEarnings": 1473874,
  "skills": [
    "PHP",
    "Java",
    "JavaScript",
    "Python",
    "Website Design"
  ],
  "topSkills": [
    {
      "skillId": "3",
      "name": "PHP",
      "categoryId": "1",
      "earnings": 905434.7502185,
      "seoUrl": "php"
    }
  ],
  "skillDetails": [
    {
      "skillId": 395,
      "name": "3D Animation",
      "isUserSkill": true,
      "repScore": 74,
      "skillJobCount": 1
    }
  ],
  "exams": [
    {
      "examId": "218",
      "examName": "Semiconductor Electronics",
      "scorePercentage": 82
    }
  ],
  "seoProfiles": [
    {
      "jobIds": ["357"],
      "headline": "Professional Website Designer",
      "seoText": "Professional website designer with development and digital marketing experience."
    }
  ],
  "portfolios": [
    {
      "title": "Huuk | Marketing Campaign",
      "description": "Branding, product, and campaign work for a social media application."
    }
  ],
  "reviewJobTitles": [
    "Build a website",
    "Frontend Developer"
  ],
  "jobReputation": [
    {
      "jobId": "3",
      "jobName": "PHP",
      "score": 5950,
      "stars": 5.0000001
    }
  ],
  "geoLocation": {
    "lat": 45.327,
    "lon": 14.442
  },
  "ratingBreakdown": {
    "quality": 4.99806631975284,
    "communication": 4.970309120061813,
    "expertise": 4.999423288347337,
    "professionalism": 5,
    "hireAgain": 4.99806631975284,
    "onBudget": 0.9986364745332539,
    "onTime": 0.9711507554071165,
    "positive": 0.031244201888307038,
    "overall": 4.993173009582965
  },
  "reviewStats": {
    "all": 246,
    "reviews": 227,
    "incompleteReviews": 2,
    "complete": 244,
    "incomplete": 2,
    "earnings": 1473874,
    "completionRate": 0.991869918699187
  },
  "insigniaIds": [102, 145, 169, 690, 694, 701],
  "poolIds": [1, 3, 17],
  "freelancerReputation": 4.1393339671874925,
  "freelancerReputationVariant": 4.904553184919854,
  "score": 11308,
  "stars": 5.0000001,
  "hidden": false,
  "isOnline": false,
  "invited": false,
  "isSponsored": false,
  "isHighlighted": false,
  "membershipBadge": "plus",
  "logoUrl": "https://cdn6.f-cdn.com/ppic/293850094/logo/24288455/profile_logo_24288455.jpg",
  "profileLogoUrl": "https://cdn3.f-cdn.com/ppic/293850093/logo/24288455/VIKvS/profile_logo_KGFPU_98ae5805fe4ef7d75e007132b8deb624.jpeg",
  "registrationDate": 1490726779,
  "coverImage": {
    "current_image": {
      "id": 206747,
      "url": "https://cdn6.f-cdn.com/files/download/206409018/c0b1c2.jpg",
      "width": 1920,
      "height": 550
    }
  },
  "portfolioCount": 86,
  "timezone": {
    "id": 292,
    "country": "HR",
    "timezone": "Europe/Zagreb",
    "offset": 2
  },
  "profileDetails": {
    "id": 24288455,
    "username": "BrightDock",
    "display_name": "BrightDock LLC",
    "public_name": "BrightDock LLC",
    "profile_description": "BrightDock helps business leaders develop successful digital products.",
    "hourly_rate": 95,
    "registration_date": 1490726779,
    "role": "freelancer",
    "preferred_freelancer": true,
    "location": {
      "country": {
        "name": "Croatia",
        "code": "hr"
      },
      "city": "Rijeka",
      "latitude": 45.327,
      "longitude": 14.442
    },
    "status": {
      "payment_verified": true,
      "email_verified": true,
      "phone_verified": true,
      "identity_verified": true,
      "profile_complete": true
    },
    "primary_currency": {
      "id": 8,
      "code": "EUR",
      "sign": "EUR",
      "name": "Euro"
    },
    "membership_package": {
      "id": 18,
      "name": "plus"
    },
    "jobs": [
      {
        "id": 3,
        "name": "PHP",
        "seo_url": "php"
      }
    ],
    "qualifications": [
      {
        "id": 533,
        "name": "Preferred Freelancer Program SLA Exam",
        "score_percentage": 84
      }
    ],
    "badges": [
      {
        "id": 31,
        "name": "All Star"
      }
    ]
  },
  "fingerprint": "f5f3da873a204f36ee7a"
}
```

Example: review (`type = "review"`)

```
{
  "type": "review",
  "reviewId": 21981776,
  "sourceUrl": "https://www.freelancer.com/freelancers?freeSearch=bright",
  "seedId": "98d6f0e364d1",
  "seedType": "query",
  "seedValue": "bright",
  "pageIndex": 1,
  "freelancerId": "24288455",
  "freelancerTitle": "BrightDock LLC",
  "freelancerPublicName": "BrightDock LLC",
  "freelancerUsername": "BrightDock",
  "freelancerUrl": "https://www.freelancer.com/u/BrightDock",
  "projectId": 40256218,
  "toUserId": 24288455,
  "fromUserId": 90996999,
  "role": "freelancer",
  "status": "active",
  "timeSubmitted": 1773327662,
  "description": "Very attentive and professional with great resources and project management. Will use again.",
  "submitDate": 1773327662,
  "rating": 5,
  "reviewProjectStatus": "complete",
  "ratingDetails": {
    "category_ratings": {
      "hire_again": 5,
      "quality": 5,
      "communication": 5,
      "expertise": 5,
      "professionalism": 5
    }
  },
  "reviewContext": {
    "review_type": "project",
    "context_id": 40256218,
    "context_name": "Resource Download Tracking System"
  },
  "projectDetails": {
    "id": 40256218,
    "title": "Resource Download Tracking System"
  }
}
```

## Field reference

### Profile fields (`type = "profile"`)

- **type** *(string, required)*: Record category.
- **id** *(number, required)*: Freelancer profile identifier.
- **url** *(string, required)*: Canonical profile URL.
- **title** *(string, required)*: Primary public title or display label.
- **publicName** *(string, optional)*: Public-facing profile name.
- **sourceUrl** *(string, optional)*: Search or directory URL where the record was found.
- **seedId** *(string, optional)*: Stable identifier for the originating input.
- **seedType** *(string, optional)*: Input type, such as `query` or `url`.
- **seedValue** *(string, optional)*: Original query or seed value.
- **pageIndex** *(integer, optional)*: Result page where the profile was discovered.
- **username** *(string, optional)*: Freelancer username.
- **description** *(string, optional)*: Public profile summary.
- **location** *(string, optional)*: Display-ready location string.
- **country** *(string, optional)*: Country name.
- **city** *(string, optional)*: City name.
- **tagline** *(string, optional)*: Short public tagline.
- **price** *(string, optional)*: Hourly rate as displayed.
- **priceNumeric** *(number, optional)*: Parsed numeric rate value.
- **currency** *(string, optional)*: Currency code or symbol context for the rate.
- **hourlyRate** *(number, optional)*: Numeric hourly rate.
- **rating** *(number, optional)*: Overall public rating.
- **reviewsCount** *(integer, optional)*: Public review count.
- **totalEarnings** *(number, optional)*: Reported lifetime earnings.
- **skills** *(array[string], optional)*: Visible skills list.
- **topSkills** *(array[object], optional)*: Summary of top skills.
- **topSkills[].skillId** *(string, optional)*: Skill identifier.
- **topSkills[].name** *(string, required)*: Skill name.
- **topSkills[].categoryId** *(string, optional)*: Skill category identifier.
- **topSkills[].earnings** *(number, optional)*: Earnings attributed to that skill when available.
- **topSkills[].seoUrl** *(string, optional)*: Skill slug.
- **skillDetails** *(array[object], optional)*: Detailed skill metadata.
- **skillDetails[].skillId** *(integer, required)*: Skill identifier.
- **skillDetails[].name** *(string, required)*: Skill name.
- **skillDetails[].isUserSkill** *(boolean, optional)*: Whether the skill is marked on the profile.
- **skillDetails[].repScore** *(integer, optional)*: Skill reputation score.
- **skillDetails[].skillJobCount** *(integer, optional)*: Number of jobs linked to the skill.
- **exams** *(array[object], optional)*: Public exam summaries.
- **exams[].examId** *(string, required)*: Exam identifier.
- **exams[].examName** *(string, optional)*: Exam name.
- **exams[].scorePercentage** *(integer, optional)*: Exam score percentage.
- **seoProfiles** *(array[object], optional)*: SEO profile snippets when available.
- **seoProfiles[].jobIds** *(array[string], optional)*: Related job identifiers.
- **seoProfiles[].headline** *(string, optional)*: Public SEO headline text.
- **seoProfiles[].seoText** *(string, optional)*: Public SEO summary text.
- **portfolios** *(array[object], optional)*: Portfolio items.
- **portfolios[].title** *(string, required)*: Portfolio title.
- **portfolios[].description** *(string, optional)*: Portfolio description.
- **reviewJobTitles** *(array[string], optional)*: Job titles associated with public review history.
- **jobReputation** *(array[object], optional)*: Per-job reputation summary.
- **jobReputation[].jobId** *(string, required)*: Job identifier.
- **jobReputation[].jobName** *(string, optional)*: Job name.
- **jobReputation[].score** *(integer, optional)*: Reputation score for the job.
- **jobReputation[].stars** *(number, optional)*: Star rating for the job.
- **geoLocation** *(object, optional)*: Geographic coordinates.
- **geoLocation.lat** *(number, optional)*: Latitude.
- **geoLocation.lon** *(number, optional)*: Longitude.
- **ratingBreakdown** *(object, optional)*: Detailed public rating components.
- **ratingBreakdown.quality** *(number, optional)*: Quality score.
- **ratingBreakdown.communication** *(number, optional)*: Communication score.
- **ratingBreakdown.expertise** *(number, optional)*: Expertise score.
- **ratingBreakdown.professionalism** *(number, optional)*: Professionalism score.
- **ratingBreakdown.hireAgain** *(number, optional)*: Hire-again score.
- **ratingBreakdown.onBudget** *(number, optional)*: On-budget score.
- **ratingBreakdown.onTime** *(number, optional)*: On-time score.
- **ratingBreakdown.positive** *(number, optional)*: Positive-feedback share.
- **ratingBreakdown.overall** *(number, optional)*: Overall score.
- **reviewStats** *(object, optional)*: Aggregated review statistics.
- **reviewStats.all** *(integer, optional)*: Total feedback entries.
- **reviewStats.reviews** *(integer, optional)*: Completed reviews.
- **reviewStats.incompleteReviews** *(integer, optional)*: Incomplete review count.
- **reviewStats.complete** *(integer, optional)*: Completed contracts.
- **reviewStats.incomplete** *(integer, optional)*: Incomplete contracts.
- **reviewStats.earnings** *(number, optional)*: Earnings total in review statistics.
- **reviewStats.completionRate** *(number, optional)*: Completion rate.
- **insigniaIds** *(array[integer], optional)*: Public insignia identifiers.
- **poolIds** *(array[integer], optional)*: Public pool identifiers.
- **freelancerReputation** *(number, optional)*: Reputation score.
- **freelancerReputationVariant** *(number, optional)*: Alternate reputation score variant.
- **score** *(integer, optional)*: Aggregate profile score.
- **stars** *(number, optional)*: Public star rating value.
- **hidden** *(boolean, optional)*: Hidden-status flag.
- **isOnline** *(boolean, optional)*: Online-status flag.
- **invited** *(boolean, optional)*: Invitation-status flag.
- **isSponsored** *(boolean, optional)*: Sponsored-listing flag.
- **isHighlighted** *(boolean, optional)*: Highlighted-listing flag.
- **membershipBadge** *(string, optional)*: Membership badge label shown on the listing.
- **logoUrl** *(string, optional)*: Logo image URL.
- **profileLogoUrl** *(string, optional)*: Profile image URL.
- **registrationDate** *(integer, optional)*: Registration timestamp when extended profile data is available.
- **coverImage** *(object, optional)*: Public cover-image object when available.
- **coverImage.current_image.id** *(integer, optional)*: Cover image identifier.
- **coverImage.current_image.url** *(string, optional)*: Cover image URL.
- **coverImage.current_image.width** *(integer, optional)*: Cover image width.
- **coverImage.current_image.height** *(integer, optional)*: Cover image height.
- **portfolioCount** *(integer, optional)*: Number of portfolio items reported by the extended profile.
- **timezone** *(object, optional)*: Public timezone metadata when available.
- **timezone.id** *(integer, optional)*: Timezone identifier.
- **timezone.country** *(string, optional)*: Country code tied to the timezone.
- **timezone.timezone** *(string, optional)*: IANA timezone name.
- **timezone.offset** *(number, optional)*: UTC offset.
- **profileDetails** *(object, optional)*: Extended public profile object when available.
- **profileDetails.id** *(number, optional)*: Profile identifier inside the extended object.
- **profileDetails.username** *(string, optional)*: Username inside the extended object.
- **profileDetails.display_name** *(string, optional)*: Display name inside the extended object.
- **profileDetails.public_name** *(string, optional)*: Public company or profile name.
- **profileDetails.profile_description** *(string, optional)*: Extended profile description.
- **profileDetails.hourly_rate** *(number, optional)*: Hourly rate in the extended object.
- **profileDetails.registration_date** *(integer, optional)*: Registration timestamp.
- **profileDetails.role** *(string, optional)*: Public role label.
- **profileDetails.preferred_freelancer** *(boolean, optional)*: Preferred Freelancer status when present.
- **profileDetails.location.country.name** *(string, optional)*: Country name.
- **profileDetails.location.country.code** *(string, optional)*: Country code.
- **profileDetails.location.city** *(string, optional)*: City name.
- **profileDetails.location.latitude** *(number, optional)*: Latitude.
- **profileDetails.location.longitude** *(number, optional)*: Longitude.
- **profileDetails.status.payment_verified** *(boolean, optional)*: Payment verification flag.
- **profileDetails.status.email_verified** *(boolean, optional)*: Email verification flag.
- **profileDetails.status.phone_verified** *(boolean, optional)*: Phone verification flag.
- **profileDetails.status.identity_verified** *(boolean, optional)*: Identity verification flag.
- **profileDetails.status.profile_complete** *(boolean, optional)*: Profile-complete flag.
- **profileDetails.primary_currency.id** *(integer, optional)*: Primary currency identifier.
- **profileDetails.primary_currency.code** *(string, optional)*: Primary currency code.
- **profileDetails.primary_currency.sign** *(string, optional)*: Primary currency sign.
- **profileDetails.primary_currency.name** *(string, optional)*: Primary currency name.
- **profileDetails.membership_package.id** *(integer, optional)*: Membership package identifier.
- **profileDetails.membership_package.name** *(string, optional)*: Membership package name.
- **profileDetails.jobs** *(array[object], optional)*: Public job or skill entries from the extended profile.
- **profileDetails.jobs[].id** *(integer, optional)*: Job identifier.
- **profileDetails.jobs[].name** *(string, optional)*: Job or skill name.
- **profileDetails.jobs[].seo_url** *(string, optional)*: Job or skill slug.
- **profileDetails.qualifications** *(array[object], optional)*: Public qualification entries when available.
- **profileDetails.badges** *(array[object], optional)*: Public badge entries when available.
- **fingerprint** *(string, optional)*: Stable record fingerprint for change tracking.

## Data guarantees & handling

- **Best-effort extraction:** fields may vary by region/session/availability/UI experiments.
- **Optional fields:** null-check in downstream code.
- **Deduplication:** recommend `type + ":" + id`.

## How to Run on Apify

1. Open the Actor in Apify Console.
2. Configure your search parameters, such as keywords, category, country, and optional rating or hourly-rate filters.
3. Set the maximum number of outputs to collect.
4. Click **Start** and wait for the run to finish.
5. Download results in JSON, CSV, Excel, or other supported formats.

## Scheduling & Automation

### Scheduling

**Automated Data Collection**

You can schedule runs to keep your freelancer dataset fresh without starting each run manually. This is useful for recurring sourcing, benchmarking, enrichment, and monitoring workflows.

- Navigate to **Schedules** in Apify Console
- Create a new schedule (daily, weekly, or custom cron)
- Configure input parameters
- Enable notifications for run completion
- Optional: add webhooks for automated processing

### Integration Options

- **Webhooks:** Trigger downstream actions when a run completes
- **Zapier:** Connect to 5,000+ apps without coding
- **Make (Integromat):** Build multi-step automation workflows
- **Google Sheets:** Export results to a spreadsheet
- **Slack/Discord:** Receive notifications and summaries
- **Email:** Send automated reports via email

## Performance

- **Small runs (< 1,000 outputs):** ~2-3 minutes
- **Medium runs (1,000-5,000 outputs):** ~5-15 minutes
- **Large runs (5,000+ outputs):** ~15-30 minutes

Execution time varies based on filters, result volume, and how much information is returned per record.

## Compliance & Ethics

### Responsible Data Collection

This actor collects publicly available freelancer profile information from [https://www.freelancer.com](https://www.freelancer.com) for legitimate business purposes, including talent market research and market analysis, sourcing and vendor discovery, and competitive benchmarking. Users are responsible for ensuring that collection, storage, and downstream use comply with applicable laws, regulations, contractual obligations, and the target site's terms. This section is informational and not legal advice.

### Best Practices

- Use collected data in accordance with applicable laws, regulations, and the target site's terms
- Respect individual privacy and personal information
- Use data responsibly and avoid disruptive or excessive collection
- Do not use this actor for spamming, harassment, or other harmful purposes
- Follow relevant data protection requirements where applicable (e.g., GDPR, CCPA)

## Support

If you need help, open an issue on the actor page or use the support section on the Apify actor listing. Include the input you used in redacted form, the run ID, a short description of expected versus actual behavior, and an optional small output sample so the issue can be reproduced quickly.