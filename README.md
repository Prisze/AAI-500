# Final Project

import zipfile
import pandas as pd
import os

file_path = '/mnt/data/online_retail.zip'

with zipfile.ZipFile(file_path, 'r') as zip_ref:
    zip_ref.extractall('/mnt/data/')

extracted_files = os.listdir('/mnt/data/')
print(extracted_files)

excel_path = '/mnt/data/Online Retail.xlsx'

df = pd.read_excel(excel_path, nrows=1000)

print(df.head())

top_10_stock_codes = df['StockCode'].value_counts().head(10)
print(top_10_stock_codes)
