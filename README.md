# 🏛️ The Oldest Businesses in the World
<img width="2400" height="2266" alt="00_The-Oldest-Company-in-Every-Country_World-Map_Hi-RES" src="https://github.com/user-attachments/assets/da972879-9fcb-4477-9f5c-eb7f6de4c1a0" />

## 📚 About the Project

This SQL-based data analysis project explores a dataset of the oldest surviving businesses from around the world. By joining and querying structured data from businesses, categories, and countries, we uncover insights about business longevity, historical economic trends, and regional industrial strengths.

---

## 🎯 Objective

To use structured SQL queries to:

* Identify the oldest businesses globally and by region
* Understand which business sectors endure the longest
* Analyze geographical patterns in business longevity
* Investigate industry trends across continents

---

## 🗂️ Dataset Description

Sourced from [BusinessFinancing.co.uk](https://www.businessfinancing.co.uk/), the dataset provides information about the oldest businesses still in operation, including their founding year, industry, and country.

It includes **three relational tables**:

### 🔹 `businesses`

| Column         | Type    | Description                       |
| -------------- | ------- | --------------------------------- |
| business       | varchar | Business name                     |
| year\_founded  | int     | Year of foundation                |
| category\_code | varchar | Foreign key to `categories` table |
| country\_code  | char    | Foreign key to `countries` table  |

### 🔹 `categories`

| Column         | Type    | Description          |
| -------------- | ------- | -------------------- |
| category\_code | varchar | Industry code        |
| category       | varchar | Industry description |

### 🔹 `countries`

| Column        | Type    | Description               |
| ------------- | ------- | ------------------------- |
| country\_code | varchar | ISO 3-letter country code |
| country       | varchar | Country name              |
| continent     | varchar | Continent name            |

---

## 🏗️ Environment Setup

```sql
%%sql
postgresql:///oldestbusinesses
```

Use this to connect your SQL Jupyter extension (e.g., ipython-sql) to the dataset.

---

## 🔍 1. Range of Founding Years

Let’s identify how old the oldest and newest businesses are.

```sql
%%sql
SELECT MIN(year_founded), MAX(year_founded)
FROM businesses;
```

🟢 **Findings**:

* **Oldest Business**: 578 AD
* **Newest Business**: 1999 AD

---

## 🔢 2. Count of Businesses Founded Before 1000 AD

```sql
%%sql
SELECT COUNT(*)
FROM businesses
WHERE year_founded < 1000;
```

🟢 **Insight**: Only **6** businesses have survived for more than a millennium.

---

## 🏛️ 3. Detailed List of Millennia-Old Businesses

```sql
%%sql
SELECT *
FROM businesses
WHERE year_founded < 1000
ORDER BY year_founded;
```

🟢 Notable examples:

* **Kongō Gumi (578, Japan)** – Construction
* **St. Peter Stifts Kulinarium (803, Austria)** – Restaurant
* **Sean’s Bar (900, Ireland)** – Bar

---

## 🔎 4. Business Category Lookup via Join

Linking `businesses` and `categories`:

```sql
%%sql
SELECT bus.business, bus.year_founded, bus.country_code, cat.category
FROM businesses AS bus
INNER JOIN categories AS cat
  ON bus.category_code = cat.category_code
WHERE year_founded < 1000
ORDER BY year_founded;
```

🟢 **Industries include**: Construction, Food & Beverage, Minting, Brewing

---

## 📊 5. Top 10 Business Categories (By Count)

```sql
%%sql
SELECT cat.category, COUNT(cat.category) AS n
FROM businesses AS bus
INNER JOIN categories AS cat
  ON bus.category_code = cat.category_code
GROUP BY cat.category
ORDER BY n DESC
LIMIT 10;
```

🟢 **Top Categories**:

1. **Banking & Finance** – 37
2. **Distillers, Vintners, & Breweries** – 22
3. **Aviation & Transport** – 19

---

## 🌍 6. Oldest Business by Continent

```sql
%%sql
SELECT MIN(bus.year_founded) as oldest, cnt.continent
FROM businesses AS bus
INNER JOIN countries as cnt
  ON bus.country_code = cnt.country_code
GROUP BY continent
ORDER BY oldest;
```

🟢 **Observations**:

* **Asia** (578 AD) and **Europe** (803 AD) lead by centuries.
* Africa and Oceania's oldest date back only to the 1700s–1800s.

---

## 🔗 7. Full Join: Businesses + Categories + Countries

```sql
%%sql
SELECT bus.business, bus.year_founded, cat.category, cnt.country, cnt.continent
FROM businesses AS bus
INNER JOIN categories as cat
  ON bus.category_code = cat.category_code
INNER JOIN countries as cnt
  ON bus.country_code = cnt.country_code;
```

🟢 This comprehensive view (163 rows) enables:

* Geographic filtering
* Industry trends per country or continent
* Visualization possibilities

---

## 🌐 8. Count of Categories by Continent

```sql
%%sql
SELECT cnt.continent, cat.category, COUNT(*) AS n
FROM businesses AS bus
INNER JOIN categories as cat
  ON bus.category_code = cat.category_code
INNER JOIN countries as cnt
  ON bus.country_code = cnt.country_code
GROUP BY cnt.continent, cat.category;
```

🟢 Helps identify continent-specific industry strengths
(e.g., **Postal Services** dominate in Africa, **Breweries** in Europe)

---

## 📉 9. Filtering for Dominant Industry–Continent Pairs

```sql
%%sql
SELECT cnt.continent, cat.category, COUNT(*) AS n
FROM businesses AS bus
INNER JOIN categories as cat
  ON bus.category_code = cat.category_code
INNER JOIN countries as cnt
  ON bus.country_code = cnt.country_code
GROUP BY cnt.continent, cat.category
HAVING COUNT(*) > 5
ORDER BY n DESC;
```

🟢 **Filtered Insight**:

* Highlights combinations with substantial presence
* Reduces noise from single-count entries

---

## 📌 Key Insights

* The **oldest business**: 🏯 **Kongō Gumi** (578 AD, Japan, Construction)
* **Banking & Finance** is the most persistent industry globally
* Europe and Asia dominate in terms of historical business longevity
* Businesses in newer continents (e.g., Oceania) emerged much later

---

## 📈 Future Improvements

* 🔍 Add visualizations (e.g., bar charts, heatmaps, timelines)
* 🗺️ Map oldest businesses by country on a world map
* 📦 Predict longevity based on founding era + sector
* ⚖️ Compare against socio-political timelines and wars

---

## 🧪 Tech Stack

* 🐘 PostgreSQL (via Jupyter `%sql` magic)
* 🐍 Python (Jupyter Notebook)
* 📊 SQL Joins, Filtering, Aggregations
* 📄 Dataset from: [BusinessFinancing.co.uk](https://www.businessfinancing.co.uk/)

# world-oldest-business
