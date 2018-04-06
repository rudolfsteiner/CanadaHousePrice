# Canada House Price Prediction (v0)

### Purpose: 
Predict what the value of a house should be.


### Dataset: 
A list of text files about historical residential real estate sales data from the MLS Calgory.


### 1. Technical decisions


### Preprocessing: 

### Features Extraction: 

#### 	Techniques: 

  Transform the raw text data into python dataframe and save as mls.csv. 

#### 	Features from the raw data: 

  {Address, Style, Bedrms, Zone, Area, Basement, Building Type, Sold Price, Heating Type, Baths Full, Amenities, 
  Roof Type, Goods Included, Foundation, Bedrms Above Grade, Flooring, Basement Development, List Price, 
  Fireplace, Land Use Code, Property Type, Tax Amount, MLS® Number, Heating Fuel, 
  Tot Flr Area A.G. (SF), Enclosed Parking, day_sold, base_description, Total Parking, 
  Features, dom, Site Influences, Listing Firm 1 Name, Yr Built, Community, Front Exposure, 
  Exterior, Parking, Beds Total, Construction Type, Tot Flr Area AG Metres, Baths Half, rooms}

####  Collect more data: 

  Get “Assessments by Community” dataset from https://data.calgory.ca. The dataset gives information
  about the median assessment value by community.

#### Data Cleaning: 

  Feature selected: {Address, Style, Bedrms, Basement, Building Type, Sold Price, Baths Full, 
  List Price, Tax Amount, MLS® Number, Tot Flr Area A.G. (SF), day_sold, Total Parking, 
  Site Influences, Yr Built, Community, Front Exposure, Parking, Beds Total, Construction Type, 
  Tot Flr Area AG Metres, Baths Half}. Other features are dropped because of domain knowledge, 
  some are dropped because I do not want to explore them in such a early stage.

  Remove error / outliners and null values. 

#### 	Decomposition: 

  Extract {Cul-De-Sac, Playground Nearby, Golf Nearby} from {Site Influences} Feature. Further 
  feature extraction may increase the performance of the models.

#### 	Aggregation: 

  Join MLS dataset and “Assessment by Community” dataset. I use the assessment value on year 2016. 
  I should use the assessment value based on Year(day_sold). However, I cannot get historical 
  assessment by community datasets. 

#### Machine Learning Ready: 

  Further remove features {Address,List Price, MLS® Number, day_sold*, Site Influences, Community} 
  and turn categorical features into dummy features.
  
  *Should explore the temporal trends in the next version

#### Models and Technique: 

  Input and Label: Use the feature {Sold Price} as label and others as input.
 
  Models: Linear Regression, Decision Tree, Random Forest, KNN, Gradient Boosting, Bagging

  Model selection metric: Root Mean Squared Error(RMSE) and R^2

####  Results:

  Use 10 fold cross-validation, the Bagging model gives the best performance. However, the performance 
  difference between Bagging, Gradient Boosting and Random Forest are minor. 



### 2. How those decisions would impact the business

#### Use the models:

  For the buyers: To determine under/overpriced properties currently on the market, compare the 
  predictions with the list prices. Predictions that are significantly above the actual asking 
  price may be underpriced properties, and the predictions that are significantly below the 
  actual asking price may be overpriced properties
  
  For the sellers: Use Predictions to estimate the market value of their properties.

#### Other concerns:
  
  If the business need explainability of the models, like how the market value was predicted, 
  Decision Tree and KNN are better choice, while Random Forest, Gradient Boosting, Bagging 
  are all ensemble-based models, thus can hardly achieve explainability.


### 3. To improve upon the model


### 	1. Collect more data and extract more features from the existing data,

  For example:

  Additional data: Historical Assessment by Community data, financial data(interest rate...), 
  additional geographical features(close to fire station, police station, park, shopping, 
  library, hospital...)

  Further features extraction: further extract features from {Amenities, Goods Included, 
  base_description, Flooring, Basement Development, Features, dom, Site Influences, rooms ...}

  Additionaly, the census and demographic data were not affected by financial circles.  while 
  the assessment data were not valuable during financial crisis. In the future, I will try to 
  use the census and financial data and discard the assessment data.


### 	2. Explore the temporal trends in housing price

  What’s more, if explainability and higher model performance are critical for the business, 
  I can design my own linear models. 


### 	3. Change the training and test dividing method and model evaluation strategy

  I should use the year 2017 sales data as test, and earlier years data as training. Because 
  of the data is temporal, I should use the earlier data to build the model and the later 
  data to test the model. 

  Technically, I cannot use cross-validation method (sklearn.model_selection) to evaluate the 
  model performance. Instead, I should use the prediction (sklearn.metrics.r2_score, 
  mean_squared_error) on the test set to calculate the RMSE and R^2 score.

  Besides, I think the python ensemble-based machine learning libraries (RandomForestRegressor, 
  GradientBoostingRegressor, BaggingRegressor) were not suitable for the task because of the 
  temporal correlation.
 

### 4. Program

Requirements: Python 3.6, numpy, pandas, matplotlib, sklearn

Run the ipython notebooks in the sequence: 

  1. Combine.ipynb
  2. Models.ipynb






