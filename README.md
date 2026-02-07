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
# vendor-performance-analysis-python-powerbi

## Project overview

This repository contains a small Python pipeline to ingest raw CSV data into a local SQLite database and produce an aggregated vendor performance table used for reporting and Power BI visualization.

Work done here is focused on data ingestion and creating the `vendor_sales_summary` table with derived performance metrics.

## Files

- `ingestion_db.py` — Ingests all `.csv` files from the current working directory into `inventory.db`. Use `python ingestion_db.py` to run; logs are written to the `logs` folder.
- `get_vendor_summary.py` — Builds the `vendor_sales_summary` table by joining `purchases`, `purchase_prices`, `sales`, and `vendor_invoice`, performs cleaning and derives metrics, then writes the result into the database.
- `Exploratory Data Analysis.ipynb`, `Ingestion_DB.ipynb`, `Vendor Performance Analysis.ipynb` — exploration and analysis notebooks (not covered here).

## How it works (short)

1. Run `python ingestion_db.py` to load CSV files into `inventory.db` (each CSV becomes a table).
2. Run `python get_vendor_summary.py` to create `vendor_sales_summary` in the same database. This script:
   - Aggregates freight cost by vendor
   - Aggregates purchases and joins to `purchase_prices`
   - Aggregates sales by vendor and brand
   - Joins the above to create a summary table
   - Cleans data and computes derived metrics such as `GrossProfit`, `ProfitMargin %`, `StockTurnover`, and `SalestoPurchaseRatio`

## Requirements

- Python 3.8+
- pandas
- sqlalchemy
- sqlite3 (builtin)

Install Python packages:

```powershell
pip install pandas sqlalchemy
```

## Notes and important details

- `ingestion_db.py` uses SQLAlchemy's `create_engine('sqlite:///inventory.db')` and writes DataFrames with `to_sql`.
- `get_vendor_summary.py` opens a `sqlite3` connection to `inventory.db` and executes SQL to build the summary. It then calls the ingestion helper to write `vendor_sales_summary` back to the DB.
- Ensure you run scripts from the project root so relative paths (for CSV discovery and `inventory.db`) resolve correctly.
- Logs are written to the `logs` directory; create it if missing.

## Running

1. From the project root, ingest CSVs:

```powershell
python ingestion_db.py
```

2. Build the vendor summary:

```powershell
python get_vendor_summary.py
```

3. Open your Power BI file and connect to `inventory.db` to visualize `vendor_sales_summary`.

## Quick checklist

- [ ] Place CSV files in the project root
- [ ] Confirm `logs` directory exists
- [ ] Run ingestion, then summary generation

If you want, I can also:
- add a `requirements.txt` and a simple script to create the `logs` folder automatically
- run the scripts here and verify the DB is created (if you want me to proceed, tell me to run them)
