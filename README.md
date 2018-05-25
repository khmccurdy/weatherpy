# "WeatherPy" Project
## Latitude vs. Weather
Results and Summary dashboard: https://khmccurdy.github.io/weatherpy/

The purpose of this project was to analyze weather patterns with respect to latitude. To accomplish this, we used the OpenWeatherMap API to collect information on weather conditions for over 500 randomly selected cities.

After assembling the dataset, we used Matplotlib to plot multiple aspects of the weather with respect to latitude. These factors were temperature, cloud cover, wind speed, and humidity. The [associated dashboard](https://khmccurdy.github.io/weatherpy/) provides the source data and visualizations created for this analysis, as well as descriptions of trends observed.

```python
import json
import openweathermapy.core as ow
from citipy import citipy
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
from config import apikey
import random
import requests
import urllib
import time
import os
```


```python
def city_string(lat,long):
    c = citipy.nearest_city(lat,long)
    return c.city_name+","+c.country_code
```


```python
outdir = "Resources"
jpath = os.path.join(outdir, "results.json")
csvpath = os.path.join(outdir, "cities.csv")
def imagepath(name):
    return os.path.join(outdir, f"{name}.png")
```


```python
# Only run this once!
i=0
#numCities=10
numCities=600
cities = []
results = []

# Loop through random coordinates, until we have enough (600) unique cities
# that return data from OWM. In the event that the city doesn't match, 
# print the result but don't count it. Add all extracted city data to a list.
while i < numCities:
    lat = random.random()*180-90
    lng = random.random()*360-180
    query=city_string(lat,lng)
    
    if query not in cities:
        cities.append(query)
        time.sleep(0.1)
        try:
            res = ow.get_current(query, units="imperial", appid = apikey)
            print(f"{i}: Gathered data for {res['name']}")
            print(f"{ow.BASE_URL}weather?q={query}&units=imperial&appid=(get your own)")
            i+=1
            results.append(res)
        except urllib.error.HTTPError as err:
            if err.code == 404:
                print(f"[{i}.x]: No data for {query}")
                print(f"{ow.BASE_URL}weather?q={query}&units=imperial&appid=(get your own)")
            elif err.code == 401:
                print("api code, man.")
                break
            else:
                raise

print(f"{numCities} out of {len(cities)} matched")
```

    0: Gathered data for Avarua
    http://api.openweathermap.org/data/2.5/weather?q=avarua,ck&units=imperial&appid=(get your own)
    1: Gathered data for Yarada
    http://api.openweathermap.org/data/2.5/weather?q=yarada,in&units=imperial&appid=(get your own)
    2: Gathered data for Mahebourg
    http://api.openweathermap.org/data/2.5/weather?q=mahebourg,mu&units=imperial&appid=(get your own)
    [3.x]: No data for taolanaro,mg
    http://api.openweathermap.org/data/2.5/weather?q=taolanaro,mg&units=imperial&appid=(get your own)
    3: Gathered data for Atuona
    http://api.openweathermap.org/data/2.5/weather?q=atuona,pf&units=imperial&appid=(get your own)
    4: Gathered data for Cabo San Lucas
    http://api.openweathermap.org/data/2.5/weather?q=cabo san lucas,mx&units=imperial&appid=(get your own)
    [5.x]: No data for airai,pw
    http://api.openweathermap.org/data/2.5/weather?q=airai,pw&units=imperial&appid=(get your own)
    5: Gathered data for Kabinda
    http://api.openweathermap.org/data/2.5/weather?q=kabinda,cd&units=imperial&appid=(get your own)
    6: Gathered data for Mahmudabad
    http://api.openweathermap.org/data/2.5/weather?q=mahmudabad,in&units=imperial&appid=(get your own)
    7: Gathered data for Inta
    http://api.openweathermap.org/data/2.5/weather?q=inta,ru&units=imperial&appid=(get your own)
    8: Gathered data for Hay River
    http://api.openweathermap.org/data/2.5/weather?q=hay river,ca&units=imperial&appid=(get your own)
    9: Gathered data for Lewisporte
    http://api.openweathermap.org/data/2.5/weather?q=lewisporte,ca&units=imperial&appid=(get your own)
    10: Gathered data for Bichena
    http://api.openweathermap.org/data/2.5/weather?q=bichena,et&units=imperial&appid=(get your own)
    11: Gathered data for Falun
    http://api.openweathermap.org/data/2.5/weather?q=falun,se&units=imperial&appid=(get your own)
    12: Gathered data for Hithadhoo
    http://api.openweathermap.org/data/2.5/weather?q=hithadhoo,mv&units=imperial&appid=(get your own)
    13: Gathered data for Chokurdakh
    http://api.openweathermap.org/data/2.5/weather?q=chokurdakh,ru&units=imperial&appid=(get your own)
    14: Gathered data for Kapaa
    http://api.openweathermap.org/data/2.5/weather?q=kapaa,us&units=imperial&appid=(get your own)
    15: Gathered data for Bandundu
    http://api.openweathermap.org/data/2.5/weather?q=bandundu,cd&units=imperial&appid=(get your own)
    [16.x]: No data for tingrela,ci
    http://api.openweathermap.org/data/2.5/weather?q=tingrela,ci&units=imperial&appid=(get your own)
    16: Gathered data for Bambous Virieux
    http://api.openweathermap.org/data/2.5/weather?q=bambous virieux,mu&units=imperial&appid=(get your own)
    17: Gathered data for Carnarvon
    http://api.openweathermap.org/data/2.5/weather?q=carnarvon,au&units=imperial&appid=(get your own)
    [18.x]: No data for mocambique,mz
    http://api.openweathermap.org/data/2.5/weather?q=mocambique,mz&units=imperial&appid=(get your own)
    [18.x]: No data for zhanatas,kz
    http://api.openweathermap.org/data/2.5/weather?q=zhanatas,kz&units=imperial&appid=(get your own)
    18: Gathered data for Novomalorossiyskaya
    http://api.openweathermap.org/data/2.5/weather?q=novomalorossiyskaya,ru&units=imperial&appid=(get your own)
    19: Gathered data for Rancho Palos Verdes
    http://api.openweathermap.org/data/2.5/weather?q=rancho palos verdes,us&units=imperial&appid=(get your own)
    20: Gathered data for Saldanha
    http://api.openweathermap.org/data/2.5/weather?q=saldanha,za&units=imperial&appid=(get your own)
	...
	... [Output condensed for obvious reasons - all 600 logs are available 
	...  in the associated .ipynb file if you really want to see all of them.]
	...
    590: Gathered data for Sao Pedro do Sul
    http://api.openweathermap.org/data/2.5/weather?q=sao pedro do sul,br&units=imperial&appid=(get your own)
    591: Gathered data for Kamenka
    http://api.openweathermap.org/data/2.5/weather?q=kamenka,ru&units=imperial&appid=(get your own)
    592: Gathered data for Port Hedland
    http://api.openweathermap.org/data/2.5/weather?q=port hedland,au&units=imperial&appid=(get your own)
    593: Gathered data for Broken Hill
    http://api.openweathermap.org/data/2.5/weather?q=broken hill,au&units=imperial&appid=(get your own)
    594: Gathered data for Ystad
    http://api.openweathermap.org/data/2.5/weather?q=ystad,se&units=imperial&appid=(get your own)
    595: Gathered data for Irtyshskiy
    http://api.openweathermap.org/data/2.5/weather?q=irtyshskiy,ru&units=imperial&appid=(get your own)
    596: Gathered data for Kormilovka
    http://api.openweathermap.org/data/2.5/weather?q=kormilovka,ru&units=imperial&appid=(get your own)
    597: Gathered data for Limenaria
    http://api.openweathermap.org/data/2.5/weather?q=limenaria,gr&units=imperial&appid=(get your own)
    598: Gathered data for Te Anau
    http://api.openweathermap.org/data/2.5/weather?q=te anau,nz&units=imperial&appid=(get your own)
    599: Gathered data for Veydelevka
    http://api.openweathermap.org/data/2.5/weather?q=veydelevka,ru&units=imperial&appid=(get your own)
    600 out of 684 matched
    


```python
#json.dump(results, jpath)
with open(jpath,'w') as jfile:
    jfile.write(json.dumps(results,indent=2,sort_keys=True))
```


```python
#results = json.load(jpath)
```


```python
# Create Pandas table from the relevant datapoints
cities_df=pd.DataFrame({"City":[x["name"]+", "+x["sys"]["country"] for x in results],
             "Latitude": [x["coord"]["lat"] for x in results],
             "Longitude": [x["coord"]["lon"] for x in results],
             "Temperature (F)": [x["main"]["temp"] for x in results],
             "Humidity (%)": [x["main"]["humidity"] for x in results],
             "Wind Speed (mph)": [x["wind"]["speed"] for x in results],
             "Cloudiness (%)": [x["clouds"]["all"] for x in results]
             })
cities_df.head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness (%)</th>
      <th>Humidity (%)</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Avarua, CK</td>
      <td>75</td>
      <td>74</td>
      <td>-21.21</td>
      <td>-159.78</td>
      <td>84.20</td>
      <td>8.05</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Yarada, IN</td>
      <td>8</td>
      <td>100</td>
      <td>17.65</td>
      <td>83.27</td>
      <td>81.18</td>
      <td>4.97</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Mahebourg, MU</td>
      <td>40</td>
      <td>88</td>
      <td>-20.41</td>
      <td>57.70</td>
      <td>78.80</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Atuona, PF</td>
      <td>80</td>
      <td>99</td>
      <td>-9.80</td>
      <td>-139.03</td>
      <td>82.17</td>
      <td>19.51</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cabo San Lucas, MX</td>
      <td>5</td>
      <td>64</td>
      <td>22.89</td>
      <td>-109.91</td>
      <td>75.45</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Kabinda, CD</td>
      <td>80</td>
      <td>94</td>
      <td>-6.14</td>
      <td>24.49</td>
      <td>67.05</td>
      <td>2.84</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mahmudabad, IN</td>
      <td>0</td>
      <td>76</td>
      <td>27.30</td>
      <td>81.13</td>
      <td>55.40</td>
      <td>4.41</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Inta, RU</td>
      <td>48</td>
      <td>63</td>
      <td>66.04</td>
      <td>60.13</td>
      <td>-8.29</td>
      <td>2.73</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Hay River, CA</td>
      <td>40</td>
      <td>51</td>
      <td>60.82</td>
      <td>-115.79</td>
      <td>35.60</td>
      <td>11.41</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Lewisporte, CA</td>
      <td>90</td>
      <td>86</td>
      <td>49.25</td>
      <td>-55.06</td>
      <td>28.40</td>
      <td>26.40</td>
    </tr>
  </tbody>
</table>
</div>




```python
cities_df.to_csv(csvpath)
#cities_df = pd.read_csv(csvpath)
```

### Temperature (F) vs. Latitude


```python
plt.scatter(cities_df["Latitude"],cities_df["Temperature (F)"],s=10)
plt.title("Temperature (F) vs. Latitude", fontsize=14)
plt.xlabel("Latitude (degrees)",fontsize=12)
plt.ylabel("Temperature (F)",fontsize=12)
#plt.vlines(0,-50,150,colors="r",)
plt.axvline(x=0,c="r",alpha=0.6)
plt.xlim(-90,90)
plt.grid(alpha=0.3)
plt.savefig(imagepath("temperature"))
plt.show()
```


![png](WeatherPy_files/WeatherPy_9_0.png)


### Humidity (%) vs. Latitude


```python
plt.scatter(cities_df["Latitude"],cities_df["Humidity (%)"],s=10)
plt.title("Humidity (%) vs. Latitude", fontsize=14)
plt.xlabel("Latitude (degrees)",fontsize=12)
plt.ylabel("Humidity (%)",fontsize=12)
#plt.vlines(0,-50,150,colors="r",)
plt.axvline(x=0,c="r",alpha=0.6)
plt.axhline(100, c="k",alpha=0.2,lw=1)
plt.axhline(0, c="k",alpha=0.2,lw=1)
plt.xlim(-90,90)
plt.grid(alpha=0.3)
plt.savefig(imagepath("humidity"))
plt.show()
```


![png](WeatherPy_files/WeatherPy_11_0.png)


### Cloudiness (%) vs. Latitude


```python
plt.scatter(cities_df["Latitude"],cities_df["Cloudiness (%)"],s=10)
plt.title("Cloudiness (%) vs. Latitude", fontsize=14)
plt.xlabel("Latitude (degrees)",fontsize=12)
plt.ylabel("Cloudiness (%)",fontsize=12)
#plt.vlines(0,-50,150,colors="r",)
plt.axvline(x=0,c="r",alpha=0.6)
plt.axhline(100, c="k",alpha=0.2,lw=1)
plt.axhline(0, c="k",alpha=0.2,lw=1)
plt.xlim(-90,90)
plt.ylim(-5,105)
plt.grid(alpha=0.3)
plt.savefig(imagepath("clouds"))
plt.show()
```


![png](WeatherPy_files/WeatherPy_13_0.png)


### Wind Speed (mph) vs. Latitude


```python
plt.scatter(cities_df["Latitude"],cities_df["Wind Speed (mph)"],s=10)
plt.title("Wind Speed (mph) vs. Latitude", fontsize=14)
plt.xlabel("Latitude (degrees)",fontsize=12)
plt.ylabel("Wind Speed (mph)",fontsize=12)
#plt.vlines(0,-50,150,colors="r",)
plt.axvline(x=0,c="r",alpha=0.6)
plt.axhline(0, c="k",alpha=0.2,lw=1)
plt.xlim(-90,90)
plt.grid(alpha=0.3)
plt.savefig(imagepath("wind"))
plt.show()
```


![png](WeatherPy_files/WeatherPy_15_0.png)


### Three Observations

1) There is a strong correlation between latitude and temperature, with temperature decreasing as distance from the equator increases.

2) There are fairly weak correlations with humidity and wind speed, with the equator having somewhat lower winds and higher humidity than more temperate regions.

3) There is almost no discernible correlation between latitude and cloud cover.
