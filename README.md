# 🍔 Fast Food in India — Power BI Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-217346?style=for-the-badge&logo=microsoftexcel&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=for-the-badge)

## 📌 Brief 
An end-to-end Power BI dashboard that cleans a deliberately messy 200-row fast food dataset and turns it into a 3-page interactive analytics report covering revenue, customer behaviour, and delivery performance across 12 major Indian fast food chains.

---

## 📖 Overview
This project analyses order-level data from **12 fast food chains** (McDonald's, KFC, Domino's, Haldiram's, Wow! Momo, Biryani By Kilo, and others) operating across Indian cities. The raw data was intentionally messy — inconsistent text casing, mixed currency symbols, blank ratings, and multiple date formats — to simulate a real-world analytics scenario. The final deliverable is a fully interactive, 3-page Power BI dashboard with KPI cards, 12 charts, DAX measures, and synced slicers.

---

## ❓ Problem Statement
Fast food chains generate large volumes of order data daily, but raw exports are rarely analysis-ready. The goal of this project was to:
- Clean and standardize a messy dataset using Power Query
- Build reliable DAX measures for revenue, ratings, delivery, and customer loyalty
- Design an interactive dashboard that lets stakeholders slice performance by chain, city, time slot, and order mode
- Surface actionable insights on revenue drivers, customer behaviour, and delivery efficiency

---

## 🗂 Dataset
| Detail | Value |
|---|---|
| File | `FastFood_Data_Raw.csv` |
| Records | 200 rows |
| Columns | 20 |
| Time Range | Jan 2019 – Dec 2024 |
| Chains Covered | Barbeque Nation, Biryani By Kilo, Box8, Burger King, Domino's, Fasos, Haldiram's, KFC, McDonald's, Pizza Hut, Subway, Wow! Momo |

**Key columns:** `Order_ID`, `Chain_Name`, `Food_Item_Type`, `City`, `Customer_Age`, `Gender`, `Order_Mode`, `Time_Slot`, `Veg_NonVeg`, `Item_Price_INR`, `Discount_Percent`, `Quantity`, `Total_Bill_INR`, `Payment_Mode`, `Customer_Rating`, `Delivery_Time_Min`, `Repeat_Visit`, `Loyalty_Points`, `Order_Date`, `Coupon_Used`

**Data quality issues resolved:**
- Inconsistent categorical text (`Dine-In` / `dine-in` / `DINE-IN` / `Zomato` / `Swiggy`) → grouped into 3 standardized order modes
- Mixed currency formatting (`₹149`, `Rs.99`, plain numbers) → converted to Whole Numbers
- `'20%'` strings mixed with integers in discount column → stripped to numeric
- Blank/`NA` customer ratings → converted to proper nulls
- `'30 min'` strings in delivery time → numeric extraction
- 10+ Yes/No variants in `Repeat_Visit` and `Coupon_Used` → standardized to Yes/No
- 5 different date formats in `Order_Date` → converted to proper Date type

---

## 🛠 Tools and Technologies
- **Power BI Desktop** — data modeling, DAX, visualization
- **Power Query (M)** — data cleaning and transformation
- **DAX** — measures and calculated columns
- **Excel/CSV** — raw data source

---

## ⚙️ Methods
1. Imported `FastFood_Data_Raw.csv` into Power Query and renamed the table to `FastFood_Data`
2. Standardized all text categories (Title Case, grouped variants) before converting data types
3. Cleaned numeric fields by stripping currency symbols and unit suffixes
4. Converted `Order_Date` to a proper Date type across 5 mixed formats
5. Built DAX measures for revenue, orders, ratings, delivery, and rate calculations (all using `DIVIDE()` for safe division)
6. Added a calculated column `Delivery Category` (Fast / Average / Slow) based on delivery time thresholds
7. Designed 8 KPI cards and 12 charts across 3 report pages
8. Configured 10 slicers, with primary filters (Chain, Date Range) synced across all pages

### Key DAX Measures
DAX
Total Orders      = COUNTROWS(FastFood_Data)
Total Revenue     = SUM(FastFood_Data[Total_Bill_INR])
Avg Bill          = AVERAGE(FastFood_Data[Total_Bill_INR])
Coupon Rate       = DIVIDE(CALCULATE([Total Orders], FastFood_Data[Coupon_Used]="Yes"), [Total Orders], 0) * 100
Repeat Rate       = DIVIDE(CALCULATE([Total Orders], FastFood_Data[Repeat_Visit]="Yes"), [Total Orders], 0) * 100
Avg Delivery      = AVERAGEX(FILTER(FastFood_Data, FastFood_Data[Delivery_Time_Min] <> BLANK()), FastFood_Data[Delivery_Time_Min])
Revenue Per Order = DIVIDE([Total Revenue], [Total Orders], 0)
Veg Share         = DIVIDE(CALCULATE([Total Revenue], FastFood_Data[Veg_NonVeg]="Veg"), [Total Revenue], 0) * 100

Delivery Cat (Calculated Column) =
IF(FastFood_Data[Delivery_Time_Min] <= 30, "Fast",
   IF(FastFood_Data[Delivery_Time_Min] <= 45, "Average", "Slow"))


---

## 💡 Key Insights
- **Fasos leads on revenue** (₹23K) and holds the **highest average customer rating (4.41)** among all 12 chains
- **Home Delivery dominates** order mode at 51.5%, followed by Dine-In (33.5%) and Takeaway (15%)
- Orders are fairly balanced across diet type: **Non-Veg 36.5%, Veg 35.5%, Vegan 28%**
- **Biryani, Sandwich, and Momos** are the top 3 ordered food types
- **McDonald's has the slowest average delivery time (42 min)** despite being a QSR chain, while **Domino's is fastest (26.9 min)**
- Customers who used a **coupon spent more on average (₹88K vs ₹82K)** — suggesting coupons drive higher basket value, not just conversions
- **Lunch orders skew female (62.5%)** while **Late Night and Breakfast skew male (~64–66%)**
- Overall **repeat visit rate stands at 52%** and **coupon usage rate at 54.5%**, both healthy engagement indicators
- **Kolkata, Pune, and Chennai** generate the highest revenue by city

---

## 📊 Dashboard / Model / Output
The report is built across **3 pages**:

**Page 1 — Chain and Revenue Overview**
- 8 KPI cards (Total Orders, Total Revenue, Avg Bill Value, Avg Delivery Time, Avg Customer Rating, Coupon Usage %, Repeat Visit %, Avg Discount %)
- Revenue by Chain (bar), Orders by Food Type (bar), Veg vs Non-Veg Split (pie)
- Slicers: Chain, City, Order Date Range

>  📸![PAGE 1](cp2%20fast%20food%20POWER%20BI_page-0001.jpg)

**Page 2 — Customer and Order Behaviour**
- Order Mode Distribution (donut), Orders by Time Slot (column), Gender Split by Time Slot (100% stacked bar), Coupon Impact on Bill (column)
- Slicers: Order Mode, Veg/Non-Veg, Coupon Used, Gender

> 📸 ![PAGE 2](cp2%20fast%20food%20POWER%20BI_page-0002.jpg)

**Page 3 — Delivery, Rating and Geography**
- Revenue by City (treemap), Average Rating by Chain (column), Average Delivery Time by Chain (bar), Order Trend by Month (line), Delivery Target % gauge
- Slicers: Time Slot, Delivery Category, Payment Mode, Food Item Type

> 📸 ![PAGE3](cp2%20fast%20food%20POWER%20BI_page-0003.jpg)


---

Open IN [POWER BI](<cp2 fast food POWER BI.pbix>)


---

## ✅ Results & Conclusion
This project demonstrates a full analytics workflow — from a messy raw CSV to a polished, decision-ready Power BI dashboard. It highlights practical Power Query cleaning techniques, robust DAX measure design (with null-safe and divide-by-zero handling), and dashboard design principles like slicer syncing and consistent theming. The resulting insights — around chain performance, delivery speed, coupon impact, and customer segments — reflect the kind of analysis a real fast food business would use to guide operational and marketing decisions.

---

## 📬 Connect With Me
- 💼 LinkedIn: [www.linkedin.com/in/samanpreet-kaur-5a8b26375]
- 🌐 Portfolio: [kaursamanpreet970-pixel]
- 📧 Email: [kaursamanpreet970@gmail.com]

---

⭐ If you found this project useful, consider giving it a star!
