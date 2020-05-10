# SoccerStocks
A personal study of how publicly traded soccer teams' stock values are affected by major wins or losses. Graphs made in python with matplotlib, pandas, and numpy.
# I downloaded the CSV of th stock price from yahoo finance, and made my own excel of dates vs 0/1 for win/loss
#Below is one example that is Manchester United
import matplotlib.pyplot as plt
from pandas_datareader import data
import pandas as pd
import numpy as np
import yfinance as yahoo
from matplotlib import pyplot as plt
import tkinter as tk
from tkinter import filedialog, Text
import xlrd
from matplotlib.offsetbox import AnchoredText
import matplotlib.ticker as plticker

SHEET1 = r'C:\Users\Leonardo\Documents\MANU.csv'
SHEET2 = r'C:\Users\Leonardo\Documents\MANU Games.xlsx'
df = pd.read_csv(SHEET1)
dfw = pd.read_excel(SHEET2, 'Win')
dfd = pd.read_excel(SHEET2, 'Draw')
dfl = pd.read_excel(SHEET2, 'Loss')
ticker1 = 'MANU'
start_date = '2017-07-01'
end_date = '2018-08-01'
# Boomsblerg formatting
plt.style.use(['dark_background'])
# plt.rcParams['axes.facecolor'] = 'navy'

# Define Variables for stock
d = df['Date']
c = df['Adj Close']
h = df['High']

vol = df['Volume']
vol3 = (vol/20000)
# Season win/loss variables
win_date = dfw['Game Date']
win_height = dfw['Height']
draw_date = dfd['Game Date']
draw_height = dfd['Height']
loss_date = dfl['Game Date']
loss_height = dfl['Height']

# Percent Change Box
pc = c.pct_change()
bc = pc.cumsum()
g = (bc.iloc[-1])
j = (g*100)
pchange = round(j, 1)
plt.text(.85, .72, "Percent Change: {}%".format(pchange),
         bbox={'facecolor': 'k', 'pad': 5, },
         ha="left", va="bottom", color='limegreen' if pchange >= 0 else 'r',
         transform=plt.gca().transAxes)
plt.text(.22, .65, 'Champions League Qualification\nand Premier League win streak',
         bbox={'facecolor': 'k', 'pad': 5, },
         ha="left", va="bottom", color='limegreen',
         transform=plt.gca().transAxes)
plt.text(.52, .70, 'Decisive losing streak,\nBoth Champions and Premeir League',
         bbox={'facecolor': 'k', 'pad': 5, },
         ha="left", va="bottom", color='r',
         transform=plt.gca().transAxes)
# Plotting
plt.bar(draw_date, draw_height, color='y', label='Draw')
plt.bar(loss_date, loss_height, color='r', label='Decisive Loss')
plt.bar(win_date, win_height, color='g', label='Decisive Victory')
plt.plot(draw_date, c, label='Close Price', color='pink')
plt.plot(draw_date, vol3, label="Trading Volume (Two Thousands)",
         color='aqua')
plt.legend()
plt.ylabel('Share Value ($)')
plt.title('Manchester United 2017-2018 Season', fontname='bold')

plt.show()
