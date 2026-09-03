# Car MSRP — Exploratory Data Analysis Report

**Dataset size:** 11,914 cars
**Target variable:** MSRP (Manufacturer's Suggested Retail Price)

This report is organized in the order a typical EDA workflow follows: understanding categorical structure → understanding the target variable → numerical relationships → categorical-numerical relationships → categorical-categorical relationships → combined/multivariate patterns → final takeaways.

---

## 1. Categorical Proportion Analysis

**Goal:** Understand how each categorical column is distributed before checking its relationship with MSRP.

### 1.1 Transmission Type
| Category | Share |
|---|---|
| Automatic | **69.40%** |
| Manual | **24.64%** |
| Automated-Manual | **5.26%** |
| Direct Drive | **0.57%** |
| Unknown | **0.16%** |

### 1.2 Driven Wheels
| Category | Share |
|---|---|
| Front-wheel drive | **40.19%** |
| Rear-wheel drive | **28.30%** |
| All-wheel drive | **19.75%** |
| Four-wheel drive | **11.78%** |

### 1.3 Vehicle Size
| Category | Share |
|---|---|
| Compact | **40.00%** |
| Midsize | **36.71%** |
| Large | **23.31%** |

### 1.4 Vehicle Style
- Sedan — **25.59%** (largest)
- 4dr SUV — **20.89%**
- Coupe — **10.17%**
- Convertible — **6.66%**
- 4dr Hatchback — **5.89%**
- Crew Cab Pickup — **5.72%**
- Remaining 10 styles — each under **5%**

### 1.5 Engine Fuel Type
- Regular unleaded — **60.24%** (dominant)
- Premium unleaded (required) — **16.87%**
- Premium unleaded (recommended) — **12.79%**
- Flex-fuel (unleaded/E85) — **7.55%**
- Diesel, electric, and remaining flex-fuel/natural-gas variants — combined **< 2.6%**

### 1.6 Engine Cylinders
- 4-cylinder — **40.14%**
- 6-cylinder — **37.68%**
- 8-cylinder — **17.05%**
- 12, 5, 10, 0 (electric), 3, and 16-cylinder combined — **5.14%**

### 1.7 Number of Doors
- 4-door — **70.18%**
- 2-door — **26.53%**
- 3-door — **3.32%**

### 1.8 Make (Brand)
- Chevrolet — **9.43%** (most frequent)
- Ford — **7.40%**
- Volkswagen — **6.79%**
- Toyota — **6.26%**
- Dodge — **5.26%**
- Total brands in dataset: **48**

### 1.9 Year
- 2015 — **18.22%**
- 2016 — **18.11%**
- 2017 — **14.00%**
- These three years together ≈ **50%** of all listings → dataset skews toward recent model years

### 1.10 Market Category
- **31.41%** of rows have no recorded value (filled as "Unknown")
- Among recorded values, **72 unique tag combinations** exist (e.g., "Luxury,Performance", "Crossover") → high cardinality field

### 📌 Section Findings
**Strongest imbalances:**
1. Engine Fuel Type — **60.24%** Regular unleaded; electric/natural gas/rare flex-fuel types each under **1%**
2. Number of Doors — **70.18%** 4-door
3. Transmission Type — **69.40%** Automatic
4. Vehicle Style — **25.59%** Sedan, but spread thin across 16 categories (10 of them under 5% each)

**Smaller categories to interpret cautiously (low sample size):**
- Direct Drive transmission — **0.57%**
- Natural gas fuel type — **0.02%**
- 16-cylinder engines — **0.03%**
- Convertible SUV body style — **0.24%**

> These small categories' average MSRP figures may be unreliable due to tiny sample sizes.

---

## 2. MSRP — Target Variable Analysis

**Goal:** Understand the distribution shape of the value being predicted.

| Metric | Value |
|---|---|
| Mean MSRP | **$40,594.74** |
| Median MSRP | **$29,995.00** |
| Minimum MSRP | **$2,000** |
| Maximum MSRP | **$2,065,902** |
| Skewness | **11.77** |
| Missing values | **0%** |
| Duplicate rows | **715 (~6% of dataset)** |

### 📌 Section Findings
- The large gap between **mean ($40.6K)** and **median ($30K)** signals a **right-skewed distribution**.
- A skewness of **11.77** confirms this is a **strong right skew**, driven by a small number of ultra-luxury outliers.
- MSRP has **zero missing values** — fully usable as a target.
- Because of the skew, a **log transformation** was applied before further numerical analysis.

---

## 3. Numerical Relationship Analysis (Correlation)

**Goal:** Measure linear relationships between numerical variables and MSRP.

| Relationship | Correlation | Strength |
|---|---|---|
| Engine HP ↔ MSRP | **+0.66** | Strong positive |
| Engine Cylinders ↔ MSRP | **+0.53** | Moderate positive |
| Year ↔ MSRP | **+0.23** | Weak positive |
| Popularity ↔ MSRP | **-0.05** | ~No relationship |
| Highway MPG ↔ MSRP | **-0.16** | Weak negative |
| City MPG ↔ MSRP | **-0.16** | Weak negative |
| Number of Doors ↔ MSRP | **-0.13** | Weak negative |
| City MPG ↔ Highway MPG | **+0.89** | Very strong positive |
| Engine Cylinders ↔ City MPG | **-0.57** | Moderate negative |
| Engine HP ↔ Engine Cylinders | **+0.77** | Strong positive |

### 📌 Section Findings
- **Engine HP** and **Engine Cylinders** are the strongest numerical predictors of MSRP.
- **Popularity** shows almost no linear relationship with price.
- **City MPG** and **Highway MPG** are near-duplicates (**+0.89**) — a **multicollinearity risk** to account for in modeling.

---

## 4. Categorical vs Numerical Relationship Analysis

**Goal:** Compare average MSRP/other numerical metrics across categorical groups.

### 4.1 Vehicle Size vs MSRP
- Large — **$53,890.50** (highest)
- Midsize — **$39,035.92**
- Compact — **$34,275.34**

### 4.2 Transmission Type vs MSRP
- Automated-Manual — **$99,508.37** (highest)
- Direct Drive — **$47,351.25**
- Automatic — **$41,110.33**
- Manual — **$26,663.64** (lowest)

### 4.3 Driven Wheels vs Engine HP
- Rear-wheel drive — **302.87 HP** (highest average)
- Front-wheel drive — **184.06 HP** (lowest average)

### 4.4 Engine Fuel Type vs City MPG
- Electric — **112.70** (highest)
- Flex-fuel (premium unleaded required/E85) — **13.30** (lowest)

### 4.5 Make vs MSRP Range
- Bugatti — widest range: **$1,500,000 – $2,065,902**
- Buick — narrow, low range: **$2,000 – $49,625**

### 📌 Section Findings
**Transmission Type** shows the most striking pattern: Automated-Manual vehicles average **$99,508**, which is:
- ~**2.4x** the Automatic average
- ~**3.7x** the Manual average

---

## 5. Categorical vs Categorical Relationship Analysis

**Goal:** Use crosstabs and chi-square testing to check independence between categorical variables.

### 5.1 Vehicle Size vs Transmission Type
| Vehicle Size | Automatic % |
|---|---|
| Compact | **44.6%** (near-even split with Manual at 45.6%) |
| Midsize | **84.1%** |
| Large | **88.7%** |

### 5.2 Driven Wheels vs Vehicle Style
- Front-wheel drive → strongly tied to **Sedans** (1,783 of 4,787 FWD cars) and **Hatchbacks**
- Rear-wheel drive → strongly tied to **Coupes** (676 cars) and **pickup trucks**
- Four-wheel drive → concentrated almost entirely in **pickups and SUVs**

### 5.3 Engine Fuel Type vs Driven Wheels (Chi-Square Test)
- **Chi-square statistic:** 2,831.12
- **p-value:** 0.0000

### 📌 Section Findings
- Since p-value **< 0.05**, there is a **statistically significant association** between Engine Fuel Type and Driven Wheels — the two are **not independent**.
- **Vehicle Size and Transmission Type** show a clear pattern: smaller vehicles skew Manual, larger vehicles are overwhelmingly Automatic.

---

## 6. Combination (Multivariate) Analysis

**Goal:** Identify which combinations of Make, Vehicle Size, and Transmission Type relate to the highest average MSRP.

- Multi-key groupings — **(Make + Vehicle Size + Transmission Type)** and **(Engine Cylinders + Driven Wheels)** — confirm that price is driven by an **interaction of factors**, not any single variable alone.
- **Luxury/exotic Makes + Automated-Manual transmission** → consistently the highest group averages.
- **Economy Makes + Manual transmission** → consistently the lowest group averages.

> ⚠️ **Caution:** Some Make–Size–Transmission combinations contain fewer than 10 cars. These figures should be read alongside their sample size, not compared directly to broader category averages.

---

## 7. Overall EDA Insights (Executive Summary)

| Insight | Detail |
|---|---|
| Strongest numerical predictor | **Engine HP** (+0.66), then **Engine Cylinders** (+0.53) |
| Weakest numerical predictor | **Popularity** (-0.05) |
| Strongest categorical pattern | **Transmission Type** — Automated-Manual ($99,508) vs Manual ($26,664), a gap of over **$72,000** |
| Statistically significant association | **Engine Fuel Type ↔ Driven Wheels** (chi-square p = 0.0000) |
| Target variable shape | MSRP is heavily **right-skewed** (skewness = 11.77), driven by ultra-luxury outliers like Bugatti → requires log transformation |
| Data quality issue | **715 duplicate rows (~6%)** should be removed before modeling |
| Multicollinearity risk | **City MPG vs Highway MPG** (+0.89) — consider dropping/combining one |

### Next Steps for Modeling
1. Remove the 715 duplicate rows.
2. Apply a log transformation to MSRP to reduce skew.
3. Address multicollinearity between City MPG and Highway MPG.
4. Treat associations as **correlational, not causal**.
5. Use feature engineering informed by the strongest signals: Engine HP, Engine Cylinders, Transmission Type, Vehicle Size.
