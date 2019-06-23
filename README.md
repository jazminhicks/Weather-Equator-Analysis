# Weather-Py

Create a Python script to visualize the weather of 500+ cities across the world of varying distance from the equator. 

To accomplish this, utilize https://pypi.python.org/pypi/citipy) and https://openweathermap.org/api)

Objective is to build a series of scatter plots to showcase the following relationships:

* Temperature (F) vs. Latitude
* Humidity (%) vs. Latitude
* Cloudiness (%) vs. Latitude
* Wind Speed (mph) vs. Latitude

The notebook includes:

* At least 500 randomly selected unique (non-repeat) cities based on latitude and longitude.
* A weather check on each of the cities using a series of successive API calls.
* A print log of each city as it's being processed with the city number and city name.
* Both a CSV of all data retrieved and png images for each scatter plot.
____________________________________________________________


#### Observable Trends

* Differing from expectations, the hottest cities are not necessarily those on or closest to the equator (latitude of zero degrees). Rather, the highest temperatures are from cities with latitudes around 20 degrees. This result may be combination of the earth’s tilt and varying weather patterns throughout the tropics. 

* The data does not present any correlation between a city’s humidity and latitude nor cloudiness and latitude. On both graphs the data points are distributed fairly evenly and there is no distinctive pattern.

* Additionally, there is not any correlation between a city’s average wind speed and latitude. However, the data does reveals that wind speeds in most cities typically do not exceed 20 mph. 

## List of Cities (from randomly generated latitudes and longitudes)

```
# Create a set of random lat and lng combinations
# use citipy to identify the city,country that matches the provided coordinates

lats = np.random.uniform(-90.000, 90.000, size=2000)
lngs = np.random.uniform(-180.000, 180.000, size=2000)

lat_lng = list(zip(lats, lngs))

cities = []
countries = []

for coordinate in lat_lng:
    
    city = citipy.nearest_city(coordinate[0], coordinate[1]).city_name
    code = citipy.nearest_city(coordinate[0], coordinate[1]).country_code
    if city not in cities:
        cities.append(city)
        countries.append(code)

city_country = list(zip(cities, countries))

len(city_country)
```