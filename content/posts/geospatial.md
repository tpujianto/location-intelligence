---
title: "Working with Geospatial Data"
date: 2020-11-24T23:47:56-05:00
draft: false
---

TO DO:
- geocoding
- common geographic units
- spatial disaggregation


Before diving into the nuts and bolts of working with geospatial data, it is necessary to clarify the differences between the terms geospatial and GIS. GIS unified software system that stores geographic information i layers. Geospatial on the other hand is a broader category of the technology and associated data that deals with geography. Geosptial data can come in the form of satellite images, lidar point clouds, polygons and coordinates, to name just a few. The one constant is that every datum, will have some way to reference a point in space and or time.

The technologies covered so far, namely Python and SQL, are perfectly capable of allowing analysis and development of geospatial data and tools. The concepts that this post will cover are geocoding, and reverse geocoding, spatial aggregation and disaggregation, and geospatial specific libraries for Python.

---

### Geocoding


[Geocoding](https://en.wikipedia.org/wiki/Geocoding) is the process of taking a textual representation of an address, and using software to return geographic coordinates. For example, taking "2920 Broadway, New York, NY 10027" and getting a latitude longitude pair = 40.807075 -73.964432. Reverse geocoding, as the name suggests, is doing the reverse – so taking 40.807075 -73.964432 and getting back "2920 Broadway, New York, NY 10027". It is useful for situations where you have a places address, but would like to locate it geographically, or when you have to merge to datasets, with differing geographic information. For example, you know latitude and longitude of a certain grocery store, but would like to determine the zip code it is in.

There are number of online services / APIs to help with geocoding. Most of them require some sort of sign-up. This course will rely heavily on the [HERE API](https://www.here.com/) for geocoding as they are generous with access limits.

The first step in geocoding with HERE is to [sign-up](https://developer.here.com/sign-up?create=Freemium-Basic&keepState=true&step=account) for an account to obtain an App ID and App Code – think of these as credentials so HERE can identify who is connecting. Store the App ID and Code in a safe place as they will come in handy in later tutorials.

The next step is to write the code that connects with the HERE API over HTTP. For this example we will search for information on an address in NYC. Python has a great tool to connect to websites called Requests – it lets you specify a website url and then download certain data from the page.

```
app_id = 'your_here_app_id'
code = 'your_here_app_code'
search = '115W 18th New York, NY 10011'

url = "https://geocoder.api.here.com/6.2/geocode.json?app_id=%s&app_code=%s&searchtext=%s&country=USA"
url = url % (app_id, code, search)

r = requests.get(url)
```

The above code creates variables for the app_code and app_id obtained from HERE and a 'search' variable for an address in Manhattan. It then combines these in a variable called 'url' that is used to specify the API address. Finally the 'r' variable sends a request to the HERE API with the specified information.

The next step is to parse the information returned from the HERE API to get the latitude and longitude for the search address.

```
features = r.json()
view = features['Response']['View'][0]
res = view['Result'][0]
loc = res['Location']
pt = loc['DisplayPosition']
lat, lng = pt['Latitude'], pt['Longitude']
```