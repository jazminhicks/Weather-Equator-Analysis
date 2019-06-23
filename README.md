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

## Perform API Calls (for each city)
```
url = "http://api.openweathermap.org/data/2.5/weather?"

unit = "imperial" 

query_url = f"{url}appid={api_key}&units={unit}&q="
```
```
print ("Beginning data retrieval...")
print ("----------------------------")

# establish lists to store the specified data for each city from the api
found_cities = []
found_countries = []
found_lats = []
found_lngs = []
dates = []
cloudiness = []
humidity = []
max_temp = []
wind_speed = []

count = 0

for place in city_country:
    
    query = place[0].replace(" ", "+") + "," + place[1].replace(" ", "+")
    
    city_data = requests.get(query_url + query).json()
    
    if city_data["cod"] == "404":
        print(f"Error: '{place[0]}' was not found...skippping")
    

    else:
        count += 1
        print (f"{count}. Processing data for {place[0]}") # print log of each city that was found and is being processed
        print (query_url + query)
        found_cities.append(place[0])
        found_countries.append(place[1])
        dates.append(city_data["dt"])
        cloudiness.append(city_data["clouds"]["all"])
        humidity.append(city_data["main"]["humidity"])
        max_temp.append(city_data["main"]["temp_max"])
        wind_speed.append(city_data["wind"]["speed"])
        found_lats.append(city_data["coord"]["lat"])
        found_lngs.append(city_data["coord"]["lon"])

print ("--------------------------")
print ("Data retrieval complete")
print ("--------------------------")
```

