---
title: "Interactive data visualization with Python"
draft: false
---


\
This tutorial builds upon previous posts on data analysis and mapping by demonstrating how to do simple interactice charts in Python. For this example we will use NYC's [tree census dataset](https://data.cityofnewyork.us/widgets/uvpi-gqnh), using the SODA API to access. Plotting will be done with Plotly, an open source visualization tool for Python.

Let's start by importing all the necessary libraries:

```
import os
import pandas as pd
import numpy as np
from sodapy import Socrata
import plotly.express as px
import plotly.graph_objects as go
```

Next we can use the Socrata Python tool to download the city tree data. Below we specify the soda developer token and the location of the data. And by default Socrata limits the amount of data that can be downloaded to 20k rows, we uppdate that limit to 600k, as thats approximately how many rows are in the tree census.

```
client = Socrata("data.cityofnewyork.us", os.environ['nyc_soda_cuny_token'])
results = client.get("uvpi-gqnh", limit=600000)
trees = pd.DataFrame.from_records(results)
```

Usually all columns in datasets downloaded from Socrata is of a string / object type. Below we specify the numeric columns as either integers or floats respectively.

```
cols_int = ['tree_dbh', 'stump_diam']
cols_float = ['latitude', 'longitude',
              'x_sp', 'y_sp']
for column in cols_int:
    trees[column] = trees[column].astype(int)
for column in cols_float:
    trees[column] = trees[column].astype(float)
```

### Simple Bar Chart

The first data point we'll visualize is the amount of trees in each of the five boroughs in NYC. There are many ways to do this with Pandas – below we use the **value_counts()** method on a Series object. This method counts the occurences elements in a DataFrame, and returns a new Series object where the element names are the index and the values are the amount.

```
trees['boroname'].value_counts()
```

Now that we have the number of trees in each borough, we need the information in a format that is understood by Plotly. Below we turn the resulting Series object above back into a DataFrame using the **reset_index()** method.

```
boro_data = trees['boroname'].value_counts().reset_index()
boro_data
```

**index **|**boroname**
:-----:|:-----:
Queens |224748
Brooklyn |150555
Staten Island |97500
Bronx |78593
Manhattan |48604

Notice the column names in the above DataFrame are nonsensical – the boroname column is the amount of trees and the actual names of the boros are in a column called index. We can change that by using **rename()** and passing a dictionary to change these around.

```
boro_data = boro_data.rename(columns={'index':'boro',
                                      'boroname': 'count'})
boro_data
```

**boro **|**count**
:-----:|:-----:
Queens |224748
Brooklyn |150555
Staten Island |97500
Bronx |78593
Manhattan |48604


Now we are ready to pass the data to Plotly for visualizing.

Below are two of the main drivers behind nearly all Plotly visuals – a **Figure** object and a graph type, in this case **Bar**. We place the Bar object inside the figure and specify the X and Y columns by passing the columns of our DataFrame above.

Plotly by default always shows a control bar at the top each plot – below we turn this off by passing a dictionary to the *config* argument.

```
fig = go.Figure(
    go.Bar(
        x=boro_data['boro'], y=boro_data['count'])
)

fig.show(
    config= {'displaylogo': False,
             'displayModeBar': False}
)
```

CHART HERE

The resulting graph above is raw and begs for a little customization. Customizing graphs is easy with Plotly with a **Layout** object. Below we specify a new background color (in RGB), a title, and set the width of the plot by giving the left + right margins. We also change the color of the bars themselves in the Bar object.

```
layout = go.Layout(
    plot_bgcolor='rgba(255,255,255,1)',
    title='Number of trees in each borough',
    margin=dict(l=500, r=500)
)

fig = go.Figure(
    go.Bar(
        x=boro_data['boro'], y=boro_data['count'],
    marker_color='rgb(55, 83, 109)'),
    layout=layout
)

fig.show(
    config= {'displaylogo': False,
             'displayModeBar': False}
)
```

CHART HERE

### Stacked Bar Charts

Next we'll use the **groupby()** method in Pandas to count the number of healthy trees in each borough. Groupby is one of the most powerful concepts within Pandas, allowing researchers to group DataFrame according to a specific (or multiple) entities, in this case Boroughs, and then aggregate values within groups. For example, below we group our DataFrame by boroname and (tree) health, and then count the occurences of good, fair, and poor trees within each boro.

```
trees.groupby(['boroname', 'health']).agg({'tree_id':'count'})
```



As we did previously, below we convert the above group by result into a dataframe and change the column names.

```
boro_health = trees.groupby(['boroname', 'health'])\
                   .agg({'tree_id':'count'})\
                   .reset_index()\
                   .rename(columns={'tree_id': 'count'})
boro_health.head()
```

