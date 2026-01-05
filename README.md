# vendor-performance-analysis-python-powerbi

## Project Overview

This project analyzes vendor performance through a comprehensive data pipeline that combines Python for data processing and Power BI for visualization. The analysis helps identify profitable vendors, optimize pricing strategies, and improve inventory management across multiple store locations.

## Project Structure

### Data Files
- **Exploratory Data Analysis.ipynb** - Initial data exploration and understanding
- **Ingestion_DB.ipynb** - Database schema validation and data ingestion workflow
- **Vendor Performance Analysis.ipynb** - Advanced analysis and calculations for vendor performance metrics

### Python Scripts
- **ingestion_db.py** - Handles raw CSV data ingestion into SQLite database
- **get_vendor_summary.py** - Creates aggregated vendor summary tables with performance metrics

### Dashboard
- **Vendor Performance Analysis Dashboard.pbix** - Power BI interactive dashboard for visualization

## Database Schema

The project uses SQLite with the following tables:

### Raw Data Tables
- **begin_inventory** - Product inventory at the start of period (206,529 records)
- **end_inventory** - Product inventory at end of period (224,489 records)
- **purchases** - Vendor purchase transactions (2,372,474 records)
- **purchase_prices** - Vendor-specific product pricing (12,261 records)
- **sales** - Store sales transactions (12,825,363 records)
- **vendor_invoice** - Vendor invoice aggregates with freight costs (5,543 records)

### Aggregated Tables
- **vendor_sales_summary** - Comprehensive vendor performance metrics (10,692 records) with 18 columns

## Key Metrics Calculated

The vendor_sales_summary table includes:
- **VendorNumber & VendorName** - Vendor identification
- **Brand & Description** - Product details
- **TotalPurchaseQuantity & TotalPurchaseDollars** - Purchase volume and cost
- **TotalSalesQuantity & TotalSalesDollars** - Sales volume and revenue
- **GrossProfit** - Sales dollars minus purchase cost
- **ProfitMargin %** - Profitability percentage
- **StockTurnover** - Sales quantity divided by purchase quantity
- **SalestoPurchaseRatio** - Revenue to cost ratio
- **FreightCost** - Vendor-specific freight charges
- **ActualPrice & Volume** - Product pricing and volume information

## Data Processing Pipeline

### 1. Data Ingestion (ingestion_db.py)
- Loads all CSV files from current directory
- Converts to pandas DataFrames
- Writes to SQLite database tables
- Logs processing time and file counts

### 2. Vendor Summary Generation (get_vendor_summary.py)
Creates vendor_sales_summary through multi-step process:
- Aggregates freight costs by vendor
- Joins purchases with product prices
- Aggregates sales by vendor and brand
- Combines all data with appropriate joins
- Cleans data (type conversion, null handling, whitespace removal)
- Calculates derived metrics (profit, margins, ratios)
- Stores in database for reporting

### 3. Data Cleaning Steps
- Converts Volume field to float
- Fills missing sales values with 0
- Removes leading/trailing whitespace from vendor and product names
- Handles data type consistency

## Analysis Use Cases

1. **Vendor Selection for Profitability** - Identify top-performing vendors by profit metrics
2. **Product Pricing Optimization** - Compare purchase vs actual selling prices
3. **Inventory Management** - Monitor stock turnover rates by vendor and product
4. **Performance Benchmarking** - Analyze sales-to-purchase ratios across vendors

## Requirements

- Python 3.x
- pandas
- numpy
- sqlite3
- sqlalchemy
- Power BI Desktop

## Usage

1. **Load Raw Data:**
   ```python
   python ingestion_db.py
   ```
   Ingests all CSV files into inventory.db

2. **Generate Vendor Summary:**
   ```python
   python get_vendor_summary.py
   ```
   Creates aggregated vendor_sales_summary table with metrics

3. **Visualize Results:**
   - Open Vendor Performance Analysis Dashboard.pbix in Power BI
   - Connect to inventory.db
   - Explore vendor performance metrics

## Key Statistics

- **126 Active Vendors** in the system
- **10,692 Vendor-Brand Combinations** analyzed
- **12.8M+ Sales Transactions** processed
- **2.4M+ Purchase Transactions** analyzed
- **Profit Range:** -$52,002 to +$1,290,667
- **Profit Margin Range:** -100% to +99%

## Performance Insights

Top vendors by profit contribution include:
- BROWN-FORMAN CORP
- MARTIGNETTI COMPANIES
- PERNOD RICARD USA
- DIAGEO NORTH AMERICA INC

## Database Relationships

```
purchases + purchase_prices --> purchase_summary (by vendor, brand)
sales --> sales_summary (by vendor, brand)
vendor_invoice --> freight_summary (by vendor)

[All tables] --> vendor_sales_summary (comprehensive metrics)
```

## Notes

- All monetary values in dollars
- Dates tracked for transactions (2024 data)
- Excise tax calculated for alcoholic beverages
- Data includes multiple store locations
- Volume measured in mL for consistency
