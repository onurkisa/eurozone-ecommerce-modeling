# Modeling E-commerce Participation and Customer Distribution Across European Markets (2023)
This repository enables e-commerce market analysis across European markets through two key capabilities:

- **Assess and compare Eurozone countries' digital maturity and purchasing power standard** to analyze and understand their relative participation in online shopping.
- **Simulate the distribution of e-commerce customers and orders at the city level** for a hypothetical platform, leveraging Eurostat national-level statistics and mapping them to cities and provinces using population weights from GeoNames.

---

## Workflow Overview

1. Load and preprocess Eurostat datasets (demographics, income, digital activity, e-commerce participation).
2. Develop and normalized the metrics for cross-country comparison
3. Perform a quick exploratory data analysis (EDA)
4. Calculate country-level frequency-based purchase segments and allocate customers/orders proportionally.
5. Map country-level allocations to cities/provinces using population weights.
6. Simulate customer and order segmentation by purchase frequency using statistical modeling (truncated Poisson distribution for "Power users" (10+ orders) segment).
7. Allocate city-level customers and orders to months, incorporating campaign/seasonal effects with weighted distributions.
8. Save all simulation outputs for use in further synthetic data generation and downstream analytics

---

## Data Sources

- **Eurostat:**  
  1. demo_pjan: Population on 1 January by age and sex
  2. ilc_di03: Median income by age and sex
  3. isoc_ci_ifp_iu: Individuals - internet use
  4. isoc_ec_ib20: Internet purchases by individuals

  [https://ec.europa.eu/eurostat](https://ec.europa.eu/eurostat)

- **GeoNames:**  
  City/province-level population and administrative divisions  
  [https://www.geonames.org/](https://www.geonames.org/)

---

## Metric Definitions & Business Rules

### Metric Definitions

- **DigitalMaturity:**  
  Mean of min-max normalized rates of internet use and e-commerce participation
- **CompositeScore:**  
  Average of normalized population, DigitalMaturity, and purchasing power standard (PPS). Used to proportionally allocate customers/orders across countries.
- **Segments:**  
  Countries are grouped into quartiles by CompositeScore: *Lowest*, *Lower-Middle*, *Upper-Middle*, *Highest*.

### Business Rules & Key Assumptions

- **Geographical Scope:**  
  - Analysis covers Eurozone countries for 2023.
  - Only cities/provinces with >100,000 inhabitants are included, except for Luxembourg (included regardless of size due to high CompositeScore).
- **Data Preparation:**  
  - Outer joins are used when merging datasets to maximize coverage.
  - Year-specific normalization ensures comparability between countries.
- **Customer & Order Allocation:**  
  - Total number of customers and orders is configurable (default: 800,000 customers, 5,000,000 orders).
  - CompositeScore determines each country’s share; city/province breakdown uses GeoNames population weights.
- **Purchase Frequency Segmentation:**  
  - Online purchase frequency buckets (e.g., "1 or 2 times", "3 to 5 times", etc.) are sourced from Eurostat data.
  - "Power users" (10+ orders) are modeled via a truncated Poisson distribution.
- **Seasonality & Campaign Effects:**  
  - Monthly distribution of orders and customers is adjusted for seasonal/campaign spikes (e.g., Black Friday, Christmas) using weighted allocation.

---

## Outputs

- **Clean tabular CSVs** detailing:
  - Country readiness ranking and quartile segments.
  - Country → city allocations of customers and orders.
  - Frequency segments (1–2, 3–5, 6–10, 10+) per city with calibrated order counts.
  - Monthly distributions of orders and new customers by city.

All outputs are validated to ensure internal consistency and are suitable for further analytics, synthetic data generation
