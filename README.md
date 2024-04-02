import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Load historical stock data
data = pd.read_csv('historical_data.csv')

# Calculate moving averages
data['MA50'] = data['Close'].rolling(window=50).mean()
data['MA200'] = data['Close'].rolling(window=200).mean()

# Generate signals based on moving average crossover
data['Signal'] = np.where(data['MA50'] > data['MA200'], 1.0, 0.0)
data['Position'] = data['Signal'].diff()

# Plotting
plt.figure(figsize=(10,5))
plt.plot(data['Close'], label='Close Price')
plt.plot(data['MA50'], label='50-day MA')
plt.plot(data['MA200'], label='200-day MA')
plt.plot(data[data['Position'] == 1.0].index, 
         data['MA50'][data['Position'] == 1.0], 
         '^', markersize=10, color='g', lw=0, label='Buy Signal')
plt.plot(data[data['Position'] == -1.0].index, 
         data['MA50'][data['Position'] == -1.0], 
         'v', markersize=10, color='r', lw=0, label='Sell Signal')
plt.title('Moving Average Crossover Strategy')
plt.legend()
plt.show()
