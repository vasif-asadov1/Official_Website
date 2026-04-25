# Advanced SQL Analysis with DuckDB

## <font color="#94d6d5">Introduction  </font>
This is the first step of the analysis. In this step, I will use DuckDB SQL to do the data ingestion and initial exploration of the data. I will read the data from the csv files and do the initial exploration of the data to understand the structure of the data. The dataset consists of the following tables: 

- `application_data`: This table contains the main information about the customers, including their demographic information, financial information, and the target variable (whether they defaulted or not).
- `bureau`: This table contains information about the customers' credit history from other financial institutions.
- `bureau_balance`: This table contains information about the monthly balance of the customers' credit history from other financial institutions.
- `credit_card_balance`: This table contains information about the customers' credit card balance and payment history.
- `installments_payments`: This table contains information about the customers' installment payments history.
- `previous_application`: This table contains information about the customers' previous loan applications and their outcomes.   
- `POS_CASH_balance`: This table contains information about the customers' point of sale and cash loan balance and payment history.

Each csv file will be read by DuckDB SQL and added to the DuckDB database. After reading the data, I will do the initial exploration of the data, I will do analysis on each table to define the unnecessary or missing features. This will help me to simplify the data and make it easier to work with in the next steps of the analysis. 


## <font color="#94d6d5"> Data Ingestion </font>

As is mentioned earlier, the data will be imported into the DuckDB database. **DuckDB** is a modern SQL engine that can run inside Python and it is optimized for analytical queries on large datasets. It is much faster than traditional Pandas reading methods and it can handle large datasets efficiently. It is my preferred method for working with large datasets, because it allows me to set hardware constraints such as *maximum usable ram*, *maximum usable core threads*. As my laptop hardware is limited with 12 GB Ram and Processor with 8 cores, I should definitely set these constraints to avoid any performance issues. 



### <font color="#8da2e0"> Library and Settings </font>

Firstly, let's import all necessary libraries and adjust the settings for the libraries. 

```python
import pandas as pd
import numpy as np
import polars as pl 
import plotly.express as px
import duckdb
import os 
import plotly.express as px # for visualization
import plotly.io as pio   
import plotly.graph_objects as go 

pd.set_option("display.max_columns", None) # to display all columns in the output of the queries
pio.renderers.default = "notebook" # for plotly visualizations in Jupyter Notebook
from itables import init_notebook_mode # for interactive tables
init_notebook_mode(all_interactive=True)
```

### <font color="#8da2e0"> DuckDB Connection and File Import </font>


Then we should define the path to the data and create a connection to the DuckDB database with hardware constraints. 


```python
directory = "/home/vasif/Desktop/Home-Credit-Risk-Analysis/data/"
con = duckdb.connect(
    database="/home/vasif/Desktop/Home-Credit-Risk-Analysis/home_credit.duckdb",
    read_only=False)
con.execute("PRAGMA threads=6;") # define the max number of cpu cores usable by duckdb
con.execute("PRAGMA memory_limit='8GB';") # define the max size of ram usage by duckdb
``` 

The above code will create the DuckDB database with the name `home_credit.duckdb` in the specified path. The `PRAGMA` statements will set the maximum number of CPU cores that DuckDB can use to 6 and the maximum amount of RAM that DuckDB can use to 8 GB. This will help to optimize the performance of DuckDB when working with large datasets.

Now, we can create the tables in the DuckDB database and read the data from the csv files into these tables. 

```python
con.execute(f""" CREATE TABLE applications AS SELECT * FROM read_csv_auto('{directory}application_train.csv'); """)

con.execute(f""" CREATE TABLE pos_cash_balance AS SELECT * FROM read_csv_auto('{directory}POS_CASH_balance.csv'); """)

con.execute(f""" CREATE TABLE bureau AS SELECT * FROM read_csv_auto('{directory}bureau.csv'); """)

con.execute(f""" CREATE TABLE bureau_balance AS SELECT * FROM read_csv_auto('{directory}bureau_balance.csv'); """)

con.execute(f""" CREATE TABLE credit_card_balance AS SELECT * FROM read_csv_auto('{directory}credit_card_balance.csv'); """)

con.execute(f""" CREATE TABLE installments_payments AS SELECT * FROM read_csv_auto('{directory}installments_payments.csv'); """)

con.execute(f""" CREATE TABLE previous_applications AS SELECT * FROM read_csv_auto('{directory}previous_application.csv'); """)
```

