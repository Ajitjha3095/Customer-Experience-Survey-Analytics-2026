# Project Brief: Customer Experience Survey Analytics

| | |
|---|---|
| **Client** | TechMart (electronics retailer, India) |
| **Requested by** | Head of Customer Experience (CX) |
| **Prepared by** | Ajit Jha, Data Analyst |
| **Project type** | Data cleaning and business intelligence reporting |
| **Tools** | Python (Jupyter Notebook), Power BI, Tableau, Looker Studio |

---

## 1. Background

TechMart sells smartphones, laptops, headphones, smart watches, tablets and speakers through four channels (Online, Store, App, Marketplace) across ten Indian cities. The company ran a customer experience survey and exported the responses to a CSV file.

The export is not ready for reporting. It contains duplicate responses, inconsistent city names, mixed date formats, impossible values and text typed into numeric fields. Leadership cannot trust the numbers, so there is no reliable view of customer satisfaction or loyalty.

## 2. Business Problem

> "We do not know which cities, products and customer groups are unhappy, and we cannot rely on the survey data to find out."

## 3. Objectives

1. Convert the raw survey export into a clean, documented, analysis-ready dataset.
2. Give the CX team one consistent dashboard, delivered in Power BI, Tableau and Looker Studio, so each team can use its preferred tool.
3. Make it easy to answer where satisfaction and NPS are strongest and weakest, by city, product, channel and age group.
4. Provide findings and recommendations that the CX head can act on.

## 4. Scope

**In scope**
- Profiling and cleaning of the raw survey file in a Jupyter notebook.
- A clean dataset export, with every cleaning decision documented.
- One dashboard design built in three tools (Power BI, Tableau, Looker Studio).
- A written insights summary with recommendations.
- A README and project documentation.

**Out of scope**
- Collecting new survey data or changing the survey form.
- Graph or network analysis (for example Neo4j).
- Predictive modelling.
- Live data refresh or automation.

## 5. Stakeholders

| Stakeholder | Interest |
|---|---|
| Head of CX | Needs the headline numbers and recommended actions |
| City and regional managers | Need performance by city |
| Product managers | Need satisfaction by product |
| Analytics team | Need a documented, reusable clean dataset |

## 6. Data Description

| Item | Detail |
|---|---|
| File | `survey_data_raw.csv` (synthetic practice data) |
| Size | 500 rows, 20 columns |
| Period | January to September 2026 |
| Identifiers | `respondent_id`, `referred_by` |
| Demographics | `age`, `gender`, `city`, `state`, `income_band` |
| Purchase | `product_purchased`, `purchase_channel`, `monthly_spend_inr`, `survey_date` |
| Experience | `satisfaction_score` (1-5), `nps_score` (0-10), `delivery_rating` (1-5), `support_rating` (1-5), `would_recommend` |
| Other | `competitors_used` (multi-value), `feedback_comment`, `full_name`, `email` |

**Known data quality issues**

| Issue | Example |
|---|---|
| Duplicate responses | Repeated `respondent_id` |
| Inconsistent city names | Bangalore, BLR, Bengaluru |
| Mixed date formats | `2026-03-05`, `05/03/2026`, `05-Mar-2026`, `03-05-26` |
| Impossible ages | -5, 150, 999, "thirty" |
| Invalid ratings | 0, 6, 4.5, "Five" |
| Invalid NPS | 11, 15, -1, blank |
| Currency symbols and outliers in spend | `₹5000`, `5000/-`, 5,000,000 |
| Inconsistent categories | M / F / male / female; lowercase product names |
| Multi-value field | `competitors_used` stored as a comma list |
| Invalid emails | Missing `@` |

## 7. Requirements

### 7.1 Data cleaning (Jupyter Notebook)

| ID | Requirement |
|---|---|
| DC-01 | Profile the data: shape, data types, missing values per column, duplicates, and value ranges |
| DC-02 | Remove duplicate responses and report how many were removed |
| DC-03 | Standardise text fields (gender, product, names): trim spaces, consistent case, one label per category |
| DC-04 | Standardise city names and check each city maps to a single state |
| DC-05 | Convert all dates to one datetime format, with a stated rule for ambiguous dates |
| DC-06 | Treat impossible ages consistently and state the rule for missing ages |
| DC-07 | Validate ratings against their ranges (satisfaction 1-5, NPS 0-10, delivery 1-5, support 1-5) and state how invalid values are handled |
| DC-08 | Clean spend values (remove symbols, handle negatives) and treat outliers with a stated method such as IQR |
| DC-09 | Validate email addresses, or remove personal fields that are not needed for the analysis |
| DC-10 | Split the multi-value competitor field into one row per respondent and competitor |
| DC-11 | Create derived fields: `Age_Group`, `survey_month`, `nps_category` (Promoter, Passive, Detractor) |
| DC-12 | Write a short markdown note in the notebook for every cleaning decision |

### 7.2 Metric definitions

