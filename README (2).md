# 📊 Meta Ad Performance Analysis | Power BI Dashboard

---

## 🧭 Executive Summary

This project delivers a comprehensive Power BI dashboard analyzing the performance of paid advertising campaigns across both Facebook and Instagram (Meta platforms). Built on a star-schema data model spanning four interrelated tables — `ad_events`, `ads`, `campaigns`, and `users` — the analysis tracks the complete conversion funnel from awareness to purchase across a combined **339.8K impressions** (216K on Facebook, 123.8K on Instagram) and a total budget of 2.54M. The dashboard supports platform-level filtering, enabling side-by-side comparison of Facebook vs. Instagram across all KPIs. Using DAX-powered measures and 10+ interactive visuals, it equips marketing teams to make confident, data-driven decisions on platform budget allocation, audience targeting, ad format selection, and campaign scheduling.

---

## 💼 Business Problem

A marketing team running paid campaigns on Facebook and Instagram lacked a unified, interactive view of campaign performance. Without it, four critical business challenges went unaddressed:

- **Invisible funnel leakage** — The team had no clear picture of where potential customers were dropping off between first impression and final purchase, making it impossible to diagnose conversion inefficiency.
- **Misallocated budget** — Spend was distributed across campaigns without visibility into which formats, platforms, or geographies were actually delivering ROI. Underperforming campaigns continued consuming budget unchecked.
- **Untargeted audience strategy** — No demographic or geographic breakdown existed to validate whether ads were reaching the right people — or to identify the highest-value audience segments.
- **Missed timing opportunities** — Without hourly and weekly trend data, ad delivery was not scheduled around peak engagement windows, leaving performance gains on the table.

The business impact was direct: wasted ad spend, low purchase conversion rates, and a marketing strategy flying partially blind. This project was built to solve all four problems in a single, decision-ready dashboard.

---

## 🏗️ Project Architecture

This project follows a structured, 4-phase end-to-end data analysis workflow — from raw data discovery to interactive business dashboards.

```
┌─────────────────────┐     ┌──────────────────────────┐     ┌──────────────────────────┐     ┌─────────────────────┐
│                     │     │                          │     │                          │     │                     │
│  01                 │     │  02                      │     │  03                      │     │  04                 │
│  Finding the        │────▶│  Data Cleaning &         │────▶│  Loading Data into       │────▶│  Data               │
│  Ideal Data         │     │  Standardization using AI│     │  Power BI & Creating     │     │  Visualization      │
│                     │     │                          │     │  DAX Measures            │     │                     │
└─────────────────────┘     └──────────────────────────┘     └──────────────────────────┘     └─────────────────────┘
  Identify and collect         Use AI to detect errors,         Import cleaned data into        Create interactive
  relevant, high-quality       remove duplicates, handle        Power BI and build DAX          dashboards and
  data from trusted sources.   missing values, and              measures to create powerful     visualizations that
                               standardize data.                analytical calculations.        turn data into
                                                                                                clear insights.
```

| Phase | What Was Done |
|---|---|
| **01 — Finding the Ideal Data** | Sourced a Meta Ads performance dataset modelled after real Facebook/Instagram ad platform data, covering 4 tables: `ad_events`, `ads`, `campaigns`, and `users` — across 216K impressions and 2.54M in total budget |
| **02 — Data Cleaning & Standardization using AI** | Applied AI-assisted data cleaning best practices to resolve missing values, duplicates, inconsistent categories, outliers, timestamp errors, misclassified event types, and orphaned foreign key records |
| **03 — Loading into Power BI & DAX Measures** | Imported cleaned tables into Power BI, designed a star-schema data model, built a Date Table for time intelligence, and engineered 12 dynamic DAX KPI measures |
| **04 — Data Visualization** | Designed a fully interactive single-page dashboard with 10+ visuals including funnel analysis, demographic breakdowns, geographic mapping, time-series trends, and campaign-tier scatter analysis |

> **Outcome:** Better Decisions · Data-Driven Insights · Accuracy & Consistency · Business Impact

---

## 🔬 Methodology

### 1. Domain Understanding & Business Requirements Gathering
- Studied the Meta Ads domain: how Facebook/Instagram capture ad events, how campaigns are structured, and how KPIs like CTR, ROAS, and Conversion Rate are defined and calculated.
- Collaborated with stakeholders to scope the report: Facebook and Instagram paid campaigns only (Messenger and Audience Network excluded; organic engagement excluded).
- Defined and documented 12 core KPIs with precise formulas agreed upon by the business.