The above code will create the tables in the DuckDB database and read the data from the csv files into these tables. The `read_csv_auto` function is a built-in function in DuckDB that automatically detects the schema of the csv file and creates the table accordingly. After executing the above code, we will have all the data from the csv files imported into the DuckDB database and we can start doing the initial exploration of the data.


### <font color="#8da2e0"> Exporting the Tables as Parquet Files </font>

As we already imported the tables into the DuckDB database, each time we start a new session, we can simply connect to the database and start doing the analysis without the need to import the data again. This will save us a lot of time and resources, especially when working with large datasets. However, to guarantee our analysis is reproducible, it is better experience to convert large csv files into **parquet files** and read the data from parquet files instead of csv files. Parquet files are a columnar storage format that is optimized for analytical queries and it is a lot much faster than reading csv files when working with large datasets. 

By using DuckDB, we can easily export our tables as parquet files with the following code: 

```python
parquet_dir = "/home/vasif/Desktop/Home-Credit-Risk-Analysis/parquet_files/"
os.makedirs(parquet_dir, exist_ok=True)

tables = [
    "applications",
    "pos_cash_balance",
    "bureau",
    "bureau_balance",
    "credit_card_balance",
    "installments_payments",
    "previous_applications"
]

for table in tables:
    file_path = os.path.join(parquet_dir, f"{table}.parquet")
    con.execute(f"""
        COPY (SELECT * FROM {table})
        TO '{file_path}' (FORMAT PARQUET)
    """)
    print(f"{table} exported to {file_path}")
```


### <font color="#8da2e0"> Initial Data Exploration </font>

After importing the data into the DuckDB database, we can start doing the initial exploration of the data. Firstly, let's check the number of rows in each table to understand the size of the data. 

```python
tables = [
    "applications",
    "pos_cash_balance",
    "bureau",
    "bureau_balance",
    "credit_card_balance",
    "installments_payments",
    "previous_applications"
]

for table in tables:
    count = con.execute(f"SELECT COUNT(*) FROM {table}").fetchone()[0]
    print(f"Table '{table}' has {count:,} rows")
```

**Output:**

```
Table 'applications' has 307,511 rows
Table 'pos_cash_balance' has 10,001,358 rows
Table 'bureau' has 1,716,428 rows
Table 'bureau_balance' has 27,299,925 rows
Table 'credit_card_balance' has 3,840,312 rows
Table 'installments_payments' has 13,605,401 rows
Table 'previous_applications' has 1,670,214 rows
```

We can observe from the results that some tables have more than 10 millions of rows. Therefore, traditional Pandas analysis methods will be very slow and inefficient when working with these tables. **This is why using DuckDB SQL is a much better option for working with this data, because it is optimized for analytical queries on large datasets and it can handle large datasets efficiently.** Hence, I will use DuckDB SQL to do all the analysis and feature engineering steps on the data, then create a final aggregated table with all the important features that I will use for building the prediction model. After creating the final aggregated table, I will switch to Pandas for data wrangling and model building steps, because the size of the final aggregated table will be much smaller than the original tables and it will be easier to work with in Pandas.

### <font color="#8da2e0"> DuckDB Functions </font>

Now we can start to do our deep analysis on the data. Before starting the analysis, I should mention some of the important functions to use in DuckDB SQL for the analysis.

After writing the SQL queries, we can execute them and retrieve the results with different functions: 

