# Project4_submission
West Nile Virus predictions for Chicago 

# Team members
Adrian Koh,
Chong Zi Liang,
Chye Wei Chiat,
Wee Yuan Liang

# Fighting West Nile Virus in Chicago

The City of Chicago has been dealing with the challenge of West Nile Virus (WNV) since 2002. WNV is a mosquito-borne virus that brings about symptoms such as fever, vomiting and even death in serious cases.  

### Goal

City officials have hired us to predict where different species of mosquitos will test positive for WNV, so that the city can effectively plan and deploy resources (such as manpower, pesticides) to specific locations to limit the spread of the life-threatening virus and protect its residents.

### Data overview

The following data from 2007 to 2014 are available for our analysis.

- __Trap data__
  - Date of trap inspection
  - Location of traps (address, longitude, latitude)
  - Trap ID
  - Species and number of mosquitos caught
  - Presence of WNV among mosquitos caught
  
  
- __Weather data__
  - Temperature
  - Dew point
  - Wetbulb
  - Total Precipitation


- __Spray data__
  - Date of spray
  - Location of spray

## Content

- [Exploratory data analysis](#exploratory-data-analysis)
- [Data preprocessing](#data-cleaning)
- [Feature selection](#feature-selection)
- [Feature engineering](#feature-engineering)
- [Modelling and model selection](#modelling-and-model-selection)
- [Conclusion](#conclusion)

<a id="exploratory-data-analysis"></a>
## Exploratory data analysis

Apart from Python, we also used other visualization tools, namely Tableau and PowerBI, to map out out geographical data. We were able to determine:

- WNV was found spread throughout the city, no hot spots where virus is concentrated.
- Virus is found in about 5% of data collected.
- Correlation between number of mosquitos and whether conditions such as temperature and wetbulb reading.
- Sprays conducted in 2011 and 2013 overlapped location of traps and observed a drop in mosquito population in those areas.

<a id="data-cleaning"></a>
## Data preprocessing

#### Trap data:
- The feature NumMosquitos (indicating number of mosquitos caught) was dropped from the training set since the feature is absent in the test set.
- Dates were segmented to Day, Week, Month and Year values.
- Dummies features were created for mosquito species and trap ID.

#### Weather data:
- Weather data from station 1 and 2 do not differ to a large extent, i.e. within 3%. Hence, weather data from the two stations are averaged to represent as a whole.
- Missing values indicated with "M" were replaced with the preceding day's value for continuity, with the assumption that the weather conditions would not change drastically.
- Trace values indicated with "T" were replaced with zero value as we assume the values are negligible.

#### Spray data:
- Time of spray contained many missing values and was dropped from our analysis altogether.

<a id="feature-selection"></a>
## Feature selection

#### Trap features:
The following features are selected. Other features related to the trap location (e.g. Address, street, block, etc.) were not selected since the latitude and longitude will sufficiently describe the trap location.
- Date
- Species of mosquitos
- Trap ID
- Latitude/ Longitude
- Presence of WNV among mosquitos caught

#### Weather features:
Weather factors affecting mosquito breeding are temperature, humidity and precipitation - mosquitos thrive in hot and humid conditions, less so in cold and dry conditions. Hence, the following weather features are selected.
- Temperature (max, min, average)
- Dew point
- Wet bulb
- Total precipitation

<a id="feature-engineering"></a>
## Feature engineering

Binarize selected weather features by setting 1 if the value is above a certain threshold, and 0 if the value is below. 
https://towardsdatascience.com/a-go-at-kaggle-723447f8d95f

As the dataset is unbalanced, the features need to engineered to allow the model to be able to identify records with the virus present. A few features are set as binary indicators to indicate that the virus is present. For these features, average count of the virus that is present is used as the threshold.
The features used are Traps, Tavg, WetBulb and Dewpoint. Engineering the Traps features is useful to reduce the features compared to doing 1 hot encoding for Traps which results in many columns for each Trap IDs.

<a id="modelling-and-model-selection"></a>
## Modelling and model selection

The following models were experimented with:
-	LogisticRegression
-	DecisionTreeClassifier
-	GradientBoostingClassifier
-	AdaBoostClassifier
-	RandomForestClassifier
-	BaggingClassifier

Modelling was carried out on train data without any feature engineering performed. Gradient Boosting Classifier yields the best train results. The Gradient Boosting Classifier was used for the kaggle submission. This results in Kaggle public score of 0.72. The modelling was then carried out the data with the feature engineering performed. This results in a public score of 0.77. After performing Girdsearch, public score was improved to 0.78.

<a id="conclusion"></a>
## Conclusion

### Findings
- For the sake of public health, it is better to boost our true positive rate and incur a higher false positive rate so that we do not miss out WNV hot spots that will put public health at risk. False positives will be subjected to pesticide sprays, but assuming no harmful effects of the pesticide to humans, this is merely an inconvenience.
- However, the issue of budget constraints come into play. The city may have a limited budget to deploy pesticide sprays. This will have an impact on how many false positives we can accept.

### Limitations
Our data did not give us information on the type of land use such as industrial, commercial, residential can give insights to which land use is most susceptible to mosquito breeding and in turn, a higher number of mosquitos caught. The type of land use might give us clue to the degree of area cleanliness, which will affect mosquito breeding, as uncleared rubbish would serve as water receptacles, clogged drains will result in stagnant water bodies, etc. With this information, our model could possibly generate better predictions.

### Further exploration

- The pesticide sprays of 2011 and 2013 seem ineffectual. Do we need to experiment with different pesticides?
- Additional data on human infection of WNV in Chicago, together with these people's residential address will be useful in adding robustness to our model.
