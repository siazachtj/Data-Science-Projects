#  ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 2 - Singapore Housing Data and Kaggle Challenge

Our project 2 premise is we are property agency that has newly arrived in Singapore. We are trying to better understand one of our potential biggest sources of income: transacting resale flats. 


**Approach:**
1. Baseline model with slight modifications such as removing na values evaluation.
2. Evaluating ridge and lasso model with no additional feature engineering and hyper parameter tuning. 
3. Feature engineering/selection and hyper parameter tune using ridge_cv and lasso_cv.
4. Evaluation of results.
5. Conclusions and discussion.



## Baseline model

This Dataset is an exceptionally detailed one with over 70 columns of different features relating to houses.
Our baseline tried to take advantage of this by doing only basic cleaning such as removing columns with "na" values.


During our EDA we saw that the distribution of the resale_price was skewed to the left, meaning more of the resale_prices in the dataset were around the 300,000 to 500,00 sgd range.

In terms of observations when graphing the target variable "resale_price" to 
other factors, there were some linear relationships, however, there is much variance between datapoints.


## Linear Regression observations

We now instantiate a Linear Regression model and apply standardscaler as well as one hot encoder on our numeric and non-numeric values.

With a Xtest score of -531356238013872.8 and rmse of 3303384616829.148.Baseline model is grossly overfit and requires feature engineering. 

Our final result is 2777 columns which should be noted was quite taxing on the computer to run models, which result in some outputs requiring kaggle gpu utilisation.

#### Ridge and Lasso default
We use ridge and lasso with default values just to seem if there is any impact on our r squared and rmse values.
Suprisingly, the default values for both turn out to be quite strong with an 
Xtrain score of ~0.936 and ~0.936 score 
and
Xtest score of ~0.934 and ~0.933 score.

The rsme is also significantly lower, at ~36708 for Ridge and ~36822 for Lasso.


#### Feature selection for further modelling
We first remove all features that had a low  'P>|t|' value (greater than 0.05).

We also check for colinearity and remove functions that would impact results other than the ones we felt were necessary as they added value from our point of view (mrt long,lat, sec_school long,lat etc were untouched as we felt it added value to the model).

#### Reimputing missing values 
This was mainly out of attempting to explore further relationship between street_name/bus_stop_name and resale_price as features such as mrt and bus stop distance didn't seem to have as high a coefficent as the various bus stop names.

We readded Mall_Nearest_Distance with imputed median values, as the distributions per town (a proxy value we used for loaction) were inconsitent in terms of normality and skew.

We imputed postal code using only the first two numbers to try and add more context for one of our important factors, street_name.
We removed some categorical features as they were already dealt with by another feature such as full_flat_type being a combination of flat_model and flat_type or storey range being repeats of median values etc.
We removed bus stop although it was a strong performer for our model as there were just too many unique variables, making it hard to solution using one hot encoder.

#### Feature engineering
As max_floor_lvl and mid_storey both have high correlations to the resale_price, we feature engineer them together to hope to get an even stronger result.

We can observe a high correlation between resale_price and 5 rooms sold as well as 3 rooms sold so this feature hopes to combine these features for a stronger resulting model.

Our 4_5_ratio feature is the sum of 5 and 4 rooms sold divided by 5,4,3,2,1 rooms sold.

We have seen a strong coeff in our ridge model for street_name and bus_stop_name. In terms of living condition its logical that food and amenities, transport availability and distance from schools would be key features in determining resale price.

To generalise the model, we will be comparing most of these features to the mean value and returning a value of 0,1,2 depending on how many criteria are met in the respective sections.


