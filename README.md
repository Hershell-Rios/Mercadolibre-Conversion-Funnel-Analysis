# 🚦 Analysis of Urban Mobility in Latin America

## 📊 Project Description
Data analysis to assess the relationship between urban mobility and economic productivity in major Latin American cities for the Latin American Development Bank (LADB).

## 🎯 Objectives
- Assess the relationship between traffic congestion and economic productivity
- Identify priority cities for infrastructure investment
- Analyze mobility patterns in 15 cities across 7 countries

## 🛠️ Technologies Used
- **Python**: pandas, numpy, seaborn, matplotlib
- **Jupyter Notebook**
- **Data sources**: TomTom Traffic Index, OECD Cities Database

## 📈 Key Findings
- **15 cities** analyzed across **7 Latin American countries**
- **Mexico City** has the highest congestion (47 additional minutes)
- **There is no direct correlation** between GDP per capita and congestion
- **Bogotá** identified as a priority city for investment

## 📁 Project Structure
```
├── mercadolibre_funnel_analysis_2025.xlsx  # Análisis completo
├── data/
│   ├── embudo_general.csv                  # Datos de conversión
│   ├── retencion_por_pais.csv             # Retención por país
│   └── retencion_por_cohort.csv           # Retención por cohorte
└── README.md

1st Phase:
# 🛒 Conversion Funnel Analysis - MercadoLibre

## 📊 Description
SQL analysis of the MercadoLibre conversion funnel using CTEs to identify user drop-off points.

## 🎯 Objective
Analyze user behavior throughout the purchase funnel, from the first visit to the final purchase.

## 📈 Funnel Stages Analyzed
1. **First Visit** - First visit to the site
2. **Select Item** - Product selection
3. **Add to Cart** - Add to cart
4. **Begin Checkout** - Start checkout
5. **Add Shipping Info** - Add shipping information
6. **Add Payment Info** - Add payment information
7. **Purchase** - Purchase completed

### 📊 Query
WITH first_visit AS (
    SELECT DISTINCT user_id
    FROM mercadolibre_funnel
    WHERE event_name = 'first_visit'
    AND event_date BETWEEN '2025-01-01'AND '2025-08-31'),
select_item AS (
    SELECT DISTINCT user_id
    FROM mercadolibre_funnel
    WHERE event_name IN ('select_item','select_promotion')
    AND event_date BETWEEN '2025-01-01'AND '2025-08-31'),
add_to_cart AS (
    SELECT DISTINCT user_id
    FROM mercadolibre_funnel
    WHERE event_name = 'add_to_cart'
    AND event_date BETWEEN '2025-01-01'AND '2025-08-31'),
begin_checkout AS (
    SELECT DISTINCT user_id
    FROM mercadolibre_funnel
    WHERE event_name = 'begin_checkout'
    AND event_date BETWEEN '2025-01-01'AND '2025-08-31'),
add_shipping_info AS (
    SELECT DISTINCT user_id
    FROM mercadolibre_funnel
    WHERE event_name = 'add_shipping_info'
    AND event_date BETWEEN '2025-01-01'AND '2025-08-31'),
add_payment_info AS (
    SELECT DISTINCT user_id
    FROM mercadolibre_funnel
    WHERE event_name = 'add_payment_info'
    AND event_date BETWEEN '2025-01-01'AND '2025-08-31'),
purchase AS (
    SELECT DISTINCT user_id
    FROM mercadolibre_funnel
    WHERE event_name = 'purchase'
    AND event_date BETWEEN '2025-01-01'AND '2025-08-31')
-- 2) Une las CTEs anclando en signup y cuenta usuarios por etapa
SELECT 
COUNT (fv.user_id) AS usuarios_first_visit,
COUNT (si.user_id) AS usuarios_select_item,
COUNT (a.user_id) AS usuarios_add_to_cart,
COUNT (bc.user_id) AS usuarios_begin_checkout,
COUNT (asi.user_id) AS usuarios_add_shipping_info,
COUNT (api.user_id) AS usuarios_add_payment_info,
COUNT (p.user_id) AS usuarios_purchase

FROM first_visit AS fv
LEFT JOIN select_item AS si ON fv.user_id = si.user_id
LEFT JOIN add_to_cart AS a ON fv.user_id = a.user_id
LEFT JOIN begin_checkout AS bc ON fv.user_id = bc.user_id
LEFT JOIN add_shipping_info AS asi ON fv.user_id = asi.user_id
LEFT JOIN add_payment_info AS api ON fv.user_id = api.user_id
LEFT JOIN purchase AS p ON fv.user_id = p.user_id

2nd Phase:
# 🛒 Conversion Funnel Analysis - MercadoLibre

## 📊 Description
SQL analysis of the MercadoLibre conversion funnel, calculating conversion rates from the first visit to the final purchase.

## 🎯 Objective
Identify conversion rates at each stage of the funnel to detect user drop-off points.

## 📈 Stages Analyzed
1. **First Visit** → **Select Item** 
2. **First Visit** → **Add to Cart**
3. **First Visit** → **Begin Checkout**
4. **First Visit** → **Add Shipping Info**
5. **First Visit** → **Add Payment Info**
6. **First Visit** → **Purchase**

## 🛠️ Technologies
- **SQL** with CTEs (Common Table Expressions)
- Data analysis: January–August 2025
- Conversion calculations using ROUND and NULLIF

### 📊 Query
WITH first_visit AS (
  SELECT DISTINCT user_id
  FROM mercadolibre_funnel
  WHERE event_name = 'first_visit'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'
),
select_item AS (
  SELECT DISTINCT user_id
  FROM mercadolibre_funnel
  WHERE event_name IN ('select_item', 'select_promotion')
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'
),
add_to_cart AS (
  SELECT DISTINCT user_id
  FROM mercadolibre_funnel
  WHERE event_name = 'add_to_cart'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'
),
begin_checkout AS (
  SELECT DISTINCT user_id
  FROM mercadolibre_funnel
  WHERE event_name = 'begin_checkout'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'
),
add_shipping_info AS (
  SELECT DISTINCT user_id
  FROM mercadolibre_funnel
  WHERE event_name = 'add_shipping_info'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'
),
add_payment_info AS (
  SELECT DISTINCT user_id
  FROM mercadolibre_funnel
  WHERE event_name = 'add_payment_info'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'
),
purchase AS (
  SELECT DISTINCT user_id
  FROM mercadolibre_funnel
  WHERE event_name = 'purchase'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'
), 
funnel_counts AS(
SELECT
  COUNT(fv.user_id) AS usuarios_first_visit,
  COUNT(si.user_id) AS usuarios_select_item,
  COUNT(a.user_id) AS usuarios_add_to_cart,
  COUNT(bc.user_id) AS usuarios_begin_checkout,
  COUNT(asi.user_id) AS usuarios_add_shipping_info,
  COUNT(api.user_id) AS usuarios_add_payment_info,
  COUNT(p.user_id) AS usuarios_purchase
FROM first_visit fv
LEFT JOIN select_item si        ON fv.user_id = si.user_id
LEFT JOIN add_to_cart a         ON fv.user_id = a.user_id
LEFT JOIN begin_checkout bc     ON fv.user_id = bc.user_id
LEFT JOIN add_shipping_info asi ON fv.user_id = asi.user_id
LEFT JOIN add_payment_info api  ON fv.user_id = api.user_id
LEFT JOIN purchase p            ON fv.user_id = p.user_id
)

    SELECT 
    ROUND(usuarios_select_item*100.0/NULLIF(usuarios_first_visit,0),2) AS conversion_select_item,
    ROUND(usuarios_add_to_cart*100.0/NULLIF(usuarios_first_visit,0),2) AS conversion_add_to_cart,
    ROUND(usuarios_begin_checkout*100.0/NULLIF(usuarios_first_visit,0),2) AS conversion_begin_checkout,
    ROUND(usuarios_add_shipping_info*100.0/NULLIF(usuarios_first_visit,0),2) AS conversion_add_shipping_info,
    ROUND(usuarios_add_payment_info*100.0/NULLIF(usuarios_first_visit,0),2) AS conversion_add_payment_info,
    ROUND(usuarios_purchase*100.0/NULLIF(usuarios_first_visit,0),2) AS conversion_purchase
    FROM funnel_counts;

3rd Phase:
# 🌎 Conversion Funnel Analysis by Country - MercadoLibre

## 📊 Description
SQL analysis of the MercadoLibre conversion funnel segmented by country, calculating conversion rates from the first visit to the final purchase.

## 🎯 Objective
Identify differences in conversion rates between countries to detect market-specific optimization opportunities.

## 📈 Funnel Stages Analyzed
1. **First Visit** - First visit to the site
2. **Select Item** - Product selection  
3. **Add to Cart** - Add to cart
4. **Begin Checkout** - Start checkout
5. **Add Shipping Info** - Add shipping information
6. **Add Payment Info** - Add payment information
7. **Purchase** - Purchase completed

## 🛠️ Technologies
- **SQL** with CTEs (Common Table Expressions)
- LEFT JOINs to retain all countries
- Conversion calculations using NULLIF to avoid division by zero
- Data analysis for January–August 2025

### 📊 Query
WITH first_visits AS (
  SELECT DISTINCT user_id, country
  FROM mercadolibre_funnel
  WHERE event_name = 'first_visit'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'),
select_item AS (
  SELECT DISTINCT user_id, country
  FROM mercadolibre_funnel
  WHERE event_name IN ('select_item', 'select_promotion')
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'),
add_to_cart AS (
  SELECT DISTINCT user_id, country
  FROM mercadolibre_funnel
  WHERE event_name = 'add_to_cart'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'),
begin_checkout AS (
  SELECT DISTINCT user_id, country
  FROM mercadolibre_funnel
  WHERE event_name = 'begin_checkout'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'),
add_shipping_info AS (
  SELECT DISTINCT user_id, country
  FROM mercadolibre_funnel
  WHERE event_name = 'add_shipping_info'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'),
add_payment_info AS (
  SELECT DISTINCT user_id, country
  FROM mercadolibre_funnel
  WHERE event_name = 'add_payment_info'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'),
purchase AS (
  SELECT DISTINCT user_id, country
  FROM mercadolibre_funnel
  WHERE event_name = 'purchase'
    AND event_date BETWEEN '2025-01-01' AND '2025-08-31'),
funnel_counts AS(
SELECT fv.country,
  COUNT(fv.user_id) AS usuarios_first_visit,
  COUNT(si.user_id) AS usuarios_select_item,
  COUNT(a.user_id) AS usuarios_add_to_cart,
  COUNT(bc.user_id) AS usuarios_begin_checkout,
  COUNT(asi.user_id) AS usuarios_add_shipping_info,
  COUNT(api.user_id) AS usuarios_add_payment_info,
  COUNT(p.user_id) AS usuarios_purchase
FROM first_visits fv
LEFT JOIN select_item si        ON fv.user_id = si.user_id  AND fv.country = si.country
LEFT JOIN add_to_cart a         ON fv.user_id = a.user_id   AND fv.country = a.country
LEFT JOIN begin_checkout bc     ON fv.user_id = bc.user_id  AND fv.country = bc.country
LEFT JOIN add_shipping_info asi ON fv.user_id = asi.user_id AND fv.country = asi.country
LEFT JOIN add_payment_info api  ON fv.user_id = api.user_id AND fv.country = api.country
LEFT JOIN purchase p            ON fv.user_id = p.user_id   AND fv.country = p.country
    GROUP BY fv.country)
SELECT country,
(usuarios_select_item*100.0/NULLIF(usuarios_first_visit,0)) AS conversion_select_item,
(usuarios_add_to_cart*100.0/NULLIF(usuarios_first_visit,0)) AS conversion_add_to_cart,
(usuarios_begin_checkout*100.0/NULLIF(usuarios_first_visit,0)) AS conversion_begin_checkout,
(usuarios_add_shipping_info*100.0/NULLIF(usuarios_first_visit,0)) AS conversion_add_shipping_info,
(usuarios_add_payment_info*100.0/NULLIF(usuarios_first_visit,0)) AS conversion_add_payment_info,
(usuarios_purchase*100.0/NULLIF(usuarios_first_visit,0)) AS conversion_purchase
FROM funnel_counts
ORDER BY conversion_purchase DESC;

4th Phase
# 📊 User Retention Analysis - MercadoLibre

## Description
SQL analysis of user retention rates by country for MercadoLibre, calculating retention percentages at 7, 14, 21, and 28 days after registration.

## 🎯 Objectives
- Calculate retention percentages by country
- Identify retention patterns across different markets
- Analyze retention trends over time

## 📈 Calculated Metrics
- **D7**: Retention at day 7
- **D14**: Retention at day 14  
- **D21**: Retention on day 21
- **D28**: Retention on day 28

## 🛠️ Technologies
- **SQL** with aggregate functions
- CASE WHEN for conditional calculations
- NULLIF to avoid division by zero
- ROUND for formatting results

### 📊 Query
SELECT
  country,
ROUND((COUNT(DISTINCT CASE WHEN day_after_signup >= 7  AND active = 1 THEN user_id END)*100.0)/NULLIF(COUNT(DISTINCT user_id),0),1)  AS retention_d7_pct,
ROUND((COUNT(DISTINCT CASE WHEN day_after_signup >= 14 AND active = 1 THEN user_id END)*100.0)/NULLIF(COUNT(DISTINCT user_id),0),1)  AS retention_d14_pct,
ROUND((COUNT(DISTINCT CASE WHEN day_after_signup >= 21 AND active = 1 THEN user_id END)*100.0)/NULLIF(COUNT(DISTINCT user_id),0),1)  AS retention_d21_pct,
ROUND((COUNT(DISTINCT CASE WHEN day_after_signup >= 28 AND active = 1 THEN user_id END)*100.0)/NULLIF(COUNT(DISTINCT user_id),0),1)  AS retention_d28_pct
FROM mercadolibre_retention
WHERE activity_date BETWEEN '2025-01-01' AND '2025-08-31'
GROUP BY country
ORDER BY country;

5th Phase
# 📊 Cohort Retention Analysis - MercadoLibre

## Description
An SQL query that calculates the user retention rate by monthly cohort on days 7, 14, 21, and 28 after registration.

## 🛠️ Objective
Identify user retention patterns grouped by registration month (cohort) to optimize engagement strategies.

## 📈 Calculated Metrics
- **D7**: % of active users on day 7
- **D14**: % of active users on day 14  
- **D21**: % of active users on day 21
- **D28**: % of active users on day 28

## 🛠️ Technologies
- SQL with CTEs (Common Table Expressions)
- Aggregation functions and CASE WHEN
- Temporal cohort analysis

### 📊 Query
WITH cohort AS (
SELECT
user_id,
TO_CHAR(DATE_TRUNC('month', MIN(signup_date)), 'YYYY-MM') AS cohort
FROM mercadolibre_retention
GROUP BY user_id),
activity AS (

SELECT
    mr.user_id,
    c.cohort,
    mr.day_after_signup,
    mr.active
FROM mercadolibre_retention AS mr
LEFT JOIN cohort AS c ON mr.user_id = c.user_id
WHERE activity_date BETWEEN '2025-01-01' AND '2025-08-31')

SELECT
  cohort,
ROUND((COUNT(DISTINCT CASE WHEN day_after_signup >= 7  AND active = 1 THEN user_id END)*100.0)/NULLIF(COUNT(DISTINCT user_id),0),1)  AS retention_d7_pct,
ROUND((COUNT(DISTINCT CASE WHEN day_after_signup >= 14 AND active = 1 THEN user_id END)*100.0)/NULLIF(COUNT(DISTINCT user_id),0),1)  AS retention_d14_pct,
ROUND((COUNT(DISTINCT CASE WHEN day_after_signup >= 21 AND active = 1 THEN user_id END)*100.0)/NULLIF(COUNT(DISTINCT user_id),0),1)  AS retention_d21_pct,
ROUND((COUNT(DISTINCT CASE WHEN day_after_signup >= 28 AND active = 1 THEN user_id END)*100.0)/NULLIF(COUNT(DISTINCT user_id),0),1)  AS retention_d28_pct
FROM activity
GROUP BY cohort
ORDER BY cohort;