### 2. Data Architecture — Star Schema Design
The dataset follows a **star schema** with one central fact table and three dimension tables:

| Table | Type | Purpose |
|---|---|---|
| `ad_events` | Fact Table | Event-level logs: Impressions, Clicks, Shares, Comments, Purchases |
| `ads` | Dimension | Ad metadata: platform, format, targeting (gender, age, interests) |
| `campaigns` | Dimension | Campaign strategy: budget, start/end dates, duration |
| `users` | Dimension | User demographics: gender, age, country, location, interests |

Relationships: `ad_events → ads → campaigns` and `ad_events → users`, enabling cross-dimensional analysis from a single fact source.

### 3. Data Collection & Cleaning
The raw dataset arrived with a range of quality issues that required systematic resolution before any analysis could begin. AI-assisted data cleaning best practices were applied throughout this stage:

**Issues identified and resolved:**

| Issue | Resolution |
|---|---|
| Missing values in `user_gender`, `user_age`, and `country` fields | Flagged nulls; classified as "Not Specified" in gender; excluded from demographic KPIs to avoid skewed rates |
| Duplicate event records in `ad_events` | Detected and removed duplicate `event_id` entries to prevent double-counting impressions and clicks |
| Inconsistent categorical values (e.g., `"female"`, `"Female"`, `"F"` in gender field) | Standardized all categorical fields to a single consistent format using value replacement |
| Outlier budget values in `campaigns` table (abnormally high/low entries) | Identified via statistical distribution checks; validated against business context before inclusion |
| Timestamp formatting inconsistencies across rows | Parsed and normalized all `timestamp` values to a unified `YYYY-MM-DD HH:MM:SS` format |
| Misclassified `event_type` values not matching the defined taxonomy | Filtered to retain only valid event types: Impression, Click, Share, Comment, Purchase |
| Orphaned records in `ad_events` with no matching `ad_id` or `user_id` | Removed via referential integrity checks against the `ads` and `users` dimension tables |

After cleaning, the dataset was loaded into Power BI and the star-schema relationships were established. A dedicated **Date Table** was built linked to `ad_events[timestamp]` to enable time intelligence across day, week, and month hierarchies. Derived columns `day_of_week` and `time_of_day` were created from normalized timestamps to support behavioral pattern analysis.

### 4. KPI Engineering (DAX Measures)
Created all 12 business KPIs as DAX measures, including:
- `CTR = (Clicks ÷ Impressions) × 100`
- `Engagement Rate = (Engagements ÷ Impressions) × 100`
- `Conversion Rate = (Purchases ÷ Clicks) × 100`
- `Purchase Rate = (Purchases ÷ Impressions) × 100`
- `Avg. Budget per Campaign = Total Budget ÷ Campaign Count`

All KPIs were made **dynamic via a parameter slicer**, allowing any metric to power all visuals simultaneously with a single selection.

### 5. Dashboard Visualization (Power BI)
Built 10+ interactive visuals across a single-page dashboard:

| Visual | Purpose |
|---|---|
| 12 KPI Cards | Instant performance snapshot at the top of the dashboard |
| Donut Chart | Purchase/engagement split by gender |
| Bar Chart | Performance by target age group |
| World Map | Geographic distribution of purchases and engagement |
| Calendar Heat Map | Daily engagement pattern by month |
| Stacked Column Chart | Weekly trend broken down by ad type |
| Area/Line Chart | Hourly engagement trend (hours 0–23) |
| Matrix Table | Ad type performance across all KPIs side by side |
| Scatter Plot | Budget vs. Purchases by campaign, color-coded by campaign tier |
| Conversion Funnel | Impressions → Clicks → Purchases drop-off visualization |

Dynamic slicers for platform (Facebook / Instagram), campaign, interests, and metric selector enable full interactivity across all visuals.

---

## 🛠️ Skills