- **`fetchall()`:** This function retrieves all the rows from the result of the query and returns them as a list of tuples. Each tuple represents a row in the result, and each element in the tuple represents a column value. This function is useful when we want to retrieve all the data from the query result and we don't mind about the memory usage. However, if the result of the query is very large, it may consume a lot of memory and cause performance issues.
- **`fetchone()`:** This function retrieves the next row from the result of the query and returns it as a tuple. Each element in the tuple represents a column value. This function is useful when we want to retrieve one row at a time from the query result, especially when the result is very large and we want to avoid memory issues. We can use this function in a loop to retrieve all the rows one by one.
- **`fetchdf()`:** This function retrieves all the rows from the result of the query and returns them as a Pandas DataFrame. Each column in the DataFrame represents a column in the query result, and each row in the DataFrame represents a row in the query result. This function is useful when we want to work with the query result in a tabular format and we want to take advantage of the functionalities provided by Pandas for data manipulation and analysis. However, if the result of the query is very large, it may consume a lot of memory and cause performance issues. 
- **`fetchpolars()`:** This function retrieves all the rows from the result of the query and returns them as a Polars DataFrame. This function is useful when we want to work with the query result in a tabular format and we want to take advantage of the functionalities provided by **Polars** for data manipulation and analysis. Polars is a fast and efficient DataFrame library that is optimized for performance, especially when working with large datasets. Therefore, using `fetchpolars()` can be a better option than `fetchdf()` when working with large query results, as it can help to avoid memory issues and improve performance.

To execute the queries and commands we have two options:

1. **`execute()`:** With this method, we should write the SQL query as a string and pass it to the `execute()` method. This method will execute the query and return a cursor object that we can use to retrieve the results with the `fetch*()` methods. This method is useful when we want to execute a single query or command and we don't need to reuse the query or command multiple times.
2. **`sql()`:** This is a more convenient method for executing SQL queries. With this method, we can write the SQL query directly as a string and pass it to the `sql` method. This method will execute the query and return the results directly without the need to use a cursor object. This method is useful when we want to execute a single query or command and we want to retrieve the results directly without the need for additional steps.



## <font color="#94d6d5"> Detailed SQL Analysis </font>

After learning the important functions we are ready to do our deep SQL analysis on the data. Each table will be considered separately and investigated with care. With this method, we will find out the necessary and predictive features for the model building step. We will also find out the patterns and insights from the data that will help us to build a better prediction model. 


### <font color="#8da2e0"> Applications Table </font>

#### <font color="#8da2e0"> Task 1. The *Family Burden* Matrix</font>

**Business Question:** Does the interaction between marital status (Single vs. Married) and
number of dependent children materially alter default risk? Is a single parent riskier than a
married parent with the same number of children?

**Objective:** Determine whether family size functions as a financial stressor or a stability anchor. Specifically, assess whether default probability increases linearly with the number of children
and whether marital status mitigates this effect. The outcome informs whether family-related variables should be penalized or interaction-adjusted in the risk model.



<details>
<summary>Show Code</summary>


```python
query = """
select 
    -- 1. Create the Marital Status Groups
    NAME_FAMILY_STATUS,

    -- 2. Bin Children (0, 1, 2, 3+) to handle outliers
    CASE 
        WHEN CNT_CHILDREN >= 3 THEN '3+'
        ELSE CAST(CNT_CHILDREN AS VARCHAR)
    END AS Num_Children,

    -- 3. Calculate Default Rate
    COUNT(*) AS Total_Count,
    ROUND(AVG(TARGET),3) AS Risk_Probability

FROM applications
WHERE NAME_FAMILY_STATUS IS NOT NULL 
  AND NAME_FAMILY_STATUS != 'Unknown'
GROUP BY 1, 2
ORDER BY 
    -- Sort logic: Children 0->3+, then Marital Status
    Risk_Probability desc
"""

t1 = con.execute(query).fetchdf()
markdown_table = t1.to_markdown(index=False)
print(markdown_table)
```

</details>

**Results:**

<div style="max-height: 400px; overflow: auto; white-space: nowrap;">

