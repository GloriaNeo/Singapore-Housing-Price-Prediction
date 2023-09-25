![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) 
## Project 2 - Singapore Housing Data and Kaggle Challenge

---

### Contents

A) Executive Summary<br>
B) Problem Statement<br>
C) Datasets<br>
D) Workflow<br>
E) Summary<br>
F) Conclusion<br>

---

### A) Executive Summary
You have been hired by Carousell Property, a Singapore-based property platform that allows property owners in Singapore to sell or rent out property independently, without a real estate agent. For the layperson, there are potential benefits and challenges to selling a house without the involvement of a real estate agent, also known as "For Sale By Owner" (FSBO). Carousell property seeks to provide a user-friendly and accessible platform that alleviates the challenges including Comparative Market Analysis (CMA), Marketing and Price Negotiation (typically services provided by real estate agents); while allowing users to reap the main benefit of cost savings through omitting agent commission fees. As the marketplace caters to the layperson, Carousell Property is seeking to create a regression model based on the Singapore Housing Dataset to enable home-owners to better price their properties based on data-driven insights.

This Dataset provided is exceptionally detailed, with 76 columns (excluding resale_price) of different features relating to the property. This includes features of the unit itself, features of the development that the unit is located within, and surrounding amenities in the vicinity. The data was first cleaned, dealing with duplicates and null values. Exploratory data analysis and feature selectin was split between numerical and categorical features. Features were screened, and dropped should they provide minimal or no predictive value, or if they share multicollinearity with other features.

42 final features were selected, a reduction from the earlier 76 columns. 3 models were created based on these features, but the ridge model gave the best R-squared and RMSE score. This shows the model can be used to predict house prices in Singapore based on features, with quite a high accuracy rate.


---
### B) Problem Statement

Carousell Property seeks to pinpoint the features from the Singapore Housing Dataset that have influence on resale prices of properties in Singapore, through the implementation of a regression model. The model can be implemented as a new feature on the platform, enabling users to plug in some feature values to gain predicted resale price and some summarised insights. This would assist layman property owners in identifying potential key selling features in their properties, and arrive at a data-driven conclusion on property pricing.<br>

Some guiding questions:<br>

* What is the general trend in the housing resale price market?
* Which features add the most value to housing resale price?
* Based on a set of identified features, what is the expected resale price of a property?
<br>

Success can be evaluated through the model metrics of RMSE as well as the kaggle submission score, to see how accurately our model is able to predict resale property prices. After implementation, Carousell Property can evaluate their userbase to compare the following performance metrics:
* If there is any increase in user base numbers?
* If there is any increase in number of successful transactions?
* User ratings of the platform

---

### C) Datasets

* [`train`](./data/train.csv): Training data for your model.
* [`test`](./data/test.csv): Test data for your model. Target variable resale_price has been omitted from this dataset.
* [`train_clean`](./pkl/train_clean.pkl): Train data post-cleaning.
* [`train_sel`](./pkl/train_sel.pkl): Train data post- feature selection.
* [`subm`](./data/output/subm.csv): Final export for kaggle submission.

---
### D) Workflow

Due to the scale of this project, the main processes / segments of the project were split up into separate notebooks. There are 5 notebooks at the moment:
* Notebook_1 - Data import and cleaning for train.csv is done here, and then exported as 'train_clean.pkl'
* Notebook_2_1 and 2_2 - 'train_clean.pkl' is imported, and used for further data cleaning, feature selection, feature engineering and EDA. Most of the transformations involving the train dataframe that contributes to the final regression model is done in notebook_2_1. Notebook_2_2 serves as supplementary EDA, or further justifications to the cleaning process that were not included in notebook_2_1.
* Notebook_3 - Train-test-split, OHE, Scaling and then modelling + hyperparameter tuning is done here. Thereafter, the post-transformed test set ('test_final.pkl') is imported in, y-predictions are derived, and conversion to kaggle submission format is done.
* Notebook_4 - Cleaning, transformations (dropping columns, OHE, Scale) is done to 'test.csv' dataset in this notebook.
<br>

---
### E) Summary

#### Part 1 - Data Import and Cleaning

The train dataset was generally quite clean, with only 7 out of 77 columns having null values. There were no duplicate rows in the dataset as well. Upon preliminary study of the datasets, we observe that the null values appear in features that are referring to surrounding amenities near the property. As such, we can conclude that the null values are not actually missing input, but they simply represent unavailability of the said amenity. We also concluded these nulls are Not Missing at Random (NMAR). 

The method of deductive imputation was used - all NaN to be replaced with 0 (since it was observed all null values are numerical in nature).


#### Part 2 - Exploratory Data Analysis

Exploratory data analysis and feature selected is broken down into 2 notebooks:<br>
Notebook 2_1 includes most of the data preparations and transformations that contribute to the final regression model, Notebook_2_2 includes supplementary EDA, or further justifications to the cleaning process that were not included in notebook_2_1.
<br>

* EDA to understand general resale price trends<br>
First, we looked at the target variable - resale prices. EDA was done specifically to look at resale price trends over time, and resale price distributions. This would enable us to address some aspects of the problem statement, in providing some basic comparative housing analysis to the users of carousell property.

* Dataset split for further analysis - numerical vs categorical features
    * Numerical features were plotted for 1) correlation amongst numerical features and 2) correlation between numerical features and resale price.
    * Categorical features were analysed for multicollinearity. also Carried out EDA for key features such as flat type and flat model, planning areas <br>

* Final feature selection
    * The final selection consisted of 42 features, of which 27 are numerical features and 15 are categorical features.


#### Part 3 - Modelling

