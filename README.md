# Supply-Chain-supplier-analysis-
Here is the raw Markdown text. You can copy everything inside the code box below and paste it directly into your GitHub `README.md` file. It includes the structural formatting (hashtags, bolding, code blocks) that GitHub reads, plus a deeper explanation of your methodology to show recruiters exactly how you think.

```markdown
# Supply Chain Supplier Performance Analysis

## 📌 Project Overview
This project analyzes a supply chain dataset to evaluate supplier performance, identify bottlenecks, and recommend data-driven procurement strategies. 

**The Business Question:** *"Which suppliers are underperforming, and how is that impacting our procurement costs and delivery reliability?"*

Instead of just looking at raw numbers, this project builds a composite scoring system to rank suppliers across multiple dimensions (quality, speed, and cost) to provide actionable recommendations for vendor management.

---

## 📊 The Dataset
* **Source:** Kaggle (Supply Chain Analysis dataset)
* **Domain:** Procurement, Logistics, Quality Assurance
* **Key Variables:** Supplier Name, Lead Times, Defect Rates, Shipping Costs, Inspection Results

---

## 🛠️ Tech Stack & Workflow Explanation

This project demonstrates an end-to-end data analyst workflow, moving from raw CSV files to a fully interactive dashboard.

### Phase 1: Data Cleaning & Preparation (Microsoft Excel / Power Query)
Real-world data is rarely ready for analysis out of the box. Before querying, the data was processed to ensure accuracy:
* **Disambiguation:** Clarified ambiguous columns by renaming `Lead times` to `Shipping Lead Time` and `Lead time` to `Supplier Lead Time`.
* **Dimensional Reduction:** Dropped irrelevant columns (e.g., Demographics, specific routes) to optimize the dataset for procurement-focused analysis.
* **Feature Engineering:** Created a new conditional column for `Delivery Performance` based on raw inspection outcomes (Pass/Fail/Pending).
* **Data Type Validation:** Ensured defect rates and costs were formatted as decimal numbers to prevent aggregation errors in SQL.

### Phase 2: Data Exploration & Logic (SQL Server Management Studio)
The cleaned dataset was imported into a SQL Server database (`SupplyChain`). T-SQL was used to extract insights and build the logic for the dashboard. 

Beyond basic aggregations, advanced SQL techniques were utilized to create a definitive supplier ranking:

**Example: Composite Supplier Performance Score (Window Functions)**
This query ranks each supplier across three key metrics and combines them into a single overall performance score (Lower score = Better).

```sql
SELECT 
    Supplier_name,
    ROUND(AVG(Defect_rates), 2) AS avg_defect_rate,
    ROUND(AVG(Supplier_Lead_Time), 2) AS avg_lead_time,
    ROUND(AVG(Shipping_costs), 2) AS avg_shipping_cost,
    RANK() OVER (ORDER BY AVG(Defect_rates) ASC) AS defect_rank,
    RANK() OVER (ORDER BY AVG(Supplier_Lead_Time) ASC) AS leadtime_rank,
    RANK() OVER (ORDER BY AVG(Shipping_costs) ASC) AS cost_rank,
    RANK() OVER (ORDER BY AVG(Defect_rates) ASC) + 
    RANK() OVER (ORDER BY AVG(Supplier_Lead_Time) ASC) + 
    RANK() OVER (ORDER BY AVG(Shipping_costs) ASC) AS overall_score
FROM supply_chain
GROUP BY Supplier_name
ORDER BY overall_score ASC;

```

### Phase 3: Data Visualization (Microsoft Power BI)

The SQL output was imported into Power BI to create an interactive dashboard for stakeholders.

* **KPI Cards:** Displaying overall averages for defect rates, lead times, and shipping costs to set the baseline.
* **Composite Score Bar Chart:** Instantly highlighting top and bottom overall performers.
* **Inspection Results Stacked Column:** Revealing the proportion of Pass/Fail/Pending orders per supplier to highlight specific quality control failures.

---

## 📈 Key Findings

1. **The Clear Leader:** Supplier 1 ranks 1st overall, achieving the lowest defect rate (1.80), the fastest lead time (14 days), and maintaining a 48% inspection pass rate.
2. **The Hidden Quality Failure:** Supplier 4 appears decent on paper due to fast delivery and standard costs. However, visualization reveals they recorded **zero inspection passes** (12 fails, 6 pending). This highlights a severe quality control deficit not fully captured by average defect rates alone.
3. **The Worst Performer:** Supplier 5 ranks last across all dimensions, recording the highest defect rate (2.67), tying for the highest shipping costs ($5.79), and maintaining slow lead times.

---

## 💡 Strategic Recommendations

* **Volume Reallocation:** Shift active procurement volume toward Supplier 1 to stabilize product quality and reduce overall lead times.
* **Performance Improvement Plans (PIP):** Place Supplier 4 under immediate review for total quality assurance failures. Place Supplier 5 under review for comprehensive underperformance.
* **Cost/Time Strategy:** Supplier 3 offers the lowest shipping costs but has the longest lead time (20 days). This vendor should be strictly reserved for non-urgent stock replenishment where preserving margin is the only priority.

---

## 📂 Repository Files

* `dataset/`: Contains the raw and cleaned `.csv` files.
* `sql_queries/`: Full `.sql` script containing all analysis and ranking queries.
* `dashboard/`: The `.pbix` Power BI file.
* `images/`: Dashboard screenshots.

```

```
