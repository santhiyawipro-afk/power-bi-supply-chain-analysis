# Synthetic PTA-style Supply Chain & Contract Dataset

All data is **synthetic** (randomly generated, fictional supplier names). It contains no real organisation data.
"Today" for the dataset is 29 Oct 2026; use a fixed reference date in DAX if you want results to match the README.

## suppliers.csv (one row per supplier)
SupplierID (PK), SupplierName, Category, Tier (Strategic/Preferred/Standard), Region

## contracts.csv (one row per contract)
ContractID (PK), SupplierID (FK), ContractTitle, StartDate, EndDate, ContractValue, RenewalOption (Yes/No), ContractOwner, Status (Active/Expired)

## purchase_orders.csv (one row per purchase order / delivery)
POID (PK), ContractID (FK), SupplierID (FK), OrderDate, PromisedDeliveryDate, ActualDeliveryDate (blank = not yet delivered), POAmount, QualityIssue (Yes/No)

## Model
Suppliers 1-* Contracts 1-* PurchaseOrders. Add a Date table (CALENDAR) related to OrderDate.

## Suggested measures
- Total Spend = SUM(purchase_orders[POAmount])
- On-Time Delivery % = DIVIDE(COUNTROWS(FILTER(purchase_orders, purchase_orders[ActualDeliveryDate] <= purchase_orders[PromisedDeliveryDate])), COUNTROWS(FILTER(purchase_orders, NOT ISBLANK(purchase_orders[ActualDeliveryDate]))))
- Days To Expiry = DATEDIFF(DATE(2026,10,29), MAX(contracts[EndDate]), DAY)
- Renewal Risk flag = contracts expiring within 90 days AND supplier on-time % below 80%
