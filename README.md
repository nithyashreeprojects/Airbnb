# Airbnb Seasonality Analysis 

## Project Overview
This project analyzes Airbnb booking patterns to uncover **seasonal demand trends** and identify factors driving occupancy rates across different listing types, price points, and neighborhoods in New York City.

**Goal:** Understand when demand peaks, which listings succeed during off-seasons, and how booking flexibility affects success.

---

## 📋 Datasets Used
- **Source:** Airbnb Open Data (Kaggle)
- **Location:** New York City
- **Records:** 50,530 listings (after cleaning)
- **Date Range:** 2012-08-25 to 2025-06-26 (13 years of historical data)
- **Key Metric:** `reviews_per_month` (proxy for booking frequency/demand)

---

## 🔄 Project Stages & Progress

### ✅ Stage 1: Data Collection
- Loaded Airbnb dataset with 102,599 rows and 27 columns
- Documented data origin and structure
- Initial exploratory checks completed

### ✅ Stage 2: Data Cleaning ([01_Data_Cleaning.ipynb](01_Data_Cleaning.ipynb))
**Tasks Performed:**
- Removed null values in critical columns (last_review, id, name, host_id)
- Corrected data types: converted price/service_fee to numeric, dates to datetime
- Standardized column names (lowercase, underscores)
- Validated and corrected data ranges:
  - minimum_nights: 1-365 days
  - availability_365: 0-365 days
  - price: $50-$1,200
  - reviews_per_month: 0-25.23
- Removed 35,987 rows (41.5%) with outliers/impossible values
- Final clean dataset: **50,530 rows**

### ✅ Stage 4: Feature Engineering ([02_Feature_Engineering.ipynb](02_Feature_Engineering.ipynb))
**Features Created (18 total):**

**Time-Based Features (8):**
- `year`, `month`, `quarter`, `day_of_week`, `day_name`, `week_of_year`
- `is_weekend` (binary flag)
- `is_peak_season` (binary flag for summer/december)

**Seasonality Indicators (1):**
- `season` (Winter/Spring/Summer/Fall)

**Demand Indicators (4):**
- `review_frequency_category` (Low/Medium/High/Very High)
- `availability_category` (Very Low/Low/Medium/High)
- `demand_score` (0-100 percentile scale)
- `demand_level` (Low/Medium/High/Very High)

**Price Features (3):**
- `price_tier` (Budget/Mid-Range/Upscale/Luxury)
- `value_score` (reviews per $100)
- `value_category` (Low Value/Good Value/Excellent Value)

**Booking Patterns (2):**
- `stay_length_category` (Nightly/Weekly/Monthly/Long-term)
- `flexibility_score` (1-10, higher = more flexible)

---

## 🎯 Key Findings (So Far)

### 1️⃣ **Booking Flexibility is CRITICAL** 
| Stay Type | Avg Reviews/Month | Impact |
|-----------|-------------------|--------|
| Nightly | 2.10 | **3.75x more demand** |
| Weekly | 1.34 | Moderate demand |
| Monthly | 0.53 | Low demand |
| Long-term | 0.56 | Lowest demand |

**Insight:** Hosts accepting nightly bookings get 4x more bookings than long-term rentals.

### 2️⃣ **Strong Seasonality Pattern Exists**
| Season | Nightly Reviews/Month | Weekly Reviews/Month |
|--------|----------------------|----------------------|
| Summer | 2.25 | 1.47 |
| Winter | 2.28 | 1.42 |
| Fall | 0.55 | 0.37 |
| Spring | TBD | TBD |

**Insight:** Summer and Winter are peak seasons. Fall has lowest demand (~4x lower).

### 3️⃣ **Price Has NO Impact on Demand**
- Budget ($50-340): 1.39 reviews/month
- Mid-Range ($341-626): 1.41 reviews/month
- Upscale ($627-914): 1.37 reviews/month
- Luxury ($915-1200): 1.41 reviews/month

**Insight:** Price tier doesn't determine success. Location and seasonality matter more.

### 4️⃣ **Correlation Analysis**
| Feature | Correlation | Impact |
|---------|-------------|--------|
| flexibility_score | 0.314 | Strong |
| value_score | 0.562 | Strong |
| demand_score | 0.834 | Strong |
| minimum_nights | -0.190 | Negative (less flexible = fewer bookings) |
| price | 0.004 | **None!** |

