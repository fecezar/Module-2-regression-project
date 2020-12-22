# Read Me

## Project Briefing:

Real estate fund beginning operations in Kings County wants to  use machine learning to build a portfolio of properties to buy for later sale or letting. 

They have asked for a study on the market and machine learning dynamics 

## Data Overview

The data used was King County House data

# Stakeholder, Questions and Assumptions

The stakeholders that will use the insights produced from this study are the management of a real estate fund looking to start a portfolio in the Kings County region. They want to implement machine learning to make investment decisions.

This notebook explores and cleans the raw data in order to answer the following questions:    

- **What are the most important predictors of a property's value?**

The underlying assumption is that a few predictors will have most of the explanatory power 
  
  


- **How do zip codes differ from each other in terms of best predictors?**

The underlying assumption is that zip codes have specific market dynamics.


- **What zip codes have the most predictable prices?**

The underlying assumption is that prices will be more predictable in certain zip codes. Those should be easier regions for the company to start its operations, since the risk of pricing would be lower. 
     

  

 # Data Exploration and Preparation

## Data Cleaning

- **waterfront and view**: missing values assumed to be 0

- **sqft_basement**: values  were recalculated to deal with missing or incorrect data. Inferred the from the difference between sqft_living and sqft_above

- **yr_renovated**: 95% of entries have a value of 0. It was assumed that missing or 0 values mean that the property has never been renovated. For the instances of missing or 0 values, we used the yr_built value

- **Outliers**: A significant number of  large outliers were observed.
Upon further investigation, it was found that all zip codes show high price outliers in a relatively equal frequency. Therefore the top 3% largest prices of each zip code were dropped 


### Multicollinearity

The following independent variables were found to be correlated:

- (sqft_above, sqft_basement): correlation of 	-0.93
    - sqft_basement was dropped, since it can be 100% inferred from other variables
- (yr_renovated, yr_depreciation_prior_to_sale): correlation of -0.99
    - yr_renovated was dropped

## Feature Engineering

- **sqft_above**: 

It was initially found to be collinear sqft_living and thus was redefined as a percentage of total living area, which solved the excess collinearity


- **yr_sold** and **month_sold**: 

Assuming that seasonality can be a relevant pattern for modeling, two new columns were created for the year and month of sale.

- **yr_depreciation_prior_to_sale**: 

It was calculated based on the difference between yr_sold and yr_renovated


- **avg_neighbor_price**: 

Common sense informs us that prices in the vicinity of a property are correlated to the price of the property itself. Therefore the average price of the close neighbors of a property was calculated and included in the data.  
Using latitude and longitude, the nearest n properties sold were found and their price computed towards an average. 
The n number of neighbors was optimized for maximum r-squared with 'price' 

## Data Visualization and Transformation

 ### Data Visualization 

The examination of price outliers suggested that eliminating the highest values could be beneficial for the quality of prediction in our models.

![outliers.png](https://github.com/fecezar/Module-2-regression-project/blob/master/outliers.png)

Also, it is clear that outliers should be evaluated in regards to the prices of each zip code

![outliers%20zipcodes.png](attachment:outliers%20zipcodes.png)

Upon visualizing their distributions and relationship with the dependent variable, some variables were found to behave like categories, and others were transformed to have a distribution closer to normality



![variable%20distributions.png](attachment:variable%20distributions.png)

### Data Transformation

Having visualized the data, the following decisions were made:

- Variables converted to categories (dummy variables):
floors,  waterfront, view, month_sold, condition, yr_built, zip code

- Variables to undergo log transformation:
avg_neighbor_price, bathrooms, sqft_living, bedrooms, sqft_living15

Additionally, all non categorical variables were rescaled by process of normalization

# Variable Selection and Model Training

A linear regression model for maximum prediction power was created using a forward variable selection process that maximizes Adjusted R-Squared. In other words, all variables that contributed to a higher Adjusted R-Squared, as long as their p-value was below the alpha of 5% and levels of collinearity were below the 75% correlation threshold.

 

# Conclusions



## What are the most important predictors of a property's value?



The top 3 most important predictors are the features **waterfront, avg_neighbor_price and sqft_living**

![top3.png](attachment:top3.png)


 Variable |Coefficient | Error  
 ------------|------------|------------
waterfront[T.1] | 257,448 | 	49,637.15
avg_neighbor_price |193,189 | 2,600.95 
sqft_living | 97,588.52| 2,609.09	


- **Conclusion**: The top 3 most important predictors are the features "waterfront", "avg_neighbor_price" and "sqft_living"

## How do zip codes differ from each other in terms of best predictors?


![top%203%20var%20zipcodes.png](attachment:top%203%20var%20zipcodes.png)

- **Conclusion**: the top variables vary significantly across different zip codes. However, it is clear that the one that are ubiquitous are **sqft_living, avg_neighbor_price, sqft_lot, grade, and sqft_lot15**.

This variability indicates that individualized modeling for each zip code is recommended.    

## What zip codes have the most predictable prices?

The underlying assumption is the predictability of prices will vary from region to region.


![predictable%20prices.png](attachment:predictable%20prices.png)

zip codes with typical error above $ 150,000:

[98040, 98119, 98112, 98105, 98004, 98102, 98109, 98033, 98039]

- **Conclusion**: with the exception of the 9 zip codes listed above all of them have similar typical errors, and therefore present comparable risks in this metric. 

It would be interesting to see if zip code specific models would eliminate this discrepancy

## Future Work

It would be interesting to see if zip code specific models would eliminate error discrepancies, as it is suggested by the variability in top predictors for different zip codes




```python

```