* Benchmark Modelling<br>
Floor area shares the most direct positive correlation to resale price, hence a benchmark model was created as a preliminary gauge of the benchmark RMSE and cross_val score that would be achieved.

* Pre-modelling Preparation<br>
    * The cleaned train dataset was split via train-test-split. The numerical and categorical features were then separated respectively within the train and test set.
    * One hot encoding was done to the categorical features to convert these categorical data variables into numbers that can be applied to train our regression model.
    * Standard scaling was applied to our numerical features to ensure variance is reduced before training our Lasso and ridge models. This is because the ridge and lasso models place a penalty on the magnitude of the coefficients of each variable, for example coefficients of variables with a large variance are small and thus less penalized.
    * The transformed numerical and categorical features are then combined again for train and test set respectively.


* Models and Model Selection
    * Linear, ridge and lasso regression was performed with the train and test holdout set, and thereafter evaluated via the metrics (R-square, cross-val and RMSE scores)
<br>

* Conclusion
    * Overall conclusion of findings from the EDA and model
<br>
* Kaggle Export<br>
    * The final cleaned and transformed test.csv data was brought in and prepared in the format for kaggle submission.

---
### F) Conclusion

#### Addressing the Problem Statement
In this project, we take a two-pronged approach in addressing the problem statement and its relevant questions. The EDA covers some basic comparative market analysis (CMA) that allows homeowners to have a better understanding of the market, and how certain features affect home resale price. The next part, the model itself, allows homeowners on the platform to input various values for the different variables, and the model would provide a prediction of potential resale price. These 2 approaches are able to mediate the knowledge gap that homeowners have if they decide to take a "For Sale By Owner" (FSBO) approach in selling their house. 

<br>

#### Findings from our EDA and Model
Through the EDA, here are some findings: 

1. Resale prices<br>
We see mean resale prices falling from mid 2013 to 2015, and then a drastic increase in mid 2020. This is proably due to government cooling measures, and changes in COVID-19 lifestyles respectively.

2. Distribution of resale prices<br>
Majority of homes are priced on the lower end, with a right-skewed pattern. This indicates that a significant number of homes have lower prices, while relatively fewer homes have higher prices. The median resale price, which is around $400,000, represents the middle value in the distributon.

3. Smaller flats of special models could outperform larger flats in terms of resale price
4. Properties located on higher storeys commanded a higher resale price.
6. Age of  property is negatively correlated to resale price.
7. Maximum floor level of the block that a unit is located in, is positively correlated to resale price.

<br>
The ridge model had near lowest RMSE for both the train and the test model, with very little variation between both scores. Test RMSE is just slightly greater than train RMSE, indicating almost no signs of underfitting, and an absence of overfitting. On the test data, the ridge model and its independent variables could explain 92.6% of variance in resale price. For our selected ridge model, the final test RMSE tells us that there could be a potential error amount of $38,718. However, to note that interpretability of this model is less effective as compared to the vanilla linear regression.


<br><br>

#### Limitations of the Model
The model we trained, is effective in providing a first cut estimation in predicting property resale price. However, we cannot deny that there several limitations of the model:

**1) Limitations of data collected<br>**
The kaggle dataset used for training the model has limitations in terms of its scope and recency. As the data only goes up to the year 2011, it does not capture the most recent market trends and developments. Real estate markets are dynamic and can experience significant changes over time, making the predictions less accurate for the current market scenario.

Another subpoint is that post-2021, we see the world facing another sociological shift as we resame a "new normal" after covid. This could make it difficult to train the model as there is no past precedences of these new changes that may impact housing.

**2) Limited features considered in the dataset<br>**
The dataset may not encompass all the relevant features that impact property resale prices. Other variables such as crime rates, or unique neighborhood characteristics (E.g. Yishun is stigmatized to be a weird part of Singapore) that could significantly influence property values. 

The kaggle dataset features encompass the HDB itself, the neighbourhood, and surrounding amenities. However, there are no features documenting characteristics of the current lived-in unit itself. For example, whether walls have been knocked down (so a 5-bedroom flat now only has 3 bedrooms), or whether the unit comes furnished or unfurnished, whether there are unique features within the house (e.g. owner installed a jacuzzi). These features may play quite a key role in influencing the resale price of the house.

**3) Changing Market Dynamics, Government policies<br>**
The housing market is subject to fluctuations and changing dynamics, which can affect the relevance of historical data. In Singapore, The Singaporean government often implements policy changes related to the housing market, such as cooling measures or tax reforms. These policy shifts can cause sudden fluctuations in housing prices, making it challenging for regression models to capture and adapt to such dynamic changes. The model's ability to predict future prices could be affected if the market experiences unexpected shifts, or should the government implement new policies impacting housing.


In general, housing prices often exhibit complex, non-linear relationships with predictors. No model would be able to capture such intricate patterns, so it is important to set a disclaimer to let owners know that the model can only act as a guideline.

<br>

#### Recommendations
Some recommendations include:
* **Data Enhancement<br>** Update the dataset with more recent data to reflect current market conditions. Consider obtaining real-time or periodic data on property transactions and market trends to capture the most up-to-date information.

* **Considering other property types<br>** Since carousell property caters to all homes in general, further models will have to be built for different categories of properties (e.g., HDB flats, private condominiums, landed properties). It is good that the models are separate for each property type to allow the models to capture the unique characteristics and price drivers of each property type more accurately.
  
* **Incorporate External Factors<br>** Take into account external economic indicators, interest rates, and government policies in the modeling process. These factors can significantly impact the real estate market, and incorporating them can improve the model's predictions under changing market dynamics.

