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
Now is time to build the html based on the dataset.

```python
html = '''
    <!doctype html>
        <html lang="en">
            <head>
                <title> Car Sales Report </title>
                <meta name="description" content="Report">
                <meta name="author" content="https://github.com/rafaelviniciusoliveira">
                <meta charset="utf-8"> 
            </head>
            <body align='center'>
'''
summary = '''
        <img src='{0}' id = 'first_logo' alt='Logo_1'>
    '''.format( PATH_FOLDER + "\\img\\logo_1.png")
   
end = "<body></html>"

update = datetime.now().strftime('%m/%d/%Y')
table = f'''
        <div id = "divUpdate">
            <p id="pUpdate">
                CAR SALES INFORMATION - LAST UPDATE ON {update}
            </p>
        </div>
        </div>
            <table class="Header" align="center" width="1250px" style = "top:70px" >
                <thead>
                    <th> Manufacturer </th>
                    <th> Model </th>
                    <th> Sales in thousands </th>
                    <th> Price in thousands </th>
                    <th> Horsepower </th>
                    <th> Fuel efficiency </th>
                    <th> Latest Launch </th>
                </thead>'''

for row in dataframe.itertuples():
    table = table + '''<tr>
    <td>{0}</td>
    <td>{1}</td>
    <td>{2}</td>
    <td>{3}</td>
    <td>{4}</td>
    <td>{5}</td>
    <td>{6}</td>
   </tr>'''.format(row.Manufacturer,row.Model,row.Sales_in_thousands,row.Price_in_thousands,row.Horsepower,row.Fuel_efficiency,row.Latest_Launch)   
table += "</table>"
```
Finally, generate the file.

```python
date = datetime.now().date().strftime('%d-%m-%Y')
file_save_path, file = PATH_FOLDER + f'\\Reports\\', 'Car_sale_report.pdf'
pdfkit.from_string(html + summary + table + end, file_save_path + file, css=CSS, configuration=CONFIG, options=OPTIONS) 
print("Report Generated!")
```
## Result
Click on the gifure in the right to see the pdf generated -->
<img src="Reports/Car_sale_report.pdf"></img>