### Data Dictionary before feature engineering/selection
|Feature|Description|dtype|
|---|---|---|
resale_price| the property's sale price in Singapore dollars. This is the target variable that you're trying to predict for this challenge.|int64
town| HDB township where the flat is located, e.g. BUKIT MERAH| object
flat_type| type of the resale flat unit, e.g. 3 ROOM| object
street_name| street name where the resale flat resides, e.g. TAMPINES ST 42| object
storey_range| floor level (range) of the resale flat unit, e.g. 07 TO 09| object
floor_area_sqm| floor area of the resale flat unit in square metres| float64
flat_model| HDB model of the resale flat, e.g. Multi Generation| object
lease_commence_date| commencement year of the flat unit's 99-year lease| int64
Tranc_Year| year of resale transaction| int64
Tranc_Month| month of resale transaction| int64
mid_storey| median value of storey_range| int64
lower| lower value of storey_range| int64
upper| upper value of storey_range| int64
mid| middle value of storey_range| int64
full_flat_type| combination of flat_type and flat_model| object
floor_area_sqft| floor area of the resale flat unit in square feet| int64
hdb_age| number of years from lease_commence_date to present year| int64
max_floor_lvl| highest floor of the resale flat| int64
year_completed| year which construction was completed for resale flat| int64
residential| boolean value if resale flat has residential units in the same block| object
commercial| boolean value if resale flat has commercial units in the same block| object
market_hawker| boolean value if resale flat has a market or hawker centre in the same block| object
multistorey_carpark| boolean value if resale flat has a multistorey carpark in the same block| object
precinct_pavilion| boolean value if resale flat has a pavilion in the same block| object
total_dwelling_units| total number of residential dwelling units in the resale flat| int64
1room_sold| number of 1-room residential units in the resale flat| int64
2room_sold| number of 2-room residential units in the resale flat| int64
3room_sold| number of 3-room residential units in the resale flat| int64
4room_sold| number of 4-room residential units in the resale flat| int64
5room_sold| number of 5-room residential units in the resale flat| int64
exec_sold| number of executive type residential units in the resale flat block| int64
multigen_sold| number of multi-generational type residential units in the resale flat block| int64
studio_apartment_sold| number of studio apartment type residential units in the resale flat block| int64
1room_rental| number of 1-room rental residential units in the resale flat block| int64
2room_rental| number of 2-room rental residential units in the resale flat block| int64
3room_rental| number of 3-room rental residential units in the resale flat block| int64
other_room_rental| number of "other" type rental residential units in the resale flat block| int64
Latitude| Latitude based on postal code| float64
Longitude| Longitude based on postal code| float64
Hawker_Nearest_Distance| distance (in metres) to the nearest hawker centre| float64
hawker_food_stalls| number of hawker food stalls in the nearest hawker centre| int64
hawker_market_stalls| number of hawker and market stalls in the nearest hawker centre| int64
mrt_nearest_distance| distance (in metres) to the nearest MRT station| float64
mrt_name| name of the nearest MRT station| object
bus_interchange| boolean value if the nearest MRT station is also a bus interchange| object
mrt_interchange| boolean value if the nearest MRT station is a train interchange station| object
mrt_latitude| latitude (in decimal degrees) of the the nearest MRT station| float64
mrt_longitude| longitude (in decimal degrees) of the nearest MRT station| float64
bus_stop_nearest_distance| distance (in metres) to the nearest bus stop| float64
bus_stop_name| name of the nearest bus stop| int64
bus_stop_latitude| latitude (in decimal degrees) of the the nearest bus stop| float64
bus_stop_longitude| longitude (in decimal degrees) of the nearest bus stop| float64
pri_sch_nearest_distance| distance (in metres) to the nearest primary school| float64
pri_sch_name| name of the nearest primary school| object
vacancy| number of vacancies in the nearest primary school| int64
pri_sch_affiliation| boolean value if the nearest primary school has a secondary school affiliation| int64
pri_sch_latitude| latitude (in decimal degrees) of the the nearest primary school| float64
pri_sch_longitude| longitude (in decimal degrees) of the nearest primary school| float64
sec_sch_nearest_dist| distance (in metres) to the nearest secondary school| float64
sec_sch_name| name of the nearest secondary school| object
cutoff_point| PSLE cutoff point of the nearest secondary school| int64
affiliation| boolean value if the nearest secondary school has an primary school affiliation| int64
sec_sch_latitude| latitude (in decimal degrees) of the the nearest secondary school| float64
sec_sch_longitude| longitude (in decimal degrees) of the nearest secondary school| float64