| NAME_FAMILY_STATUS   | Num_Children   |   Total_Count |   Risk_Probability |
|:---------------------|:---------------|--------------:|-------------------:|
| Civil marriage       | 3+             |           304 |              0.158 |
| Single / not married | 3+             |            98 |              0.153 |
| Widow                | 3+             |            67 |              0.119 |
| Civil marriage       | 2              |          1936 |              0.116 |
| Single / not married | 2              |           958 |              0.111 |
| Single / not married | 1              |          5578 |              0.109 |
| Civil marriage       | 1              |          6588 |              0.103 |
| Civil marriage       | 0              |         20947 |              0.096 |
| Single / not married | 0              |         38810 |              0.096 |
| Separated            | 2              |          1111 |              0.095 |
| Married              | 3+             |          3665 |              0.094 |
| Separated            | 1              |          4389 |              0.094 |
| Separated            | 3+             |           138 |              0.087 |
| Married              | 1              |         43696 |              0.085 |
| Married              | 2              |         22496 |              0.084 |
| Separated            | 0              |         14132 |              0.077 |
| Widow                | 2              |           248 |              0.073 |
| Married              | 0              |        126575 |              0.071 |
| Widow                | 1              |           868 |              0.067 |
| Widow                | 0              |         14905 |              0.057 |

</div>
<br>



**Insights:**

The data reveals a clear red flag: default risk spikes significantly for families with 3 or more children. What stands out most is that 'Civil Marriage' applicants mimic the high-risk behavior of single applicants rather than married ones. This suggests that raising a large family without the legal bond of marriage points to a lower level of stability or formal commitment, making this specific combination—Civil Marriage with 3+ kids—the riskiest profile in our dataset. Single applicants trail closely behind as the second highest risk group, while widowed individuals proved to be the safest. Ultimately, the driving force here is likely simple economics: households with 3+ children operate on tighter budgets, leaving them little room for error when repaying loans.



#### <font color="#8da2e0"> Task 2. The ”Leverage vs. Education” Paradox  </font>

**Business Question: Do higher-educated clients handle high debt burdens better than lower-
educated clients?**


**Objective:** To test the ”Financial Literacy” hypothesis. High debt (high annuity) usually
predicts default. However, we suspect that highly educated clients can manage high debt better
than less educated ones. If true, we can allow higher loan amounts for educated applicants
without increasing risk.

**Solution:** For each of the education level (`NAME_EDUCATION_TYPE`), we should create bins using the ratio of annual credit amount (`AMT_ANNUITY`) to the total income amount (`AMT_INCOME_TOTAL`). Then for each education level - ratio bin groups, total loan volume and risk probability rate can be calculated.


<details>
<summary>Click here to see the solution</summary>

```python
query = """ 
    select
        NAME_EDUCATION_TYPE as education,
        case 
            when amt_annuity / nullif(amt_income_total, 0) < 0.1 then '0-10%'
            when amt_annuity / nullif(amt_income_total, 0) < 0.2 then '10-20%'
            when amt_annuity / nullif(amt_income_total, 0) < 0.3 then '20-30%'
            when amt_annuity / nullif(amt_income_total, 0) < 0.4 then '30-40%'
            else '40%+'
        end as leverage_bin,
        count(*) as total_count, 
        round(avg(target),3) as risk_probability   
    from applications
    group by 
        1,2   
    order by
        risk_probability desc
"""
t2 = con.execute(query).fetchdf()
t2
```

</details>


**Results:**

<div style="max-height: 400px; overflow: auto; white-space: nowrap;">

