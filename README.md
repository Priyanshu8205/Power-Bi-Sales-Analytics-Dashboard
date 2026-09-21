# Sales Analytics Dashboard --- Power BI

An interactive **Sales Analytics Dashboard built in Microsoft Power BI**
using imported sales, customer, product, store, returns, date, and
sales-target datasets.

This is a standalone Power BI analytics project. It is **not connected
to the SQL Data Warehouse project** in my portfolio.

## 📌 Project Overview

The project transforms transactional sales data into an interactive
Power BI report for analyzing:

-   Sales and profit performance
-   Monthly sales trends
-   Product performance
-   Customer segments and gender
-   Regional sales
-   Returned orders and return reasons
-   Year-over-year sales growth
-   Actual sales versus monthly targets
-   Target achievement
-   Product-level drill-through analysis

The report is designed as a practical portfolio project demonstrating
data modeling, DAX, interactive reporting, and analytical storytelling
in Power BI.

------------------------------------------------------------------------

## 🎯 Objectives

The main objectives of the project are to:

1.  Build a structured analytical model from multiple datasets.
2.  Create reusable DAX measures for business KPIs.
3.  Analyze sales, profit, orders, quantity, and returns.
4.  Compare actual sales against monthly targets.
5.  Implement time-based analysis including YoY growth.
6.  Provide interactive filtering across important business dimensions.
7.  Add drill-through and report-page tooltip functionality.
8.  Present the analysis in a professional multi-page Power BI report.

------------------------------------------------------------------------

## 🗂️ Data Sources

The Power BI model uses imported tabular datasets representing sales
transactions and supporting business dimensions.

  Dataset                 Approx. Rows Purpose
  --------------------- -------------- -------------------------------------------
  `Fact_Sales.csv`             100,000 Transaction-level sales data
  `Fact_Returns.csv`             7,414 Returned orders and return reasons
  `Dim_Customers.csv`            1,000 Customer attributes
  `Dim_Products.csv`               120 Product and pricing information
  `Dim_Stores.csv`                  25 Store and regional information
  `Dim_Date.csv`                 1,096 Calendar and time-intelligence attributes
  `Sales_Targets.csv`              180 Monthly regional sales targets

### Main fields

**Fact_Sales** - OrderID - OrderDate - CustomerID - ProductID -
StoreID - Quantity - UnitPrice - DiscountPct - GrossSales -
DiscountAmount - NetSales - Cost - Profit - PaymentMethod - Channel

**Fact_Returns** - OrderID - ReturnDate - ReturnReason

**Dim_Customers** - CustomerID - CustomerName - Gender - Age - City -
Region - Segment

**Dim_Products** - ProductID - ProductName - Category - BrandTier -
UnitCost - UnitPrice

**Dim_Stores** - StoreID - StoreName - City - Region - StoreType

**Dim_Date** - Date - DateKey - Year - Quarter - MonthNumber - Month -
MonthShort - WeekNumber - Day - DayName - IsWeekend

**Sales_Targets** - Month - Region - SalesTarget

------------------------------------------------------------------------

## 🏗️ Data Model

The report uses a dimensional/star-schema-oriented model.

### Core model

-   `Dim_Customers` → `Fact_Sales`
-   `Dim_Products` → `Fact_Sales`
-   `Dim_Stores` → `Fact_Sales`
-   `Dim_Date` → `Fact_Sales`

### Returns analysis

A helper table called `Dim_Orders` contains distinct order IDs and
connects sales orders with returned orders.

This allows return analysis to respond to sales-side dimensions such as:

-   Region
-   Category
-   Year
-   Return Reason

### Sales targets

A helper `Month` table is used to connect monthly target data with the
calendar model and support month-level actual-versus-target analysis.

The model therefore contains the source tables plus the helper tables
required for filtering and target/return analysis.

------------------------------------------------------------------------

## 📊 Report Pages

### 1. Executive Sales Dashboard

The main overview page provides a high-level view of business
performance.

**KPIs:** - Total Sales - Total Profit - Profit Margin % - Total
Orders - Total Quantity - Return Rate % - Target Achievement %

**Visual analysis:** - Monthly Sales Trend - Sales by Category

**Interactive filters:** - Year - Region - Category

------------------------------------------------------------------------

### 2. Product & Customer Analysis

This page focuses on product and customer-level analysis.

**Visuals:** - Top 10 Products by Sales - Sales by Customer Segment -
Sales by Gender - Sales by Region

The Top 10 product analysis uses a Top-N filter based on Total Sales.

------------------------------------------------------------------------

### 3. Return Analysis

This page focuses on returned orders and performance against targets.

**Analysis includes:** - Returned Orders - Returned Orders by Region -
Returned Orders by Reason - YoY Sales Growth % - Actual Sales vs
Target - Return Reason %

