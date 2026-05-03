# SKU Intelligence Hub
### Portfolio Rationalization & Margin Recovery System

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-336791?style=flat&logo=postgresql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=flat&logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Complete-2ECC71?style=flat)
![Domain](https://img.shields.io/badge/Domain-D2C%20%7C%20Retail%20%7C%20Ecommerce-8E44AD?style=flat)

---

## The Business Problem

A D2C personal care brand with 280 SKUs and ₹100Cr in annual revenue was operating with a bloated product catalog. Revenue looked healthy. Margins did not.

**Nobody could answer three basic questions:**

- Which SKUs are actually profitable after carrying costs?
- Which SKUs should be discontinued - and in what order?
- Which low-revenue SKUs are secretly driving new customer acquisition?

Without answers, the brand was making procurement decisions on gut
feel and internal politics. Dead-weight SKUs survived every review
cycle. Complexity costs mounted invisibly. Margin leaked quietly.

This project builds the system that answers all three questions.

---

## The Decision This System Enables

> *"Which SKUs should we discontinue, consolidate, or accelerate -
> and in what order - to maximise margin without breaking the
> customer acquisition funnel?"*

---

## Key Findings

| Finding | Result |
|---|---|
| SKUs driving 80% of revenue | 60 out of 258 (23%) |
| SKUs generating only 10% of revenue | 143 SKUs (55% of catalog) |
| SKUs with negative true margin | 44 SKUs |
| SKUs recommended for immediate removal | 39 SKUs |
| Avg true margin of Kill SKUs | -54% |
| Gateway repeat rate (customers worth protecting) | 75.1% |

**The critical insight:** 44 SKUs appeared profitable on gross margin
but flipped to negative once complexity costs - storage, handling,
and packaging overhead - were allocated per SKU. Standard reporting
missed this entirely.

---

## What Makes This Project Different

Most SKU analysis stops at revenue contribution.

This system adds three layers that change the decision:

**Layer 1 - True Margin**
Gross margin minus complexity cost per SKU. Reveals SKUs that look
profitable but are actively destroying value after ops overhead.

**Layer 2 - Demand Velocity**
Not just current revenue - but whether each SKU is growing,
stable, or dying. A declining SKU with decent revenue today is
a guaranteed problem tomorrow.

**Layer 3 - Gateway Intelligence**
Some low-revenue SKUs are the first product 600+ customers ever
buy. Those customers return at a 75% rate with ₹1,490 in 90-day
LTV. Killing a gateway SKU doesn't just lose its own revenue -
it closes a customer acquisition door. This layer protects against
that mistake.

---

## Project Architecture

SKU Rationalization Project/
│
├── Data/
│   ├── Raw/                        ← Simulated source data (4 tables)
│   └── Cleaned/                    ← Cleaned, validated datasets
│
├── notebooks/
│   ├── 01_data_generation.ipynb    ← Simulate realistic D2C dataset
│   ├── 02_data_cleaning.ipynb      ← Clean, validate, enrich
│   ├── 03_exploratory_analysis.ipynb ← EDA — revenue, velocity, gateway
│   ├── 04_core_analysis.ipynb      ← ABC-XYZ, true margin, scoring model
│   └── 05_insights_findings.ipynb  ← Business insights & findings
│
├── outputs/
│   ├── sku_rationalization_master.csv  ← Scored SKU table (all dimensions)
│   ├── kill_list.csv                   ← 39 SKUs prioritised for removal
│   ├── recommendation_summary.csv      ← Summary by recommendation bucket
│   ├── cannibalization_analysis.csv    ← New SKU launch impact analysis
│   ├── 01_pareto_revenue.png           ← Revenue concentration chart
│   ├── 02_monthly_trend_by_category.png ← Category revenue trends
│   └── 03_sku_portfolio_matrix.png     ← Revenue vs margin scatter
│
└── dashboard/
└── SKU_Intelligence_Hub.pbix   ← Power BI dashboard (5 pages)

---

## Dataset

Simulated D2C personal care brand dataset.
Designed to reflect realistic business patterns.

| Table | Rows | Description |
|---|---|---|
| sku_master | 280 | Product catalog with MRP, cost, category, launch date |
| sales_transactions | 85,000 | Order-level data, Jan 2024 - Dec 2025 |
| sku_ops_cost | 280 | Storage, handling, packaging complexity per SKU |
| customer_first_purchase | 33,881 | First SKU bought per customer + LTV + repeat behavior |

**Built-in business patterns:**
- 80/20 revenue concentration (60 SKUs drive 80% of revenue)
- 8% return rate (realistic for personal care D2C)
- 73.7% repeat customer rate
- Seasonal Q4 demand spike

---

## Analytical Framework

### Step 1 - ABC Classification (Revenue Contribution)
Segments SKUs into three tiers based on cumulative revenue share.

| Class | SKU Count | Revenue Share |
|---|---|---|
| A | 38 | 69.3% |
| B | 77 | 20.7% |
| C | 143 | 10.0% |

### Step 2 - XYZ Classification (Demand Stability)
Coefficient of Variation (CV) on monthly revenue per SKU.
Low CV = predictable demand (X). High CV = erratic (Z).

| Class | SKU Count | Demand Pattern |
|---|---|---|
| X | 113 | Stable, predictable |
| Y | 145 | Moderate variation |
| Z | 0 | Erratic (none in this dataset) |

### Step 3 - True Margin Calculation

True Profit = Gross Profit
- Total Complexity Cost (storage + packaging overhead × 24 months)
- Total Handling Cost (pick/pack per unit × units sold)True Margin % = True Profit ÷ Total Revenue × 100

### Step 4 - Gateway Value Score
Measures each SKU's role in acquiring new customers.

Gateway Score Raw = Times as First Purchase × Repeat Rate

Normalised to 1-3 scale:

- 3 = High gateway value (top 34% of acquisition SKUs)
- 2 = Medium
- 1 = Low or no gateway role

### Step 5 - Rationalization Score (Composite)

- Rationalization Score = ABC Score  × 0.35   (revenue contribution - most weight)
- Margin Score × 0.30  (true margin health)
- XYZ Score  × 0.20   (demand stability)
- Gateway Score × 0.15 (acquisition role)

Scale: 1.0 (worst) to 3.0 (best)

### Step 6 - Recommendation Engine

- Score ≥ 2.5                          → Accelerate
- Score < 2.5, ABC = A                 → Protect
- Gateway Score = 3, acquisitions ≥ 300 → Protect (override)
- True margin < 0, Declining, Gateway = 1 → Kill
- Score < 1.5                          → Kill
- All others                           → Watch

---

## Rationalization Output

| Recommendation | SKU Count | Revenue Share | Avg True Margin |
|---|---|---|---|
| Protect | 51 | 77.0% | 51.3% |
| Watch | 137 | 14.7% | 19.1% |
| Accelerate | 31 | 6.9% | 43.1% |
| Kill | 39 | 1.4% | -54.0% |

**39 SKUs flagged for immediate discontinuation.**
They generate 1.4% of revenue at -54% average true margin.

---

## The Gateway Trap - Why This Is Hard

The most important insight in the project.

Some SKUs score poorly on revenue and margin but are
disproportionately responsible for bringing customers into the brand.

Gateway SKUs (score = 3)  : 93 SKUs
Avg repeat rate            : 75.1%
Avg 90-day LTV             : ₹1,490
At-risk gateways (Watch/Kill): flagged for manual review

A standard rationalization model would kill some of these.
This system protects them using a gateway override rule -
and flags every at-risk gateway SKU for manual review
before any discontinuation decision is made.

---

## Category Intelligence

| Category | SKUs | Revenue Share | Avg True Margin | Kill Count |
|---|---|---|---|---|
| Haircare | 42 | 63.9% | 51.6% | 0 |
| Skincare | 46 | 17.3% | 22.5% | 8 |
| Baby Care | 42 | 5.5% | 26.6% | 5 |
| Nutrition | 41 | 4.4% | -8.2% | 5 |
| Oral Care | 44 | 4.8% | 14.3% | 7 |
| Body Care | 43 | 4.0% | -3.6% | 14 |

**Haircare carries the brand.** It generates 63.9% of revenue
at 51.6% true margin with zero SKUs flagged for removal.

**Body Care and Nutrition are the problem categories.**
Combined negative average true margin and 19 of the 39
kill candidates. This is a category strategy finding -
not just an SKU finding.

---

## Power BI Dashboard - 5 Pages

| Page | Purpose | Key Visual |
|---|---|---|
| Executive Command | C-suite portfolio overview | Donut + category treemap |
| Portfolio Matrix | SKU-level decision view | 4-quadrant scatter plot |
| Margin Intelligence | True vs gross margin story | Margin waterfall chart |
| Gateway & Risk | Acquisition protection layer | Gateway risk scatter |
| Kill List & Actions | Discontinuation command centre | Priority-ranked kill list |

**Dashboard features:**
- Cross-page slicer sync (Category, Recommendation, ABC Class)
- Tooltip pages on scatter plots (rich SKU detail on hover)
- Dynamic alert banners (context-sensitive warnings)
- Conditional formatting on all decision tables
- Page navigation buttons (no tab bar)
- Priority rank column (lowest score = act first)

---

## Tools & Skills

| Tool | How It Was Used |
|---|---|
| Python | Data generation, cleaning, ABC-XYZ classification, true margin modeling, gateway scoring, cannibalization detection |
| PostgreSQL | Aggregation queries, SKU-level joins, revenue concentration analysis |
| Power BI | 5-page interactive dashboard, DAX measures, cross-filter slicers, tooltip pages, conditional formatting |
| Excel | Scenario modeling for margin impact and procurement planning |

**Python libraries:** pandas, numpy, matplotlib, scipy, os

---

## Business Impact

| Action | Impact |
|---|---|
| Remove 39 Kill SKUs | Eliminate -54% avg margin drag, recover ops cost |
| Protect 51 core SKUs | Safeguard 77% of portfolio revenue |
| Review 137 Watch SKUs quarterly | Convert hidden Accelerate/Kill candidates |
| Protect gateway SKUs | Preserve 75.1% repeat rate customer funnel |
| Category strategy review | Address Body Care and Nutrition margin crisis |

**Without this system:** SKU decisions made on gut feel.
Complexity costs invisible. Gateway SKUs killed by mistake.
Margin continues leaking quietly across 143 underperforming SKUs.

---

## How to Run

**1. Clone the repository**
```bash
git clone https://github.com/yourusername/sku-intelligence-hub.git
cd sku-intelligence-hub
```

**2. Install dependencies**
```bash
pip install pandas numpy matplotlib scipy
```

**3. Run notebooks in order**

01_data_generation.ipynb    → generates Raw data files
02_data_cleaning.ipynb      → produces Cleaned data files
03_exploratory_analysis.ipynb → produces charts in outputs/
04_core_analysis.ipynb      → produces scored master table
05_insights_findings.ipynb  → produces final insight charts

**4. Open dashboard**

Open dashboard/SKU_Intelligence_Hub.pbix in Power BI Desktop
Refresh data source paths to your local outputs/ directory

---

## Author

**Mohsin Raza**
Business / BI Data Analyst
[LinkedIn](https://linkedin.com/in/mohsinraza-data) •
[GitHub](https://github.com/MohsinR11)

---

*Built as part of a portfolio of decision intelligence projects
for Business Analyst and BI Analyst roles in the Indian market.*
