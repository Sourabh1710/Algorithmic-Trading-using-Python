# Algorithmic-Trading-using-Python.github.io

import pandas as pd
import plotly.graph_objs as go
from plotly.subplots import make_subplots
import plotly.express as px
import yfinance as yf
# Get Apple's stock data from yahoo finance
stock = yf.Ticker("AAPL")
data = stock.history(period="1y")
print(data.head())
                                 Open        High         Low       Close  \
Date                                                                        
2023-05-17 00:00:00-04:00  170.802922  172.016463  169.519728  171.777740   
2023-05-18 00:00:00-04:00  172.086101  174.314273  171.668322  174.125275   
2023-05-19 00:00:00-04:00  175.458194  175.458194  174.015856  174.234695   
2023-05-22 00:00:00-04:00  173.060916  173.787070  172.533717  173.279755   
2023-05-23 00:00:00-04:00  172.215431  172.464110  170.375197  170.653717   

                             Volume  Dividends  Stock Splits  
Date                                                          
2023-05-17 00:00:00-04:00  57951600        0.0           0.0  
2023-05-18 00:00:00-04:00  65496700        0.0           0.0  
2023-05-19 00:00:00-04:00  55772400        0.0           0.0  
2023-05-22 00:00:00-04:00  43570900        0.0           0.0  
2023-05-23 00:00:00-04:00  50747300        0.0           0.0  
# Calculation of momentum
data['momentum'] = data['Close'].pct_change()
# Creating subplots to show momentum and buying/selling markers
figure = make_subplots(rows=2, cols=1)
figure.add_trace(go.Scatter(x=data.index, 
                         y=data['Close'], 
                         name='Close Price'))
figure.add_trace(go.Scatter(x=data.index, 
                         y=data['momentum'], 
                         name='Momentum', 
                         yaxis='y2'))
# Adding the buy and sell signals
figure.add_trace(go.Scatter(x=data.loc[data['momentum'] > 0].index, 
                         y=data.loc[data['momentum'] > 0]['Close'], 
                         mode='markers', name='Buy', 
                         marker=dict(color='green', symbol='triangle-up')))

figure.add_trace(go.Scatter(x=data.loc[data['momentum'] < 0].index, 
                         y=data.loc[data['momentum'] < 0]['Close'], 
                         mode='markers', name='Sell', 
                         marker=dict(color='red', symbol='triangle-down')))

figure.update_layout(title='Algorithmic Trading using Momentum Strategy',
                  xaxis_title='Date',
                  yaxis_title='Price')
figure.update_yaxes(title="Momentum", secondary_y=True)
figure.show()
 
