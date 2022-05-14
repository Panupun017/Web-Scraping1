# Web-Scraping1

# Install Library
pip install bs4
pip install requests
pip install pandas
pip install openpyxl

# Import Library
import pandas as pd
import bs4
import requests
import openpyxl

#Scraping
data = requests.get('https://www.khaosod.co.th/covid-19')
soup = bs4.BeautifulSoup(data.text)
image_list = []
news_list = []
time_list = []
for c in soup.find_all('div',{'udblock udblock--top_down'}):
    image_list.append(c.find('img')['data-src'])
    news_list.append(c.find('h3',{'class':'udblock__title'}).find('a').text)
    time_list.append(c.find('span').text)
table = pd.DataFrame([news_list,image_list,time_list]).transpose()
table.columns = ['news','image','time']
table.set_index('news')

# Extract Excel File
table.to_excel('News_Covid19.xlsx',engine='openpyxl')
