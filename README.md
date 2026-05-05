# 📊 Meta Ad Performance Analysis | Power BI Dashboard

---

## 🧭 Executive Summary

This project delivers a comprehensive Power BI dashboard analyzing the performance of paid advertising campaigns running on Facebook and Instagram (Meta platforms). Built on a star-schema data model spanning four interrelated tables — `ad_events`, `ads`, `campaigns`, and `users` — the analysis tracks the complete conversion funnel from awareness to purchase across 216K impressions and a total budget of 2.54M. Using DAX-powered KPIs and 10+ interactive visuals, the dashboard equips marketing teams to make confident, data-driven decisions on budget allocation, audience targeting, ad format selection, and campaign scheduling.

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

<img width="1000" height="529" alt="ChatGPT Image May 5, 2026, 10_46_52 AM" src="https://github.com/user-attachments/assets/2f8a6dc1-0bdd-4b66-b5cf-368e524cd5aa" />


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

### Key Performance Findings

| KPI | Value | Interpretation |
|---|---|---|
| Impressions | 216K | Strong reach achieved across campaigns |
| CTR | 11.76% | Far above industry average (~1–2%) — creatives are highly effective |
| Engagement Rate | 13.56% | Strong content resonance with the target audience |
| Conversion Rate | 5.21% | Moderate — meaningful room for improvement via funnel optimization |
| Purchase Rate | 0.61% | Low — sharp drop-off from engagement to purchase identified |
| Total Budget | 2.54M | Avg. 50.7K per campaign across multiple active campaigns |

### Ad Format Performance Breakdown

| Ad Type | Impressions | Clicks | CTR | Conversion Rate | Engagement Rate |
|---|---|---|---|---|---|
| Video | 46K | 5K | **11.9%** | **5.2%** | **13.7%** |
| Stories | 72K | 8K | 11.8% | 5.2% | 13.6% |
| Image | 51K | 6K | 11.7% | 4.9% | 13.5% |
| Carousel | 48K | 6K | 11.7% | 5.1% | 13.4% |

### Business Recommendations

**1. Fix the Conversion Funnel — Priority #1**
The ads excel at awareness and engagement, but only 0.61% of impressions result in a purchase. This is the single largest opportunity in the project. Invest immediately in landing page optimization, stronger and more targeted CTAs, and retargeting campaigns to re-engage users who clicked but did not convert.

**2. Shift Budget to Video and Stories Formats**
Video ads deliver the highest CTR (11.9%), Conversion Rate (5.2%), and Engagement Rate (13.7%). Stories ads generate the most impressions (72K) with strong conversion. Reallocate budget away from Carousel and Image formats toward these two high-performing formats.

**3. Center Targeting on Young Females, Ages 18–30**
Females account for 43% of engagements vs. 22% for males. The 18–30 age group drives the majority of interactions, with a sharp drop-off after age 35. All creative strategy and audience targeting parameters should be built around this primary segment.

**4. Execute a Two-Track Geographic Strategy**
- **India & Brazil:** High volume, high engagement markets — run broad, high-frequency campaigns with localized creative.
- **Germany & UK:** Higher purchasing power markets — run premium, conversion-optimized campaigns with stronger offers and sharper landing page experiences.

**5. Concentrate Ad Delivery in Afternoon & Evening Windows**
Hourly trend data shows consistent engagement peaks between 15:00 and 20:00, with the lowest activity from 0:00 to 5:00. Re-weight automated bid strategies and ad scheduling budgets toward the 15:00–20:00 window to maximize impressions during peak user activity.

**6. Formalize a Weekly Promotional Cadence**
Calendar data shows recurring engagement spikes around the 19th–21st and 25th–27th of each month, likely tied to promotions or campaign launches. Structuring a repeatable bi-weekly promotional schedule could systematically replicate these performance peaks.

**7. Audit and Reallocate Overspending Campaign Budgets**
The Budget vs. Performance scatter analysis reveals a cluster of campaigns in the "Overspending" tier — high budget, disproportionately low purchases. Immediately review these campaigns and redirect their budgets toward "High Performer" and "Hidden Gem" tier campaigns.

---

## 🚀 Next Steps

- **Predictive Purchase Modeling:** Build a machine learning model (logistic regression or gradient boosting) to predict purchase probability per user based on demographic profile, ad type, time of day, and platform — enabling smarter bid optimization and audience pre-qualification.
- **Targeting Accuracy Analysis:** Cross-reference `ads[target_gender]` and `ads[target_age_group]` against actual user engagement data from the `users` table to quantify targeting mismatch and recommend audience refinements.
- **A/B Testing Framework:** Establish a structured A/B testing pipeline for landing pages and ad creatives to systematically lift conversion rate through controlled experimentation.
- **Platform Deep Dive (Facebook vs. Instagram):** Expand the analysis to compare Facebook and Instagram performance head-to-head across all KPIs — impressions, CTR, conversion rate, and purchase rate — to inform platform-specific budget splits.
- **Automated Reporting Pipeline:** Deploy the dashboard to Power BI Service with scheduled daily data refresh and configure automated email alerts for KPI threshold breaches (e.g., CTR dropping below 8%, Purchase Rate below 0.4%).
- **Customer Lifetime Value (CLV) Integration:** Enrich the dataset with post-purchase behavioral data to connect ad spend to long-term customer value — shifting optimization from single-purchase conversions to high-LTV customer acquisition.
- **Multi-Month & Seasonality Analysis:** Extend the dataset beyond a single month to detect seasonal performance patterns and build an annual campaign planning model.