| Metric | Definition |
|---|---|
| Total respondents | Count of unique `respondent_id` after cleaning |
| Average satisfaction | Mean of `satisfaction_score` (1-5) |
| NPS | (% Promoters, score 9-10, minus % Detractors, score 0-6) x 100 |
| Average monthly spend | Mean of `monthly_spend_inr` after outlier treatment |
| % Would recommend | Share of respondents answering "Yes" |

The handling of invalid NPS entries (counted as Detractors or excluded) must be stated, and the effect on the NPS shown.

### 7.3 Dashboard requirements (same design in all three tools)

**KPI cards:** total respondents, average satisfaction, NPS, average monthly spend, % would recommend.

**Visuals**

| ID | Visual | Purpose |
|---|---|---|
| V-01 | Satisfaction trend by month | See movement over time |
| V-02 | Purchase channel share | Understand channel mix |
| V-03 | Product vs average satisfaction | Compare products |
| V-04 | NPS by city | Find the strongest and weakest cities |
| V-05 | Age group vs average satisfaction | Compare customer segments |
| V-06 | Delivery vs support vs satisfaction | See how service ratings relate |
| V-07 | Competitor usage | Show which competitors customers also use |

**Filters:** City, Product, Channel; a date-range control where the tool supports it and the data spans enough time.

**Tool-specific requirements**

| Tool | Requirement |
|---|---|
| Power BI | At least three DAX measures: NPS, Promoter %, and month-over-month change |
| Tableau | At least two calculated fields and one parameter or dashboard action |
| Looker Studio | At least one calculated field (for example NPS) and a date-range control |

**Design standards:** consistent colours, clear titles and axis labels, data labels where useful, bar charts starting at zero, and a one-line headline insight under the title.

### 7.4 Quality requirements

- The same headline figures must appear in all three dashboards.
- Every number must trace back to the clean dataset.
- Averages must exclude missing values, not treat them as zero (except where a documented rule says otherwise).

## 8. Deliverables

| # | Deliverable | Format |
|---|---|---|
| 1 | Cleaning notebook with decision notes | `.ipynb` |
| 2 | Clean dataset | `.csv` |
| 3 | Competitor table (one row per respondent and competitor) | `.csv` |
| 4 | Power BI dashboard | `.pbix` plus screenshots |
| 5 | Tableau dashboard | Tableau Public link |
| 6 | Looker Studio dashboard | Shareable link |
| 7 | Insights summary with recommendations | Document (this brief, Sections 12-13) |
| 8 | README | `.md` |

## 9. Success Criteria

The project is accepted when:

1. The clean dataset has no duplicates, invalid ratings, mixed date formats or unlabelled outliers, and each decision is documented.
2. All seven visuals, five KPI cards and the filters are present in each tool.
3. The three dashboards agree on every KPI.
4. The tool-specific requirements are met (DAX measures, calculated fields, parameter or action, date control).
5. The CX head can state, from the dashboards alone, the weakest city, product and age group.

## 10. Assumptions

- The dataset is synthetic and used for demonstration.
- Slash-format dates are day/month; hyphenated two-digit-year dates are month-day-year.
- Names and emails are not needed for the analysis and may be removed.
- Invalid NPS entries are counted as Detractors unless stated otherwise, and the alternative result is reported.

## 11. Risks

| Risk | Impact | Mitigation |
|---|---|---|
| Ambiguous dates read incorrectly | Wrong monthly trend | Parse with explicit formats and check against the raw file |
| Filling invalid ratings with 0 | Understated averages, wrong NPS | Keep invalid values blank, or document the rule |
| Outliers in spend | Inflated average spend | Apply IQR cap and report both figures |
| Different logic across tools | Conflicting numbers | Use one clean file and the same metric definitions |
| Small sample in a group or month | Misleading conclusions | Show respondent counts and flag small groups |

## 12. Findings (from the completed analysis)

1. **NPS is negative at -34.08.** Excluding the 31 invalid entries gives -29.63.
2. **Hyderabad (about -55) and Saharanpur (about -51) have the weakest NPS.** Pune is the strongest at about -12.
3. **Smart Watch and Laptop lead on satisfaction (3.7); Smartphone is lowest (3.3).**
4. **The 18-24 group is the most satisfied (3.65); the 35-54 group the least (3.43).**
5. **Satisfaction peaks in March and drops sharply in September.**
6. **The four channels each hold about a quarter of responses.**

## 13. Recommendations

1. Investigate Hyderabad and Saharanpur first, using delivery and support ratings and comments.
2. Review the Smartphone experience.
3. Find what drives lower scores in the 35-54 group.
4. Confirm whether the September drop is real or a smaller sample before acting.
5. Prevent data issues at the source: use dropdowns for city, product and gender, and limit rating fields to valid ranges on the survey form.

## 14. Project Phases

| Phase | Activities |
|---|---|
| 1. Understand | Review the raw file, list issues, agree metric definitions |
| 2. Clean | Jupyter cleaning, derived fields, clean exports, decision notes |
| 3. Build | Power BI first, then Tableau and Looker Studio using the same design |
| 4. Validate | Reconcile KPIs across tools and against the clean file |
| 5. Deliver | Insights summary, README, final links |
