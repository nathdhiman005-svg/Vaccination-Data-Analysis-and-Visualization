# Vaccination Data Analysis and Visualization: Final Project Report

## 1. Executive Summary
This project aimed to build an automated, end-to-end data pipeline to analyze global vaccination coverage and disease incidence. The ultimate goal was to provide actionable insights for global health organizations to monitor progress toward the 95% vaccination target, identify regional disparities, and deploy targeted interventions. The project successfully bridged Data Engineering (Python/SQL) and Business Intelligence (Power BI) to create a fully interactive tracking dashboard.

## 2. Project Process & Pipeline Architecture

The pipeline was executed in three distinct phases:

### Phase 1: Data Wrangling and Exploratory Data Analysis (Python)
- **Extraction:** Raw data was extracted from complex, multi-tab Excel files containing decades of historical WHO data.
- **Transformation:** Utilizing the `pandas` library in Jupyter Notebooks, the data underwent rigorous cleaning. We reshaped (melted) wide-format yearly columns into a vertical time-series format, handled missing/null values, and standardized naming conventions across disparate datasets.
- **EDA:** We conducted statistical summaries and correlation analysis, identifying immediate trends (e.g., lower-income regions lagging in coverage).

### Phase 2: Database Engineering (PostgreSQL / Neon Tech)
- **Architecture:** We designed a robust **Star Schema** data model to optimize the data for reporting. 
- **Implementation:** The cleaned data was segregated into three Dimension tables (`dim_country`, `dim_disease`, `dim_vaccine`) and two Fact tables (`fact_coverage`, `fact_incidence`).
- **Automation:** We authored an automated Python script (`upload_to_neon.py`) using `SQLAlchemy` to extract the cleaned dataframes directly from the Jupyter Notebook environment and bulk-insert them into a cloud-hosted Neon PostgreSQL database.

### Phase 3: Business Intelligence (Power BI)
- **Integration:** Power BI was connected directly to the Neon PostgreSQL database via a Direct/Import connection, enabling scheduled cloud refreshes.
- **Data Modeling:** The Star Schema relationships were actively mapped in Power BI, allowing dimensions to filter multiple fact tables seamlessly.
- **Visualization:** We built an interactive dashboard containing Geographical Heatmaps (highlighting regional disparities), 40-year Historical Trend Lines, Scatter Plots (correlating high coverage with low incidence), and KPI Indicators tracking the 95% global target. 

---

## 3. Challenges Faced

1. **Complex and Denormalized Raw Data:** 
   The initial raw datasets were presented in pivot-table-like formats where years were spread horizontally across dozens of columns, making programmatic analysis nearly impossible.
   
2. **Disparate Identifiers and Missing Keys:** 
   The `coverage` dataset and the `incidence` dataset used completely different naming conventions for vaccines and diseases (e.g., "Polio" vs "POL3"). Additionally, many niche vaccines contained only a single year of historical data, which broke standard trend visualizations.

3. **Power BI Aggregation and Cross-Filtering Conflicts:**
   During the visualization phase, attempting to plot cross-table metrics (Coverage vs. Incidence) resulted in broken aggregations (e.g., Power BI collapsing 190 countries into a single global average dot). Furthermore, Power BI's automatic relationship AI struggled to link the Star Schema perfectly due to partial nulls in historical keys.

---

## 4. Solutions Implemented

1. **Programmatic Data Reshaping:**
   We utilized the `pd.melt()` function in pandas to forcefully unpivot the yearly columns into a strict, machine-readable format (`year`, `value`). This allowed for seamless aggregation and merging later in the pipeline.

2. **Star Schema Centralization:**
   To solve the naming and linking inconsistencies, we decoupled the descriptive data from the numerical data. By extracting a centralized `dim_country` table that acted as a bridge between the two independent fact tables, we ensured that a single filter (e.g., "Afghanistan") would accurately query both tables simultaneously without data loss.

3. **Explicit Visualization Formatting:**
   To solve the Power BI aggregation issues in the Scatter Plot, we explicitly forced the granularity of the visual by assigning the `country_name` from our centralized dimension bridge into the `Legend` bucket. For vaccines with only a single year of data, we dynamically altered the visual properties to render Data Markers, ensuring no data was lost due to Power BI's default continuous-line rendering logic.

---

## 5. Key Business Insights
- **Vaccine Efficacy:** The correlation scatter plot definitively proved that regions with vaccination coverage exceeding 90% experience near-zero incidence rates for related diseases.
- **Geographical Disparity:** The African Region consistently flags as a high-risk zone on the geographical heatmap, requiring immediate targeted campaign funding.
- **Booster Drop-off:** While initial dose coverage is high globally, subsequent booster doses see a sharp decline, indicating a need for public health follow-up campaigns.

## 6. Conclusion
By transitioning from static, messy Excel spreadsheets to a fully automated, cloud-hosted relational database feeding into a dynamic Power BI dashboard, we have established a highly scalable architecture. Global health stakeholders can now monitor the 95% global target in real-time and make data-driven decisions regarding vaccine deployment and funding.
