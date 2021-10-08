
# How much can I charge for my place on AirBnB 

*Airbnb is one of the largest and most popular online peer-to-peer marketplaces in the United States. It’s one of the ways to earn extra income. However, how do you know how much your space with its unique combinations of features would be worth? How do you know if you undervalued or overvalued your space? In this projet, I will create a model to predict a market-supported value for nightly rental price for a new location in San Diego next month using features of pre-existing locations for rent on Airbnb*

## 1. Data
Data can be found on insideairbnb website (http://insideairbnb.com/get-the-data.html)
* listings.csv : detailed listings data
* calendar.csv : detailed calendar data
* reviews.csv : detailed review data
* listings_sum.csv : summary information and metrics for listing (good for visulalization)
* reviews_sum.csv : summary review data and listing ID (faciliate time based analystics and visialisations linked to a listing)
* neighbourhoods.csv : neighbourhood list for geo filter. Sourced from city or open sources GIS files
* neighbourhoods.geojson : GeoJSON file for the neighbourhoods of the city

## 2. Data Cleaning 

[Data Wrangling Notebook](https://github.com/Hienquang/AirbnbCapstone/blob/main/Notebook/Data%20Wrangling.ipynb)

This dataset is relatively cleanned and only required a small amout of cleaning

* **Problem 1:** There are a few listings which are priced at zero dollar. These listings also don't contains other useful information. **Solution:** These rows are dropped

* **Problem 2:** There are 4 columns that doesn't hold any values at all: 'license','bathrooms','neighbourhood_group_cleansed' and 'calendar_updated'.  **Solution:** These columns are also dropped

## 3. EDA

[EDA Notebook](https://github.com/Hienquang/AirbnbCapstone/blob/main/Notebook/Exploratory%20Data%20Analysis.ipynb)

* There is a higher concentration in Downtown San Diego  compared to other county
 
![](https://github.com/Hienquang/AirbnbCapstone/blob/main/pic/concentration.png)

* There are a few county where there is no listings

![](https://github.com/Hienquang/AirbnbCapstone/blob/main/pic/missing.png)

* Even though there is a higher concentration in Downtown San Diego, it's not where the highest average nightly rental is

![](https://github.com/Hienquang/AirbnbCapstone/blob/main/pic/high_price.png)

* As predicted, most pricing is very skewed

![](https://github.com/Hienquang/AirbnbCapstone/blob/main/pic/price.png)
![](https://github.com/Hienquang/AirbnbCapstone/blob/main/pic/price_under_1000.png)

## 4. Pre-processing

[Pre-processing Notebook](https://github.com/Hienquang/AirbnbCapstone/blob/main/Notebook/Pre-processing%20and%20training.ipynb)

Pre-processing work mostly include parsing out amentities listing and encoding categorical data. Object columns such as bio and description is converted to binary instead (whether a listing has a description or if it's left empty, etc. )

## 5. Modeling

[Modeling Notbook](https://github.com/Hienquang/AirbnbCapstone/blob/main/Notebook/Modeling_updated.ipynb)

Using Pycaret, I was able to test a few regression models at once to pick a baseline. The initial models weren't great because of how skewed rental pricing is. There are probably clusters of different type of listings which contributed to their price. At a glance, this dataset can be splitted into typical listings and luxury listings. It's unlikely that including luxury listings would help in predicting typical listing price. Therefore, dataset is filtered to included only listing that is priced $1000 or under. THis has drastically improved models performance accross the board.

The best performed model is Light Gradient Boosting Machine:
* MAE: 54.1981
* MSE: 8945.4116
* RMSE: 94.3155
* R2: 0.6664
* Time: 0.318 sec

## 6. Interpretation 
![](https://github.com/Hienquang/AirbnbCapstone/blob/main/pic/shap.png)
Some features have a much bigger impact on the pricing. Unsurprisingly, the number of bedrooms and bathrooms are on top of this list. The more bathrooms and bathrooms you have, the higher price you can charge for your place. The next important feature is number of reviews and not the review score itself. The more people reviews your place, the higher price you can command. Longitude is another interesting feature. Listings that are closer to the beach can ask for higher price than listings that are inland. 

## 7. Future Improvements

* In the future, I could spend more time and segmenting listings instead of just setting an arbitrary ceiling listing price at $1000
* I could also use NLP to analyse listing description and give it sentimental score instead of binary yes/no like here
* There might be certain amenities that is higher demand than others which could contribute to the price. This could also be analyzed further






