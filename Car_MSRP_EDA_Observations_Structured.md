## Categorical Proportion Analysis

The categorical proportion analysis was performed to understand the distribution of different categories within the dataset (11,914 cars) before analyzing their relationship with MSRP.

### Key Observations

* **Transmission Type:** Automatic is the dominant category at **69.40%**, followed by Manual at **24.64%**, Automated-Manual at **5.26%**, Direct Drive at **0.57%**, and Unknown at **0.16%**.

* **Driven_Wheels:** Front-wheel drive is the largest category at **40.19%**, followed by Rear-wheel drive (**28.30%**), All-wheel drive (**19.75%**), and Four-wheel drive (**11.78%**).

* **Vehicle Size:** Compact represents **40.00%**, Midsize **36.71%**, and Large **23.31%**.

* **Vehicle Style:** Sedan is the largest category at **25.59%**, followed by 4dr SUV (**20.89%**), Coupe (**10.17%**), Convertible (**6.66%**), 4dr Hatchback (**5.89%**), and Crew Cab Pickup (**5.72%**); the remaining 10 styles each represent under **5%**.

* **Engine Fuel Type:** Regular unleaded dominates at **60.24%**, followed by premium unleaded (required) at **16.87%** and premium unleaded (recommended) at **12.79%**; flex-fuel (unleaded/E85) represents **7.55%**, while diesel, electric, and the remaining flex-fuel/natural-gas variants together make up less than **2.6%**.

* **Engine Cylinders:** 4-cylinder engines are most common at **40.14%**, followed by 6-cylinder (**37.68%**) and 8-cylinder (**17.05%**); 12, 5, 10, 0 (electric), 3, and 16-cylinder configurations together account for the remaining **5.14%**.

* **Number of Doors:** 4-door vehicles represent **70.18%**, 2-door **26.53%**, and 3-door **3.32%**.

* **Make:** Chevrolet is the most frequent brand at **9.43%**, followed by Ford (**7.40%**), Volkswagen (**6.79%**), Toyota (**6.26%**), and Dodge (**5.26%**); the dataset spans 48 brands in total.

* **Year:** 2015, 2016, and 2017 together account for roughly **50%** of all listings (**18.22%**, **18.11%**, and **14.00%** respectively), indicating the dataset is weighted heavily toward recent model years.

* **Market Category:** **31.41%** of rows have no recorded value (filled as "Unknown"); among recorded values, the field spans **72** unique tag combinations (e.g., "Luxury,Performance," "Crossover"), reflecting high cardinality.

### Important Findings

The dataset contains both moderately balanced and strongly skewed categorical variables.

The strongest category imbalances are observed in:

1. **Engine Fuel Type:** **60.24% Regular unleaded**, with electric, natural gas, and rarer flex-fuel types each under **1%**
2. **Number of Doors:** **70.18% 4-door**
3. **Transmission Type:** **69.40% Automatic**
4. **Vehicle Style:** **25.59% Sedan** but spread thin across 16 categories, ten of which fall under **5%** each

Other smaller categories worth noting:

- **Direct Drive transmission:** **0.57%**
- **Natural gas fuel type:** **0.02%**
- **16-cylinder engines:** **0.03%**
- **Convertible SUV body style:** **0.24%**

These smaller categories should be interpreted carefully, since their average MSRP figures may be influenced by very small sample sizes.

---

## MSRP-Focused Findings (Target Variable)

The numerical distribution of MSRP was examined directly, since it is the variable being predicted.

* **Mean MSRP:** **$40,594.74**, compared with a **Median MSRP** of **$29,995.00** — a wide gap indicating a right-skewed distribution.

* **Minimum MSRP:** **$2,000**; **Maximum MSRP:** **$2,065,902**.

* **Skewness:** **11.77**, confirming a strong right skew driven by a small number of ultra-luxury outliers.

* **Missing values:** MSRP itself has **0%** missing values — the target variable is fully populated.

* **Duplicate rows:** **715** duplicate rows were identified in the dataset (**~6%** of all rows).

### Important Findings

The MSRP distribution is dominated by a long tail of extremely high-priced vehicles rather than a symmetric spread, which is why a **log transformation** was applied before further numerical analysis.

---

## Numerical Relationship Analysis

Correlation analysis was performed to examine linear relationships between numerical variables and MSRP.

* **Engine HP vs MSRP:** **+0.66**, indicating a **strong positive relationship** — higher horsepower is associated with higher price.

* **Engine Cylinders vs MSRP:** **+0.53**, a **moderate positive relationship**.

* **Year vs MSRP:** **+0.23**, a **weak positive relationship** — newer cars are only slightly more expensive on average.

* **Popularity vs MSRP:** **-0.05**, indicating **almost no linear relationship**.

* **highway MPG vs MSRP:** **-0.16**; **city mpg vs MSRP:** **-0.16** — both show a **weak negative relationship**, meaning higher-priced cars tend to be slightly less fuel-efficient.

* **Number of Doors vs MSRP:** **-0.13**, a **weak negative relationship**.

