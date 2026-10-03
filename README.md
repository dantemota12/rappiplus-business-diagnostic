# RappiPlus: From Data to Business Decisions

End-to-end analysis of **RappiPlus**, a subscription e-commerce service: data quality, profitability, conversion funnel, user retention, an A/B test on the checkout UI, and an executive dashboard in Power BI.

## Business Questions

1. Can we trust the data? (data quality)
2. Is the business profitable? (revenue, costs, marketing spend, profit)
3. Where do users drop off? (conversion funnel)
4. Do users come back? (cohort retention)
5. Does the new checkout UI improve conversion? (statistical test)
6. How do we communicate the results? (BI dashboard)

## Datasets

| Source | Content |
|---|---|
| `rappiplus_orders_raw.csv` | Orders, prices, discounts and revenue (25,100 raw rows) |
| `rappiplus_catalog.csv` | Product costs, categories and suppliers |
| `rappiplus_marketing_spend.csv` | Marketing spend by channel and country |
| `events`, `users`, `user_activity` (SQL) | User behavior on the platform |
| `experiment_checkout_ui.csv` | A/B test results on the checkout (10,000 users) |

> Column names and some category values are in Spanish, as in the original datasets.

## Methodology

### 1. Data Quality (Python, pandas)
- Converted dates to datetime
- Removed rows with invalid quantities and 100 duplicate orders
- Checked consistency between `monto_total` and `cantidad × precio_unitario − descuento`
- Handled null values by labeling them as unknown
- Exported clean datasets for the dashboard

### 2. Profitability (Python, pandas, Matplotlib)
Merged orders with the product catalog to calculate revenue, product costs, marketing spend and profit, plus average ticket, average quantity per order, best-selling product and spend by channel.

### 3. Conversion Funnel (SQL)
Counted unique users per event and calculated step-by-step and overall conversion rates across the six funnel stages.

### 4. Cohort Retention (SQL)
Grouped users by sign-up month and calculated weekly retention (weeks 1 to 3) for each cohort, visualized as a heatmap.

### 5. A/B Test (Python, statsmodels)
Two-proportion z-test comparing conversion between the control and treatment checkout designs (α = 0.05).

### 6. Dashboard (Power BI)
Two-page interactive report with a date table (`Dim_Fecha`), DAX measures (Revenue, Profit, Marketing Spend, Average Ticket, Average Quantity per Order, Revenue YTD), slicers and a detail page with drill-through.

## Key Results

### Profitability

| Metric | Value |
|---|---|
| Revenue | $51,966,981.56 |
| Product costs | $43,124,069.01 |
| Marketing spend | $2,871,843.53 |
| **Profit** | **$5,971,069.02** |
| Average ticket | $2,083.18 |
| Average quantity per order | 7.12 |
| Best-selling product | Laptop-Gaming-16GB |

Marketing spend by channel: Social $918,043.21 · Organic $913,533.01 · Paid Search $863,088.21 · Unknown $177,179.10.

### Conversion Funnel

| Stage | Unique users |
|---|---|
| first_visit | 7,796 |
| select_item | 7,582 |
| add_to_cart | 7,634 |
| begin_checkout | 7,208 |
| add_payment_info | 6,250 |
| purchase | 6,240 |

- Overall conversion from first visit to purchase: **80.04%**.
- The biggest drop happens between `begin_checkout` and `add_payment_info` (86.7% step conversion).
- `add_to_cart` exceeds 100% of the previous step because users can add products to the cart directly from the home page or recommendations, without a separate item-selection event.

### Retention

Retention is stable across the January–May 2025 cohorts, at roughly **40–43%** weekly activity during weeks 1 to 3, with no sharp decay.

### A/B Test

| Group | Conversion |
|---|---|
| Control | 15.69% |
| Treatment | 16.29% |

z = -0.81, **p = 0.416**: the difference (0.6 percentage points) is **not statistically significant**, so the change is not recommended based on this experiment alone.

## Recommendations

1. **Audit marketing tracking:** $177,179.10 of spend has no channel assigned, which makes it impossible to measure the true ROI of those campaigns.
2. **Optimize the payment step:** simplify the payment form, offer local payment methods and reduce mandatory checkout steps.
3. **Boost early retention:** run automated email or push campaigns between weeks 1 and 3 after sign-up to encourage repeat purchases.
4. **Do not ship the new checkout UI** on conversion grounds alone, since the test showed no significant effect.

## Dashboard

**Page 1: Executive Summary**
- KPI cards: Revenue, Profit, Marketing Spend, Average Ticket, Average Quantity per Order
- Monthly revenue line chart and Revenue YTD line chart
- Revenue and profit by product (bar chart)
- Slicers: product category, year-month, country, device

**Page 2: Detail Analysis**
- Detailed table by product: quantity, revenue, unit cost, profit and order ID
- Quantity sold by product (column chart)
- Drill-through navigation

## Tools

Python · pandas · Matplotlib · Seaborn · statsmodels · SQL (PostgreSQL) · SQLAlchemy · Power BI · DAX

## Files

- `sprint 12 - cuaderno de jupyter - S12 Estudiante Proyecto Final.ipynb`: full analysis notebook (data quality, profitability, funnel, retention, A/B test and recommendations).
- `sprint 12 - archivo de power bi - Sprint 12 Proyecto Final Dashboard.pbix`: Power BI dashboard.
- `images/`: screenshots of the dashboard pages.
- [Download the dashboard and clean datasets from Google Drive](https://drive.google.com/file/d/1whCeWcOD1uFywF_LTG1dBvkmSMOD2Iff/view?usp=drive_link)

## Author

**Dante Mota**: [GitHub](https://github.com/dantemota12)
