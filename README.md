# Freight Economics & Customer Geography Analysis — Teus Logistics Company

## Overview

This project investigates how shipping costs impact order economics across different customer regions for **Teus Logistics Company**, prepared for the **Growth & Marketing Manager**.

### Business Question
> **Where is freight cost putting the greatest pressure on order economics?**

In e-commerce logistics, customers in core hubs often pay freight fees that represent a small fraction of their basket value, while shoppers in outlying regions face disproportionately high delivery fees that act as a "tax" and drive cart abandonment. Using a 12,000-order sample from the Brazilian Olist e-commerce dataset, this analysis quantifies order-level revenue, freight burden, delivery timelines, and regional cost disparities to guide shipping and marketing strategy.

---

## Dataset & Data Validation

The analysis combines three relational CSV datasets. Prior to joining, every table was audited for row counts, data types, key uniqueness, missing key values, and table grain to prevent double-counting.

| Table | File | Rows | Columns | Primary / Composite Key | Null / Duplicate Keys | Table Grain |
| :--- | :--- | :---: | :---: | :--- | :---: | :--- |
| **Orders** | `olist_orders_dataset.csv` | 12,000 | 8 | `order_id` | 0 / 0 | 1 row = 1 order |
| **Order Items** | `olist_order_items_dataset.csv` | 13,767 | 7 | `order_id`, `order_item_id` | 0 / 0 | 1 row = 1 item within an order |
| **Customers** | `olist_customers_dataset.csv` | 12,000 | 5 | `customer_id` | 0 / 0 | 1 row = 1 customer order record* |

*\*Note: A returning shopper receives a new `customer_id` per order; `customer_unique_id` is used to identify unique individuals.*

### Reconciliation Checks
* **Order Count Match:** Confirmed `12,000` unique orders in both `orders` and `order_items`.
* **Revenue Reconciliation:** Verified that raw item-level revenue (`R$1,637,360.43`) matches aggregated order-level revenue and the final merged dataset (`final_df`, shape `13,767 × 18`) with zero variance.

---

## Key Metrics & Findings

| Metric | Value | Notes |
| :--- | :---: | :--- |
| **Total Product Revenue** | `R$1,637,360.43` | 100% of sampled orders are `delivered` |
| **Total Freight Revenue** | `R$272,500.77` | ~16.6% of total product revenue overall |
| **Average Order Value (AOV)** | `R$136.45` | Median: `R$85.90` \| Max: `R$5,934.60` |
| **Average Freight per Order** | `R$22.71` | Median: `R$17.21` \| Max: `R$497.08` |
| **Order-Level Freight % of Price** | `30.80%` (mean) | Median: `22.94%` \| IQR: `13.20% – 38.05%` |
| **Average Items per Order** | `1.15` | Median: `1` (89.7% of orders are single-item; max: `12`) |
| **Average Delivery Time** | `12.26 days` | Median: `10 days` \| IQR: `6 – 15 days` \| Max: `194 days` |

### Core Insights
1. **Single-Item Dominance:** Out of 12,000 orders, **10,769 (89.7%)** contain just a single item. Because freight is not spread across multiple products, order-level freight ratios are heavily exposed on low-ticket items.
2. **Severe Regional Freight Disparity:**
   * **High-Pressure States:** Remote northern and northeastern states face the steepest average item freight fees—including **AC** (`R$53.47`), **RR** (`R$45.77`), **SE** (`R$41.53`), **TO** (`R$38.65`), **PB** (`R$38.57`), **PA** (`R$36.71`), and **AL** (`R$36.67`).
   * **High-Efficiency Core Markets:** High-volume markets like **São Paulo (SP)** maintain the lowest average freight (`R$15.00`) and the lowest freight-to-revenue burden, driving the largest share of total revenue.
3. **Long-Tail Delivery Outliers:** While 75% of orders arrive within 15 days, extreme delivery delays stretch up to **194 days**, compounding friction for customers already paying high shipping fees.

---

## Business Recommendations

1. **Implement Threshold-Based Free Shipping:**
   Since ~90% of orders are single-item purchases with an AOV of `R$136.45`, introduce a free or discounted shipping threshold (e.g., orders above `R$175` or 2+ items) to encourage basket-building and amortize freight costs across higher cart values.
2. **Regional Freight Subsidies & Micro-Hubs in High-Pressure States:**
   For remote states where average freight exceeds `R$35–R$53` (e.g., AC, RR, SE, TO), partner with regional 3PL providers or establish fulfillment micro-hubs to lower shipping fees and reduce checkout abandonment.
3. **Protect Core High-Efficiency Corridors:**
   Maintain aggressive customer acquisition campaigns in high-volume, low-friction states like São Paulo (`SP`), where freight consumes the smallest share of revenue.
4. **Establish Delivery SLAs & Guarantees:**
   With delivery averaging `12.3` days and extreme delays reaching `194` days, introduce predictable delivery SLAs to improve customer trust—particularly for long-distance orders with high shipping costs.

---

## Limitations

1. **Survivorship Bias (100% Delivered Orders):** The dataset only contains `delivered` orders and excludes canceled, returned, or abandoned checkouts, preventing direct measurement of freight-driven cart abandonment.
2. **Sample Size & Small-State Variance:** The 12,000-order subset results in small sample sizes for remote states (e.g., AC, RR, AP), making state-level averages sensitive to isolated outliers.
3. **Missing Product Weight & Dimensions:** Product weight and volume are not included; high freight fees in some orders may reflect bulky items rather than geographic distance alone.
4. **Absence of Seller Geography:** Only `customer_state` is present. Without seller origin locations, intra-state and cross-country shipments cannot be separated.

---

## Recommended Next Action

Investigate the states with the highest freight-cost burden and evaluate logistics partnerships, fulfillment optimization, or regional inventory placement—while continuing to invest in high-performing core markets that generate strong freight-adjusted revenue.

---

## Tableau Dashboard

[View Interactive Logistics Dashboard on Tableau Public](https://public.tableau.com:9443/views/FreightLogisticsDashboard/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

![Tableau Dashboard Preview](./images/tableau-dashboard.png)
## Tech Stack & Project Setup

* **Language:** Python 3.12+
* **Libraries:** `pandas`, `numpy`, `matplotlib`, `seaborn`, `pathlib`

### Running the Notebook
1. Place the three CSV files (`olist_orders_dataset.csv`, `olist_order_items_dataset.csv`, `olist_customers_dataset.csv`) in the same directory as the notebook (or update the file paths to point to your `data/` directory).
2. Install the required packages:
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```
3. Launch Jupyter Notebook and run all cells sequentially:
   ```bash
   jupyter notebook
   ```
