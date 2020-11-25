---
title: "Working with Geospatial Data"
date: 2020-11-24T23:47:56-05:00
draft: false
---

Before diving into the nuts and bolts of working with geospatial data, it is necessary to clarify the differences between the terms geospatial and GIS. GIS unified software system that stores geographic information i layers. Geospatial on the other hand is a broader category of the technology and associated data that deals with geography. Geosptial data can come in the form of satellite images, lidar point clouds, polygons and coordinates, to name just a few. The one constant is that every datum, will have some way to reference a point in space and or time.

The technologies covered so far, namely Python and SQL, are perfectly capable of allowing analysis and development of geospatial data and tools. The concepts that this post will cover are geocoding, and reverse geocoding, spatial aggregation and disaggregation, and geospatial specific libraries for Python.

---

### Geocoding


[Geocoding](https://en.wikipedia.org/wiki/Geocoding) is the process of taking a textual representation of an address, and using software to return geographic coordinates. For example, taking "2920 Broadway, New York, NY 10027" and getting a latitude longitude pair = 40.807075 -73.964432. Reverse geocoding, as the name suggests, is doing the reverse – so taking 40.807075 -73.964432 and getting back "2920 Broadway, New York, NY 10027".

There are number of online services / APIs to help with this task. Most of them require some sort of sign-up. This course will rely heavily on the [HERE API](https://www.here.com/) for geocoding needs – it is easy to sign-up and they are generous with access limits.