```
Beginning data retrieval...
----------------------------
1. Processing data for cabo san lucas
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cabo+san+lucas,mx
2. Processing data for hermanus
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hermanus,za
3. Processing data for kenora
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kenora,ca
4. Processing data for punta arenas
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=punta+arenas,cl
5. Processing data for beckley
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=beckley,us
Error: 'mataura' was not found...skippping
6. Processing data for new norfolk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=new+norfolk,au
7. Processing data for te anau
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=te+anau,nz
8. Processing data for guiren
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=guiren,cn
9. Processing data for broome
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=broome,au
10. Processing data for castro
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=castro,cl
Error: 'attawapiskat' was not found...skippping
11. Processing data for ushuaia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ushuaia,ar
12. Processing data for swan river
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=swan+river,ca
13. Processing data for nome
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nome,us
14. Processing data for balkhash
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=balkhash,kz
15. Processing data for east london
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=east+london,za
Error: 'tambul' was not found...skippping
16. Processing data for kidal
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kidal,ml
17. Processing data for katsuura
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=katsuura,jp
18. Processing data for sitka
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sitka,us
19. Processing data for pathein
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pathein,mm
20. Processing data for acapulco
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=acapulco,mx
21. Processing data for yellowknife
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yellowknife,ca
22. Processing data for port augusta
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port+augusta,au
23. Processing data for kapaa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kapaa,us
24. Processing data for hamilton
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hamilton,bm
25. Processing data for rikitea
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=rikitea,pf
26. Processing data for namibe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=namibe,ao
27. Processing data for ostrovnoy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ostrovnoy,ru
Error: 'asfi' was not found...skippping
28. Processing data for albertville
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=albertville,us
29. Processing data for trairi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=trairi,br
30. Processing data for tierralta
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tierralta,co
Error: 'tsihombe' was not found...skippping
31. Processing data for aklavik
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=aklavik,ca
32. Processing data for suhbaatar
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=suhbaatar,mn
33. Processing data for lloydminster
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lloydminster,ca
Error: 'louisbourg' was not found...skippping
34. Processing data for bethel
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bethel,us
35. Processing data for tasiilaq
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tasiilaq,gl
36. Processing data for kruisfontein
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kruisfontein,za
Error: 'benha' was not found...skippping
37. Processing data for jamestown
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=jamestown,sh
38. Processing data for hobart
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hobart,au
39. Processing data for chokurdakh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=chokurdakh,ru
40. Processing data for ostashkov
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ostashkov,ru
41. Processing data for hualmay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hualmay,pe
42. Processing data for norman wells
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=norman+wells,ca
43. Processing data for dikson
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dikson,ru
44. Processing data for upernavik
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=upernavik,gl
45. Processing data for teya
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=teya,ru
Error: 'kuche' was not found...skippping
46. Processing data for muroto
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=muroto,jp
47. Processing data for kerman
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kerman,us
48. Processing data for alofi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=alofi,nu
Error: 'kushmurun' was not found...skippping
49. Processing data for faanui
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=faanui,pf
Error: 'amderma' was not found...skippping
50. Processing data for qaanaaq
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=qaanaaq,gl
51. Processing data for bredasdorp
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bredasdorp,za
52. Processing data for college
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=college,us
53. Processing data for mizdah
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mizdah,ly
54. Processing data for kavieng
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kavieng,pg
55. Processing data for havoysund
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=havoysund,no
56. Processing data for lorengau
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lorengau,pg
57. Processing data for abbeville
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=abbeville,us
58. Processing data for san cristobal
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san+cristobal,ec
59. Processing data for busselton
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=busselton,au
60. Processing data for bloemfontein
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bloemfontein,za
61. Processing data for tigzirt
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tigzirt,dz
62. Processing data for cherskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cherskiy,ru
63. Processing data for souillac
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=souillac,mu
64. Processing data for georgetown
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=georgetown,sh
65. Processing data for yumen
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yumen,cn
66. Processing data for lompoc
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lompoc,us
67. Processing data for cayenne
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cayenne,gf
Error: 'mananara' was not found...skippping
Error: 'laguna' was not found...skippping
68. Processing data for puerto baquerizo moreno
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=puerto+baquerizo+moreno,ec
69. Processing data for avon lake
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=avon+lake,us
70. Processing data for pevek
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pevek,ru
71. Processing data for carnarvon
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=carnarvon,au
72. Processing data for atuona
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=atuona,pf
73. Processing data for puerto ayora
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=puerto+ayora,ec
74. Processing data for vanderhoof
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vanderhoof,ca
75. Processing data for okhotsk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=okhotsk,ru
Error: 'airai' was not found...skippping
Error: 'illoqqortoormiut' was not found...skippping
Error: 'nizhneyansk' was not found...skippping
76. Processing data for hithadhoo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hithadhoo,mv
77. Processing data for ust-nera
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ust-nera,ru
78. Processing data for port elizabeth
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port+elizabeth,za
79. Processing data for avarua
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=avarua,ck
80. Processing data for inirida
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=inirida,co
81. Processing data for narsaq
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=narsaq,gl
82. Processing data for evensk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=evensk,ru
83. Processing data for san patricio
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san+patricio,mx
84. Processing data for roma
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=roma,au
85. Processing data for saskylakh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saskylakh,ru
86. Processing data for sept-iles
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sept-iles,ca
87. Processing data for ende
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ende,id
88. Processing data for alora
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=alora,es
89. Processing data for mar del plata
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mar+del+plata,ar
Error: 'taolanaro' was not found...skippping
90. Processing data for leh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=leh,in
91. Processing data for nikolskoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nikolskoye,ru
Error: 'olafsvik' was not found...skippping
92. Processing data for puerto escondido
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=puerto+escondido,mx
93. Processing data for pilas
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pilas,es
94. Processing data for barrow
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=barrow,us
95. Processing data for talnakh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=talnakh,ru
Error: 'paradwip' was not found...skippping
Error: 'tarudant' was not found...skippping
96. Processing data for lima
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lima,pe
97. Processing data for poum
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=poum,nc
98. Processing data for norsup
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=norsup,vu
99. Processing data for saldanha
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saldanha,za
100. Processing data for salalah
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=salalah,om
101. Processing data for guerrero negro
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=guerrero+negro,mx
102. Processing data for filadelfia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=filadelfia,py
103. Processing data for taoudenni
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=taoudenni,ml
104. Processing data for myaundzha
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=myaundzha,ru
Error: 'avera' was not found...skippping
105. Processing data for monrovia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=monrovia,lr
106. Processing data for port alfred
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port+alfred,za
107. Processing data for hofn
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hofn,is
Error: 'vila' was not found...skippping
108. Processing data for nicoya
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nicoya,cr
109. Processing data for auki
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=auki,sb
110. Processing data for bluff
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bluff,nz
Error: 'vaitupu' was not found...skippping
111. Processing data for severnoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=severnoye,ru
112. Processing data for ouallam
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ouallam,ne
113. Processing data for san juan
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san+juan,ar
114. Processing data for champoton
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=champoton,mx
115. Processing data for san pedro de macoris
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san+pedro+de+macoris,do
116. Processing data for soyo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=soyo,ao
Error: 'gat' was not found...skippping
117. Processing data for tabou
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tabou,ci
Error: 'codrington' was not found...skippping
118. Processing data for raymond
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=raymond,ca
119. Processing data for jinchang
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=jinchang,cn
120. Processing data for lebu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lebu,cl
121. Processing data for cidreira
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cidreira,br
122. Processing data for bandarbeyla
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bandarbeyla,so
123. Processing data for wynyard
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=wynyard,ca
124. Processing data for basco
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=basco,ph
125. Processing data for mildura
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mildura,au
126. Processing data for catumbela
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=catumbela,ao
127. Processing data for geraldton
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=geraldton,au
128. Processing data for sinnamary
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sinnamary,gf
129. Processing data for luanda
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=luanda,ao
130. Processing data for abu dhabi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=abu+dhabi,ae
Error: 'grand river south east' was not found...skippping
131. Processing data for great yarmouth
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=great+yarmouth,gb
132. Processing data for vao
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vao,nc
133. Processing data for butaritari
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=butaritari,ki
134. Processing data for mount pleasant
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mount+pleasant,us
135. Processing data for fallon
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=fallon,us
136. Processing data for oxapampa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=oxapampa,pe
137. Processing data for victoria
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=victoria,sc
138. Processing data for tuktoyaktuk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tuktoyaktuk,ca
139. Processing data for bambous virieux
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bambous+virieux,mu
140. Processing data for pisco
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pisco,pe
141. Processing data for sistranda
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sistranda,no
142. Processing data for bonavista
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bonavista,ca
143. Processing data for albany
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=albany,au
144. Processing data for baykit
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=baykit,ru
145. Processing data for beringovskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=beringovskiy,ru
146. Processing data for sao filipe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sao+filipe,cv
147. Processing data for ponta do sol
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ponta+do+sol,cv
Error: 'barentsburg' was not found...skippping
148. Processing data for kirksville
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kirksville,us
149. Processing data for sterling
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sterling,us
150. Processing data for kungurtug
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kungurtug,ru
151. Processing data for torbay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=torbay,ca
152. Processing data for ziro
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ziro,in
153. Processing data for isangel
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=isangel,vu
154. Processing data for tiksi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tiksi,ru
155. Processing data for vestmannaeyjar
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vestmannaeyjar,is
156. Processing data for caiaponia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=caiaponia,br
157. Processing data for nadym
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nadym,ru
158. Processing data for roald
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=roald,no
159. Processing data for nhulunbuy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nhulunbuy,au
160. Processing data for vaini
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vaini,to
161. Processing data for kaitangata
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kaitangata,nz
162. Processing data for nyuksenitsa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nyuksenitsa,ru
163. Processing data for northam
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=northam,au
164. Processing data for majene
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=majene,id
165. Processing data for hilo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hilo,us
166. Processing data for inuvik
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=inuvik,ca
167. Processing data for raudeberg
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=raudeberg,no
168. Processing data for cape town
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cape+town,za
169. Processing data for berlevag
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=berlevag,no
170. Processing data for hastings
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hastings,gb
171. Processing data for san blas
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san+blas,mx
172. Processing data for haines junction
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=haines+junction,ca
173. Processing data for gondanglegi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=gondanglegi,id
174. Processing data for saint-philippe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saint-philippe,re
175. Processing data for vanimo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vanimo,pg
176. Processing data for korhogo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=korhogo,ci
177. Processing data for duku
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=duku,ng
Error: 'tumannyy' was not found...skippping
178. Processing data for christchurch
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=christchurch,nz
179. Processing data for paramonga
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=paramonga,pe
180. Processing data for clyde river
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=clyde+river,ca
181. Processing data for port macquarie
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port+macquarie,au
182. Processing data for half moon bay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=half+moon+bay,us
183. Processing data for porto walter
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=porto+walter,br
184. Processing data for kragujevac
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kragujevac,rs
185. Processing data for burgeo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=burgeo,ca
186. Processing data for mount gambier
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mount+gambier,au
187. Processing data for phalombe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=phalombe,mw
Error: 'balasinor' was not found...skippping
Error: 'karakendzha' was not found...skippping
188. Processing data for armacao de pera
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=armacao+de+pera,pt
189. Processing data for sangar
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sangar,ru
190. Processing data for saint george
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saint+george,bm
191. Processing data for senanga
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=senanga,zm
192. Processing data for baruun-urt
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=baruun-urt,mn
193. Processing data for mahebourg
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mahebourg,mu
194. Processing data for tanout
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tanout,ne
195. Processing data for gizo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=gizo,sb
Error: 'samusu' was not found...skippping
196. Processing data for pacific grove
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pacific+grove,us
197. Processing data for hasaki
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hasaki,jp
198. Processing data for longyearbyen
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=longyearbyen,sj
199. Processing data for moree
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=moree,au
200. Processing data for makakilo city
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=makakilo+city,us
201. Processing data for reinosa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=reinosa,es
202. Processing data for west wendover
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=west+wendover,us
203. Processing data for solwezi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=solwezi,zm
204. Processing data for klaksvik
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=klaksvik,fo
205. Processing data for khatanga
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=khatanga,ru
206. Processing data for pingyin
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pingyin,cn
207. Processing data for kindersley
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kindersley,ca
208. Processing data for saint-pierre
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saint-pierre,pm
209. Processing data for olinda
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=olinda,br
210. Processing data for maceio
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=maceio,br
211. Processing data for campos novos
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=campos+novos,br
212. Processing data for luau
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=luau,ao
213. Processing data for lodja
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lodja,cd
214. Processing data for palmerston
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=palmerston,au
215. Processing data for brownsville
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=brownsville,us
216. Processing data for chuy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=chuy,uy
217. Processing data for grindavik
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=grindavik,is
218. Processing data for pavia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pavia,it
Error: 'mahon' was not found...skippping
219. Processing data for saint-augustin
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saint-augustin,ca
220. Processing data for jacareacanga
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=jacareacanga,br
221. Processing data for ust-charyshskaya pristan
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ust-charyshskaya+pristan,ru
222. Processing data for dongsheng
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dongsheng,cn
223. Processing data for aksarka
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=aksarka,ru
224. Processing data for troitskoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=troitskoye,ru
225. Processing data for messina
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=messina,za
226. Processing data for balabac
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=balabac,ph
227. Processing data for kasongo-lunda
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kasongo-lunda,cd
228. Processing data for nizhniy baskunchak
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nizhniy+baskunchak,ru
229. Processing data for coolum beach
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=coolum+beach,au
230. Processing data for pakaur
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pakaur,in
Error: 'yandoon' was not found...skippping
231. Processing data for mokrousovo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mokrousovo,ru
232. Processing data for tulsa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tulsa,us
233. Processing data for fairbanks
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=fairbanks,us
234. Processing data for mitsamiouli
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mitsamiouli,km
235. Processing data for palu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=palu,id
236. Processing data for vostok
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vostok,ru
237. Processing data for naze
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=naze,jp
238. Processing data for magdagachi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=magdagachi,ru
239. Processing data for kangaatsiaq
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kangaatsiaq,gl
240. Processing data for luderitz
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=luderitz,na
241. Processing data for port hedland
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port+hedland,au
242. Processing data for tura
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tura,ru
243. Processing data for leningradskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=leningradskiy,ru
244. Processing data for ancud
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ancud,cl
245. Processing data for caravelas
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=caravelas,br
246. Processing data for constitucion
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=constitucion,mx
247. Processing data for chitrakonda
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=chitrakonda,in
248. Processing data for saint-joseph
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saint-joseph,re
249. Processing data for grand-santi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=grand-santi,gf
250. Processing data for querecotillo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=querecotillo,pe
251. Processing data for vila velha
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vila+velha,br
252. Processing data for actopan
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=actopan,mx
253. Processing data for esperance
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=esperance,au
254. Processing data for bocaiuva
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bocaiuva,br
255. Processing data for port-cartier
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port-cartier,ca
256. Processing data for yerbogachen
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yerbogachen,ru
257. Processing data for yar-sale
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yar-sale,ru
258. Processing data for portsoy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=portsoy,gb
259. Processing data for margate
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=margate,za
260. Processing data for tuatapere
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tuatapere,nz
261. Processing data for campana
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=campana,ar
262. Processing data for kataysk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kataysk,ru
263. Processing data for yoichi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yoichi,jp
264. Processing data for phitsanulok
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=phitsanulok,th
265. Processing data for takaungu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=takaungu,ke
266. Processing data for najran
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=najran,sa
267. Processing data for namatanai
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=namatanai,pg
268. Processing data for nyeri
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nyeri,ke
269. Processing data for severo-kurilsk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=severo-kurilsk,ru
Error: 'palabuhanratu' was not found...skippping
270. Processing data for maracaju
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=maracaju,br
271. Processing data for toora-khem
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=toora-khem,ru
272. Processing data for cheremukhovo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cheremukhovo,ru
273. Processing data for tazovskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tazovskiy,ru
274. Processing data for provideniya
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=provideniya,ru
275. Processing data for ahipara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ahipara,nz
276. Processing data for abay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=abay,kz
277. Processing data for daru
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=daru,pg
278. Processing data for huarmey
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=huarmey,pe
279. Processing data for general roca
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=general+roca,ar
Error: 'sentyabrskiy' was not found...skippping
280. Processing data for coahuayana
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=coahuayana,mx
281. Processing data for sulangan
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sulangan,ph
Error: 'gurupa' was not found...skippping
282. Processing data for chapadinha
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=chapadinha,br
283. Processing data for thompson
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=thompson,ca
284. Processing data for encs
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=encs,hu
285. Processing data for eureka
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=eureka,us
286. Processing data for upington
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=upington,za
287. Processing data for qasigiannguit
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=qasigiannguit,gl
288. Processing data for iqaluit
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=iqaluit,ca
289. Processing data for nueve de julio
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nueve+de+julio,ar
290. Processing data for kodiak
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kodiak,us
Error: 'belushya guba' was not found...skippping
Error: 'da nang' was not found...skippping
Error: 'mys shmidta' was not found...skippping
291. Processing data for huangzhai
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=huangzhai,cn
292. Processing data for hambantota
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hambantota,lk
293. Processing data for visby
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=visby,se
294. Processing data for san andres
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san+andres,co
295. Processing data for buala
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=buala,sb
296. Processing data for paamiut
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=paamiut,gl
Error: 'rungata' was not found...skippping
297. Processing data for sao joao do piaui
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sao+joao+do+piaui,br
298. Processing data for adrar
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=adrar,dz
299. Processing data for matagami
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=matagami,ca
300. Processing data for launceston
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=launceston,au
301. Processing data for bom jesus da lapa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bom+jesus+da+lapa,br
302. Processing data for alyangula
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=alyangula,au
303. Processing data for lixourion
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lixourion,gr
304. Processing data for crestview
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=crestview,us
305. Processing data for brookings
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=brookings,us
306. Processing data for pingdingshan
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pingdingshan,cn
307. Processing data for sabang
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sabang,id
308. Processing data for bud
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bud,no
309. Processing data for hami
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hami,cn
310. Processing data for palmer
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=palmer,us
311. Processing data for sao joao da barra
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sao+joao+da+barra,br
312. Processing data for henties bay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=henties+bay,na
Error: 'bengkulu' was not found...skippping
313. Processing data for slave lake
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=slave+lake,ca
314. Processing data for krasnyy chikoy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=krasnyy+chikoy,ru
315. Processing data for flinders
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=flinders,au
316. Processing data for moroni
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=moroni,km
317. Processing data for oberursel
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=oberursel,de
318. Processing data for nyrob
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nyrob,ru
319. Processing data for varhaug
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=varhaug,no
320. Processing data for matara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=matara,lk
321. Processing data for almaznyy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=almaznyy,ru
322. Processing data for prieska
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=prieska,za
323. Processing data for kortkeros
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kortkeros,ru
324. Processing data for yulara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yulara,au
325. Processing data for kilindoni
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kilindoni,tz
326. Processing data for banda aceh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=banda+aceh,id
327. Processing data for mugur-aksy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mugur-aksy,ru
328. Processing data for gamba
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=gamba,ga
Error: 'burica' was not found...skippping
Error: 'dzhusaly' was not found...skippping
Error: 'sinkat' was not found...skippping
329. Processing data for twinsburg
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=twinsburg,us
Error: 'kadykchan' was not found...skippping
330. Processing data for amargosa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=amargosa,br
331. Processing data for jasper
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=jasper,ca
332. Processing data for arlit
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=arlit,ne
333. Processing data for tamala
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tamala,ru
334. Processing data for pervomayskoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pervomayskoye,ru
335. Processing data for flin flon
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=flin+flon,ca
336. Processing data for port-gentil
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port-gentil,ga
Error: 'tambopata' was not found...skippping
337. Processing data for leshukonskoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=leshukonskoye,ru
338. Processing data for kosh-agach
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kosh-agach,ru
Error: 'tobermory' was not found...skippping
339. Processing data for pangnirtung
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pangnirtung,ca
340. Processing data for dukat
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dukat,ru
341. Processing data for meulaboh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=meulaboh,id
342. Processing data for anloga
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=anloga,gh
343. Processing data for kirakira
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kirakira,sb
344. Processing data for damara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=damara,cf
345. Processing data for mayo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mayo,ca
346. Processing data for ulladulla
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ulladulla,au
347. Processing data for shimanovsk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=shimanovsk,ru
348. Processing data for kazachinskoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kazachinskoye,ru
349. Processing data for sarkand
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sarkand,kz
350. Processing data for dalen
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dalen,no
351. Processing data for wenling
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=wenling,cn
352. Processing data for honiara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=honiara,sb
353. Processing data for sargatskoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sargatskoye,ru
Error: 'chardara' was not found...skippping
354. Processing data for lucera
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lucera,it
355. Processing data for vila franca do campo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vila+franca+do+campo,pt
356. Processing data for inhambane
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=inhambane,mz
357. Processing data for varkaus
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=varkaus,fi
Error: 'wulanhaote' was not found...skippping
358. Processing data for belyy yar
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=belyy+yar,ru
359. Processing data for baghdad
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=baghdad,iq
Error: 'barawe' was not found...skippping
360. Processing data for san felipe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san+felipe,mx
361. Processing data for seoul
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=seoul,kr
362. Processing data for hombourg-haut
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hombourg-haut,fr
363. Processing data for cam ranh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cam+ranh,vn
364. Processing data for granadilla de abona
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=granadilla+de+abona,es
365. Processing data for dafeng
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dafeng,cn
366. Processing data for cavalcante
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cavalcante,br
Error: 'yirol' was not found...skippping
367. Processing data for mayumba
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mayumba,ga
368. Processing data for cedar city
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cedar+city,us
369. Processing data for rocha
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=rocha,uy
370. Processing data for solnechnyy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=solnechnyy,ru
371. Processing data for rawson
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=rawson,ar
372. Processing data for quelimane
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=quelimane,mz
373. Processing data for beoumi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=beoumi,ci
374. Processing data for dunedin
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dunedin,nz
375. Processing data for riyadh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=riyadh,sa
376. Processing data for coquimbo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=coquimbo,cl
377. Processing data for harper
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=harper,lr
378. Processing data for north bend
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=north+bend,us
379. Processing data for tarko-sale
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tarko-sale,ru
380. Processing data for salym
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=salym,ru
381. Processing data for nelson bay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nelson+bay,au
382. Processing data for lyuban
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lyuban,by
383. Processing data for hong gai
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hong+gai,vn
384. Processing data for ofaqim
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ofaqim,il
Error: 'raga' was not found...skippping
385. Processing data for bambanglipuro
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bambanglipuro,id
386. Processing data for oistins
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=oistins,bb
387. Processing data for wajima
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=wajima,jp
388. Processing data for tlacotepec
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tlacotepec,mx
389. Processing data for carutapera
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=carutapera,br
390. Processing data for trinidad
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=trinidad,cu
391. Processing data for ticuantepe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ticuantepe,ni
392. Processing data for taksimo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=taksimo,ru
393. Processing data for san
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san,ml
394. Processing data for prado
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=prado,br
Error: 'lolua' was not found...skippping
395. Processing data for mabaruma
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mabaruma,gy
396. Processing data for port lincoln
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port+lincoln,au
397. Processing data for tibiri
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tibiri,ne
Error: 'dalinghe' was not found...skippping
Error: 'urfa' was not found...skippping
398. Processing data for korla
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=korla,cn
399. Processing data for ipixuna
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ipixuna,br
400. Processing data for voskresenskoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=voskresenskoye,ru
401. Processing data for usinsk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=usinsk,ru
402. Processing data for lieksa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lieksa,fi
403. Processing data for itacurubi de la cordillera
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=itacurubi+de+la+cordillera,py
404. Processing data for beloha
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=beloha,mg
405. Processing data for ossora
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ossora,ru
406. Processing data for kavaratti
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kavaratti,in
407. Processing data for la ronge
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=la+ronge,ca
408. Processing data for kamenka
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kamenka,ru
409. Processing data for sechura
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sechura,pe
Error: 'sperkhias' was not found...skippping
410. Processing data for mount isa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mount+isa,au
411. Processing data for serebryanyy bor
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=serebryanyy+bor,ru
412. Processing data for vanavara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vanavara,ru
413. Processing data for ust-maya
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ust-maya,ru
414. Processing data for port keats
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port+keats,au
415. Processing data for uruacu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=uruacu,br
416. Processing data for peniche
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=peniche,pt
417. Processing data for shingu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=shingu,jp
418. Processing data for ankang
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ankang,cn
419. Processing data for stornoway
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=stornoway,gb
420. Processing data for likasi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=likasi,cd
421. Processing data for kahului
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kahului,us
422. Processing data for quatre cocos
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=quatre+cocos,mu
423. Processing data for guasdualito
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=guasdualito,ve
424. Processing data for denpasar
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=denpasar,id
425. Processing data for ilulissat
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ilulissat,gl
426. Processing data for mikhaylovka
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mikhaylovka,kz
Error: 'coetupo' was not found...skippping
427. Processing data for nueva loja
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nueva+loja,ec
428. Processing data for belonia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=belonia,in
429. Processing data for oktyabrskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=oktyabrskiy,ru
Error: 'buqayq' was not found...skippping
Error: 'malakal' was not found...skippping
430. Processing data for tagab
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tagab,af
431. Processing data for mezen
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mezen,ru
432. Processing data for semey
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=semey,kz
433. Processing data for dali
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dali,cn
434. Processing data for pilar
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pilar,ph
435. Processing data for toyooka
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=toyooka,jp
436. Processing data for idaho falls
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=idaho+falls,us
437. Processing data for marawi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=marawi,sd
Error: 'elat' was not found...skippping
438. Processing data for monroe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=monroe,us
439. Processing data for zeya
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=zeya,ru
440. Processing data for penon blanco
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=penon+blanco,mx
441. Processing data for santa cruz del sur
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=santa+cruz+del+sur,cu
Error: 'sabla' was not found...skippping
442. Processing data for nyurba
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nyurba,ru
Error: 'burkhala' was not found...skippping
443. Processing data for mehamn
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mehamn,no
444. Processing data for abadiania
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=abadiania,br
445. Processing data for agadir
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=agadir,ma
446. Processing data for vila do maio
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vila+do+maio,cv
447. Processing data for dodge city
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dodge+city,us
448. Processing data for samarai
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=samarai,pg
449. Processing data for diamantino
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=diamantino,br
450. Processing data for belaya gora
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=belaya+gora,ru
451. Processing data for xining
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=xining,cn
452. Processing data for kapuskasing
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kapuskasing,ca
453. Processing data for beneditinos
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=beneditinos,br
454. Processing data for blythe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=blythe,us
455. Processing data for gushikawa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=gushikawa,jp
456. Processing data for praia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=praia,cv
457. Processing data for shevchenkove
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=shevchenkove,ua
458. Processing data for payson
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=payson,us
459. Processing data for pindiga
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pindiga,ng
460. Processing data for excelsior springs
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=excelsior+springs,us
461. Processing data for kuusamo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kuusamo,fi
462. Processing data for saurimo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saurimo,ao
463. Processing data for roebourne
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=roebourne,au
464. Processing data for chifeng
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=chifeng,cn
465. Processing data for ishigaki
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ishigaki,jp
466. Processing data for ribeira grande
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ribeira+grande,pt
467. Processing data for goure
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=goure,ne
468. Processing data for nishihara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nishihara,jp
469. Processing data for benghazi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=benghazi,ly
470. Processing data for charters towers
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=charters+towers,au
471. Processing data for axim
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=axim,gh
472. Processing data for toki
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=toki,jp
473. Processing data for andenes
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=andenes,no
474. Processing data for blagoyevo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=blagoyevo,ru
475. Processing data for fortuna
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=fortuna,us
476. Processing data for sarh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sarh,td
Error: 'gulshat' was not found...skippping
477. Processing data for brigantine
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=brigantine,us
478. Processing data for mandalay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mandalay,mm
479. Processing data for kaabong
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kaabong,ug
480. Processing data for hutchinson
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hutchinson,us
481. Processing data for kabinda
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kabinda,cd
482. Processing data for hit
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hit,iq
483. Processing data for luangwa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=luangwa,zm
Error: 'tawkar' was not found...skippping
484. Processing data for salinopolis
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=salinopolis,br
485. Processing data for torbat-e jam
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=torbat-e+jam,ir
Error: 'artyk' was not found...skippping
486. Processing data for florianopolis
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=florianopolis,br
Error: 'chagda' was not found...skippping
487. Processing data for tateyama
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tateyama,jp
Error: 'ayan' was not found...skippping
Error: 'viligili' was not found...skippping
488. Processing data for ballina
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ballina,ie
489. Processing data for touros
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=touros,br
490. Processing data for marsa matruh
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=marsa+matruh,eg
Error: 'kamenskoye' was not found...skippping
491. Processing data for hervey bay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hervey+bay,au
492. Processing data for arraial do cabo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=arraial+do+cabo,br
493. Processing data for linqing
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=linqing,cn
494. Processing data for poso
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=poso,id
495. Processing data for warrington
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=warrington,us
496. Processing data for borujerd
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=borujerd,ir
497. Processing data for ban nahin
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ban+nahin,la
498. Processing data for huilong
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=huilong,cn
499. Processing data for dicabisagan
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dicabisagan,ph
500. Processing data for komsomolskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=komsomolskiy,ru
501. Processing data for porangatu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=porangatu,br
502. Processing data for moron
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=moron,mn
503. Processing data for tchollire
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tchollire,cm
504. Processing data for buraydah
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=buraydah,sa
505. Processing data for broken hill
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=broken+hill,au
506. Processing data for dandong
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dandong,cn
507. Processing data for kon tum
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kon+tum,vn
508. Processing data for vardo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vardo,no
509. Processing data for maniitsoq
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=maniitsoq,gl
510. Processing data for kaputa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kaputa,zm
511. Processing data for hearst
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hearst,ca
Error: 'galiwinku' was not found...skippping
512. Processing data for vestmanna
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vestmanna,fo
513. Processing data for kribi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kribi,cm
514. Processing data for kruszwica
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kruszwica,pl
515. Processing data for necochea
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=necochea,ar
516. Processing data for samagaltay
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=samagaltay,ru
517. Processing data for kemi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kemi,fi
518. Processing data for stutterheim
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=stutterheim,za
519. Processing data for qaqortoq
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=qaqortoq,gl
520. Processing data for duobao
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=duobao,cn
521. Processing data for kovdor
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kovdor,ru
522. Processing data for port hardy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=port+hardy,ca
523. Processing data for nanortalik
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nanortalik,gl
524. Processing data for los llanos de aridane
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=los+llanos+de+aridane,es
525. Processing data for praia da vitoria
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=praia+da+vitoria,pt
526. Processing data for shafter
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=shafter,us
527. Processing data for havre-saint-pierre
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=havre-saint-pierre,ca
528. Processing data for ekhabi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ekhabi,ru
529. Processing data for asyut
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=asyut,eg
530. Processing data for iisalmi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=iisalmi,fi
531. Processing data for dudinka
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dudinka,ru
532. Processing data for wanganui
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=wanganui,nz
Error: 'barbar' was not found...skippping
533. Processing data for athabasca
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=athabasca,ca
534. Processing data for umm lajj
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=umm+lajj,sa
535. Processing data for chapada dos guimaraes
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=chapada+dos+guimaraes,br
536. Processing data for yarim
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yarim,ye
537. Processing data for leticia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=leticia,co
538. Processing data for iracoubo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=iracoubo,gf
539. Processing data for strzelce krajenskie
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=strzelce+krajenskie,pl
540. Processing data for san policarpo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=san+policarpo,ph
541. Processing data for piacabucu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=piacabucu,br
542. Processing data for mongu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mongu,zm
543. Processing data for portland
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=portland,au
544. Processing data for lethem
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lethem,gy
545. Processing data for elko
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=elko,us
546. Processing data for hebi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hebi,cn
547. Processing data for burns lake
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=burns+lake,ca
Error: 'aporawan' was not found...skippping
548. Processing data for los amates
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=los+amates,gt
549. Processing data for velyka mykhaylivka
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=velyka+mykhaylivka,ua
550. Processing data for acajutla
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=acajutla,sv
551. Processing data for ontario
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ontario,us
552. Processing data for alice springs
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=alice+springs,au
553. Processing data for kalianget
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kalianget,id
Error: 'mullaitivu' was not found...skippping
554. Processing data for matehuala
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=matehuala,mx
555. Processing data for zhigansk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=zhigansk,ru
556. Processing data for wiang sa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=wiang+sa,th
557. Processing data for huaraz
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=huaraz,pe
558. Processing data for bestobe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bestobe,kz
559. Processing data for ocos
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ocos,gt
560. Processing data for zharkent
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=zharkent,kz
561. Processing data for soto la marina
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=soto+la+marina,mx
562. Processing data for narasannapeta
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=narasannapeta,in
Error: 'tidore' was not found...skippping
563. Processing data for deputatskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=deputatskiy,ru
564. Processing data for gazanjyk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=gazanjyk,tm
565. Processing data for kijang
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kijang,id
566. Processing data for zhangye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=zhangye,cn
567. Processing data for jahangirabad
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=jahangirabad,in
568. Processing data for severodvinsk
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=severodvinsk,ru
569. Processing data for gurupi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=gurupi,br
570. Processing data for aranda de duero
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=aranda+de+duero,es
571. Processing data for ambilobe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ambilobe,mg
Error: 'tasbuget' was not found...skippping
572. Processing data for mandalgovi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mandalgovi,mn
573. Processing data for wattegama
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=wattegama,lk
574. Processing data for umm kaddadah
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=umm+kaddadah,sd
575. Processing data for tarata
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tarata,pe
576. Processing data for yuanping
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yuanping,cn
577. Processing data for santo antonio do sudoeste
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=santo+antonio+do+sudoeste,br
578. Processing data for langxiang
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=langxiang,cn
Error: 'qafsah' was not found...skippping
579. Processing data for atar
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=atar,mr
580. Processing data for kundiawa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kundiawa,pg
Error: 'samalaeulu' was not found...skippping
581. Processing data for blackwater
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=blackwater,au
582. Processing data for petropavlovsk-kamchatskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=petropavlovsk-kamchatskiy,ru
583. Processing data for kinkala
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kinkala,cg
584. Processing data for karatuzskoye
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=karatuzskoye,ru
Error: 'meyungs' was not found...skippping
585. Processing data for progress
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=progress,ru
586. Processing data for helong
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=helong,cn
587. Processing data for horsham
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=horsham,au
588. Processing data for meiktila
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=meiktila,mm
589. Processing data for vitim
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=vitim,ru
590. Processing data for coihaique
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=coihaique,cl
591. Processing data for itarema
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=itarema,br
Error: 'san lazaro' was not found...skippping
592. Processing data for jiexiu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=jiexiu,cn
Error: 'lar gerd' was not found...skippping
593. Processing data for boende
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=boende,cd
594. Processing data for krasnorechenskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=krasnorechenskiy,ru
595. Processing data for kolyshley
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kolyshley,ru
596. Processing data for pozo colorado
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pozo+colorado,py
597. Processing data for saint-francois
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=saint-francois,gp
598. Processing data for turan
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=turan,ru
599. Processing data for springdale
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=springdale,ca
600. Processing data for fossano
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=fossano,it
601. Processing data for grand gaube
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=grand+gaube,mu
602. Processing data for ayagoz
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ayagoz,kz
603. Processing data for shimoda
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=shimoda,jp
604. Processing data for taybad
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=taybad,ir
605. Processing data for tual
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tual,id
606. Processing data for soe
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=soe,id
607. Processing data for sao gabriel da cachoeira
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sao+gabriel+da+cachoeira,br
608. Processing data for mozarlandia
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mozarlandia,br
Error: 'saint anthony' was not found...skippping
Error: 'tubruq' was not found...skippping
609. Processing data for duyun
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=duyun,cn
610. Processing data for ixtapa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ixtapa,mx
611. Processing data for pimentel
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=pimentel,pe
612. Processing data for mana
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mana,gf
613. Processing data for beisfjord
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=beisfjord,no
614. Processing data for tungor
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tungor,ru
615. Processing data for temiscaming
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=temiscaming,ca
Error: 'sungai kolok' was not found...skippping
Error: 'karaul' was not found...skippping
616. Processing data for ortona
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ortona,it
617. Processing data for guanaja
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=guanaja,hn
618. Processing data for paso del macho
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=paso+del+macho,mx
619. Processing data for yeppoon
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yeppoon,au
620. Processing data for padang
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=padang,id
621. Processing data for rock sound
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=rock+sound,bs
622. Processing data for erzin
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=erzin,ru
623. Processing data for newport
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=newport,us
624. Processing data for conde
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=conde,br
625. Processing data for belgaum
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=belgaum,in
626. Processing data for itoman
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=itoman,jp
627. Processing data for mecca
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mecca,sa
628. Processing data for road town
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=road+town,vg
629. Processing data for ezerelis
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ezerelis,lt
630. Processing data for mudgee
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mudgee,au
631. Processing data for nuevo progreso
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nuevo+progreso,mx
632. Processing data for santa maria
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=santa+maria,us
633. Processing data for tromso
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tromso,no
Error: 'marcona' was not found...skippping
634. Processing data for kontagora
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kontagora,ng
635. Processing data for lufilufi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=lufilufi,ws
636. Processing data for labuhan
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=labuhan,id
637. Processing data for rio claro
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=rio+claro,tt
Error: 'ijaki' was not found...skippping
638. Processing data for feldkirch
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=feldkirch,at
639. Processing data for kenai
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kenai,us
640. Processing data for ornskoldsvik
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ornskoldsvik,se
641. Processing data for bantry
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=bantry,ie
642. Processing data for nizwa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nizwa,om
643. Processing data for we
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=we,nc
644. Processing data for tortoli
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tortoli,it
645. Processing data for mocuba
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mocuba,mz
646. Processing data for kamen-na-obi
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kamen-na-obi,ru
647. Processing data for muravlenko
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=muravlenko,ru
648. Processing data for curup
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=curup,id
Error: 'sorvag' was not found...skippping
649. Processing data for mogadishu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=mogadishu,so
650. Processing data for sobolevo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sobolevo,ru
651. Processing data for soria
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=soria,es
652. Processing data for dordrecht
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dordrecht,za
653. Processing data for tsogni
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tsogni,ga
654. Processing data for dingle
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=dingle,ie
655. Processing data for spassk-ryazanskiy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=spassk-ryazanskiy,ru
656. Processing data for oranjestad
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=oranjestad,aw
657. Processing data for uusikaupunki
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=uusikaupunki,fi
658. Processing data for karratha
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=karratha,au
659. Processing data for yangcun
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=yangcun,cn
660. Processing data for hay river
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=hay+river,ca
661. Processing data for santa rosa
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=santa+rosa,bo
Error: 'lasa' was not found...skippping
Error: 'tabiauea' was not found...skippping
Error: 'sumbawa' was not found...skippping
662. Processing data for jaguarari
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=jaguarari,br
663. Processing data for oster
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=oster,ua
664. Processing data for cabedelo
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cabedelo,br
665. Processing data for nemuro
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=nemuro,jp
Error: 'nguiu' was not found...skippping
666. Processing data for caraquet
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=caraquet,ca
667. Processing data for sarangani
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sarangani,ph
668. Processing data for kargopol
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kargopol,ru
669. Processing data for selje
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=selje,no
670. Processing data for kawalu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=kawalu,id
Error: 'angra' was not found...skippping
671. Processing data for estelle
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=estelle,us
672. Processing data for sao luiz gonzaga
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sao+luiz+gonzaga,br
Error: 'dinsor' was not found...skippping
673. Processing data for halifax
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=halifax,ca
674. Processing data for sao felix do xingu
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sao+felix+do+xingu,br
675. Processing data for priiskovyy
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=priiskovyy,ru
676. Processing data for ojinaga
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=ojinaga,mx
677. Processing data for tilichiki
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tilichiki,ru
678. Processing data for stykkisholmur
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=stykkisholmur,is
679. Processing data for buenos aires
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=buenos+aires,cr
680. Processing data for cabra
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=cabra,ph
681. Processing data for sorong
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=sorong,id
682. Processing data for urucara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=urucara,br
683. Processing data for tara
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=tara,ru
684. Processing data for huixtla
http://api.openweathermap.org/data/2.5/weather?appid=292a495dc0dd3ff2d9e85214e81bc030&units=imperial&q=huixtla,mx
Error: 'kismayo' was not found...skippping
--------------------------
Data retrieval complete
--------------------------
```
## DataFrame of each city's weather data
```
# create dataframe using data from api
weather_df = pd.DataFrame({"City" : found_cities, "Country" : found_countries, "Date" : dates,
                   "Lat" : found_lats, "Lng" : found_lngs, "Max Temp": max_temp, "Humidity" : humidity, 
                   "Cloudiness" : cloudiness, "Wind Speed" : wind_speed})

weather_df.to_csv("../Output/city_weather.csv") # write data frame to csv file and save

weather_df.head()
```
## Temperature v Latitude
```
# create scatter plot of the relationship between temperature and latitude
plt.figure(figsize = (10,5))
plt.scatter(weather_df["Lat"], weather_df["Max Temp"], facecolor = "slateblue", edgecolor = "black", alpha = .9, s = 30);

plt.title("Temperature (F) v. City Latitude   " + readable_date );
plt.ylabel("Max Temperature (F)");
plt.xlabel("Latitude");


plt.ylim(weather_df["Max Temp"].min() - 2, weather_df["Max Temp"].max() + 2)
plt.xlim(weather_df["Lat"].min() - 2, weather_df["Lat"].max() + 2)
         
plt.savefig("../Images/temp_lat.png")
```

