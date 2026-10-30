# Supply Chain Spend \& Contract Performance Dashboard (Power BI)

An interactive Power BI report that gives a public transport organisation visibility of supplier spend by category, supplier and tier, alongside on-time delivery performance.

> \*\*Data note:\*\* All data in this project is \*\*synthetic\*\*, generated for demonstration. Supplier names are fictional and no real organisation data is used. The scenario is modelled on the kind of commercial reporting I have done in supply chain and contract management.

## Business problem

Procurement and contract teams need to know:

1. Where is the money going, and is spend concentrated in a few categories or suppliers?
2. Which suppliers deliver on time, and which put operations at risk?
3. Which contracts are due for renewal, and which of those suppliers are underperforming?

## Dashboard preview

!\[Spend Analysis page](images/spend\_overview.png)

## Dataset

|Table|Rows|Description|
|-|-|-|
|Suppliers|30|Supplier, category (7 categories), tier, region|
|Contracts|59|Contract value, start and end dates, renewal option, owner|
|Purchase orders|1,510|Order, promised and actual delivery dates, amount, quality issue flag|

## Data model

Suppliers (1) → Contracts (many) → Purchase Orders (many) ← Date (1)

A snowflake-style model using one-to-many relationships with single-direction filters, and a dedicated Date table marked as the date table.

## Report pages

|Page|Status|
|-|-|
|**1. Spend Analysis**: KPI cards, spend by category, monthly trend, top 10 suppliers, spend by supplier tier, slicers for year, category and tier|Complete|
|**2. Supplier Performance**: on-time delivery, quality issues and delays by supplier and tier|Planned|
|**3. Contract Expiry \& Renewal Risk**: contracts nearing expiry flagged by supplier performance|Planned|

## Key measures (DAX)

* **Total Spend**: sum of purchase order amounts
* **On-Time Delivery %**: delivered orders on or before the promised date divided by all delivered orders
* **Supplier Count** (counted from Contracts so row-level security applies), **Active Contracts**

## Row-level security

Five static roles restrict the report by contract owner (Bus Operations, Rail Operations, Infrastructure, Corporate Services, Customer \& Digital). Filters on the Contracts table flow down to Purchase Orders, so spend and delivery metrics follow automatically. Supplier Count is calculated from Contracts so that it respects each role. Roles were tested with View as in Power BI Desktop.

## Key findings (Spend Analysis page)

1. Total spend is about **$77M** across **30 suppliers**, with overall on-time delivery of **85.8%**.
2. **Energy \& Fuel** is the largest category, at roughly a quarter or more of total spend, followed by Fleet \& Bus Maintenance and Rail Infrastructure.
3. A single supplier, **Gascoyne Energy Supply**, accounts for the largest share of spend by a clear margin, which is a concentration risk worth reviewing.
4. **Preferred** suppliers account for about **50.5%** of spend, Standard about **27%**, and Strategic about **22%**.

## Next steps

* Build the Supplier Performance page.
* Build the Contract Expiry \& Renewal Risk page, flagging contracts expiring within 90 days where the supplier's on-time delivery is below 80%.

## Tools

Power BI Desktop, Power Query, DAX

## Files

* `Supply\_Chain\_Dashboard.pbix`: Power BI report
* `data/`: synthetic CSV files and data dictionary
* `images/`: screenshots

## About me

Santhiya Nagamanickam, Data and Reporting Analyst, Perth WA.

