---
title: "Geocoding Data"
date: 2020-12-26T23:47:56-05:00
draft: false
---

![missing image](img/geocode.jpg "Title")

\
Before diving into the nuts and bolts of working with geospatial data and geocoding, it is necessary to clarify the differences between the terms geospatial and GIS. GIS is unified software system that stores geographic information in layers. While the term geospatial is a broader category of the technology and associated data that deals with geography. Geosptial data can come in the form of satellite images, lidar point clouds, polygons and coordinates, to name just a few. The one constant is that every datum, will have some way to reference a point or region on the Earth's surface.

Geocoding is one of the most common tasks researchers working with geospatial data do. It is the process of taking a textual representation of an address, and using software to return geographic coordinates. This post explores the basics of geocoding in Python using the HERE API.

---

### Geocoding


[Geocoding](https://en.wikipedia.org/wiki/Geocoding) is the process of taking a textual representation of an address, and using software to return geographic coordinates. For example, taking "2920 Broadway, New York, NY 10027" and getting a latitude longitude pair = 40.807075 -73.964432. Reverse geocoding, as the name suggests, is doing the reverse – so taking 40.807075 -73.964432 and getting back "2920 Broadway, New York, NY 10027". It is useful for situations where you have a places address but would like to locate it geographically, or when you have to merge to datasets with differing geographic information. For example, you know latitude and longitude of a certain grocery store, but would like to determine the zip code it is in.

There are number of online services / APIs to help with geocoding. Most of them require some sort of sign-up. This post will utilize the [HERE API](https://www.here.com/) for geocoding as they are generous with access limits.

The first step in geocoding with HERE is to [sign-up](https://developer.here.com/sign-up?create=Freemium-Basic&keepState=true&step=account) for an account to obtain an App ID and App Code – think of these as credentials so HERE can identify who is connecting to their system. Store the App ID and Code in a safe place as they will come in handy in later tutorials.

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
```

The above 'features' variable reads the data returned from HERE in JSON format. JSON is a file format common in web development to store text data in (**hiearchical**) human readable form. The output from features should look something like:

```
{
  "Response": {
    "MetaInfo": {
      "Timestamp": "2020-12-03T04:54:08.020+0000"
    },
    "View": [
      {
        "_type": "SearchResultsViewType",
        "ViewId": 0,
        "Result": [
          {
            "Relevance": 0.92,
            "MatchLevel": "houseNumber",
            "MatchQuality": {
              "State": 1,
              "City": 1,
              "Street": [
                0.85
              ],
              "HouseNumber": 1,
              "PostalCode": 1
            },
            "MatchType": "interpolated",
            "Location": {
              "LocationId": "NT_e1JSr5eaAsWrBHzTuGJn3D_xETN",
              "LocationType": "address",
              "DisplayPosition": {
                "Latitude": 40.74032,
                "Longitude": -73.99585
              } ...
```

We can ignore most of the information returned in the above JSON. We are only interested in the latitude and longitude values at the bottom. To get these values we have to give the name of the keys, as follows:

```
view = features['Response']['View'][0]
res = view['Result'][0]
loc = res['Location']
pt = loc['DisplayPosition']
lat, lng = pt['Latitude'], pt['Longitude']
```

In the last line we are left with two variables lat, lng that together are the coordinates points that lets us locate the address on the globe.
