# ⚽ FIFA 21 Data Analysis & Dashboard Project

A comprehensive end-to-end data analytics project featuring data wrangling, cleaning, and transformation using **Excel (Power Query)**, followed by interactive visualization and exploratory data analysis (EDA) using **Power BI**.

Based on the popular Kaggle dataset: [FIFA 21 messy, raw dataset for cleaning/ exploring](https://www.kaggle.com/datasets/yagunnersya/fifa-21-messy-raw-dataset-for-cleaning-exploring).

---

## 📋 Project Overview
Most online datasets are pre-cleaned and structured, making data preprocessing skills hard to practice. This project utilizes a notoriously raw, unformatted web-scraped dataset from FIFA 21. The goal was to transform unstructured text, fix inconsistent measurements, handle messy string notations, and build an insightful dashboard to evaluate player valuations, skill ratings, and club attributes.

---

## 🛠️ Tools & Technologies Used
* **Microsoft Excel / Power Query:** For ETL (Extract, Transform, Load), string manipulation, type conversion, and handling data irregularities.
* **Power BI Desktop:** For data modeling, DAX measures, and designing an interactive executive dashboard.

---

## 🧹 Data Cleaning & Transformation (The ETL Phase)
The raw dataset contained nearly 19,000 player records and over 70 columns. Key cleaning procedures handled via **Power Query** included:

1. **Text Cleanup:** Eliminated redundant whitespace or newline characters (`\n`) embedded across text fields.

2. **Currency & Financial Conversions:** Stripped Euro symbols (`€`) and text multipliers (`M` for Millions, `K` for Thousands) from columns like `Value`, `Wage`, and `Release Clause`, converting them into proper numerical data types.
    - =IF(IFERROR(SEARCH("M"; [column]); 0) <> 0; TRIM(SUBSTITUTE(SUBSTITUTE([column];"€";""); "M"; ""))*1000000; IF(IFERROR(SEARCH("K"; [column]); 0) <> 0; TRIM(SUBSTITUTE(SUBSTITUTE([column];"€";""); "K"; ""))*1000; TRIM(SUBSTITUTE([column];"€";""))*1))

3. **Physical Attribute Standardization:** 
   * Converted mixed height formats (feet/inches and cm) strictly into numerical centimeters (`cm`).
    - =SWITCH()
	- =TRIM(SUBSTITUTE([column]; "cm"; ""))
   * Converted weight entries (lbs and kg) into uniform numerical kilograms (`kg`).
    - =IF(IFERROR(SEARCH("lbs"; [column]); 0) <> 0; ROUND(TRIM(SUBSTITUTE([column]; "lbs"; "")) / 2,205;0); [column])
	- =TRIM(SUBSTITUTE([column]; "kgs"; ""))

4. **Date-Time Formatting:** Transformed the `Joined` string column into a genuine `Date` format to accurately compute player tenure.

5. **Special Characters Removal:** Cleaned rating/attribute columns contaminated with unwanted symbol strings (such as `★` stars in Weak Foot, Skill Moves, and International Reputation).

6. **Contract Splitting:** Split concatenated contract columns into discrete `Contract Start` and `Contract End` fields.

7. **Player Contract Status:** Created a new column called `Player Status` for player contract statuses, to eliminate mixed values in the `Contract Start` and `Contract End` fields.
    - =IF(IFERROR(SEARCH("On Loan"; [column]); 0) <> 0; "On Loan"; IF(IFERROR(SEARCH("Free"; [column]); 0) <> 0; "Free"; "Permanent"))

8. **Loan Date Column Removal:** Removed loan dates from the entire dataset as dates were incohesive (start dates matched end dates). Imputed the missing date values with 0 (to avoid calculation errors)
    - =IF(LEN(TRIM(K2)) <> 4; 0; K2)

---

## 📊 Exploratory Data Analysis (EDA) & Key Insights {IN PROGRESS}
Using the cleaned data model in Power BI, the analysis answered several core business and scouting questions:

* **Market Value vs. Wages:** Highlighted "hidden gems"—undervalued players operating on low weekly wages relative to their high overall skill ratings and market worth.
* **Age vs. Potential:** Mapped player peaks, contrasting current overall ratings (`OVA`) against potential ceilings (`POT`) to pinpoint high-potential wonderkids.
* **Club Dominance:** Evaluated top-heavy team distributions, total squad market valuations, and average player ratings across premier leagues.
* **Physical & Technical Profiles:** Explored correlations between player positions, physical traits (height/weight), and specialized attribute ratings (shooting, pacing, defending).

---

## 🖥️ Power BI Dashboard Preview {TO-DO}
*(Insert screenshots or a brief structural description of your Power BI report pages here)*
* **Page 1: Player Overview & Scouting Matrix** — Filterable search card for players, comparing Overall Rating, Potential, Market Value, and Wage.
* **Page 2: Financial & Club Analysis** — Deep dive into club wage bills, squad sizes, and total market capitalization.

---

## 🚀 Getting Started & Replication
If you want to run or replicate this project locally:

1. Download the raw dataset files from [Kaggle](https://www.kaggle.com/datasets/yagunnersya/fifa-21-messy-raw-dataset-for-cleaning-exploring).
2. Open **Microsoft Excel**, launch **Power Query**, and step through the transformation queries (or load your cleaned dataset if provided in the repository).
3. Open the `.pbix` file in **Power BI Desktop** to explore the pre-built data model, relationships, and visual layouts.

---

## 💡 Future Improvements
* Incorporate machine learning regression models in Python to predict player market values based on physical and performance attributes.
* Expand dashboard interactivity with advanced bookmarks and dynamic page navigation.