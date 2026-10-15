# Retail-Sales-Analysis

An end-to-end retail analytics project: a Jupyter Notebook that analyses sales, product, store and inventory data, and a self-contained **executive dashboard** (single HTML file) that presents the findings to senior management.

**Reporting period:** Jan 2021 – Oct 2025 · **Inventory snapshot:** 31 Oct 2025

## Project Overview

BlueMart operates 50 stores across three cities (Dubai, Abu Dhabi, Sharjah) and sells 200 products through five channels. This project answers one question for management:

> After analysing this retail data, what do we need to know?

The work has two parts:

1. **Analysis notebook:** loads, checks and cleans the data, joins the tables and answers five business questions plus three ad hoc inventory and profitability questions.
2. **Executive dashboard:** presents only the final business results (KPIs, charts, findings and takeaways). Data cleaning, code and methodology are intentionally left out.

## Repository Structure

```
.
├── Retail_SalesAnalysis.ipynb     # Full analysis (data prep, analysis, charts)
├── retail_dashboard.html          # Self-contained executive dashboard
├── data/                          # Source CSV files (add locally, see below)
│   ├── bm_sales.csv
│   ├── bm_skus.csv
│   ├── bm_stores.csv
│   └── bm_inventory.csv
└── README.md
```

> The CSV files are not included in this repository. Place them in the same folder as the notebook (or update the paths in the first cells).

## Data

| File | Rows | Description | Key columns |
|---|---|---|---|
| `bm_sales.csv` | 641,843 | Sales transactions | `date`, `store_id`, `sku_id`, `customer_id`, `quantity`, `unit_price`, `total_value`, `channel`, `discount_pct` |
| `bm_skus.csv` | 200 | Product master | `sku_id`, `sku_name`, `category`, `subcategory`, `unit_price`, `cost_price`, `brand` |
| `bm_stores.csv` | 50 | Store master | `store_id`, `store_name`, `city`, `store_type`, `opening_date` |
| `bm_inventory.csv` | 8,735 | Stock by store and product | `store_id`, `sku_id`, `stock_on_hand`, `reorder_point`, `safety_stock`, `last_restock_date`, `snapshot_date` |

**Data preparation (in the notebook)**

- Dates converted to datetime types.
- 45 duplicate sales rows removed.
- Checked: no missing values in products or stores; `customer_id` is blank for 159,821 sales rows (anonymous purchases); every sale links to a valid store and product.
- Sales joined to products (on `sku_id`) and stores (on `store_id`).
- **Profit** = `total_value` − (`quantity` × `cost_price`).
- Inventory is kept separate from the sales table and only combined with sales quantity by `sku_id` for the stockout-risk analysis.

## Analysis

**Business questions**

1. How are sales changing over time? *(monthly trend)*
2. Which categories generate the highest sales and profit?
3. Which products are the most profitable? *(top 10 product groups)*
4. Which stores and cities perform best?
5. Which sales channel contributes the most?

**Ad hoc questions**

1. Which high-selling products are at risk of stockout? *(above-median sales quantity and below-median stock)*
2. Which products are below their reorder point?
3. Which products have high sales but low profit? *(above-median sales and below-median profit)*

## Key Findings

| Metric | Value |
|---|---|
| Total sales | ≈ 73.2M |
| Total profit | ≈ 21.1M |
| Profit margin | ≈ 28.8% |
| Transactions (after removing duplicates) | 641,798 |
| Average sale per transaction | ≈ 114.07 |
| Stores / Products / Categories | 50 / 200 / 7 |

- **Electronics** is the top category: about 40.8M sales and 11.8M profit, far ahead of Household (8.2M) and Personal Care (8.2M).
- **Batteries, Cables and Chargers** are the most profitable product groups.
- Sales are **seasonal**: strong in January–February and weakest in September–November each year.
- **Dubai** leads by city, followed by Abu Dhabi and Sharjah. Store-level sales are closely matched (about 1.43M–1.50M per store).
- **Stores** are the largest channel, followed by Website, MobileApp, Amazon.ae and Noon.
- **Inventory:** no store-product record is below its reorder point, but 2,098 of 8,735 records combine high sales with low stock.
- **Margin watch:** 8 products have high sales but relatively low profit, with Grocery_Rice_1121 showing the thinnest margin.

## Dashboard

`retail_dashboard.html` is a single file that runs in any modern browser with no Python or Jupyter. It loads Chart.js from a CDN, so an internet connection is needed to render the charts.

**Sections**

- Header with reporting period
- KPI cards
- Executive summary
- Key business insights (trend, category, product groups, city, channel)
- Store view
- Product and sales view
- Inventory view
- Management takeaways

**Interactivity**

- Category filter: updates KPIs and product charts
- Year filter: updates the monthly trend
- Hover tooltips on all charts
- Reset filters button
- Automatic light and dark mode

## Getting Started

**Download the Dashboard html file**

Open `retail_dashboard.html` in a browser. No installation is needed.


## Limitations

- The dashboard was built from the notebook's saved outputs, not from the raw CSV files.
  - Category, product group, high-sales-low-profit and inventory figures are **exact**.
  - **Monthly, store, city and channel values are approximate**, read from the notebook's charts.
- Only the Category and Year filters work. Store, city, channel and date filters need the underlying transaction data.
- "High sales" and "low stock" or "low profit" are relative to the **median**, not a business-defined threshold.
- Currency is not stated in the data, so no currency symbol is shown.
- No customer count or store-type view is included because the notebook does not calculate them.


## Tech Stack

- **Analysis:** Python, pandas, NumPy, Matplotlib, Jupyter Notebook
- **Dashboard:** Power BI