### Data Dictionary after feature engineering/selection
|Feature|Description|dtype|
|---|---|---|
resale_price| the property's sale price in Singapore dollars. This is the target variable that you're trying to predict for this challenge.|int64
town| HDB township where the flat is located, e.g. BUKIT MERAH| object
flat_type| type of the resale flat unit, e.g. 3 ROOM| object
street_name| street name where the resale flat resides, e.g. TAMPINES ST 42| object
storey_range| floor level (range) of the resale flat unit, e.g. 07 TO 09| object
floor_area_sqm| floor area of the resale flat unit in square metres| float64
flat_model| HDB model of the resale flat, e.g. Multi Generation| object
lease_commence_date| commencement year of the flat unit's 99-year lease| int64
Tranc_Year| year of resale transaction| int64
Tranc_Month| month of resale transaction| int64
mid_storey| median value of storey_range| int64
lower| lower value of storey_range| int64
upper| upper value of storey_range| int64
mid| middle value of storey_range| int64
full_flat_type| combination of flat_type and flat_model| object
floor_area_sqft| floor area of the resale flat unit in square feet| int64
hdb_age| number of years from lease_commence_date to present year| int64
max_floor_lvl| highest floor of the resale flat| int64
year_completed| year which construction was completed for resale flat| int64
residential| boolean value if resale flat has residential units in the same block| object
commercial| boolean value if resale flat has commercial units in the same block| object
market_hawker| boolean value if resale flat has a market or hawker centre in the same block| object
multistorey_carpark| boolean value if resale flat has a multistorey carpark in the same block| object
precinct_pavilion| boolean value if resale flat has a pavilion in the same block| object
total_dwelling_units| total number of residential dwelling units in the resale flat| int64
1room_sold| number of 1-room residential units in the resale flat| int64
2room_sold| number of 2-room residential units in the resale flat| int64
3room_sold| number of 3-room residential units in the resale flat| int64
4room_sold| number of 4-room residential units in the resale flat| int64
5room_sold| number of 5-room residential units in the resale flat| int64
exec_sold| number of executive type residential units in the resale flat block| int64
multigen_sold| number of multi-generational type residential units in the resale flat block| int64
studio_apartment_sold| number of studio apartment type residential units in the resale flat block| int64
1room_rental| number of 1-room rental residential units in the resale flat block| int64
2room_rental| number of 2-room rental residential units in the resale flat block| int64
3room_rental| number of 3-room rental residential units in the resale flat block| int64
other_room_rental| number of "other" type rental residential units in the resale flat block| int64
Latitude| Latitude based on postal code| float64
Longitude| Longitude based on postal code| float64
Hawker_Nearest_Distance| distance (in metres) to the nearest hawker centre| float64
hawker_food_stalls| number of hawker food stalls in the nearest hawker centre| int64
hawker_market_stalls| number of hawker and market stalls in the nearest hawker centre| int64
mrt_nearest_distance| distance (in metres) to the nearest MRT station| float64
mrt_name| name of the nearest MRT station| object
bus_interchange| boolean value if the nearest MRT station is also a bus interchange| object
mrt_interchange| boolean value if the nearest MRT station is a train interchange station| object
mrt_latitude| latitude (in decimal degrees) of the the nearest MRT station| float64
mrt_longitude| longitude (in decimal degrees) of the nearest MRT station| float64
bus_stop_nearest_distance| distance (in metres) to the nearest bus stop| float64
bus_stop_name| name of the nearest bus stop| int64
bus_stop_latitude| latitude (in decimal degrees) of the the nearest bus stop| float64
bus_stop_longitude| longitude (in decimal degrees) of the nearest bus stop| float64
pri_sch_nearest_distance| distance (in metres) to the nearest primary school| float64
pri_sch_name| name of the nearest primary school| object
vacancy| number of vacancies in the nearest primary school| int64
pri_sch_affiliation| boolean value if the nearest primary school has a secondary school affiliation| int64
pri_sch_latitude| latitude (in decimal degrees) of the the nearest primary school| float64
pri_sch_longitude| longitude (in decimal degrees) of the nearest primary school| float64
sec_sch_nearest_dist| distance (in metres) to the nearest secondary school| float64
sec_sch_name| name of the nearest secondary school| object
cutoff_point| PSLE cutoff point of the nearest secondary school| int64
affiliation| boolean value if the nearest secondary school has an primary school affiliation| int64
sec_sch_latitude| latitude (in decimal degrees) of the the nearest secondary school| float64
sec_sch_longitude| longitude (in decimal degrees) of the nearest secondary school| float64
Mall_Nearest_Distance| Mall_Nearest_Distance with imputed median in na values| float64
postal_number| first two digits of postal number| int64
storey_relative_max| median storey in storey_range divided by max_floor_lvl| float64
4_5_ratio| sum of 4 and 5 room flats sold divided by 5,4,3,2,1 room flats sold| float 64
food_and_ammen| value of 0,1,2 corresponding to if the resale flat has a greater distance to the nearest hawker centre and/or mall as compared to the mean hawker centre distance and mall distance in the dataset| float64
transport| value of 0,1,2 corresponding to if the resale flat has a greater distance to the nearest bus stop/mrt station as compared to the mean bus stop distance and mrt distance in the dataset| float64
sec_pri_distance | value of 0,1,2 corresponding to if the resale flat has a greater distance to the nearest primary/secondary school as compared to the mean secondary school and primary school distance in the dataset| float64