- **Domain Knowledge (Digital Advertising)** — Understanding Meta ad platform architecture, funnel metrics, and campaign lifecycle
- **Business Requirements Analysis** — Translating stakeholder needs into scoped KPI definitions and dashboard specifications (BRD)
- **AI-Assisted Data Cleaning** — Applying AI data analysis best practices to detect and resolve missing values, duplicates, inconsistent categories, outliers, timestamp formatting issues, misclassified event types, and orphaned records across a multi-table dataset
- **Data Modeling (Power BI)** — Designing and implementing a star-schema data model with a fact table and three dimension tables
- **DAX (Data Analysis Expressions)** — Engineering 12 calculated KPI measures including CTR, Engagement Rate, Conversion Rate, and Purchase Rate
- **Data Visualization (Power BI)** — Building multi-layer, interactive dashboards with dynamic metric selectors and cross-filtering
- **Funnel Analysis** — Identifying and quantifying conversion drop-off at each stage of the awareness-to-purchase journey
- **Audience Segmentation** — Demographic and geographic breakdown of ad performance to validate targeting efficiency
- **Time-Series & Behavioral Analysis** — Extracting hourly, daily, and weekly engagement patterns to optimize campaign scheduling
- **Campaign Performance Analysis** — Benchmarking ad formats, platforms, budget tiers, and campaign-level ROI
- **Data Storytelling** — Structuring analytical findings into clear, business-oriented narratives for non-technical stakeholders

---

## 📈 Results & Business Recommendations

### Platform-Level KPI Comparison

The dashboard supports platform filtering, revealing meaningful performance differences between Facebook and Instagram across every core KPI:

| KPI | 🔵 Facebook | 📷 Instagram | Insight |
|---|---|---|---|
| **Impressions** | 216K | 123.8K | Facebook delivers 75% more reach |
| **Clicks** | 25.4K | 14.7K | Facebook generates ~73% more clicks |
| **Purchases** | 1.3K | 708 | Facebook drives nearly 2× more conversions |
| **CTR** | 11.76% | **11.86%** | Instagram edges ahead on click-through rate |
| **Conversion Rate** | **5.21%** | 4.82% | Facebook converts clicks to purchases more efficiently |
| **Engagement Rate** | 13.56% | **13.60%** | Instagram marginally stronger on engagement |
| **Purchase Rate** | **0.61%** | 0.57% | Facebook more efficient at turning impressions into sales |
| **Total Budget** | 2.54M (shared) | 2.54M (shared) | Same total budget across both platforms |

> **Key takeaway:** Facebook wins on volume and conversion efficiency. Instagram wins on CTR and engagement rate. Both platforms are strong at the top of the funnel but leak purchases at the bottom — suggesting the conversion gap is a landing page / offer problem, not a platform-specific creative problem.

---

### Ad Format Performance by Platform

**🔵 Facebook — Ad Type Breakdown**

| Ad Type | Impressions | Purchases | Clicks | CTR | Engagement | Purchase Rate |
|---|---|---|---|---|---|---|
| Stories | 24,219 | 156 | 2,885 | 11.91% | 3,352 | 0.64% |
| Image | 17,233 | 111 | 2,042 | 11.85% | 2,337 | 0.64% |
| **Video** | 15,542 | 93 | 1,919 | **12.35%** | 2,222 | 0.60% |
| Carousel | 16,217 | 95 | 1,904 | 11.74% | 2,185 | 0.59% |

> On Facebook, **Video** leads with the highest CTR (12.35%). **Stories** drives the most impressions and purchases in absolute terms.

**📷 Instagram — Ad Type Breakdown**

| Ad Type | Impressions | Purchases | Clicks | CTR | Engagement | Purchase Rate |
|---|---|---|---|---|---|---|
| **Carousel** | 13,223 | 90 | 1,536 | 11.62% | 1,765 | **0.68%** |
| Image | 12,655 | 68 | 1,510 | **11.93%** | 1,731 | 0.54% |
| Stories | 12,683 | 71 | 1,490 | 11.75% | 1,713 | 0.56% |
| Video | 3,561 | 12 | 412 | 11.57% | 476 | 0.34% |

> On Instagram, **Carousel** achieves the highest Purchase Rate (0.68%) and **Image** leads on CTR (11.93%). Notably, **Video performs significantly worse on Instagram** (3,561 impressions, 0.34% purchase rate) — a sharp contrast to its dominance on Facebook.

---

### Audience Breakdown by Platform

| Segment | 🔵 Facebook | 📷 Instagram |
|---|---|---|
| Female | **43%** of engagements | 33% of engagements |
| Male | 20% of engagements | 27% of engagements |
| Peak Age Group | 18–30 | 18–30 |