| education                     | leverage_bin   |   total_count |   risk_probability |
|:------------------------------|:---------------|--------------:|-------------------:|
| Lower secondary               | 20-30%         |          1053 |              0.13  |
| Lower secondary               | 30-40%         |           371 |              0.113 |
| Lower secondary               | 10-20%         |          1733 |              0.106 |
| Incomplete higher             | 30-40%         |           606 |              0.097 |
| Secondary / secondary special | 20-30%         |         55254 |              0.096 |
| Lower secondary               | 40%+           |           137 |              0.095 |
| Incomplete higher             | 10-20%         |          5041 |              0.09  |
| Secondary / secondary special | 10-20%         |        104022 |              0.089 |
| Secondary / secondary special | 40%+           |          6261 |              0.087 |
| Incomplete higher             | 20-30%         |          2150 |              0.087 |
| Secondary / secondary special | 30-40%         |         17916 |              0.086 |
| Secondary / secondary special | 0-10%          |         34938 |              0.084 |
| Lower secondary               | 0-10%          |           522 |              0.079 |
| Incomplete higher             | 40%+           |           170 |              0.071 |
| Incomplete higher             | 0-10%          |          2310 |              0.069 |
| Higher education              | 40%+           |          1376 |              0.062 |
| Higher education              | 30-40%         |          4586 |              0.058 |
| Higher education              | 20-30%         |         15454 |              0.056 |
| Higher education              | 10-20%         |         36998 |              0.053 |
| Higher education              | 0-10%          |         16449 |              0.049 |
| Academic degree               | 10-20%         |            73 |              0.027 |
| Academic degree               | 0-10%          |            49 |              0.02  |
| Academic degree               | 20-30%         |            25 |              0     |
| Academic degree               | 30-40%         |            13 |              0     |
| Academic degree               | 40%+           |             4 |              0     |

</div>
<br>

**Insights:**

The analysis reveals a strong association between education level and default risk, as well as a nonlinear relationship between the debt-to-income (DTI) ratio and default probability. Default risk is elevated among applicants with secondary or lower secondary education and materially lower among those with academic or higher education credentials. Risk concentration is most pronounced within the lower secondary education group, particularly for DTI ratios between 10% and 40%, peaking around the 20%–30% range.

This pattern is consistent with the hypothesis that higher education correlates with greater income stability, financial literacy, and job security, which collectively improve debt-servicing capacity. Conversely, lower education levels are associated with income volatility and limited financial buffers, increasing sensitivity to leverage. These interpretations are correlational and do not imply causality.



#### <font color="#8da2e0"> Task 3. The "Age & Occupation" Risk Heatmap</font>

**Business Question: Which specific career stage creates the highest risk?**

**Objective:** To identify ”High-Risk Clusters” that standard models might miss. Age alone is
a strong predictor, but a ”Young Manager” might behave totally differently from a ”Young
Laborer.” Identifying these specific intersections allows us to adjust the credit score for specific
job types based on the applicant’s life stage.

**Solution:** Since the `DAYS_BIRTH` column indicating the applicant age in unit of days is negative, the first thing must be done is changing its sign and divide by 365 which will yield the applicant age in years. Then, ages will be binned with 10 years bin size and for each group joined with the occupation type, total number of loans and risk probability will be obtained.

<details>
<summary>Click here to see the solution</summary>
<br>

```python
query = """
    select
        case 
            when round(-days_birth/365,0) < 25 then '<25'
            when round(-days_birth/365,0) < 35 then '25-35'
            when round(-days_birth/365,0) < 45 then '35-45'
            when round(-days_birth/365,0) < 55 then '45-55'
            else '55+'
        end as age_group, 
        occupation_type,
        count(*) as total_loans, 
        round(avg(target), 3) * 100 as risk_probability

        from 
            applications
        group by 
            1, 2
        order by 
            risk_probability desc            
"""

t3 = con.execute(query).fetchdf()
```
</details>

<br>

**Results:**

<div style="max-height: 400px; overflow: auto; white-space: nowrap;">

