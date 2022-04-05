# Hamoye-Stage-E
import numpy as np
import pandas as pd 
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline

from datetime import datetime

custom_date_parser = lambda x: datetime.strptime(x, "%Y-%m-%d %H:%M:%S")

data = pd.read_csv("Time_series_analysis_and_forecast_DATASET.csv", parse_dates=['FullDate'],index_col=['FullDate'])
data.head()

data.isna().sum()

data.describe()

data_daily = data.resample('D').sum()
data_daily.info()

plt.figure(figsize=(15,10))
plt.plot(data_daily.index, data_daily.ElecPrice, '--')
plt.grid()
plt.xlabel('Day')
plt.ylabel('G_A_P')

from statsmodels.tsa.stattools import adfuller

p_sysload = adfuller(data_daily['SysLoad'])
print(f'p-value: {round(p_sysload[1],6)}')

p_gasprice = adfuller(data_daily['GasPrice'])
print(f'p-value: {round(p_gasprice[1],6)}')

c_gasprice = adfuller(data_daily['GasPrice'])
print('Critical Values:')
for k, v in c_gasprice[4].items():
 print(f'{k} : {v}')
 
 c_elecprice = adfuller(data_daily['ElecPrice'])
print('Critical Values:')
for k, v in c_elecprice[4].items():
 print(f'{k} : {v}')
