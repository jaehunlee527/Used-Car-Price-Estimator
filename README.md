## Used Car Price Listing Scraper & Price Prediction ##

### Introduction ###
From the onset of Coronavirus pandemic, the U.S used car market has seen unprecedented rate of increase in the second hand automobiles’ prices. This phenomenon is mainly attributed to two causes: decreased production by automakers due to microchip shortages and increased demand from consumers returning to work. With prices still surging fast, buying a car has become more difficult, as knowing the benchmark price is harder; I decided to build a price estimator for the potential buyers in the market who wish to know the base price for particular model and mileage.

&nbsp;
<p align="center">
  <img src="images/used_car_price.JPG" width="600" height="300">
</p>
<p align="center">  
  <em>Source: cargurus.com</em>
</p>

----

### Data 
All of the data has been scraped from [Cargurus](cargurus.com). It is one of the most widely used platforms for buying/selling used cars. 
Each row of the training data consists of 28 features and 1 target (Listed Price). Features include basic information about the vehicle including Model ID, MSRP, body type, transmission, include options and etc. 

----

### Summary
The project consists of three parts:

**1) Scraper that gathers listings of used and new cars**

**2) Feature Engineering, Model Training**
* Linear Regression
* Polynomial Regression
* Random Forest Regressor
* XGB Regressor
* Multilayer Perceptron

**3) Model Performance Analysis**

----

### Web Scraper 
**Used Car Scraper:** The goal is to collect used car listings on Cargurus. We would like our lists to include the inventories of various brands, models, types and seller states. First, using [uszipcode API](https://uszipcode.readthedocs.io/index.html), generate shuffled list of zipcodes of largest states in the U.S by population. The assumption is that in the areas with greater population, we will observe more supplies of automobiles with diverse types. It fetches information of automobiles listed on each search page. In my notebook, I searched 1000 pages (1000 different zipcode searches). The gathered data had a size of total 25,000 vehicles.

**MSRP Scraper:** Later I realized that nearly 70% of the inventories were missing the MSRP (manufacturer's suggested retail price) which is crucial in estimating the target. Accordingly, I added a scraper function that fetches MSRP for each model. 

----

### EDA
&nbsp;
<p align="center">
  <img src="images/used_car_range.JPG" width="700" height="400">
</p>

The price range is centered around $30,000 with mean price of $32,000 and median price of $29,500.

&nbsp;
<p align="center">
  <img src="images/used_car_corr.JPG" width="800" height="500">
</p>

Let's see how some features are correlated with the price, our target. As I anticipated, the MSRP displays by far the greatest correlation; it is the initial sticker price, and the prices on the second hand car market are expected to regress from that benchmark as the years/mileages increase. The yearold & mileage also showed strong negative correlation with respect to the price - the older and more used vehicles are, the more their values will depreciate. Finally, the options features such as navigation system, heated seats and Bluetooth also showed some degree of correlation with the prices as well.

What is also interesting about the plot above is the rectangle of blue in the bottom right section. When cars are manufactured in various trims, some higher package models are likely to have multiple options included as a bundle. Or, it's possible that the recent models (regardless of particular brands) contain what used to be extra features in the past.  

----

### Imputation

MSRP is the most important factor in price estimator, yet a lot of rows were missing this piece - 70% of the used car listings did not have MSRP filled out. With MSRP scraper, I gathered MSRP lists of each model and filled these entries. For the still missing rows, I used the mean MSRP value with respect to the model. These steps reduced the missing ratio to below 0.5%.  

For missing cateogrical features (Body Type & Fuel Type), they have been filled with the most frequent value with respect to the model ID. For numerical features (Owner Count & Accident Count), I filled in with the average values based on the YearOld feature since we would expect the number of previous owners and accident counts to increase proportionately as the cars become older.  

----

### Encoding

For columns with fewer unique values - transmission, body type - I used one hot encoding. For all the other categorical columns, target encoder has been used, as they displayed clear correlations with the listed price.

----

### Models

Out of 5 models trained, XGB Regressor displayed the best performances after hyper parameter tuning.

| Model | Mean Squared Error | Mean Absolute Error |
|---|---|---|
| Linear Regression | | |
| Polynomial Regression | | |
| Random Forest Regressor | | |
| XGB Regressor | | |
| Multilayer Perceptron | | |

### XGB Model Performance

&nbsp;
<p align="center">
  <img src="images/used_car_prediction.JPG" width="700" height="400">
</p>