| age_group   | OCCUPATION_TYPE       |   total_loans |   risk_probability |
|:------------|:----------------------|--------------:|-------------------:|
| 25-35       | Low-skill Laborers    |           664 |               20   |
| <25         | Low-skill Laborers    |           110 |               20   |
| <25         | HR staff              |            26 |               19.2 |
| 25-35       | Cleaning staff        |           515 |               17.7 |
| 35-45       | Low-skill Laborers    |           707 |               16.8 |
| 25-35       | Security staff        |          1071 |               16.1 |
| 25-35       | Cooking staff         |          1375 |               15.9 |
| <25         | Cleaning staff        |            38 |               15.8 |
| <25         | Laborers              |          2134 |               14.4 |
| 55+         | Low-skill Laborers    |           133 |               14.3 |
| <25         | Medicine staff        |           244 |               14.3 |
| 25-35       | Waiters/barmen staff  |           492 |               14.2 |
| <25         | Drivers               |           549 |               14.2 |
| 45-55       | Low-skill Laborers    |           479 |               13.8 |
| <25         | Security staff        |           132 |               13.6 |
| 25-35       | Drivers               |          4540 |               13.4 |
| <25         | Secretaries           |            83 |               13.3 |
| 25-35       | Laborers              |         15364 |               13.3 |
| <25         | Waiters/barmen staff  |           205 |               13.2 |
| <25         | Cooking staff         |           199 |               13.1 |
| <25         | Sales staff           |          1949 |               12.9 |
| <25         |                       |          1765 |               12.8 |
| 25-35       | Sales staff           |         10343 |               11.7 |
| 25-35       | Realty agents         |           275 |               11.6 |
| 35-45       | Security staff        |          1897 |               11.1 |
| 35-45       | Drivers               |          6353 |               10.9 |
| 35-45       | Cooking staff         |          1987 |               10.7 |
| <25         | Core staff            |          1525 |               10.7 |
| 35-45       | Cleaning staff        |          1170 |               10.7 |
| 35-45       | Laborers              |         18825 |               10.6 |
| 45-55       | IT staff              |            57 |               10.5 |
| 45-55       | Drivers               |          5263 |               10.5 |
| <25         | IT staff              |            40 |               10   |
| 25-35       |                       |         11814 |                9.6 |
| 45-55       | Security staff        |          2155 |                9.6 |
| <25         | Managers              |           261 |                9.6 |
| 25-35       | Medicine staff        |          1683 |                9.4 |
| 55+         | Drivers               |          1898 |                9.3 |
| <25         | High skill tech staff |           443 |                9.3 |
| 25-35       | Private service staff |           787 |                9.3 |
| 35-45       | Waiters/barmen staff  |           356 |                9.3 |
| 35-45       | Sales staff           |         10780 |                9.2 |
| <25         | Private service staff |           120 |                9.2 |
| 45-55       | Cleaning staff        |          1794 |                8.8 |
| 35-45       | Secretaries           |           396 |                8.8 |
| <25         | Accountants           |           286 |                8.7 |
| 45-55       | Waiters/barmen staff  |           223 |                8.5 |
| 25-35       | Managers              |          4798 |                8.5 |
| 45-55       | Laborers              |         14168 |                8.4 |
| 35-45       |                       |         13917 |                8.2 |
| 25-35       | IT staff              |           249 |                8   |
| 55+         | Security staff        |          1466 |                7.8 |
| 45-55       | Cooking staff         |          1907 |                7.6 |
| 25-35       | Core staff            |          9507 |                7.5 |
| 25-35       | HR staff              |           188 |                7.4 |
| 45-55       | Sales staff           |          7484 |                7.3 |
| 35-45       | Realty agents         |           265 |                7.2 |
| 35-45       | Medicine staff        |          3017 |                7.1 |
| 25-35       | High skill tech staff |          3403 |                7   |
| 45-55       |                       |         15536 |                6.8 |
| 25-35       | Secretaries           |           466 |                6.7 |
| 55+         | Laborers              |          4695 |                6.5 |
| 45-55       | Private service staff |           615 |                6.3 |
| 35-45       | High skill tech staff |          3512 |                6.2 |
| 35-45       | HR staff              |           180 |                6.1 |
| 25-35       | Accountants           |          2923 |                6.1 |
| 55+         | Cleaning staff        |          1136 |                5.9 |
| 35-45       | Core staff            |          8676 |                5.9 |
| 55+         | Sales staff           |          1546 |                5.8 |
| 35-45       | Managers              |          7850 |                5.7 |
| 45-55       | Secretaries           |           252 |                5.6 |
| 45-55       | High skill tech staff |          2864 |                5.6 |
| 45-55       | Managers              |          6290 |                5.5 |
| 55+         |                       |         53359 |                5.1 |
| 45-55       | Medicine staff        |          2508 |                4.8 |
| 35-45       | Private service staff |          1004 |                4.8 |
| 45-55       | Core staff            |          5808 |                4.7 |
| 55+         | Managers              |          2172 |                4.7 |
| 45-55       | Realty agents         |           152 |                4.6 |
| 45-55       | HR staff              |           115 |                4.3 |
| 45-55       | Accountants           |          2409 |                4.3 |
| 55+         | Cooking staff         |           478 |                4.2 |
| 35-45       | Accountants           |          3206 |                4.2 |
| 55+         | Waiters/barmen staff  |            72 |                4.2 |
| 55+         | Medicine staff        |          1085 |                4.1 |
| 55+         | Core staff            |          2054 |                3.7 |
| 55+         | High skill tech staff |          1158 |                3.6 |
| 55+         | Private service staff |           126 |                3.2 |
| 55+         | Accountants           |           989 |                3.1 |
| 55+         | Realty agents         |            34 |                2.9 |
| 35-45       | IT staff              |           150 |                2.7 |
| 55+         | HR staff              |            54 |                1.9 |
| 55+         | Secretaries           |           108 |                0.9 |
| 55+         | IT staff              |            30 |                0   |
| <25         | Realty agents         |            25 |                0   |

