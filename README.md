<div align="center">
  
  <img width="745" height="449" alt="image" src="https://github.com/user-attachments/assets/4c900579-3bf1-4e32-88ec-14004cbc06dc" />



# Customer Experience Survey Analytics

### End-to-End BI Project — Data Cleaning → Power BI · Tableau · Looker Studio

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![Tableau](https://img.shields.io/badge/Tableau-E97627?style=for-the-badge&logo=tableau&logoColor=white)
![Looker Studio](https://img.shields.io/badge/Looker%20Studio-4285F4?style=for-the-badge)
![DAX](https://img.shields.io/badge/DAX-KPI%20Measures-5E5E5E?style=for-the-badge)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

> A complete end-to-end analyst portfolio project built across three industry-standard BI platforms.
> Raw survey data → Cleaning in Python → Clean dataset → Power BI · Tableau · Looker Studio


</div>

## Live Demos

A single-page BI dashboard analyzing customer satisfaction, NPS, and spend for a 2026 electronics-retail survey — built in Power BI, Tableau, and Looker Studio from the same cleaned dataset.

| Platform       | Link |
| -------------- | ---- |
| Power BI       | See `/dashboards/powerbi` folder (`.pbix` file) |
| Tableau Public | [Customer Experience Survey Analytics](https://public.tableau.com/app/profile/ajit.jha/viz/CustomerExperienceSurveyAnalytics/Dashboard1) |
| Looker Studio  | [Live Report](https://datastudio.google.com/s/rzDBAKVRg4g) |
| GitHub Repo    | [Ajitjha3095/Customer-Experience-Survey-Analytics](https://github.com/Ajitjha3095/Customer-Experience-Survey-Analytics-2026) |

## 📑 Table of Contents

- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Business Requirements](#business-requirements)
- [Dataset](#dataset)
- [Data Cleaning](#data-cleaning)
- [KPIs & Calculations](#kpis--calculations)
  - [DAX (Power BI)](#dax-power-bi)
  - [Tableau (Calculated Fields)](#tableau-calculated-fields)
  - [Looker Studio (Calculated Fields)](#looker-studio-calculated-fields)
- [Dashboard Layout](#dashboard-layout)
- [Key Insights](#key-insights)
- [Tech Stack](#tech-stack)
- [Skills Demonstrated](#skills-demonstrated)
- [Repository Structure](#repository-structure)
- [How to Run](#how-to-run)
- [Author](#author)
- [License](#license)

---

## Project Overview

This project demonstrates an end-to-end data analytics and business intelligence workflow using Python and modern visualization tools. A messy 500-row customer survey export is profiled, cleaned and transformed in a Jupyter Notebook, then turned into one consistent dashboard in Power BI, Tableau, and Looker Studio.

The project covers data profiling, data cleansing, data validation, KPI design, and cross-platform dashboard development, with every cleaning decision documented.

## Business Problem

TechMart, a fictional electronics retailer, sells smartphones, laptops, headphones, smart watches, tablets and speakers through four channels (Online, Store, App, Marketplace) across ten Indian cities. A customer experience survey was exported to CSV, but the file was inconsistent and leadership could not trust the numbers.

Leadership needed visibility into:

- What overall satisfaction and NPS look like, and how they move month to month
- Which cities and products have the weakest customer experience
- Which customer age groups are least satisfied
- How delivery and support ratings relate to overall satisfaction

This dashboard was built to answer those questions in a single view, for both technical stakeholders and non-technical management.

## Business Requirements

|   | Requirement |
| - | ----------- |
| 1 | Clean, documented dataset with no duplicates, invalid ratings or mixed date formats |
| 2 | Single-page dashboard, understandable by non-technical management in under 30 seconds |
| 3 | Five headline KPIs: respondents, satisfaction, NPS, average spend, % would recommend |
| 4 | Break down satisfaction and NPS by Month, City, Product, Channel and Age Group |
| 5 | Support filtering by City, Product and Channel without page reload |
| 6 | Reporting period: January to September 2026 |
| 7 | Rebuildable across multiple BI tools (Power BI, Tableau, Looker Studio) with identical KPI values |

---

## Dataset

**Source file:** `survey_data_raw.csv` (synthetic data, created for practice and intentionally messy)

**Size:** 500 rows × 20 columns (490 rows after removing duplicates)

| Column | Description |
| ------ | ----------- |
| `respondent_id` | Unique respondent identifier |
| `age` | Respondent age |
| `gender` | Male, Female, Other |
| `city` / `state` | Delhi, Mumbai, Bengaluru, Pune, Lucknow, Saharanpur, Jaipur, Kolkata, Chennai, Hyderabad |
| `survey_date` | Date of survey response |
| `product_purchased` | Smartphone, Laptop, Headphones, Smart Watch, Tablet, Speaker |
| `purchase_channel` | Online, Store, App, Marketplace |
| `satisfaction_score` | Overall satisfaction, 1 to 5 |
| `nps_score` | Likelihood to recommend, 0 to 10 |
| `delivery_rating` | Delivery rating, 1 to 5 |
| `support_rating` | Support rating, 1 to 5 |
| `monthly_spend_inr` | Monthly spend in INR |
| `income_band` | <3L, 3-6L, 6-10L, 10-20L, 20L+ |
| `competitors_used` | Competitors the customer also uses (multi-value) |
| `would_recommend` | Yes / No |

Raw and cleaned data live in `/data`. The cleaning notebook lives in `/notebooks`.

---

## Data Cleaning

| Issue found in raw data | Fix applied |
| ----------------------- | ----------- |
| Duplicate respondents | Removed on `respondent_id` (500 → 490) |
| Inconsistent gender and product labels | Standardised case and labels, trimmed spaces |
| City variants (Bangalore, BLR, Bombay) | Mapped to one standard name per city |
| Four mixed date formats | Parsed with explicit formats; slash dates read as day/month |
| Ratings as text or out of range | Converted to numbers; invalid delivery and support ratings left blank |
| Currency symbols in spend (`₹`, `/-`) | Removed and converted to numeric; negatives set to blank |
| Names and emails | Dropped (not needed for analysis, reduces personal data) |
| Derived fields needed for charts | Added `Age_Group`, `survey_month`, `nps_category`, `delivery_missing` |

**NPS decision:** 31 respondents entered invalid NPS values (11, 15, -1 or blank). These are counted as 0 (Detractor), which gives an NPS of **-34.08**. Excluding them gives **-29.63**.

---

## KPIs & Calculations

| KPI | Logic |
| --- | ----- |
| **Total Respondents** | Count of unique `respondent_id` |
| **Average Satisfaction** | Average of `satisfaction_score` (1 to 5) |
| **NPS** | (% Promoters, score 9-10, minus % Detractors, score 0-6) × 100 |
| **Average Monthly Spend** | Average of `monthly_spend_inr` |
| **% Would Recommend** | Respondents answering "Yes" ÷ total respondents |

### DAX (Power BI)

```
Total Respondents =
DISTINCTCOUNT(survey_data_clean[respondent_id])

Avg Satisfaction =
AVERAGE(survey_data_clean[satisfaction_score])

NPS =
VAR Total = COUNTROWS(survey_data_clean)
VAR Promoters = CALCULATE(COUNTROWS(survey_data_clean), survey_data_clean[nps_score] >= 9)
VAR Detractors = CALCULATE(COUNTROWS(survey_data_clean), survey_data_clean[nps_score] <= 6)
RETURN DIVIDE(Promoters - Detractors, Total) * 100

Avg Monthly Spend =
AVERAGE(survey_data_clean[monthly_spend_inr])

Pct Would Recommend =
DIVIDE(
    CALCULATE(COUNTROWS(survey_data_clean), survey_data_clean[would_recommend] = "Yes"),
    COUNTROWS(survey_data_clean)
)
```

### Tableau (Calculated Fields)

```
Total Respondents     : COUNTD([Respondent Id])
Avg Satisfaction      : AVG([Satisfaction Score])
NPS                   : (SUM(IF [Nps Score] >= 9 THEN 1 ELSE 0 END) - SUM(IF [Nps Score] <= 6 THEN 1 ELSE 0 END)) / COUNT([Respondent Id]) * 100
Avg Monthly Spend     : AVG([Monthly Spend Inr])
Pct Would Recommend   : SUM(IF [Would Recommend] = "Yes" THEN 1 ELSE 0 END) / COUNT([Respondent Id])
```

### Looker Studio (Calculated Fields)

```
Total Respondents     : COUNT_DISTINCT(respondent_id)
Avg Satisfaction      : AVG(satisfaction_score)
NPS                   : (SUM(CASE WHEN nps_score >= 9 THEN 1 ELSE 0 END) - SUM(CASE WHEN nps_score <= 6 THEN 1 ELSE 0 END)) / COUNT(respondent_id) * 100
Avg Monthly Spend     : AVG(monthly_spend_inr)
Pct Would Recommend   : SUM(CASE WHEN would_recommend = "Yes" THEN 1 ELSE 0 END) / COUNT(respondent_id)
```

---

## Dashboard Layout

- **Header** — Title + City / Product / Channel slicers
- **KPI Row** — 5 cards: Total Respondents, Average Satisfaction, NPS, Average Monthly Spend, % Would Recommend
- **Satisfaction Trend by Month** — Line chart
- **Purchase Channel Share** — Donut chart
- **Product vs Average Satisfaction** — Horizontal bar chart, sorted descending
- **Delivery vs Support vs Satisfaction** — Scatter plot by product
- **NPS by City** — Horizontal bar chart, sorted by NPS
- **Age Group vs Satisfaction** — Horizontal bar chart

---

## Key Insights

- Overall NPS is **-34.08**, so detractors clearly outnumber promoters. Excluding invalid entries it is still **-29.63**.
- **Hyderabad (about -55)** and **Saharanpur (about -51)** have the weakest NPS. **Pune (about -12)** is the strongest of the ten cities, though still negative.
- **Smart Watch and Laptop** lead on satisfaction (**3.7**), while **Smartphone** is lowest (**3.3**).
- The **18-24 age group** is the most satisfied (**3.65**); the **35-54 group** is the least satisfied (**3.43**).
- Satisfaction peaks in **March** and falls sharply in **September**.
- All four purchase channels hold roughly a quarter of responses each, so no single channel dominates.

---

## Tech Stack

- **BI Tools:** Power BI, Tableau Public, Looker Studio
- **Data Prep:** Python (Pandas, NumPy), Jupyter Notebook
- **Data Format:** CSV
- **Version Control:** GitHub

## Skills Demonstrated

### Data Analytics

- Python
- Data Profiling
- Data Cleaning
- Data Transformation
- Data Validation
- Data Preparation

### Data Modeling

- KPI Development
- Business Metrics Design
- Calculated Fields
- Derived Fields

### Business Intelligence

- Power BI
- DAX
- Tableau
- Tableau Calculated Fields
- Google Looker Studio
- Looker Studio Calculated Fields
- Interactive Dashboard Development
- Data Visualization
- Data Storytelling

### Analytical Skills

- Customer Satisfaction Analysis
- NPS Analysis
- Customer Segmentation
- Performance Monitoring
- Dashboard Design

## Repository Structure

```
Customer Experience Survey Analytics/
│
├── data/                       # Datasets
│   ├── survey_data_raw.csv     # Raw survey export (500 rows)
│   └── survey_data_clean.csv   # Cleaned dataset (490 rows)
│
├── notebooks/
│   └── cleaning.ipynb          # Data profiling, cleaning and export
│
├── dashboards/                 # Business Intelligence Dashboards
│   ├── powerbi/                # Power BI project file (.pbix)
│   └── tableau/                # Tableau workbook files
│
├── images/                     # Dashboard previews
│   ├── powerbi.png             # Power BI dashboard screenshot
│   ├── tableau.png             # Tableau dashboard screenshot
│   └── looker.png              # Looker Studio dashboard screenshot
│
├── project_brief_final.md      # Project requirements brief
├── LICENSE                     # Project license
└── README.md                   # Project documentation
```

## How to Run

### 1. Clone the Repository

```
git clone https://github.com/Ajitjha3095/Customer-Experience-Survey-Analytics-2026.git
cd Customer-Experience-Survey-Analytics-2026
```

### 2. Run the Cleaning Notebook

```
pip install pandas numpy jupyter
jupyter notebook notebooks/cleaning.ipynb
```

Set the input path to `data/survey_data_raw.csv`, run all cells, and export `survey_data_clean.csv`.

### 3. Open the Dashboard

Choose your preferred BI tool:

- **Power BI:** Open the `.pbix` file using Power BI Desktop.
- **Tableau:** Open the Tableau Public link above, or the `.twbx` file in Tableau Desktop/Public.
- **Looker Studio:** Open the live report, or connect `survey_data_clean.csv` to Looker Studio and add the calculated fields above.

### 4. Explore the Dashboard

Use the available filters and slicers to analyze:

- Satisfaction and NPS trends
- City performance
- Product performance
- Age group differences
- Purchase channel mix

## Author

**Ajit Jha**
*Data Analytics Portfolio Project*

- **GitHub:** [Ajitjha3095](https://github.com/Ajitjha3095)
- **LinkedIn:** [Ajit Jha](https://www.linkedin.com/in/ajitjha01/)

Passionate about building industry-oriented data analytics projects and transforming data into meaningful business insights. Always interested in learning, collaborating, and exploring new opportunities in Data Analytics and BI.

Feel free to connect or reach out for project discussions, collaboration, or professional opportunities!

---

## License

This project is licensed under the **MIT License**.
See the [LICENSE](LICENSE) file for details.