## Humidity v Latitude

```
# create scatter plot of the relationship between humidity and latitude
plt.figure(figsize = (10,5))
plt.scatter(weather_df["Lat"], weather_df["Humidity"], facecolor = "slateblue", edgecolor = "black", alpha = .9, s = 30);


plt.title("Humidity (%) v. City Latitude   " + readable_date);
plt.ylabel("Humidity (%)");
plt.xlabel("Latitude");

plt.ylim(weather_df["Humidity"].min() - 5, weather_df["Humidity"].max() + 5)
plt.xlim(weather_df["Lat"].min() - 2, weather_df["Lat"].max() + 2)

plt.savefig("../Images/humidity_lat.png")
```
## Cloudiness v Latitude

```
# create scatter plot of the relationship between cloudiness and latitude
plt.figure(figsize = (10,5))
plt.scatter(weather_df["Lat"], weather_df["Cloudiness"], facecolor = "slateblue", edgecolor = "black", alpha = .9, s = 30);


plt.title("Cloudiness (%) v. City Latitude   " + readable_date);
plt.ylabel("Cloudiness (%)");
plt.xlabel("Latitude");


plt.ylim(weather_df["Cloudiness"].min() - 5, weather_df["Cloudiness"].max() + 5)
plt.xlim(weather_df["Lat"].min() - 2, weather_df["Lat"].max() + 2)

plt.savefig("../Images/clouds_lat.png")
```

## Wind Speed v Latitude

```
# create scatter plot of the relationship between wind speed and latitude
plt.figure(figsize = (10,5))
plt.scatter(weather_df["Lat"], weather_df["Wind Speed"], facecolor = "slateblue", edgecolor = "black", alpha = .9, s = 30);


plt.title("Wind Speed (mph) v. City Latitude   " + readable_date);
plt.ylabel("Wind Speed (mph)");
plt.xlabel("Latitude");

plt.ylim(weather_df["Wind Speed"].min() - 1, weather_df["Wind Speed"].max() + 5)
plt.xlim(weather_df["Lat"].min() - 2, weather_df["Lat"].max() + 2)

plt.savefig("../Images/wind_lat.png")
```