# 📊 Sales Performance & Business Health Dashboard
### KS AI & Cloud Solutions — Data Analyst Assessment

---

## 🧩 Business Problem

A regional sales company operates across multiple regions with several Regional Managers responsible for driving sales performance. The Sales Manager was unable to clearly explain:
- Whether the business is improving year over year
- Whether Regional Managers are achieving their category targets
- Whether product returns are under control
- Whether delivery delays are damaging customer experience

This dashboard solves that problem by turning scattered spreadsheet data into a clear, decision-making tool for monthly business reviews.

---

## 📁 Dataset

| File | Description |
|---|---|
| order_2015.csv to order_2018.csv | All order transactions across 4 years (~10,000 rows) |
| Returns.csv | Returned order records |
| Target.csv | Category-wise sales targets per year |
| People.csv | Regional Manager to Region mapping |

---

## 🧹 Data Cleaning (Power Query)

| Issue Found | Fix Applied |
|---|---|
| Dates stored as Excel serial numbers | Converted using `#date(1899,12,30) + #duration()` |
| Quantity stored as text (one, six, nine, ten) | Replaced with numeric values |
| 2 exact duplicate rows in Orders | Removed using Remove Duplicates |
| 353 duplicate Order IDs in Returns | Deduplicated on Order ID |
| Sales Target stored as "170K" text | Stripped K and multiplied by 1000 |
| Year 2019 rows in Target (no order data exists) | Filtered out |
| 4 year files were separate | Merged into one Orders table using Append |
| New columns needed | Added Year, Month Number, Month Name, Delivery Days, Is Late Delivery |

---

## 📊 Dashboard Pages

### Page 1 — Business Overview
Shows the big picture — is the business growing?

- **6 KPI Cards:** Total Sales 2018, Sales Growth vs 2017, Total Profit 2018, Profit Growth vs 2017, Total Orders 2018, Orders Growth vs 2017
- **Clustered Bar Chart:** Sales by Category — 2018 vs 2017
- **Line Chart:** Monthly Sales Trend across all 4 years (2015–2018)
- **Year Slicer** (tile style) — filter any year

### Page 2 — Sales vs Targets & Manager Performance
Are targets being hit? Which manager and category is missing?

- **Clustered Bar Chart:** Sales vs Target by Category
- **Gauge Chart:** Overall Target Achievement % (0% to 200%)
- **Table:** Regional Manager Performance — Sales CY, Sales PY, YoY Growth %, Profit Margin %, Return Rate %
- **Stacked Bar Chart:** Manager Sales Split by Category
- **People & Region Slicers**

### Page 3 — Product Returns
Are returns increasing? Where are they coming from?

- **5 KPI Cards:** Returns 2018, YoY Returns %, Returns 2017, Return Rate %, Total Returns
- **Table:** Returns by Regional Manager — Returns 2018, Total Sales, Return Rate %
- **Clustered Bar Chart:** Returns by Region — 2018 vs 2017
- **Clustered Bar Chart:** Returns by Category — 2018

### Page 4 — Delivery Delays
Where are deliveries failing?

- **4 KPI Cards:** Late Deliveries, On Time Deliveries, Late Delivery %, Avg Delivery Days
- **Clustered Bar Chart:** Late Delivery Rate by Ship Mode
- **Clustered Bar Chart:** Late Delivery Rate by Region
- **Clustered Bar Chart:** Late Deliveries by Product Category
- **Table:** Delivery Performance by Regional Manager — Late Deliveries, Late Delivery %, Avg Delivery Days

---

## 🔑 Key DAX Measures

```dax
Total Sales = SUM(Orders[Sales])

Total Profit = SUM(Orders[Profit])

Profit Margin % = DIVIDE([Total Profit], [Total Sales])

Sales CY = CALCULATE([Total Sales], Orders[Year] = 2018)

Sales PY = CALCULATE([Total Sales], Orders[Year] = 2017)

YoY Sales % = DIVIDE([Sales CY] - [Sales PY], [Sales PY])

Target Achievement % = DIVIDE([Total Sales], [Total Target])

Sales vs Target Gap = [Total Sales] - [Total Target]

Total Returns = CALCULATE(COUNTROWS(Returns), CROSSFILTER(Orders[Order ID], Returns[Order ID], BOTH))

Return Rate % = DIVIDE([Total Returns], [Total Orders])

Late Deliveries = CALCULATE(COUNTROWS(Orders), Orders[Is Late Delivery] = "Yes")

Late Delivery % = DIVIDE([Late Deliveries], COUNTROWS(Orders))

Avg Delivery Days = AVERAGE(Orders[Delivery Days])
```

---

## 💡 Key Business Insights

| Area | Insight |
|---|---|
| Overall Sales | **+36.2% growth** in 2018 vs 2017 — strong business expansion |
| Profit | **+31.0% profit growth** — margin slightly down (12.4% vs 12.9%) |
| Orders | **+26.7% more orders** placed in 2018 vs 2017 |
| Target Achievement | **125% overall** — all 3 categories exceeded 2018 targets |
| Technology | Highest achiever at 229% of target |
| Furniture Turnaround | Was 78% of target in 2017, recovered to 133% in 2018 |
| Best Manager YoY | Ross DeVincentis (North) — **+58.9% growth** |
| Best Profit Margin | Ross DeVincentis (North) — **14.7% margin** |
| Returns | +16.3% more returns in volume but return rate stable at **6.2%** |
| Delivery | **50.4% of all orders delivered late** — Standard Class is the biggest risk |

---

## ⚠️ Key Risks & Action Areas

| Priority | Risk | Action |
|---|---|---|
| 🔴 High | Standard Class has 62% late delivery rate | Review shipping partner SLAs, shift high-value orders to First Class |
| 🟡 Medium | Profit margin slightly down despite sales growth | Review discounting practices — especially in Technology |
| 🟡 Medium | Central region accounts for 52% of all returns | Emily Burns to review top returned SKUs and delivery quality |
| 🟢 Opportunity | North region grew 58.9% — fastest in company | Replicate Ross DeVincentis strategies in South region |
| 🟢 Planning | All categories significantly beat 2018 targets | Set more ambitious targets for 2019 |

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Power BI Desktop | Dashboard design and publishing |
| Power Query | Data cleaning and transformation |
| DAX | All calculated measures |
| CSV files | Source data (Orders, Returns, Target, People) |

---

## 📂 Files in this Repository

| File | Description |
|---|---|
| [📥 Sales Performance & Business Health Dashboard.pbix](https://drive.google.com/file/d/1LemSsv_GBpVQAg-N0lUQnCXQ5NH8A85s/view?usp=sharing) | Power BI Dashboard — click to download |
| `README.md` | Project documentation |

---

## 🔗 How to Use

1. Download the dashboard: [Click here to download PBIX](https://drive.google.com/file/d/1LemSsv_GBpVQAg-N0lUQnCXQ5NH8A85s/view?usp=sharing)
2. Open in **Power BI Desktop**
3. If data source error appears: Home → Transform Data → Data Source Settings → update file paths
4. Explore all 4 pages of the dashboard

---

## 🗂️ Data Model (Star Schema)

```
Orders (fact table — center)
   ├── Returns     [Order ID → Order ID]      Many to One
   ├── People      [Region → Region]           Many to One
   └── Target      [Cat Year Key → Cat Year Key]  Many to One
```

---

*Submitted for KS AI & Cloud Solutions — Data Analyst Role Assessment*
