# Transform-data-into-PDF-reports

## Dataset

In order to demonstrate a real life problem, we will create a report to show the top 20 best selling cars. The Dataset used can be found in the site https://www.kaggle.com/datasets/gagandeep16/car-sales

## Step by Step

The first step is import all the necessary libraries

```python
!pip install pdfkit
import os
import pdfkit

import pandas as pd

import pathlib
from datetime import datetime, timedelta, date
```

After that, we need to get the dataset and make the desired treatments

```python
file_path = "C:/Users/Rafael/Desktop/repos/Transform-data-into-PDF-reports/Car_sales.csv"
dataframe = pd.read_csv(file_path)
# Filtering the columns
dataframe = dataframe[['Manufacturer','Model','Sales_in_thousands', 'Price_in_thousands','Horsepower','Fuel_efficiency','Latest_Launch']]
# Get the top 20 sales 
dataframe = dataframe.sort_values(by=['Sales_in_thousands'],ascending=False)
dataframe = dataframe[:20]
```

Having our final dataset, we need to set the definitions of the pdf.

```python 
# First definitions
OPTIONS = {
    'page-size': 'A4',
    'orientation': 'Landscape',
    'quiet': '',
    'enable-local-file-access': True
}
PATH_WKHTMLTOPDF = r'C:\Program Files\wkhtmltopdf\bin\wkhtmltopdf.exe'
CONFIG = pdfkit.configuration(wkhtmltopdf=PATH_WKHTMLTOPDF)
PATH_FOLDER = os.getcwd()
CSS = ('./styles/style.css')
```

