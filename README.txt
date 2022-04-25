import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from pandas.api.types import is_string_dtype
from pandas.api.types import is_numeric_dtype
pd.set_option('precision', 3)

dataset = pd.read_html("https://en.wikipedia.org/wiki/List_of_countries_by_carbon_dioxide_emissions")
countries = dataset[1] #Dataset: This article needs to be updated verwijderen

countries.columns = ['country', 'y1990', 'y2005', 'y2017', '2017%_of_world', '2017vs1990_change%', '2017pla', '2017pc', '2018incl_LUCF', '2018excl_LUCF']
countries = countries.loc[3:] #eerste drie regels verwijderen
countries = countries.drop(67) #Europese Unie verwijderen #inplace+True?

top5countries = countries.sort_values(['y1990','y2005','y2017'], ascending = False).head(5)
display(top5countries)

#Graph 1: CO2 of the bigger countries 5 biggest CO2 producers
fig, ax = plt.subplots()

years = ['y1990', 'y2005', 'y2017'] 

for index, row in top5countries.iterrows():
  plt.plot(years, row[1:4], label=row[0])  

plt.title('The 5 biggest CO2 producers in the world')
plt.xlabel('Years')
plt.ylabel('Emissions')
plt.grid()
plt.legend(loc='best', bbox_to_anchor=(1, 0.5))
plt.show()

#Graph 2: worst and best changers
#top5countries = countries.sort_values(['1990','2005','2017'], ascending = False).head(5)
#display(top5countries)

%precision 2
pd.set_option('precision', 2)

#changerslist = countries.sort_values('2017vs1990_change%')
#display(changerslist)
#top3changers = changerslist.head(3)
#bottom3changers = changerslist.tail(3)
#display(top3changers)
#display(bottom3changers)

change90_05 = round(countries.y1990 / countries.y2005 * 100, 2)
change05_17 = round(countries.y2005 / countries.y2017 * 100, 2)

countries['change90_05'] = change90_05
countries['change05_17'] = change05_17
#display(countries)

top3changers = countries.sort_values(['change90_05', 'change90_05'], ascending = False).head(3)
bottom3changers = countries.sort_values(['change90_05', 'change90_05'], ascending = False).tail(3)

display(top3changers)
display(bottom3changers)

fig, ax = plt.subplots()
years = ['y1990', 'y2005', 'y2017'] 

#plot 1
for index, row in top3changers.iterrows():
  plt.plot(years, row[1:4], 'o-', label=row[0])  

plt.title('Top 3 BEST changers in the world')
plt.xlabel('Years')
plt.ylabel('CO2 emitted')
plt.grid()
plt.legend(loc='best', bbox_to_anchor=(1, 0.5))
plt.show()

#plot 2
for index, row in bottom3changers.iterrows():
  plt.plot(years, row[1:4],'o-', label=row[0])  

plt.title('Top 3 BOTTOM changers in the world')
plt.xlabel('Years')
plt.ylabel('CO2 emitted')
plt.grid()
plt.legend(loc='best', bbox_to_anchor=(1, 0.5))
plt.show()
