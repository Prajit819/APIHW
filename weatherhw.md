

```python
# Dependencies
import csv
import matplotlib.pyplot as plt
import requests as req
import pandas as pd
```


```python
api_key = "25bc90a1196e6f153eece0bc0b0fc9eb"
units = "imperial"

```


```python
# Import the census data into a pandas DataFrame

cities = pd.read_csv("worldcities.csv")

# Preview the data
cities.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>City</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ad</td>
      <td>andorra la vella</td>
      <td>42.500000</td>
      <td>1.516667</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ad</td>
      <td>canillo</td>
      <td>42.566667</td>
      <td>1.600000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ad</td>
      <td>encamp</td>
      <td>42.533333</td>
      <td>1.583333</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ad</td>
      <td>la massana</td>
      <td>42.550000</td>
      <td>1.516667</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ad</td>
      <td>les escaldes</td>
      <td>42.500000</td>
      <td>1.533333</td>
    </tr>
  </tbody>
</table>
</div>




```python
#take a sample of cities 
selected_cities = cities.sample(n=500)
selected_cities.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>City</th>
      <th>Latitude</th>
      <th>Longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>37617</th>
      <td>ru</td>
      <td>peskovka</td>
      <td>59.044781</td>
      <td>52.360571</td>
    </tr>
    <tr>
      <th>32182</th>
      <td>ro</td>
      <td>baia</td>
      <td>44.716667</td>
      <td>28.666667</td>
    </tr>
    <tr>
      <th>15646</th>
      <td>hu</td>
      <td>janossomorja</td>
      <td>47.786207</td>
      <td>17.136033</td>
    </tr>
    <tr>
      <th>17346</th>
      <td>in</td>
      <td>burla</td>
      <td>21.500000</td>
      <td>83.866667</td>
    </tr>
    <tr>
      <th>5631</th>
      <td>cl</td>
      <td>bulnes</td>
      <td>-36.733333</td>
      <td>-72.300000</td>
    </tr>
  </tbody>
</table>
</div>




```python
selected_cities["Temperature"] = ""
selected_cities["Humidity"] = ""
selected_cities["Cloudiness"] = ""
selected_cities["Wind Speed"] = ""
row_count = 0

for index, row in selected_cities.iterrows():
    
    # Create endpoint URL
    target_url = "http://api.openweathermap.org/data/2.5/weather?appid=%s&q=%s" % (api_key, row["City"])
    
    #THIS IS WHERE THE ERROR IS, SOMETHING WITH THE URL IS WRONG
    
    # Print log to ensure loop is working correctly
    #print("Now retrieving city # " + str(row_count))
    #print(target_url)
    #row_count += 1
    
    w_city = req.get(target_url).json()
    
    try: 
        city_temp = w_city["main"]["temp"]
        city_hum = w_city["main"]["humidity"]
        city_clouds = w_city["clouds"]["all"]
        city_wind = w_city["wind"]["speed"]
        
        selected_cities.set_value(index, "Temperature", city_temp)
        selected_cities.set_value(index, "Humidity", city_hum)
        selected_cities.set_value(index, "Cloudiness", city_clouds)
        selected_cities.set_value(index, "Wind Speed", city_wind)
    except:
        continue
selected_cities.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>City</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature</th>
      <th>Humidity</th>
      <th>Cloudiness</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>37617</th>
      <td>ru</td>
      <td>peskovka</td>
      <td>59.044781</td>
      <td>52.360571</td>
      <td>264.913</td>
      <td>78</td>
      <td>64</td>
      <td>1.28</td>
    </tr>
    <tr>
      <th>32182</th>
      <td>ro</td>
      <td>baia</td>
      <td>44.716667</td>
      <td>28.666667</td>
      <td>282.15</td>
      <td>76</td>
      <td>0</td>
      <td>4.1</td>
    </tr>
    <tr>
      <th>15646</th>
      <td>hu</td>
      <td>janossomorja</td>
      <td>47.786207</td>
      <td>17.136033</td>
      <td>284.88</td>
      <td>76</td>
      <td>0</td>
      <td>2.1</td>
    </tr>
    <tr>
      <th>17346</th>
      <td>in</td>
      <td>burla</td>
      <td>21.500000</td>
      <td>83.866667</td>
      <td>299.013</td>
      <td>83</td>
      <td>0</td>
      <td>1.83</td>
    </tr>
    <tr>
      <th>5631</th>
      <td>cl</td>
      <td>bulnes</td>
      <td>-36.733333</td>
      <td>-72.300000</td>
      <td>287</td>
      <td>72</td>
      <td>0</td>
      <td>4.1</td>
    </tr>
  </tbody>
</table>
</div>




```python
selected_cities.to_csv("citytemp_data.csv", encoding="utf-8", index=False)
```


```python
selected_cities['Temperature'] = pd.to_numeric(selected_cities['Temperature'])
selected_cities['Humidity'] = pd.to_numeric(selected_cities['Humidity'])
selected_cities.plot(kind="scatter", x="Latitude", y="Temperature", grid=True, figsize=(20,10),
              title="Latitude v. Temperature")
plt.show()

```


![png](output_6_0.png)



```python
selected_cities['Cloudiness'] = pd.to_numeric(selected_cities['Cloudiness'])
selected_cities.plot(kind="scatter", x="Latitude", y="Cloudiness", grid=True, figsize=(20,10),
              title="Latitude v. Cloudiness")
plt.show()

```


![png](output_7_0.png)



```python
selected_cities['Wind Speed'] = pd.to_numeric(selected_cities['Wind Speed'])
selected_cities.plot(kind="scatter", x="Latitude", y="Wind Speed", grid=True, figsize=(20,10),
              title="Latitude v. Wind Speed")
plt.show()

```


![png](output_8_0.png)



```python
selected_cities.plot(kind="scatter", x="Latitude", y="Humidity", grid=True, figsize=(20,10),
              title="Latitude v. Humidity")
plt.show()
```


![png](output_9_0.png)

