# Shopify Sales & Customer Funnel – Power BI Dashboard

This repository contains a Power BI dashboard that analyzes **Shopify sales and customer behavior**, focusing on the end-to-end **sales funnel**, **retention**, and **regional performance**.

![Shopify Power BI Dashboard](https://github.com/ths887/Shopify-Sales-Customer-Funnel-Power-BI-Dashboard/blob/main/SHOPIFY_PROJECT.png)


---

## 📌 Objectives

- Track **Net Sales**, **Total Quantity**, and **Average Order Value**
- Understand **customer purchase behavior** (single-order vs repeat customers)
- Monitor **retention metrics** like **Lifetime Value**, **Repeat Rate**, and **Purchase Frequency**
- Analyze **regional performance** across provinces and cities
- Compare **sales by payment gateway** and **product types**

---

## 🧱 Dataset

- **Source:** Shopify sales export (CSV / Excel)
- **Main Table:** `shopify_sales`
- **Key Columns Used:**
  - `Order Number`
  - `Customer Id`
  - `Order Date / Date`
  - `Net Sales`
  - `Quantity`
  - `Province`, `City`
  - `Gateway / Payment Method`
  - `Product Type`


---

## 📊 Dashboard Contents

### 1. Transaction Performance (Top KPIs)

- **Net Sales**
- **Total Quantity**
- **Net Avg Order Value**

### 2. Customer Purchase Behavior

- **Total Customers**
- **Single Order Customers**
- **Repeat Customers**

### 3. Retention & KPI’s

- **Lifetime Value**
- **Repeat Rate**
- **Purchase Frequency**

### 4. Visuals

- **Net Sales Trend Over Time** – line chart
- **Daily/Monthly Sales Bar Chart**
- **Regional Overview**
  - Map by **province** (Net Sales)
  - Map by **city** (Net Sales)
- **Top Provinces/Cities by Net Sales** – bar chart
- **Net Sales by Gateway / Payment Method** – donut chart
- **Net Sales by Product Type** – bar chart

### 5. Slicers / Filters

- **Select Measure** (Net Sales / Total Quantity / Total Customers / Repeat Customers)
- **Gateway** (Shopify payments, PayPal, Amazon payments, Manual, etc.)
- **Province**

These filters are fully interactive and update all relevant visuals on the page.

---

## 🧮 DAX Measures

```DAX
-- Repeat Customers
Repeat Customers =
CALCULATE(
    COUNTROWS(VALUES(shopify_sales[Customer Id])),
    FILTER(
        VALUES(shopify_sales[Customer Id]),
        CALCULATE(DISTINCTCOUNT(shopify_sales[Order Number])) > 1
    )
)

-- Single Order Customers
Single Order Customers =
CALCULATE(
    COUNTROWS(VALUES(shopify_sales[Customer Id])),
    FILTER(
        VALUES(shopify_sales[Customer Id]),
        CALCULATE(DISTINCTCOUNT(shopify_sales[Order Number])) = 1
    )
)

-- Select Measure (for dynamic measure selection)
Select Measure = {
    ("Net Sales",        NAMEOF('shopify_sales'[Net Sales]),          0),
    ("Total Quantity",   NAMEOF('shopify_sales'[Total Quantity]),     1),
    ("Total Customers",  NAMEOF('shopify_sales'[Total Customers]),    2),
    ("Repeat Customers", NAMEOF('shopify_sales'[Repeat Customers]),   3)
}

-- Dynamic Title based on selected measure
Dynamic Title =
IF(
    'select Measure'[select Measure Order] = 0, "Net Sales",
    IF(
        'select Measure'[select Measure Order] = 1, "Total Quantity",
        IF(
            'select Measure'[select Measure Order] = 2, "Total Customers",
            IF(
                'select Measure'[select Measure Order] = 3, "Repeat Customers",
                "Other"
            )
        )
    )
)