#### Final results
|Regression name|Xtrain.score|Xtest.score|rmse|alpha| cross_val_score.mean()
|---|---|---|---|---|---|
Linear Regression| 0.936498267702631|-531356238013872.8|3303384616829.148| NA| -5.8989
**Ridge(default)**| **0.9362975025749736**| **0.9343861104913228**| **36708.28896446506**| **1.0(default)**| 0.9328418459824075
Lasso(default)| 0.9355590385519732| 0.9339754061175116| 36822.9958990134| 1.0(default)| 0.9325061689853278
Ridge_CV| 0.9275783189933805| 0.9273417746949457| 38628.5710725679| 1.0| NA
Lasso_CV| 0.9276360506438114| 0.9274178422099526| 38608.34522157874| 0.23357214690901212| NA
 

### Submission to kaggle
Multiple submissions were made to kaggle.
Overall, the strongest model in terms of kaggle score was the default Ridge() model with minimal feature engineering. With a final score of 36770.12865 public and 36731.69337.

## Conclusions, further observations and improvements
The strongest conclusion that can be made from our study is that if the resale flat is in the tiong bahru area, it is likely to be costly. We can conclude that proximity to certain bus stops makes a difference in the resale price, perhaps due to missing factors such as the availability of bus routes to the central business district, where the majority of office workers commute.

Another potential explanation is perhaps there is an optimal radius around the central business district/office buildings wherein housing would tend to be more expensive, not exactly where the cbd/offices are, but is in commutable proximity. This observation is mainly on two permises:
* 1. Spaces such as Tiong bahru, Changi Village and jurong west (including tao ching rd) which are known to be commutable to major office spaces for certain sectors such as supply chain management and financial instiutions e.g. PSA, DBS (both offices in changi and in the CBD) etc.
* 2. Hawker centres and mall distance seemed to have a negliable impact in all models meaning other factors not recorded on this dataset might be at play.
An interesting future datapoint is perhaps number of office clusters along bus routes.

## Tying back to problem statement
Tiong bahru is one of the strongest contenders in terms of potential investment when acquiring resale flats to put to market. Factors such as availability of hawkers and mrts are not as strong a pull factor and we should therefore be looking out more for bus related trends. The type of house and number of rooms play a great factor in the decision to refurbish and sell as terrace houses have a strong impact on our strongest linear model for resale prices. Schooling seems to be a priority more towards the "Woodlands" area (Fuchun and Marsling primary have high coefficents to resale price).