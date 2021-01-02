---
title: "Data Analysis with Python"
date: 2020-12-24T23:47:56-05:00
draft: false
---


\
This tutorial builds upon the previous posts on Pandas and data wrangling by demonstrating methods to conduct data analysis with Python. [*Jupyter notebook here*](https://github.com/carlobailey/location-intelligence/blob/main/_notebooks/data-analysis.ipynb)

For this example we will use NYC's PLUTO dataset which is the city's building data from the Planning department. Data can either be accessed from the course AWS SQL database or downloaded from this link.

Let's start by importing all the necessary libraries we'll need:

- Scipy: is a scientific computing library that has comprehensive functionality for doing statistics in Python
- Geopandas: similar to pandas with extensions to make it easier to work with geospatial data.
- Matplotlib: the defacto library for drawing/plotting in Python
- Psycopg2: is a utility library that makes it easier to connect to postgresql
- Cenpy: An interface to explore and download US Census data

```
import pandas as pd
import numpy as np
import geopandas as gpd
import psycopg2 as pg
import os
import matplotlib.pyplot as plt
import cenpy
from scipy.stats import pearsonr
```

Next we can load the Pluto dataset from where ever you saved the file on your machine.
```
file_location = 'path_to_file'
df = pd.read_csv(file_location, low_memory=False,
                 dtype={'block': str, 'lot': str,
                        'cd': str, 'ct2010': str,
                        'cb2010': str, 'schooldist': str,
                        'council': str, 'zipcode': str,
                        'landuse': str, 'bbl': str})
df['bbl'] = df['bbl'].str.replace('.00000000', '')
```

### Descriptive Stats

This next section demonstrates how to surface basic statistics about buildings in NYC – by answering a few basic questions about floor height, building area, and land use across the city.



#### What is the average number of floors in buildings across the city?
The first question is very simple to answer in Pandas, requiring only one line of code, thanks to the **.mean()** method for Series objects which returns the average of any numeric column.

```
avg_floors = df['numfloors'].mean()

print('The average number of floors within buildings is: %s' % avg_floors)

The average number of floors within buildings is: 2.3348885465632216
```


#### What is the average number of floors in residential buildings across the city?
The next question requires a little more effort as we have to filter the column to only return residential buildings. Thanks to the PLUTO [data dictionary](https://www1.nyc.gov/assets/planning/download/pdf/data-maps/open-data/pluto_datadictionary.pdf?v=20v7), we know that the land use codes for residential are 01, 02, and 03.

```
resi_code = ['01', '02', '03']
mask = df['landuse'].isin(resi_code)
avg_floors = df['numfloors'].loc[mask].mean()

print('The average number of floors within residential is: %s' % avg_floors)

The average number of floors within residential is: 2.3240842889835496
```

#### What is the average number of floors in commercial buildings in Manhattan?
We can do a similar filter for commercial buildings, just changing the land use to 05 and the borough to 'MN'.

```
com_code = ['05']
mask1 = df['landuse'].isin(com_code)
mask2 = df['borough'] == 'MN'
avg_floors = df['numfloors'].loc[(mask1) & (mask2)].mean()

print('The average number of floors within commercial is: %s' % avg_floors)

The average number of floors within commercial is: 9.829079317052086
```

#### What is the area distribution for residential buildings in Manhattan?
Again this question requires filtering on two columns, and this time we will plot a histogram with the **.hist()** method. To get a more granular binning of values we can specify the number of bins.

```
mask1 = df['landuse'] == '01'
mask2 = df['borough'] == 'MN'
_ = df['bldgarea'].loc[(mask1) & (mask2)].hist(bins=100)
```

![missing image](img/resi_dist.png "Title")


The histogram shows a slightly skewed distribution of building areas – meaning there are a large amount of moderately sized homes with a small amount of gigantic residences over 15-20k. We can add a little more color to the distribution by plotting 1.6x the standard deviations, indicating the range we can expect 90% of values to fall within.

The **.std()** method in Pandas is a quick way to find the standard deviation of a distribution.

```
std = temp.std()
avg = temp.mean()

fig, ax = plt.subplots()
temp.hist(ax=ax, bins=100)
ax.axvspan(avg + (std * 1.645), temp.max(),
           alpha=0.2, color='grey')
ax.axvspan(0, avg - (std * 1.645),
               alpha=0.2, color='grey')
ax.axvline(avg, color='red')
plt.grid(False)
_ = plt.xlim(0, temp.max())
```

A more robust measure for studying distributions, particularly skewed distributions, is to look at quartiles or percentiles. Percentiles signify the proportion of values we can expect to fall within a range. For example, 3024 is the 25th percentile of residential area for homes in Manhattan. This means 25% of single family buildings in Manhattan have an area smaller than 3024.

Percentiles are easily found in Pandas with the **.quantile()** method.

```
temp = df['bldgarea'].loc[(mask1) & (mask2)]
qrt_25 = temp.quantile(0.25)

print("25% of residences in Manhattan have an area less than: {0}".format(qrt_25))

25% of residences in Manhattan have an area less than: 3024.0
```

### Data Aggregation

A fundamental aspect of data analysis is the aggregation of data to a specified unit. For instance, if we wanted to know the zipcode with the largest proportion of retail space, we would have to aggregate data by zip code.

To do this we can utilize the very powerful **.groupby()** method in Pandas. It allows us to specify which column(s) to aggregate the data with, and also the aggregate function to operate. For the below example we will groupby zip codes and then take the total retail and building area in each zip code.

```
# First lets calculate the total retail area per zip code
rtl_sum = df.groupby('zipcode')['retailarea'].sum()

# Next we can calculate the total building area per zip code
bld_area = df.groupby('zipcode')['bldgarea'].sum()

# Next we can calculate the retail percentage per zipcode
rtl_pct = rtl_sum / bld_area
```

The result of the above calculation will be a series with the index being the zip codes and the values being the retail percentage.

Below we can show the top 5 zip codes with the most retail space. The top one is a small zip code in Midtown, while number 5 is Soho.

```
rtl_pct.sort_values(ascending=False)[:5].reset_index()
```

**zipcode**|**0**
:-----:|:-----:
10155|0.350104
12345|0.167348
10120|0.150617
10151|0.14385
10012|0.127777

We can again visualize the distribution of retail area with a choropleth map. To do that we can download the zipcode geometry data from the AWS db using psycopg2 or from the class G-drive.

```
gdf = gpd.read_postgis('''
    SELECT
        region_id,
        geom
    FROM geographies.zipcodes
    ''', conn)
```

Next we can merge the zip codes geometry with the retail percentage values. We will have to use the **.reset_index()** method on the rtl_pct variable to convert it from a Series to a DataFrame in order to enable the merge.

We also filter the combined GeoDataFrame using the **notnull()** method so that we are only plotting zip codes with a retail area value.

```
rtl = gdf.merge(rtl_pct.reset_index(),
                how='left', left_on='region_id',
                right_on='zipcode')
rtl[rtl[0].notnull()].plot(column=0, cmap='OrRd', legend=True)
plt.axis(False)
_ = plt.box(False)
```

![](img/retail_choro.png)

### Analyzing the relationship between land use and income

The final demonstration in this tutorial will merge income data from the American Community Survey to check the relationship between the percentage of factory area in a zip code and the median household income. The assumption being that there will be more factories in less affluent parts of the city.

To do this we will again use the Cenpy lirbary to access the data – and refer to the [Census variable table](https://github.com/carlobailey/urban-data-science/blob/master/ACS/table_name_variables.md) to get the code for the income variable (it's **B19013_001E**).

```
conn = cenpy.products.APIConnection("ACSDT5Y2018")
names = ['B19013_001E']
```

Here we specify the geographic unit, zip code, and have to change the data type to float as the Cenpy downloads all columns as text values.

```
data = conn.query(names, geo_unit='zip code tabulation area')
data.rename(columns={'B19013_001E': 'income'}, inplace=True)
data['income'] = data['income'].astype(float)
```

The final step before merging is replacing NULL values and calculating the percentage of factory area in each zip code.

```
data = data.replace(-666666666.0, np.nan)
fct_pct = df.groupby('zipcode')['factryarea'].sum() / bld_area
temp = data.merge(fct_pct.reset_index(),
                  left_on='zip code tabulation area',
                  right_on='zipcode').dropna()
```

Then we can utilize the Pearsonr function from Scipy Stats to calculate the Pearson correlation between household income and factory area.

Somewhat expectedly there is a small but significant relationship. This quick analysis suggests a small negative correlation, meaning as income decreases we can expect to fine more factories.

```
pearsonr(temp['income'], temp[0])

(-0.1931690659775329, 0.009374203979288875)
```
