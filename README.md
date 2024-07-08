# Currency Exchange Rates Scraper

This repository contains a Python script that scrapes currency exchange rates from the Bank of Taiwan website. The script uses the BeautifulSoup library to parse HTML content and extract specific data on currency rates, which is then stored in a pandas DataFrame.

## Sample Code

```python
import requests
from bs4 import BeautifulSoup
import pandas as pd


url = 'https://rate.bot.com.tw/xrt?Lang=zh-TW'

r = requests.get(url)

# Parse the HTML content using BeautifulSoup with html5lib parser
soup = BeautifulSoup(r.text, 'html5lib')

# Find all the currency names
currency = soup.find_all('div',class_="visible-phone print_hide")

# Find all the cash buy rates
cash_buy_rate = soup.find_all('td', class_="rate-content-cash text-right print_hide",attrs={'data-table':'本行現金買入'})

# Find all the cash sell rates
cash_sell_rate = soup.find_all('td', class_='rate-content-cash text-right print_hide', attrs={'data-table': '本行現金賣出'})

# Find all the sight buy rates
# Updated selector to correctly target sight buy rates
sight_buy_rate = soup.find_all('td', class_='rate-content-sight text-right print_hide', attrs={'data-table':'本行即期買入'})

# Find all the sight sell rates
# Updated selector to correctly target sight sell rates
sight_sell_rate = soup.find_all('td', class_='rate-content-sight text-right print_hide', attrs={'data-table':'本行即期賣出'})

# Initialize an empty list to store the data
list=[]

for i in range(len(currency)):
#strip any whitespace characters in currency_names, cash_buy, cash_sell, sight_buy, and sight_sell
    currency_name = currency[i].text.strip()
    cash_buy = cash_buy_rate[i].text.strip()
    cash_sell = cash_sell_rate[i].text.strip()
    sight_buy = sight_buy_rate[i].text.strip()
    sight_sell = sight_sell_rate[i].text.strip()
    
    # Append the extracted data to the list
    list.append([currency_name, cash_buy, cash_sell, sight_buy, sight_sell])

# Create a DataFrame from the list with the specified column names
df = pd.DataFrame(list, columns=['幣別', '現金買入', '現金賣出', '即期買入', '即期賣出'])

print(df)
