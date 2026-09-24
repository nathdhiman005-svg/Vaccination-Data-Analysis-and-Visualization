# Global Vaccination Data Analysis & Visualization 🌍💉

![Python](https://img.shields.io/badge/Python-3.11-blue.svg)
![Pandas](https://img.shields.io/badge/Pandas-Data_Wrangling-yellow)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Neon_Tech-336791.svg)
![Power BI](https://img.shields.io/badge/Power_BI-Data_Visualization-F2C811.svg)

## 📌 Project Overview
This project is an end-to-end data engineering and business intelligence capstone designed to track global vaccination efforts and analyze their direct impact on disease incidence. 

Starting from complex, unstructured datasets provided by the World Health Organization (WHO), this project architects a robust **ETL Pipeline**, stores the transformed data in a **Cloud-hosted Star Schema Database**, and surfaces real-time insights through a dynamic **Power BI Dashboard**. 

The ultimate business objective is to empower global health stakeholders to monitor progress toward the WHO's **95% vaccination target**, pinpoint regional vulnerabilities, and deploy resources efficiently to prevent disease outbreaks.

---

## 🛠️ Technology Stack
* **Data Wrangling & Analysis:** Python, Pandas, Matplotlib, Seaborn, Jupyter Notebooks
* **Database & Cloud Storage:** PostgreSQL, Neon Tech, SQLAlchemy, Psycopg2
* **Business Intelligence & Visualization:** Microsoft Power BI

---

## 🏗️ Pipeline Architecture

### Phase 1: Data Wrangling (Python)
- **Data Source:** Raw historical WHO Excel/CSV datasets spanning decades (Coverage vs. Incidence).
- **Transformation:** Unpivoted heavily denormalized datasets (using `pd.melt()`) to convert wide-format yearly columns into a strict, time-series vertical format.
- **Cleaning:** Handled massive arrays of nulls, standardized disparate naming conventions across datasets, and established cohesive mapping logic.

### Phase 2: Database Engineering (Neon PostgreSQL)
- **Data Modeling:** Architected a relational **Star Schema** to eliminate redundancy and optimize business intelligence querying.
- **Structure:**
  - **Dimensions:** `dim_country`, `dim_disease`, `dim_vaccine` (Centralized bridging tables)
  - **Facts:** `fact_coverage`, `fact_incidence` (Independent metric tables)
- **Automation:** Authored a Python pipeline (`upload_to_neon.py`) using `SQLAlchemy` to programmatically extract the cleaned dataframes and bulk-insert them into the Neon Cloud Database.

### Phase 3: Business Intelligence (Power BI)
- **Integration:** Connected Power BI directly to the Neon PostgreSQL database for live/scheduled cloud refreshes.
- **Visuals Developed:**
  - 🗺️ **Geographical Heatmap:** Highlights average regional coverage disparities.
  - 📈 **Historical Trend Lines:** 40-year tracking of vaccination growth by WHO regions.
  - 🔍 **Correlation Scatter Plot:** Exploded country-level visualization proving the inverse relationship between high vaccination coverage and disease incidence.
  - 🎯 **Target KPI Gauge:** Live tracker measuring global progress against the 95% target.

---

## 🚀 How to Run Locally

If you wish to clone this repository and run the Python ETL scripts locally, follow these steps:

**1. Clone the repository**
```bash
git clone https://github.com/nathdhiman005-svg/Vaccination-Data-Analysis-and-Visualization.git
cd Vaccination-Data-Analysis-and-Visualization
```

**2. Set up the virtual environment**
```bash
python -m venv venv
venv\Scripts\activate
```

**3. Install dependencies**
```bash
pip install -r requirements.txt
```

**4. Set up your Environment Variables**
Create a `.env` file in the root directory and add your Neon PostgreSQL credentials:
```env
NEON_DATABASE_URL="postgresql://user:password@your-neon-hostname.neon.tech/neondb?sslmode=require"
```

**5. Execute the Notebook**
Run `Vaccination_EDA.ipynb` to execute the data cleaning pipeline, which will push the data directly to your configured cloud database.

---

## 📊 Key Findings & Business Insights
1. **Vaccines Work:** Our correlation analysis definitively proves that regions achieving **>90% vaccination coverage** experience near-zero incidence rates for preventable diseases.
2. **Regional Disparities:** The Geographical Heatmaps highlight a systemic vulnerability in the **African Region**, which consistently lags behind global coverage averages and requires immediate targeted funding.
3. **Booster Drop-off:** While primary (1st dose) coverage is high globally, the data shows a severe drop-off rate for follow-up booster doses, highlighting a critical need for public health follow-up campaigns.

---
*This repository contains the Python scripts and project reports. Due to GitHub file size limits and privacy restrictions, the `.pbix` Power BI dashboard file and raw datasets are stored locally.*