</div>

<br>

**Insights:**
The results shows default risk across occupations and age groups, where color represents risk level and bubble size reflects the number of applicants. Across all age groups, the majority of applicants are concentrated in laborer (physical worker) occupations, making this segment the largest in the portfolio.

Default risk among laborers varies noticeably with age. Younger laborers exhibit higher default rates, while risk declines steadily for older groups. The default rate for younger laborers is around 14%, which is more than 35% higher than that observed among older laborers. Other low-skill occupations—such as cleaning staff, cooking staff, and security personnel—also display elevated risk levels, generally in the 17%–20% range.

Higher-skilled occupations tend to have lower default risk overall. However, younger applicants within these roles still fall into a moderate-risk category, whereas older applicants are consistently low risk. Overall, the results suggest that age is a strong predictor of default risk, especially when considered together with occupation and education level.


#### <font color="#8da2e0"> Task 4. The ”Asset Shield” Analysis</font>

**Business Question: Does owning ”Hard Assets” (Car/Real Estate) serve as a safety net for clients who don’t own their home (Renters/Living with Parents)?**

**Objective:** To quantify the ”Safety Net” effect of assets. We need to know if owning a car or
real estate compensates for a ”risky” housing situation (like renting). This helps us approve
loans for Renters if they demonstrate stability through other assets (like owning a car)


<details>
<summary>Click here to see the solution</summary>
<br>

```python
query = """ 
    select 
        case
            when flag_own_car = 'Y' then 'Have Car'
            else 'No Car'
        end as Owned_Car, 

        case 
            when flag_own_realty = 'Y' then 'Have Estate'
            else 'No Estate'
        end as Owned_Estate,
        count(*) as total_loans,
        round(avg(target) * 100, 2) as risk_probability
    from applications
    group by 1,2
    order by 4 desc
"""
t4 = con.execute(query).fetchdf()
t4
```
</details>

**Results:**

| Owned_Car   | Owned_Estate   |   total_loans |   risk_probability |
|:------------|:---------------|--------------:|-------------------:|
| No Car      | No Estate      |         61972 |               8.99 |
| No Car      | Have Estate    |        140952 |               8.28 |
| Have Car    | Have Estate    |         72360 |               7.33 |
| Have Car    | No Estate      |         32227 |               7.04 |


**Insights:**
The results show no strong relationship between owning a car or real estate and default risk, as the risk rates across groups are very close to each other. Car ownership appears to be weakly associated with lower risk, since applicants who own a car have slightly lower default rates. However, real estate ownership does not show a clear or consistent pattern, as both the highest and lowest risk groups include applicants without real estate. Overall, these variables provide limited predictive power on their own and are unlikely to be strong standalone risk indicators.





















