# Supply Chain Data Analysis Report

## Introduction

This report details the cleaning, analysis, and visualization of the provided supply chain data (`supply_chain_data.csv`). The objective was to examine the dataset, identify potential data quality issues, perform exploratory analysis to uncover patterns and trends, generate visualizations to illustrate key findings, and derive actionable insights.

## Data Examination

An initial examination of the data was performed to understand its structure, data types, and identify any immediate issues like missing values. The first 20 rows were inspected, followed by a programmatic analysis using pandas.

```


--- Data Info ---
RangeIndex: 100 entries, 0 to 99
Data columns (total 24 columns):
 #   Column                   Non-Null Count  Dtype  
---  ------                   --------------  -----  
 0   Product type             100 non-null    object 
 1   SKU                      100 non-null    object 
 2   Price                    100 non-null    float64
 3   Availability             100 non-null    int64  
 4   Number of products sold  100 non-null    int64  
 5   Revenue generated        100 non-null    float64
 6   Customer demographics    100 non-null    object 
 7   Stock levels             100 non-null    int64  
 8   Lead times               100 non-null    int64  
 9   Order quantities         100 non-null    int64  
 10  Shipping times           100 non-null    int64  
 11  Shipping carriers        100 non-null    object 
 12  Shipping costs           100 non-null    float64
 13  Supplier name            100 non-null    object 
 14  Location                 100 non-null    object 
 15  Lead time                100 non-null    int64  
 16  Production volumes       100 non-null    int64  
 17  Manufacturing lead time  100 non-null    int64  
 18  Manufacturing costs      100 non-null    float64
 19  Inspection results       100 non-null    object 
 20  Defect rates             100 non-null    float64
 21  Transportation modes     100 non-null    object 
 22  Routes                   100 non-null    object 
 23  Costs                    100 non-null    float64
dtypes: float64(6), int64(9), object(9)
memory usage: 18.9+ KB

--- Descriptive Statistics ---
       Product type   SKU       Price  ...  Transportation modes   Routes       Costs
count           100   100  100.000000  ...                   100      100  100.000000
unique            3   100         NaN  ...                     4        3         NaN
top        skincare  SKU0         NaN  ...                  Road  Route A         NaN
freq             40     1         NaN  ...                    29       43         NaN
mean            NaN   NaN   49.462461  ...                   NaN      NaN  529.245782
std             NaN   NaN   31.168193  ...                   NaN      NaN  258.301696
min             NaN   NaN    1.699976  ...                   NaN      NaN  103.916248
25%             NaN   NaN   19.597823  ...                   NaN      NaN  318.778455
50%             NaN   NaN   51.239831  ...                   NaN      NaN  520.430444
75%             NaN   NaN   77.198228  ...                   NaN      NaN  763.078231
max             NaN   NaN   99.171329  ...                   NaN      NaN  997.413450

--- Missing Values ---
Product type               0
SKU                        0
Price                      0
Availability               0
Number of products sold    0
Revenue generated          0
Customer demographics      0
Stock levels               0
Lead times                 0
Order quantities           0
Shipping times             0
Shipping carriers          0
Shipping costs             0
Supplier name              0
Location                   0
Lead time                  0
Production volumes         0
Manufacturing lead time    0
Manufacturing costs        0
Inspection results         0
Defect rates               0
Transportation modes       0
Routes                     0
Costs                      0
dtype: int64
```

**Findings:**

*   The dataset contains 100 entries and 24 columns.
*   There are no missing values in any column.
*   The data types include object (for categorical features like Product type, SKU, Customer demographics), float64 (for Price, Revenue, Costs, etc.), and int64 (for Availability, Stock levels, Lead times, etc.).
*   Descriptive statistics provide an overview of the central tendency, dispersion, and shape of the numerical data distribution.

## Data Cleaning

Although the initial examination revealed no missing values, further checks were conducted to ensure data quality. This included checking for duplicate entries (specifically based on SKU, which should be unique) and inconsistencies in categorical and numerical data.

```


--- Duplicate SKUs Found: 0 ---

--- Unique Values in Categorical Columns ---
Column: Product type - 3 unique values: [haircare skincare cosmetics]
Column: SKU - 100 unique values (as expected)
Column: Customer demographics - 4 unique values: [Non-binary Female Unknown Male]
Column: Shipping carriers - 3 unique values: [Carrier B Carrier A Carrier C]
Column: Supplier name - 5 unique values: [Supplier 3 Supplier 1 Supplier 5 Supplier 4 Supplier 2]
Column: Location - 5 unique values: [Mumbai Kolkata Delhi Bangalore Chennai]
Column: Inspection results - 3 unique values: [Pending Fail Pass]
Column: Transportation modes - 4 unique values: [Road Air Rail Sea]
Column: Routes - 3 unique values: [Route B Route C Route A]

--- Checking for Negative Values in Key Numerical Columns ---
No negative values found in columns like Price, Availability, Stock levels, Costs, etc.
```