The page supports interactive filtering by relevant business dimensions,
including return reason.

------------------------------------------------------------------------

### 4. Product Details

A dedicated **drill-through page** for product-level analysis.

When a product is selected from the product analysis page, the report
can drill through to detailed information for that product.

**Includes:** - Selected Product - Total Sales - Total Profit - Total
Orders - Monthly Sales Trend - Monthly detail table - Total Quantity

------------------------------------------------------------------------

### 5. Product Tooltip

A dedicated **report-page tooltip** provides additional product
information when hovering over the Top 10 Products visual.

**Tooltip KPIs:** - Total Sales - Total Profit - Total Orders

------------------------------------------------------------------------

## 📈 Key DAX Measures

The report uses reusable DAX measures for core business calculations.

### Total Sales

``` dax
Total Sales =
SUM(Fact_Sales[NetSales])
```

### Total Profit

``` dax
Total Profit =
SUM(Fact_Sales[Profit])
```

### Total Orders

``` dax
Total Orders =
DISTINCTCOUNT(Fact_Sales[OrderID])
```

### Profit Margin

``` dax
Profit Margin % =
DIVIDE([Total Profit], [Total Sales], 0)
```

### Total Quantity

``` dax
Total Quantity =
SUM(Fact_Sales[Quantity])
```

### Returned Orders

``` dax
Returned Orders =
DISTINCTCOUNT(Fact_Returns[OrderID])
```

### Return Rate

``` dax
Return Rate % =
DIVIDE([Returned Orders], [Total Orders], 0)
```

### Year-over-Year Sales Growth

``` dax
YoY Sales % =
VAR PreviousYearSales =
    CALCULATE(
        [Total Sales],
        DATEADD(Dim_Date[Date], -1, YEAR)
    )
RETURN
    DIVIDE(
        [Total Sales] - PreviousYearSales,
        PreviousYearSales,
        0
    )
```

### Sales Target

``` dax
Sales Target =
SUM(Sales_Targets[SalesTarget])
```

### Sales Variance

``` dax
Sales Variance =
[Total Sales] - [Sales Target]
```

### Target Achievement

``` dax
Target Achievement % =
DIVIDE(
    [Total Sales],
    [Sales Target],
    0
)
```

------------------------------------------------------------------------

## 🛠️ Tools & Technologies

-   **Microsoft Power BI Desktop**
-   **Power Query**
-   **DAX**
-   **Data Modeling**
-   **Star-schema concepts**
-   **Time intelligence**
-   **Interactive filtering**
-   **Drill-through**
-   **Report-page tooltips**

------------------------------------------------------------------------

## 🔄 Analytical Features

The report demonstrates:

-   KPI design
-   Data relationships
-   Cross-filtering
-   Time-series analysis
-   YoY calculations
-   Target versus actual analysis
-   Variance analysis
-   Return-rate analysis
-   Top-N analysis
-   Drill-through navigation
-   Report-page tooltips
-   Interactive slicers
-   Product-level detail analysis

------------------------------------------------------------------------
------------------------------------------------------------------------

## 📁 Project Structure

``` text
Power-Bi-Sales-Analytics-Dashboard/
│
├── Sales Analytics Dashboard.pbix
├── README.md
│
├── data/
│   ├── Dim_Customers.csv
│   ├── Dim_Date.csv
│   ├── Dim_Products.csv
│   ├── Dim_Stores.csv
│   ├── Fact_Returns.csv
│   ├── Fact_Sales.csv
│   └── Sales_Targets.csv
│
└── screenshots/
    ├── executive-dashboard.png
    ├── product-customer-analysis.png
    ├── return-analysis.png
    └── product-details.png
```

------------------------------------------------------------------------

## 🚀 How to Use

1.  Download or clone the repository.
2.  Open `Sales Analytics Dashboard.pbix` using Microsoft Power BI Desktop.
3.  If prompted, update the source-file paths to match the location of
    the CSV files.
4.  Refresh the data if required.
5.  Explore the report pages and interact with the slicers.
6.  Use the Top 10 Products visual to test the product drill-through and
    tooltip functionality.

------------------------------------------------------------------------

## 📚 Skills Demonstrated

This project demonstrates practical experience with:

-   Power BI report development
-   Power Query data loading
-   Data modeling
-   Fact and dimension tables
-   Relationship design
-   DAX measures
-   Filter context
-   Time intelligence
-   KPI development
-   Sales and profitability analysis
-   Return analysis
-   Target and variance analysis
-   Interactive dashboards
-   Drill-through reports
-   Report-page tooltips
-   Business-oriented data visualization

------------------------------------------------------------------------

## 📄 License

## 📄 License

This project is licensed under the MIT License.

The datasets used in this project are synthetic/practice data and are intended for educational and portfolio purposes.
