---
title: "Scraping Google Maps with Python"
date: 2020-12-23T23:21:50-05:00
draft: false
---


\
[Web scraping](https://en.wikipedia.org/wiki/Web_scraping) is the process of extracting data from web pages using software. There are many [techniques](https://en.wikipedia.org/wiki/Web_scraping#Techniques) to scrape data: computer vision, manual copy and pasting, pattern matching, etc – for this tutorial, we will use [HTML parsing](https://en.wikipedia.org/wiki/Web_scraping#HTML_parsing) with HTTP programming. Given the high costs of downloading large amounts of proprietary data, web scraping is sometimes the only option for small scale research projects. Google maps for example, the defacto mapping and navigation platform (as of 2020/2021), has a monopoly on information about the location of businesses across the globe. To get access to this information is very very costly (problems of monoply rule), therefore scraping the data is one of the only options for students.

This tutorial demonstrates how to scrape Google Maps POI (points of interest) data in NYC with Python using the Selenium and Beautiful Soup libraries. [*Jupyter notebook here*](https://github.com/carlobailey/location-intelligence/blob/main/_notebooks/scraping.ipynb)

The first step is to import all the necessary libraries:
- [Requests](https://requests.readthedocs.io/en/master/): a HTTP library for Python
- [Selenium](https://www.selenium.dev/selenium/docs/api/py/): a tool for automating web browsing
- [BeautfiulSoup](https://en.wikipedia.org/wiki/Beautiful_Soup_(HTML_parser)): a package to parse HTML elements
- [Shapely](https://shapely.readthedocs.io/en/stable/manual.html): a library for manipulaitng and analysing geometry
- [Geopandas](https://geopandas.org/index.html): similar to pandas with extensions to make it easier to work  with geospatial data.


```
import requests
from bs4 import BeautifulSoup
import os
import pandas as pd
import geopandas as gpd
from shapely.geometry import Point
from selenium import webdriver
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.options import Options
```

### Search requests

Every request to google maps usually starts from a location and a search query. This first section involves gathering a list of locations (latitudes & longitudes) and search queries (types of POI) to feed into the base URL below. The base url is the request we send to Google and what we'll get back is a selection of POI (businesses) for that location related to the search category.

```
base_url = "https://www.google.com/maps/search/{0}/@{1},{2},16z"
```

### Search locations

Any search in Google maps requires a location to search from, typically a latitude and longitude pair. Below we use the centroids of select zipcodes within NYC to use as our search locations. There are only 11 zip codes below but this could easily be scaled to the entire city or country.

```
coords = gdf['geom'].centroid
xs = coords.apply(lambda x: x.x).tolist()
ys = coords.apply(lambda x: x.y).tolist()
```

Here we outline the search categories.

```
searches = ['Restaurants', 'bakery', 'coffee', 'gym', 'yoga',
            'school', 'clothing', 'electronics',
            'beauty', 'hardware', 'daycare', 'galleries',
            'museums', 'Hotels', 'deli',
            'liquor', 'bar', 'Groceries', 'Takeout',
            'Banks', 'Pharmacies']
```

### Sending a GET request

The next step is to send a request to Google's servers about the information we would like returned. This type of request is called a [GET request](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods) in HTTP programming.

Below we'll send a single test request to the servers to see what information we get back. We insert a single lat/lng pair into the **base_url** variable.

```
url = base_url.format(searches[0], xs[0], ys[0])
```


The next portion of code starts a selenium web driver (the vehicle that powers automated web browsing) and specifies a few browsing options. Specifically we state the web browser should run headless, meaning it should not open up a new browser window, and install a Chrome browser.

```
delay = 10
chrome_options = Options()
chrome_options.add_argument("--headless")
driver = webdriver.Chrome(ChromeDriverManager().install(),
                           options=chrome_options)
```

### Intentionally slowing down

Although web scraping is not [illegal](https://twitter.com/OrinKerr/status/1171116153948626944), researchers should be careful when scraping private websites. Not due to legality issues, but due to being blocked by the company you're trying to scrape from. Web scraping tools like Selenium browse pages in light speed, way faster than any human, therefore it is easy for company's servers to spot when individuals are trying to scrape their sites. This next section will introduce a few options to intentionally slow down our browsing. Specifically we tell the driver object to wait until certain conditions have been met before moving forward. Once conditions have been met we download the HTML information returned by Google.

```
driver.get(url)
try:
    # wait for button to be enabled
    WebDriverWait(driver, delay).until(
        EC.element_to_be_clickable((By.CLASS_NAME, 'section-result'))
    )
    driver.implicitly_wait(40)
    html = driver.find_element_by_tag_name('html').get_attribute('innerHTML')
except TimeoutException:
    print('Loading took too much time!')
else:
    print('getting html')
    html = driver.page_source
finally:
    driver.quit()
```


Now that we have the HTML data, we feed this into Beautiful Soup for parsing, without having to worry about tripping up Google's servers.

```
soup = BeautifulSoup(html)
results = soup.find_all('a', {'class': 'section-result'})
```

Let's view one item from the results returned

```
results[0]

<a aria-label="Breads Bakery" class="section-result" data-result-index="1" data-section-id="ZXf8if7__or:1" jsaction="pane.resultSection.click;keydown:pane.resultSection.keydown...
```

As you can see, there is a ton of information returned but it is not in a human readable format.

To interpret this information we will have to go to a page from google maps and understand what our information is represented as in [HTML format](https://www.w3schools.com/whatis/whatis_htmldom.asp).

![](img/web_elements.png)

Above is an example of a restaurant in Brooklyn as seen from Google maps, alongside the HTML representation. To open up the pop-up, right click anywhere on a page in Google maps and click **inspect elements**. Now hovering over a certain piece of information will tell you what HTML tag, class or ID the element has been assigned. For example, the name of the restaurant above, Park Plaza, is in a **H3** tag inside another **span** tag (highlighted in bliue at the bottom). Now we know the HTML representation, we can more easily parse the data with Beautiful Soup to return the information we want.

Below we ask Beatiful Soup to find a H3 tag element and then get the text from the subsequent span element.

```
results[0].find('h3').span.text

'Breads Bakery'
```

We can now scale the above logic to get all the attributes we're interested in (name, address, price, tags, etc.), from all the zip codes, and all the POI categories. We'll wrap the POI parsing in a nested loop and save the results to a CSV using Pandas. We also add Python's built-in **try** and **except** objects to the HTML parsing logic so that the loop continues even when there data is not returned as expected (a common thing in web scraping). At the end of the loop we will delay the next loop by 15 seconds to not trigger Google's servers.

```
for search in searches:   
    for coord in zip(ys, xs):
        filename = search + str(coord[0]).replace('.', '') + '_' + str(coord[1]).replace('.', '') + '.csv'
        if not os.path.isfile('data/'+filename):
            url = base_url.format(search, coord[0], coord[1])
            delay = 10
            chrome_options = Options()
            chrome_options.add_argument("--headless")
            driver = webdriver.Chrome(ChromeDriverManager().install(),
                                       options=chrome_options)
            driver.get(url)

            try:
                # wait for button to be enabled
                WebDriverWait(driver, delay).until(
                    EC.element_to_be_clickable((By.CLASS_NAME, 'section-result'))
                )
                driver.implicitly_wait(40)
                html = driver.find_element_by_tag_name('html').get_attribute('innerHTML')
            except TimeoutException:
                print('Loading took too much time!')
            else:
                print('getting html')
                html = driver.page_source
            finally:
                driver.quit()
            soup = BeautifulSoup(html)
            results = soup.find_all('a', {'class': 'section-result'})
            data = []
            for item in results:
                try:
                    name = item.find('h3').span.text
                except:
                    name = None
                try:
                    rating = item.find('span', {'class': 'cards-rating-score'}).text
                except:
                    rating = None
                try:
                    num_reviews = item.find('span', {'class': 'section-result-num-ratings'}).text
                except:
                    num_reviews = None
                try:
                    cost = item.find('span', {'class': 'section-result-cost'}).text
                except:
                    cost = None
                try:
                    tags = item.find('span', {'class': 'section-result-details'}).text
                except:
                    tags = None
                try:
                    addr = item.find('span', {'class': 'section-result-location'}).text
                except:
                    addr = None
                try:
                    descrip = item.find('div', {'class': 'section-result-description'}).text
                except:
                    descrip = None
                data.append({'name': name, 'rating': rating, 'num_revs': num_reviews,
                             'cost': cost, 'tags': tags, 'address': addr,
                             'description': descrip})
            temp_df = pd.DataFrame(data)
            temp_df.to_csv('data/' + filename)
            time.sleep(15)
```

End!
