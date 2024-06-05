# Final Project

import zipfile
import pandas as pd

zip_path = '/mnt/data/online_retail 1.zip'
extracted_path = '/mnt/data/online_retail_1.xlsx'

with zipfile.ZipFile(zip_path, 'r') as zip_ref:
    zip_ref.extractall('/mnt/data/')

corrected_extracted_path = '/mnt/data/Online Retail.xlsx'

df_full = pd.read_excel(corrected_extracted_path, sheet_name=0)

# Adding a column for the day of the week
df_full['InvoiceDate'] = pd.to_datetime(df_full['InvoiceDate'])
df_full['DayOfWeek'] = df_full['InvoiceDate'].dt.day_name()

# Most purchased products and frequency
product_counts_full = df_full['StockCode'].value_counts().head(10)

# Most common day of the week
top_10_items_full = product_counts_full.index
common_day_per_item = df_full[df_full['StockCode'].isin(top_10_items_full)].groupby('StockCode')['DayOfWeek'].agg(lambda x: x.value_counts().idxmax())

# Summary
summary_df = pd.DataFrame({
    'StockCode': top_10_items_full,
    'Most Common Day': common_day_per_item.values
})

import ace_tools as tools; tools.display_dataframe_to_user(name="Top 10 Items and Most Common Day of the Week", dataframe=summary_df)

summary_df
