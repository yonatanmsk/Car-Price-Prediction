# Car-Price-Prediction

-------------------------------

### Description

In our project we research the correlations between different cars features and try to predict the price of a car
by given features.

----

### Data Collection

Using the libraries `requests` and `BeautifulSoup4` we crawl Craigslist website searching for cars, and their features
according to the categories which were noted in the `cars.toml` file.

```toml
[features]
types = [
    "Region",
    "Price",
    "Year",
    "Model",
    "condition",
    "Odometer",
    "Fuel type",
    "Transmission",
    "Paint color"
]
```
----

#### Preparation

First, we made a list of the regions URLs with a crawler.
Second, we entered a URL and made a list of cars URLs.
Third, we fetched the features we needed for every car URL.

```python
url = 'https://www.craigslist.org/about/sites'
page = requests.get(url, headers=headers)
page_soup = BeautifulSoup(page.content, 'html.parser')
url_soup = page_soup.find("div", class_="colmask")
url_list=[u['href'] for u in url_soup.find_all("a", href=True)]
```

```python
url = reg_url + 'd/cars-trucks/search/cta'
soup_cars_trucks = BeautifulSoup(requests.get(url, headers=headers).content)
button_next = {'href':'/d/cars-trucks/search/cta'}
while button_next is not None:
    url = region_url + button_next['href']
    soup_cars_trucks = BeautifulSoup(requests.get(url, headers=headers).content)
    car_urls = [car_url.get("href") for car_url in soup_cars_trucks.find_all("a", class_="result-title")]
    arr=[]
    for single_car_url in car_urls:
        try:
            arr.append(singleCarDict(single_car_url))
            time.sleep(3)
        except Exception:
            pass
    region_df = region_df.append(arr, ignore_index=True)
    button_next = soup_cars_trucks.find("a", class_="button next")
```

----

#### Scraping

Now that we have a cars dataframe, we are able to continue to our next step - data cleaning.

```python
for region_url in url_list:
    main_df = main_df.append(regionDF(region_url))
```

```python
soup_cars = BeautifulSoup(requests.get(url, headers=headers).content)
dictionary['Region'] = url.split(".")[0].split("/")[2]
car_price = soup_cars.find("span", class_="price")
if car_price is not None:
    dictionary['Price'] = car_price.text.replace(',', '').replace('$', '')
...
```

----

### Data Cleaning

For each of our features (columns), we removed rows with NULL features. After that we analyzed the names of each model and fixed misspelling names, 
and then we removed outliers and continued to our next step - visualization.

----

### Visualization

Now that our data is cleaned we started to make plots, graphs and charts to see our data properly.


----

### Data Prediction

We ran multiple traditional machine learning algorithms against our data. We found that using the `Linear Regression`
method gave us the best results for the `Car price prediction` project standing at about `~75%` accuracy.

As a result, we can conclude that linear algorithms worked best for our data type.