* **city mpg vs highway MPG:** **+0.89**, a **very strong positive relationship** — the two fuel-economy measures move almost interchangeably.

* **Engine Cylinders vs city mpg:** **-0.57**, a **moderate negative relationship** — more cylinders is associated with lower fuel efficiency.

* **Engine HP vs Engine Cylinders:** **+0.77**, a **strong positive relationship**, as expected mechanically.

### Important Findings

**Engine HP** and **Engine Cylinders** show the strongest linear relationships with MSRP among all numerical variables, while **Popularity** shows almost no relationship at all. **city mpg** and **highway MPG** are near-duplicates of each other (**+0.89**), which is worth accounting for to avoid redundancy in later modeling.

---

## Categorical vs Numerical Relationship Analysis

Average MSRP and related numerical metrics were compared across categorical groups.

* **Vehicle Size vs MSRP:** Large vehicles have the highest average MSRP at **$53,890.50**, followed by Midsize at **$39,035.92** and Compact at **$34,275.34**.

* **Transmission Type vs MSRP:** Automated-Manual has the highest average MSRP at **$99,508.37**, followed by Direct Drive (**$47,351.25**) and Automatic (**$41,110.33**); Manual has the lowest at **$26,663.64**.

* **Driven_Wheels vs Engine HP:** Rear-wheel drive vehicles have the highest average Engine HP at **302.87 HP**, while front-wheel drive vehicles have the lowest at **184.06 HP**.

* **Engine Fuel Type vs city mpg:** Electric vehicles have the highest average city mpg at **112.70**, while flex-fuel (premium unleaded required/E85) has the lowest at **13.30**.

* **Make vs MSRP range:** Bugatti has the widest MSRP range, from **$1,500,000 to $2,065,902**; Buick shows a much narrower and lower range, from **$2,000 to $49,625**.

### Important Findings

**Transmission Type** shows a particularly striking pattern — Automated-Manual transmissions are associated with dramatically higher average MSRP (**$99,508**) than any other type, nearly **2.4x** the Automatic average and roughly **3.7x** the Manual average.

---

## Categorical vs Categorical Relationship Analysis

Crosstabs and a chi-square test were used to examine relationships between categorical variables.

* **Vehicle Size vs Transmission Type:** Within Compact vehicles, Automatic (**44.6%**) and Manual (**45.6%**) are nearly evenly split; within Large vehicles, Automatic dominates at **88.7%**; within Midsize vehicles, Automatic represents **84.1%**.

* **Driven_Wheels vs Vehicle Style:** Front-wheel drive is strongly associated with Sedans (**1,783** of **4,787** front-wheel-drive cars) and Hatchbacks; Rear-wheel drive is strongly associated with Coupes (**676** cars) and pickup trucks; Four-wheel drive is concentrated almost entirely in pickups and SUVs.

* **Engine Fuel Type vs Driven_Wheels (Chi-Square Test):** **Chi-square statistic = 2,831.12**, **p-value = 0.0000**.

### Important Findings

Since the p-value is far below **0.05**, the analysis indicates a **statistically significant association between Engine Fuel Type and Driven_Wheels** — the two variables are not independent.

**Vehicle Size and Transmission Type** also show a clear categorical relationship: smaller vehicles are far more likely to be Manual, while larger vehicles are overwhelmingly Automatic.

---

## Combination Analysis

A multi-key grouping was performed to identify which combinations of Make, Vehicle Size, and Transmission Type are associated with the highest average MSRP.

Multi-key groupings (**Make + Vehicle Size + Transmission Type**, and **Engine Cylinders + Driven_Wheels**) confirmed that price is driven by the **interaction** of brand, size, and transmission rather than any single factor alone — luxury/exotic Makes combined with Automated-Manual transmissions consistently produced the highest group averages, while economy Makes with Manual transmissions produced the lowest.

Because combination-based groups can contain very few observations (in some cases fewer than 10 cars per Make-Size-Transmission combination), these figures should be interpreted alongside their sample sizes rather than compared directly to broader category averages.

---

## Overall EDA Insights

The exploratory analysis identified several vehicle and specification characteristics associated with MSRP.

The strongest numerical relationship with price was observed for **Engine HP** (**+0.66**), followed by **Engine Cylinders** (**+0.53**), while **Popularity** (**-0.05**) showed almost no relationship.

The strongest categorical pattern was observed for **Transmission Type**, where Automated-Manual vehicles averaged **$99,508** compared with **$26,664** for Manual vehicles — a gap of over **$72,000**.

The chi-square test confirmed a statistically significant association between **Engine Fuel Type and Driven_Wheels** (**p = 0.0000**).

MSRP itself is heavily right-skewed (**skewness = 11.77**), driven by a small number of ultra-luxury outliers such as Bugatti, which required a log transformation for further analysis.

Overall, these findings provide useful direction for **feature engineering, multicollinearity checks (city mpg vs highway MPG), and MSRP prediction modeling**. However, the observed relationships represent **associations rather than causal relationships**, and the **715 duplicate rows** identified earlier should be removed before any modeling proceeds.
