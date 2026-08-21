# 🐼 FoodPanda Sales & Customer Analytics Dashboard

An interactive **Power BI** dashboard built on foodpanda's order, user, and restaurant data — delivering a unified view of sales performance, customer acquisition vs. churn, and city-level operations, from a company-wide summary all the way down to a single city's scorecard.

The report merges order, user, restaurant, and menu-level data into a single analytical model, letting a viewer move from top-line KPIs (orders, revenue, ratings) down to gender-and-age-level customer behavior and city-by-city performance — all through one guided, navigable interface.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Measures-D00362?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

---

 ## 🚦 Live Dashboard
 
<p align="center">
  <a href="https://app.powerbi.com/view?r=eyJrIjoiYzcwZTgwYTEtYTE5Yi00Mzc5LWEwOWUtNDA5YzQ1MTVjYWNmIiwidCI6IjNjZDA3OTg4LTUyNjMtNDA2NC1hZDU1LWU5NTZhYjNkZDExNyIsImMiOjEwfQ%3D%3D">
    <img src="https://img.shields.io/badge/🚀_Launch_Live_Dashboard-F2C811?style=for-the-badge&logo=powerbi&logoColor=black" alt="Launch Live Dashboard" />
  </a>
</p>
<p align="center">
  🖱️ <b>Click above</b> &nbsp;→&nbsp; 🌐 <b>Opens in browser</b> &nbsp;→&nbsp; 🚫 <b>No install needed</b> &nbsp;→&nbsp; 🖱️ <b>Fully interactive</b>
</p>

---


## 📊 Preview

| Home | Overview | User Performance | City Performance |
|:---:|:---:|:---:|:---:|
| ![Home](Home.jpg) | ![Overview](Overview.jpg) | ![User Performance](User_Performance.jpg) | ![City Performance](City_Perfomance.jpg) |

---

## 🧭 Report Pages

### 1️⃣ Home
A fully branded landing page ("**meal for one — no min. spend**") with a single **Dashboard** navigation button that routes straight into the Overview page — a clean, app-like entry point rather than a data-heavy first screen.

### 2️⃣ Overview
The executive summary page.
- **KPI cards** — Amount (₹987M), Quantity (2M), Ratings collected (240K), Orders (150K)
- **Category split** — Non-veg, Veg, and Other order volume shown *with* their individual rating scores side by side (830K orders / 79.6K rating, 798K / 80.1K, 816K / 79.9K), so volume and satisfaction are read together
- **All City Quantity** ranked bar chart with **Top 5 / 10 / 20 / 50 / 100 / All** quick-filter buttons — Dhaka leads at 383K, more than 2.9× the next city (Pirojpur, 132K)
- **Sale by Year** trend line (2017–2020) tracking order value over time
- A global **Amount / Quantity** toggle re-labels the KPI cards and charts without duplicating visuals

### 3️⃣ User Performance
The customer-health page.
- **KPI cards** — Active Users (78K), Total Users (78K), Ratings (240K), Orders (150K)
- **Gain Users** — 12K total (Male 6.6K, Female 5.1K)
- **Lost Users** — 33K total (Male 18.9K, Female 14.1K)
- **Users by Age** column chart showing the age distribution of the user base, peaking in the mid-20s bracket
- Same **Amount / Quantity** toggle and city/type slicers carried over from the Overview page for consistent filtering

### 4️⃣ City Performance
The operational drill-down page.
- **KPI cards** — Active Users (78K), Users (78K), Ratings (240K), Orders (150K)
- **City scorecard table** — City × Sales × Orders × Gain Users × Lost Users, sortable, with a grand-total row (₹986.5M sales / 150,281 orders / 11,635 gained / 32,798 lost across the listed cities)
- **Sales by City, Rating by City, Users by City** — three parallel bar charts, so a city that ranks high on one metric (e.g. sales) can be checked against how it ranks on the others (ratings, user base)
- Dropdown + text-search city filters for one-click drill-down

---

## 🗃️ Data Sources

Per the dashboard's own summary banner: **foodpanda is operating across 87 cities, serving 100,000 unique customers through 150,281 total orders.**

| Source Table | Role |
|---|---|
| `orders` | Fact table — transactions (date, amount, quantity, city, user, status) |
| `orders_Type` | Order category breakdown (Veg / Non-Veg / Other) |
| `users` | Customer dimension — gender, age, active/gained/lost status |
| `restaurant` | Restaurant dimension — city, ratings |
| `menu` | Menu / item-level dimension |
| `food` | Food category dimension |

Source files live in the **`Data/`** folder and are shaped independently in Power Query before being loaded into the model.

---

## 🧱 Data Model

```
orders (Fact)                       users (Dimension)
├─ order_date                       ├─ user_id
├─ Amount                           ├─ Gender
├─ Quantity                         ├─ Age
├─ city  ───────────────────────►   ├─ city
├─ Type  ──► orders_Type            ├─ Active_User
└─ user_id ─────────────────────►   ├─ Gain Users
                                     └─ LostCustomers

restaurant (Dimension)               menu / food (Dimension)
├─ city                              ├─ item / category
└─ Ratings                           └─ (Veg / Non-Veg / Other)
```