---

### ✅ Stage 3: Exploratory Data Analysis ([03_Exploratory_Data_Analysis.ipynb](03_Exploratory_Data_Analysis.ipynb))

**Sections Completed:**
- 3.1: Load Data & Setup
- 3.2: Seasonality Analysis (Month x Season patterns)
- 3.3: Demand by Booking Type (Nightly vs Weekly vs Monthly)
- 3.4: Correlation Analysis & Feature Relationships
- 3.5: Summary & Key Takeaways

**Visualizations Created:**
1. ✅ Seasonality heatmaps (Month x Season)
2. ✅ Demand distribution by booking type
3. ✅ Correlation matrix (feature importance)
4. ✅ Neighborhood seasonality patterns
5. ✅ Price tier comparison analysis

## 📊 EXPLORATORY DATA ANALYSIS RESULTS

### Section 3.2: Seasonality Patterns

**Key Findings:**
- Peak months: February (2.25) & June (2.32) reviews/month
- Off-peak months: July (0.30) & August (0.28) reviews/month
- Peak-to-Off-Peak swing: **267.5% difference**

**Monthly Breakdown:**

**Seasonal Averages:**
- Winter: 1.48 reviews/month
- Summer: 1.55 reviews/month
- Fall: 0.42 reviews/month (off-peak)

---

### Section 3.3: Impact of Booking Flexibility

**Critical Finding: Flexibility = 4x Demand Difference**

| Stay Type | Reviews/Month | Market Share | Seasonality |
|-----------|---------------|--------------|-------------|
| Nightly | 2.10 ⭐⭐⭐ | 24.8% | 0.55 → 2.28 (4.1x) |
| Weekly | 1.34 ⭐⭐ | 58.7% | 0.37 → 1.47 (4.0x) |
| Monthly | 0.53 | 15.2% | 0.41 → 0.57 (1.4x) |
| Long-term | 0.56 | 1.3% | 0.34 → 0.64 (1.9x) |

**Insight:** Hosts accepting nightly bookings receive 4x more reviews!

---

### Section 3.4: Feature Correlation with Demand

**Top Demand Drivers (Ranked):**

| Rank | Feature | Correlation | Impact |
|------|---------|-------------|--------|
| 1 | demand_score | 0.834 | ⭐⭐⭐ Strong |
| 2 | value_score | 0.562 | ⭐⭐⭐ Strong |
| 3 | flexibility_score | 0.314 | ⭐⭐⭐ Strong |
| 4 | is_peak_season | 0.223 | ⭐⭐ Moderate |
| 5 | availability_365 | 0.104 | ⭐ Weak |
| 6 | is_weekend | 0.050 | Very Weak |
| 7 | price | **0.004** | **❌ None** |
| 8 | minimum_nights | -0.190 | Negative |

**Critical Insight:** Price has essentially ZERO correlation (0.004) with demand!

**Price Tier Comparison:**
- Budget ($50-340): 1.39 reviews/month (39.9% very high demand)
- Mid-Range ($341-626): 1.41 reviews/month (40.1% very high demand)
- Upscale ($627-914): 1.37 reviews/month (39.0% very high demand)
- Luxury ($915-1200): 1.41 reviews/month (40.2% very high demand)

**Conclusion:** All price tiers perform identically. Price is NOT a demand driver.

---

### Section 3.5: Neighborhood Analysis

**Performance by Neighborhood:**

| Neighborhood | Reviews/Month | Market % | Nightly % | Seasonality |
|--------------|---------------|----------|-----------|-------------|
| Bronx | 1.85 ⭐ | 2.6% | 31.7% | Strong |
| Queens | 1.79 ⭐ | 13.0% | 36.2% | Strong |
| Staten Island | 1.69 | 0.9% | 26.8% | Moderate |
| Brooklyn | 1.34 | 41.8% | 21.4% | Moderate |
| Manhattan | 1.29 | 41.7% | 24.3% | Weak |

**Key Finding:** Manhattan (premium area) has **LOWER demand than Bronx/Queens!**
- Bronx outperforms Manhattan by 43%
- Location matters more than price
- Outer boroughs favor nightly rentals