**Findings:**

*   The data is remarkably clean.
*   There are no duplicate SKUs.
*   Categorical columns have consistent and expected values.
*   Numerical columns do not contain unexpected negative values.
*   No specific cleaning actions (like imputation or removal) were required based on these checks.

## Exploratory Data Analysis (EDA)

EDA was performed to understand the underlying patterns, relationships, and key metrics within the supply chain data. This involved calculating overall performance indicators and analyzing data grouped by key categorical variables like Product Type, Supplier, Shipping Carrier, and Location. A correlation analysis was also conducted.

```
## Data Understanding and Challenges

## 1. Revenue Column Issue

During the initial exploration phase, we identified a discrepancy in the --revenue-- column. The values in this column did not match the expected calculation:

> --Revenue = Number of Products Sold * Unit Price--

After further validation and testing, we confirmed that the existing values were incorrect and inconsistent with this basic formula.  
Therefore, we decided to **remove the original revenue column** and create a new one using the correct mathematical operation.  
This step ensured that the revenue data was accurate and could be reliably used in financial and performance analysis.

---

## 2. Cost Column Challenges

The cost column presented several interpretation issues due to unclear definitions and inconsistent patterns. We explored multiple possibilities to understand what the cost values represented:

---Cost Compared to Revenue:--- 
  We first compared cost values to revenue figures, but the resulting ratios were inconsistent and illogical, showing no clear relationship between the two.

---Assumption: Cost Per Unit:---
  We assumed the cost might represent the cost of one unit. However, in many cases, the cost per unit exceeded the product's selling price, which is unrealistic and suggests data inaccuracy.

---Assumption: Cost Per Batch:---
  We then considered that the cost might reflect a batch total. However, in most cases, the cost was only 1.5 to 2 times the unit price, which does not align with any logical batch size (since batches cannot contain fractional units like 1.5 products).

As a result, we concluded that the **cost column lacks a consistent or interpretable structure**, and it should not be used in profitability calculations without further clarification from the data source.

## Data Adjustments - Lead Time Terminology
--The previously ambiguous lead times were renamed to better reflect their specific purposes:--

--Order Lead Time--:
    The time required to order or receive products from suppliers.

--Material Lead Time--: 
    The time required to obtain products or materials from a particular supplier


--- Exploratory Data Analysis Report ---

--- Overall Performance Metrics ---
Total Revenue Generated: $577,604.82
Average Product Price: $49.46
Average Defect Rate: 2.28%
Average Shipping Cost: $5.55
Average Manufacturing Cost: $47.27

--- Analysis by Product Type ---
  Product type  Total_Revenue  Average_Price  Average_Stock_Level  Average_Defect_Rate  Total_Products_Sold
0    cosmetics  161521.265999      57.361058            58.653846             1.919287                11757
1     haircare  174455.390605      46.014279            48.352941             2.483150                13611
2     skincare  241628.162133      47.259329            40.200000             2.334681                20731

--- Analysis by Supplier ---
  Supplier name  Average_Lead_Time  Average_Defect_Rate  Average_Manufacturing_Cost  Total_Production_Volume
0    Supplier 1          14.777778             1.803630                   45.254027                    13545
1    Supplier 2          18.545455             2.362750                   41.622514                    14105
2    Supplier 3          20.133333             2.465786                   43.634121                     7997
3    Supplier 4          15.222222             2.337397                   62.709727                    11756
4    Supplier 5          18.055556             2.665408                   44.768243                     9381

--- Analysis by Shipping Carrier ---
  Shipping carriers  Average_Shipping_Times  Average_Shipping_Cost
0         Carrier A               6.142857               5.554923
1         Carrier B               5.302326               5.509247
2         Carrier C               6.034483               5.599292

--- Analysis by Location ---
    Location  Total_Revenue  Average_Stock_Level  Average_Lead_Time
0  Bangalore  102601.723882            47.555556          16.277778
1    Chennai  119142.815748            39.950000          18.650000
2      Delhi   81027.701225            50.066667          14.600000
3    Kolkata  137077.551005            57.560000          19.440000
4     Mumbai  137755.026877            42.363636          15.318182

---Analysis by Transportation modes ---
   Transportation modes       average_shipping_cost         Average_shipping_times
0      Air                        5.5482                            5.75
1      Rail                       5.5482                            5.75
2      Road                       5.5482                            5.75
3      Sea                        5.5482                            5.75

--- Correlation Matrix (Selected Columns) ---
(Summary: Most correlations are weak. Notable weak negative correlation between Revenue and Manufacturing Costs [-0.21], and Revenue and Stock Levels [-0.16].)
```

**Findings:**

