# Capstone-DA
import pandas as pd

import numpy as np

import datetime

from pandas_datareader import data,wb      #importing modules 

%matplotlib inline

start=datetime.datetime(2008,1,1)

end=datetime.datetime(2021,1,1)                         #Assigning start and end time

BAC=data.DataReader('BAC','yahoo',start,end)



C=data.DataReader('C','yahoo',start,end)

JPM=data.DataReader('JPM','yahoo',start,end)

MS=data.DataReader('MS','yahoo',start,end)     #To get stock data of the banks

WFC=data.DataReader('WFC','yahoo',start,end)



tickers=['BAC','C','JPM','MS','WFC']

bank_stocks=pd.concat([BAC,C,JPM,MS,WFC],axis=1,keys=tickers)     #Setting tickers to dispaly in the final result


bank_stocks.columns.names=['bank','stock_info']
bank_stocks.head()

bank_stocks.xs(key='Close',axis=1,level='stock_info').max()     #prints max price of each stock

returns=pd.DataFrame()
for tick in tickers:
     returns[tick+' Return'] = bank_stocks[tick]['Close'].pct_change() 
        #create and prints return dataframe
print(returns)

import seaborn as sns
sns.pairplot(returns[1:])

returns.idxmax(axis=0)   #returns All time High value

returns.idxmin(axis=0)   #returns All time low value

returns.std()

returns['MS Return']['2015-01-01':'2015-12-31'].std()

sns.distplot(returns['MS Return']['2015-01-01':'2015-12-31'],color='green',bins=50)

sns.distplot(returns['C Return']['2008-01-01':'2008-12-31'],color='red',bins=50)

for tick in tickers:

    bank_stocks[tick]['Close'].plot(label=tick,figsize=(14,3))
    
plt.legend()

import seaborn as sns

import matplotlib.pyplot as plt

sns.set_style('whitegrid')

%matplotlib inline

import cufflinks as cf

import plotly 

cf.go_offline()


bank_stocks.xs(key='Close',axis=1,level='stock_info').iplot()

BAC['Close']['2008-01-01':'2008-12-31'].rolling(window=30).mean().plot(label='30 days mov avg',figsize=(12,4))

BAC['Close']['2008-01-01':'2008-12-31'].plot(label='Stock price')

plt.legend()

sns.heatmap(bank_stocks.xs(key='Close',axis=1,level='stock_info').corr(),annot=True)      #dispalys heatmap for getting business insights


sns.clustermap(bank_stocks.xs(key='Close',axis=1,level='stock_info').corr(),annot=True)  #dispalys clustermap for getting business insights

cl_corr=bank_stocks.xs(key='Close',axis=1,level='stock_info').corr()  

cl_corr.iplot(kind='heatmap',colorscale='rdylbu') #dispalys correlation between stocks

bac_cand=BAC[['High','Low','Open','Close']]['2015-01-01':'2016-01-01']

bac_cand.iplot(kind='candle')

MS['Close']['2016-01-01':'2017-01-01'].ta_plot(study='sma',periods=(20,50,100))   #dispalys technical indicator which is 20 days,50days and 100 days Simple Moving Average

#MS[['Open','High','Low','Close']]['2016-01-01':'2017-01-01'].iplot(kind='candle') 

MS['Close']['2016-01-01':'2016-12-31'].ta_plot(study='boll')  #displays technical Indicator Bollinger band