> Facebook audiences are strongly female-skewed (43% vs 20% male). Instagram shows a more balanced gender split. Both platforms peak in the 18–30 age group with sharp drop-off after 35.

---

### Business Recommendations

**1. Fix the Conversion Funnel on Both Platforms — Priority #1**
Both Facebook (0.61%) and Instagram (0.57%) have critically low purchase rates despite strong CTR and engagement. The drop-off is happening after the click — pointing to landing page experience, offer relevance, or checkout friction. Invest in landing page optimization, stronger CTAs, and retargeting campaigns on both platforms.

**2. Allocate Budget Differently Per Platform**
Facebook delivers higher volume and stronger conversion efficiency — it should receive the larger share of the budget. Instagram's edge in CTR and engagement rate makes it valuable for awareness and top-of-funnel campaigns. Suggested split: **~60% Facebook / ~40% Instagram**, with Instagram spend focused on reach and brand engagement.

**3. Use Video on Facebook, Carousel & Image on Instagram**
Video ads dominate on Facebook (CTR: 12.35%) but dramatically underperform on Instagram (CTR: 11.57%, Purchase Rate: 0.34%). On Instagram, Carousel achieves the highest purchase rate (0.68%) and Image leads on CTR (11.93%). Run platform-specific creative strategies — do not simply reuse the same ad format across both.

**4. Target Female Audiences on Facebook, Broader Audiences on Instagram**
Facebook's strongly female-skewed engagement (43%) warrants female-targeted creative and messaging. Instagram's more balanced gender distribution (33% female / 27% male) supports broader creative approaches. Both platforms should prioritize the 18–30 age group.

**5. Execute a Two-Track Geographic Strategy**
- **India & Brazil:** High volume, high engagement markets — run broad, high-frequency campaigns with localized creative.
- **Germany & UK:** Higher purchasing power markets — run premium, conversion-optimized campaigns with stronger offers and sharper landing page experiences.

**6. Concentrate Ad Delivery in Afternoon & Evening Windows**
Hourly trend data on both platforms shows consistent engagement peaks between 15:00 and 20:00, with lowest activity from 0:00 to 5:00. Re-weight automated bid strategies and ad scheduling budgets toward the 15:00–20:00 window on both platforms.

**7. Formalize a Weekly Promotional Cadence**
Calendar data shows recurring engagement spikes around the 19th–21st and 25th–27th of each month, likely tied to promotions or campaign launches. Structuring a repeatable bi-weekly promotional schedule could systematically replicate these performance peaks.

**8. Audit and Reallocate Overspending Campaign Budgets**
The Budget vs. Performance scatter analysis reveals a cluster of campaigns in the "Overspending" tier — high budget, disproportionately low purchases. Immediately review these campaigns and redirect their budgets toward "High Performer" and "Hidden Gem" tier campaigns.

---

## 🚀 Next Steps

- **Predictive Purchase Modeling:** Build a machine learning model (logistic regression or gradient boosting) to predict purchase probability per user based on demographic profile, ad type, time of day, and platform — enabling smarter bid optimization and audience pre-qualification.
- **Targeting Accuracy Analysis:** Cross-reference `ads[target_gender]` and `ads[target_age_group]` against actual user engagement data from the `users` table to quantify targeting mismatch and recommend audience refinements per platform.
- **A/B Testing Framework:** Establish a structured A/B testing pipeline for landing pages and ad creatives — with separate test tracks for Facebook and Instagram given their distinct format performance profiles.
- **Platform Budget Optimization Model:** Build a data-driven budget allocation model that dynamically recommends the optimal Facebook vs. Instagram spend split based on real-time KPI performance and campaign objectives (awareness vs. conversion).
- **Automated Reporting Pipeline:** Deploy the dashboard to Power BI Service with scheduled daily data refresh and configure automated email alerts for KPI threshold breaches per platform (e.g., Facebook Conversion Rate dropping below 4.5%, Instagram Purchase Rate below 0.4%).
- **Customer Lifetime Value (CLV) Integration:** Enrich the dataset with post-purchase behavioral data to connect ad spend — broken down by platform — to long-term customer value, shifting optimization from single-purchase conversions to high-LTV customer acquisition.
- **Multi-Month & Seasonality Analysis:** Extend the dataset beyond a single month to detect seasonal performance patterns per platform and build an annual campaign planning model.