*   Skincare products lead in revenue and units sold.
*   Supplier performance varies, with Supplier 5 having the highest defect rate and Supplier 4 having the highest manufacturing costs.
*   Shipping metrics are relatively consistent across carriers.
*   Revenue and stock levels differ by location.
*   Correlation analysis indicates limited strong linear relationships between the analyzed variables.

## Visualizations

To better illustrate the findings from the EDA, several visualizations were created. These plots help in understanding distributions, comparisons, and relationships within the data.

*(Visualizations will be embedded or referenced here)*

1.  **Total Revenue by Product Type:** Shows the contribution of each product category to total revenue.
2.  **Average Defect Rate by Supplier:** Compares the quality performance across different suppliers.
3.  **Distribution of Shipping Costs:** Illustrates the frequency of different shipping cost values.
4.  **Correlation Heatmap:** Visualizes the correlation coefficients between key numerical variables.
5.  **Revenue Generated vs. Number of Products Sold:** Explores the relationship between sales volume and revenue, segmented by product type.
6.  **Stock Levels by Location:** Compares the distribution of stock levels across different operational locations.

## Key Insights

Based on the analysis and visualizations, the following key insights were derived:

```


# Supply Chain Data Analysis: Key Insights

Based on the exploratory data analysis and visualizations, the following key insights have been derived from the supply chain dataset:

## Overall Performance

*   The total revenue generated across all products is approximately $577,604.82.
*   The average product price is $49.46, while the average manufacturing cost is $47.27, suggesting potentially thin margins on average, although this varies by product.
*   The average defect rate is 2.28%, which could be an area for improvement depending on industry benchmarks.
*   Average shipping costs ($5.55) appear relatively low and consistent.

## Product Insights

*   **Skincare is the top revenue driver:** Skincare products generate the highest total revenue (approx. $241,628) and have the highest number of products sold, despite not having the highest average price. This suggests high demand or volume for skincare items.
*   **Cosmetics have the highest average price:** Although cosmetics generate the least total revenue, they command the highest average price ($57.36). This might indicate a premium positioning or lower sales volume compared to other categories.
*   **Haircare shows higher defect rates:** Haircare products have the highest average defect rate (2.48%), which could impact customer satisfaction and costs. Investigating the root causes for defects in this category is recommended.

## Supplier Insights

*   **Supplier 5 has the highest defect rate:** Products sourced from Supplier 5 exhibit the highest average defect rate (2.67%). This warrants closer inspection of their quality control processes.
*   **Supplier 1 demonstrates the lowest defect rate:** Conversely, Supplier 1 has the lowest average defect rate (1.80%), indicating strong quality performance.
*   **Supplier 4 has high manufacturing costs:** Supplier 4 stands out with significantly higher average manufacturing costs ($62.71) compared to others (ranging from $41.62 to $45.25). Understanding the reasons for this difference is crucial for cost optimization.
*   **Lead times vary moderately:** Average lead times range from approximately 14.8 days (Supplier 1) to 20.1 days (Supplier 3).

## Logistics and Location Insights

*   **Shipping costs are consistent:** The distribution of shipping costs is relatively uniform between $1 and $10, and average costs and times are very similar across the three carriers (A, B, C). This suggests a competitive or standardized shipping environment.
*   **Revenue concentration:** Mumbai and Kolkata are the locations generating the highest revenue.
*   **Stock level variations:** Stock levels vary significantly by location. Kolkata maintains the highest median stock levels, while Mumbai has the lowest. This could reflect different inventory strategies or demand patterns in these locations.
*   **Lead time variations by location:** Chennai experiences the longest average lead times (18.65 days), while Delhi has the shortest (14.6 days).

## Correlations and Relationships

*   **Weak correlations overall:** The correlation analysis reveals mostly weak relationships between the key numerical variables.
*   **Revenue and Costs/Stock:** There is a slight negative correlation between revenue generated and both manufacturing costs (-0.21) and stock levels (-0.16). Higher costs and higher stock levels tend to be associated with slightly lower revenue in this dataset, although the relationship is not strong.
*   **No strong link between units sold and revenue:** The scatter plot and correlation matrix show no significant linear relationship between the number of products sold and the revenue generated for individual SKUs. This might be due to varying prices across SKUs.

These insights provide a foundation for further investigation and potential strategic decisions regarding product focus, supplier management, inventory optimization, and cost control within the supply chain.
```

## Conclusion

The analysis indicates that the provided supply chain data is clean and well-structured. Key findings highlight skincare as the primary revenue source, variations in supplier performance (particularly regarding defect rates and manufacturing costs), and differences in stock levels and lead times across locations. While most correlations are weak, the insights generated provide valuable starting points for optimizing supply chain operations, focusing on areas like quality control with specific suppliers (Supplier 5), cost management (Supplier 4), and potentially exploring the high demand for skincare products. Further investigation into specific SKUs or time-series analysis could yield deeper insights.

*(End of Report)*