- **Relationships** connect `orders` to `users`, `restaurant`, and `orders_Type` on their respective keys, forming a star schema.
- **Supporting calculation tables** — `_RankTable`, `All_Measure_Table`, and `_All_Calcuclation` — hold the DAX measures that drive the Top-N ranking logic and dynamic, filter-aware chart titles (e.g. the chart re-titling itself between "City By Sales" and "City By Units").

---

## 🧮 Key DAX Measures

| Measure | Purpose |
|---|---|
| `Amount` / `Quantity` / `orders` | Core additive measures over the `orders` fact table |
| `Dynamic Title` / `Dynamic_SubHeading` | Re-labels chart titles and headers based on the active Amount/Quantity toggle |
| `TopN_Sale` / `TopN_Sale_for_data_label` | Powers the Top 5/10/20/50/100 city-ranking filter on the Overview page |
| `Active_User` / `user_count` | Tracks the current active/total user base |
| `Gain Users` / `LostCustomers` | Customer acquisition and churn counts, sliceable by gender and city |
| `Ratings` | Aggregated rating count/score used across all four pages |
| `Veg_Ratings` / `Non-Veg_Ratings` / `Other_Ratings` | Category-level rating scores shown alongside category order volume |

*(Full formulas are viewable in Power BI Desktop under Model view → Fields pane, inside `All_Measure_Table`.)*

---

## 🛠️ Tech Stack

- **Power BI Desktop** — report authoring, data modeling, DAX
- **Power Query (M)** — data cleaning and shaping for `orders`, `users`, `restaurant`, `menu`, and `food`
- **Custom Visuals** — Text Search Slicer, Advanced Slicer (AppSource)
- **Bookmarks & Page Navigation buttons** — for guided navigation (Home → Overview → User Performance → City Performance)
- **DAX** — dynamic titles, Top-N ranking, gain/loss and category-level KPIs

---

## 📁 Repository Structure

```
FoodPanda-Dashboard/
├── FoodPanda Dashboard.pbix     # Main Power BI report (data model + visuals)
├── Data/                        # Source datasets (orders, users, menu, restaurant)
├── Dashboard_Img/               # Static screenshots of each report page
├── img/                         # Icons & background images used inside visuals
├── Dashboard_Video/             # Screen-recorded walkthrough / demo of the dashboard
├── Home.jpg
├── Overview.jpg
├── User_Performance.jpg
├── City_Perfomance.jpg
└── README.md
```

---

## 🚀 How to Use

1. Clone or download this repository.
2. Open **`FoodPanda Dashboard.pbix`** in Power BI Desktop.
3. If prompted, point the data source to the files inside the **`Data/`** folder and refresh.
4. Navigate using the on-screen buttons, starting from the **Home** page.
5. Use the **Amount / Quantity** toggle and the **City / Type** slicers on any page to drill into a specific segment.

---

## 📌 Key Insights

- **Churn is outpacing acquisition by nearly 3×** — 11,635 users gained vs. 32,798 lost across the tracked cities, a net loss of roughly 21,000 customers that the Gain/Lost breakdown makes immediately visible.
- **Dhaka dominates order volume** — 383K in quantity, more than 2.9× the second-ranked city (Pirojpur, 132K) — but the highest-*sales* city (Feni, ₹55.9M) and highest-*order-count* city (Narsingdi, 4,463 orders) are different cities entirely, showing that volume, order count, and revenue leadership don't line up in one place.
- **Veg orders convert to the best ratings** — Veg has the lowest order volume of the three categories (798K) but the highest rating score (80.1K), while Non-veg has the highest volume (830K) but a slightly lower rating (79.6K) — a volume/satisfaction trade-off worth investigating.
- **Sales peaked in 2018 and have been declining since** — order value rose from 0.2M (2017) to 1.0M (2018), then fell to 0.9M (2019) and 0.4M (2020), a trend the year-over-year line chart flags early rather than only in a year-end review.
- **Churn skews slightly male** — of the 33K lost users, 18.9K (57%) are male vs. 14.1K (43%) female, a similar split to Gained users (55% male / 45% female), suggesting the churn issue is broad-based rather than concentrated in one gender segment.

---

## 👤 About the Author

**Md Idris (Shawon)**
Junior Data Analyst — Power BI • Excel • SQL • Python

- 🌐 Portfolio: [shawon-analyst-portfolio.vercel.app](https://shawon-analyst-portfolio.vercel.app)
- 💻 GitHub: [@shawon-analyst](https://github.com/shawon-analyst)
- 💼 LinkedIn: [shawonanalyst](https://linkedin.com/in/shawonanalyst)
- 📧 Email: idris.shawon@gmail.com

⭐ If you found this project useful, consider giving it a star on GitHub!