**boroname**|**health**|**count**
:-----:|:-----:|:-----:
Bronx|Fair|9587
Bronx|Good|61985
Bronx|Poor|2774
Brooklyn|Fair|20298
Brooklyn|Good|118480

Now we're ready to feed the above dataframe into the Plotly **Bar** object. However, to create a stacked bar chart, we need to feed the data into plotly as seperate dataframes for each group. Below we filter the data by the health condition and create 3 seperate Bar objects within the Figure, and specify the *barmode* argument as **stack**.

```
fig = go.Figure(
    data=[
        go.Bar(name='Poor',
               x=boro_health['boroname'].loc[boro_health['health']=='Poor'],
               y=boro_health['count'].loc[boro_health['health']=='Poor']),
        go.Bar(name='Fair',
               x=boro_health['boroname'].loc[boro_health['health']=='Fair'],
               y=boro_health['count'].loc[boro_health['health']=='Fair']),
        go.Bar(name='Good',
               x=boro_health['boroname'].loc[boro_health['health']=='Good'],
               y=boro_health['count'].loc[boro_health['health']=='Good'])
])

fig.update_layout(barmode='stack')

fig.show(
    config= {'displaylogo': False,
             'displayModeBar': False}
)
```

CHART HERE

We can improve our code above slightly by using list comprehension. Rather than manually specifying a dataframe for each of the 3 health conditions, we can use list comprehension to do this for us automatically.

Further, we add custom colors for the bars, change the width, and background color.

```
health_levels = ['Poor', 'Fair', 'Good']
colors = ['steelblue', 'grey', 'firebrick']

fig = go.Figure(
    data=[
        go.Bar(name=health,
               x=boro_health['boroname'].loc[boro_health['health']==health],
               y=boro_health['count'].loc[boro_health['health']==health],
               marker_color=colors[idx]) for idx, health in enumerate(health_levels)
])

fig.update_layout(barmode='stack',
                  plot_bgcolor='rgba(255,255,255,1)',
                  autosize=False,
                  width=500)

fig.show(
    config= {'displaylogo': False,
             'displayModeBar': False}
)
```

CHART HERE

### Scatter Plots

For the final example we visualize 2 variables at the same time and at a more granular geographic unit, zip code. We use the groupby method once again to gather: how many trees are alive in each zipcode, and how many trees are in poor health condition.

```
zip_status = trees.groupby(['boroname', 'zipcode', 'status'])\
                  .agg({'status':'count'})\
                  .rename(columns={'status':'count'})\
                  .reset_index()
zip_health = trees.groupby(['boroname', 'zipcode', 'health'])\
                  .agg({'health':'count'})\
                  .rename(columns={'health':'h_count'})\
                  .reset_index()

data = pd.merge(zip_status, zip_health, on=['boroname', 'zipcode'])
data.head()
```

**boroname**|**zipcode**|**status**|**count**|**health**|**hcount**
:-----:|:-----:|:-----:|:-----:|:-----:|:-----:
Bronx|10451|Alive|2189|Fair|439
Bronx|10451|Alive|2189|Good|1565
Bronx|10451|Alive|2189|Poor|185
Bronx|10451|Dead|109|Fair|439
Bronx|10451|Dead|109|Good|1565

```
data = data[(data['status']=='Alive') & (data['health']=='Poor')]
data.head()
```

**boroname**|**zipcode**|**status**|**count**|**health**|**h\_count**
:-----:|:-----:|:-----:|:-----:|:-----:|:-----:
Bronx|10451|Alive|2189|Poor|185
Bronx|10452|Alive|2845|Poor|165
Bronx|10453|Alive|2793|Poor|98
Bronx|10454|Alive|1352|Poor|46
Bronx|10455|Alive|1639|Poor|57

```
fig = px.scatter(data, x="count", y="h_count",
                 size="h_count", color="boroname",
                 hover_name="zipcode", log_y=True, size_max=60)
fig.update_layout(barmode='stack',
                  plot_bgcolor='rgba(255,255,255,1)')
fig.show(
    config= {'displaylogo': False,
             'displayModeBar': False}
)
```

CHART HERE

### Exporting charts

Finally, Plotly has various methods for exporting charts as static images or interactive html files.

<br>

Exporting images can be done with the **write_image()** method – specifying the image type by adding the file extension at the end.

*note: ensure the Kaleido library is intsalled by running --> pip install -U kaleido*

```
fig.write_image("fig1.png")
```

Similarly, exporting html files can be done with the **write_html()** method.

```
fig.write_html("fig1.html")
```

End!
