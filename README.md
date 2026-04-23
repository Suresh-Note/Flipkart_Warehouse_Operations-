# 🏭 Flipkart Warehouse Operations — Analyst Dashboard

> **An interactive Power BI dashboard analyzing 3 years of warehouse operations across Bangalore, Chennai, and Hyderabad — covering ₹9.24bn in revenue, 50K orders, and multi-category performance from 2022–2024.**

<img width="1406" height="791" alt="Flipkart Warehouses Report" src="https://github.com/user-attachments/assets/e40f30a8-373b-4c80-8a12-a4971676cc9b" />


---

## 📌 Overview

This project simulates a real-world **Warehouse Operations Analyst Dashboard** for Flipkart's South India fulfillment network. It tracks revenue performance, order fulfillment health, cancellation impact, and category-level profitability — enabling data-driven decisions across warehouse operations.

Built entirely in **Power BI** with a star schema data model, time intelligence DAX measures, and a clean KPI-first layout.

---

## 📊 Dashboard Snapshot

| KPI | Value |
|-----|-------|
| 💰 Total Revenue | ₹ 9.24 Billion |
| 📈 Total Profit | ₹ 2.0 Billion |
| 📦 Total Orders | 50,000 |
| ❌ Cancel Rate | 17.89% |
| 💸 Cancellation Loss | ₹ 2 Billion |

> ⚠️ **Key Finding:** The 17.89% cancel rate caused ₹2bn in revenue loss — nearly equal to total profit. Reducing cancellations is the single highest-ROI operational lever.

---

## 📉 Visuals Included

### 1. Total Revenue Trend (2022–2024)
Line chart tracking monthly revenue fluctuations between ₹220M–₹280M. Reveals volatility patterns and a notable dip in mid-2023.

### 2. Revenue by Location
Bar chart comparing warehouse contribution across 3 cities:
- 🥇 **Bangalore** — ~₹3.8bn (highest)
- 🥈 **Chennai** — ~₹2.9bn
- 🥉 **Hyderabad** — ~₹2.2bn

### 3. Revenue by Category
5-category breakdown with near-equal distribution across Fashion, Home Appliances, Accessories, Footwear, and Electronics — all between ₹1.4bn–₹2.0bn.

### 4. Quarterly Revenue Trend (Multi-Year)
Multi-line chart overlaying Q4→Q3 performance for 2022, 2023, and 2024. Shows that **2023 had the sharpest Q1 dip** before recovering strongly in Q3.

### 5. Order Status Donut Chart
| Status | Share |
|--------|-------|
| ✅ Delivered | 72% |
| ❌ Cancelled | 18% |
| 🔄 Returned | 10% |

### 6. Profit Margin % by Category
Horizontal bar chart — **Electronics leads** with the highest margin (~20%+), followed by Footwear and Fashion. Home Appliances records the lowest margin.

---

## 🎛️ Filters & Slicers

The dashboard is fully interactive with cross-filtering across all visuals:

| Slicer | Options |
|--------|---------|
| **Date Range** | 01-01-2022 to 31-12-2024 (date picker + slider) |
| **Year** | 2022, 2023, 2024 |
| **Quarter, Year** | Q1–Q4 per year |
| **Category** | Accessories, Electronics, Fashion, Footwear, Home Appliances |
| **Location** | Bangalore, Chennai, Hyderabad |

---

## 🗄️ Data Model — Star Schema

```
                    ┌──────────────────┐
                    │   Fact_Orders    │
                    │  (Central Table) │
                    └────────┬─────────┘
           ┌─────────────────┼─────────────────┐
           ▼                 ▼                 ▼
  ┌──────────────┐   ┌──────────────┐  ┌──────────────┐
  │  Dim_Product │   │   Dim_Date   │  │ Dim_Location │
  │  (Category,  │   │ (Year, Qtr,  │  │ (Bangalore,  │
  │   SKU, etc.) │   │  Month, Day) │  │  Chennai,    │
  └──────────────┘   └──────────────┘  │  Hyderabad)  │
                                        └──────────────┘
```

---

## 🧮 Key DAX Measures

```dax
-- Total Revenue
Total Revenue = SUM(Fact_Orders[Revenue])

-- Total Profit
Total Profit = SUM(Fact_Orders[Profit])

-- Cancel Rate %
Cancel Rate % =
DIVIDE(
    CALCULATE(COUNTROWS(Fact_Orders), Fact_Orders[Status] = "Cancelled"),
    COUNTROWS(Fact_Orders), 0
) * 100

-- Cancellation Revenue Loss
Cancellation Loss =
CALCULATE(SUM(Fact_Orders[Revenue]), Fact_Orders[Status] = "Cancelled")

-- Profit Margin % by Category
Profit Margin % =
DIVIDE([Total Profit], [Total Revenue], 0) * 100

-- YoY Revenue Growth
YoY Revenue Growth % =
VAR CY = CALCULATE([Total Revenue], DATESYTD(Dim_Date[Date]))
VAR PY = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Dim_Date[Date]))
RETURN DIVIDE(CY - PY, PY, 0) * 100
```

---

## 📁 Repository Structure

```
Flipkart_Warehouse_Operations/
│
├── 📊 FlipkartWarehouse.pbix           # Power BI report file
├── 📂 Data/
│   ├── Fact_Orders.csv
│   ├── Dim_Product.csv
│   ├── Dim_Date.csv
│   └── Dim_Location.csv
├── 📂 Screenshots/
│   └── Flipkart_Warehouses_Report.png  # Dashboard preview (above)
└── README.md
```

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Dashboard design, data modeling, publishing |
| **DAX** | KPI measures, time intelligence, margin calculations |
| **Power Query (M)** | Data cleaning, type casting, table relationships |
| **Excel / CSV** | Source data preparation |

---

## 🚀 How to Run

1. **Clone the repo**
   ```bash
   git clone https://github.com/Suresh-Note/Flipkart_Warehouse_Operations-.git
   ```

2. **Open in Power BI Desktop**
   - Download [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
   - Open `FlipkartWarehouse.pbix`

3. **Fix data source paths** *(if needed)*
   - `Home → Transform Data → Data Source Settings`
   - Update paths to your local `Data/` folder → click **Refresh**

4. **Interact with the dashboard**
   - Use date, year, category, and location slicers to filter all visuals
   - Hover over charts for exact values and tooltips

---

## 💡 Business Insights

- **Bangalore dominates revenue** at ~41% of total — a potential single-point risk if operations there face disruption
- **₹2bn cancellation loss equals the entire profit pool** — a 5% reduction in cancel rate would directly improve net margin
- **Electronics has the highest profit margin** despite not being the top revenue category, suggesting strong pricing power and lower return rates
- **2023 Q1 was the weakest quarter** across all three years, likely driven by post-festive demand drop before a Q3 recovery
- **72% delivery success with 10% returns** — healthy last-mile performance, but return logistics optimization could recover additional margin

---

## 👤 Author

**Suresh Kanchamreddy**
- 🔗 GitHub: [@Suresh-Note](https://github.com/Suresh-Note)
- 💼 LinkedIn: [linkedin.com/in/suresh-kanchamreddy](https://linkedin.com/in/suresh-kanchamreddy)
- 🎓 B.Tech CSE | Data Analyst | Power BI · Python · SQL · ML

---

## 📄 License

Portfolio project. Dataset is synthetic and does not represent actual Flipkart operational data.
