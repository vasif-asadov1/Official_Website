# Data Cleaning with Python

## <font color="#94d6d5"> Introduction </font>

Until here, we completed: 

- **Deep SQL Analysis:** We performed a comprehensive analysis of the Home Credit dataset using SQL queries and obtained insights about predictive features. 
- **Feature Engineering with SQL:** We engineered a rich set of features by combining information from multiple tables and created a final aggregated dataset in parquet format.

In this step, we will load the final parquet file into a pandas dataframe and perform some final preprocessing steps before feeding it into the model. We will also do some exploratory data analysis to understand the distribution of the features and their relationship with the target variable. We will handle missing values and do some feature scaling if necessary. Finally, we will be ready to train our machine learning model on this rich dataset and make predictions on the test set.


## <font color="#94d6d5"> Library and Import Statements </font>

Firstly, we need to import the necessary libraries for data manipulation and analysis. We will use pandas for data handling, matplotlib and seaborn for visualization, and scikit-learn for preprocessing and modeling. As we are working with a large dataset, we will also use polars for efficient data manipulation. Additionally, we will use duckdb to run SQL queries on our parquet file if needed. Finally, we will use lightgbm for modeling and shap for interpretability. Here are the import statements:

```python
import polars as pl
import pandas as pd
import numpy as np
import duckdb
import matplotlib.pyplot as plt
import plotly.express as px
import plotly.io as pio
import polars.selectors as cs
import plotly.graph_objects as go
import lightgbm as lgb
import shap
from sklearn.preprocessing import LabelEncoder
import warnings

warnings.filterwarnings("ignore")
pio.renderers.default = "notebook"
# Reset options (optional, to avoid messing up future prints)
pd.reset_option('display.max_rows')
pd.reset_option('display.max_columns')
```

Now import the final parquet file we created in the previous step into a pandas dataframe:

```python























