# Power Outage Characteristics In Urban and Rural Areas

## Introduction

### Our Dataset
Our dataset is information on significant power outages that took place in the continental U.S. 
The information about the power outages contains information about the region the power outages occurred, 
information about the power outage itself, and economic details from the state where the outage occurred. 

### Selected Question: Do power outages that occur in areas with high population density last longer than those that are in more sparsely populated areas?

The information in this dataset is important since it gives us a great insight into American infrastructure. This question is important because it can tell us if we are neglecting utilities' response for those in rural areas. 

The information in this dataset is important since it gives us a great insight into American infrastructure. This question is important because it can tell us if we are neglecting utilities' response for those in rural areas. 

There are 1534 rows in our dataset and 57 columns. We used a subset of those columns to answer our question.

These columns are: YEAR, MONTH, U.S._STATE, OUTAGE.START.DATE, OUTAGE.START.TIME, 
               OUTAGE.RESTORATION.DATE, OUTAGE.RESTORATION.TIME, OUTAGE.DURATION,
               POPULATION, POPPCT_URBAN, POPDEN_URBA', POPDEN_RURAL.

- YEAR: This is the year that the power outage took place
- MONTH: The month the outage took place
- U.S. State: This is the state (contintental) took place
- START.DATE: This is the date that the outage took place
- START.TIME: This is the time that the power outage began
- RESTORATION.DATE: This is the date that the power was turned back on and the outage ended
- RESTORATION.TIME: This is the time in the day that the power was turned back on
- DURATION: This is the time, represented as minutes, that the power outage lasted
- POPULATION: This is the population of the U.S. state that the power outage was located in. 
- POPPCT_URBAN: The percentage of residents in that state that are URBAN. The inverse of this statistic is the amount of people that are rural.
- POPDEN_URBAN: The density of those living within urban areas.
- POPDEN_RURAL: The density of those living within rural areas.
- START: The combined column of pd.Timestamp type from START.DATE and START.TIME
- RESTORATION: The combined column of pd.Timestamp type from RESTORATION.DATE and RESTORATION.TIME
- LENGTH: The total time difference of RESTORATION - START
 
## Cleaning and EDA

### Data Cleaning

#### Data Cleaing Steps
We did our initial dataframe analysis in Excel, where we identified columns that might be helpful to answering our question. Then, we downloaded the file as a .csv and added the .csv file to DataLore, where we performed more data cleaning. The first thing we did was get rid of the strings that descirbed the dataset, such as the title. We then set the columns of DataFrame and selected the ones we planned on using in our analysis. Then, we fixed some of the datatypes and calculated the length of the outages by turning OUTAGE.START and OUTAGE.END into datetime objects. We then calculated the length of the outages and turned it into minutes to be similar to the OUTAGE.DUARTION column that was provided to us.
#### Head of Cleaned DataFrame

|   YEAR |   MONTH | U.S._STATE   | OUTAGE.START.DATE         | OUTAGE.START.TIME   | OUTAGE.RESTORATION.DATE    | OUTAGE.RESTORATION.TIME   |   OUTAGE.DURATION |   POPULATION |   POPPCT_URBAN |   POPDEN_URBAN |   POPDEN_RURAL | OUTAGE.START        | OUTAGE.RESTORATION   |   OUTAGE.LENGTH | urban   |
|-------:|--------:|:-------------|:--------------------------|:--------------------|:---------------------------|:--------------------------|------------------:|-------------:|---------------:|---------------:|---------------:|:--------------------|:---------------------|----------------:|:--------|
|   2011 |       7 | Minnesota    | Friday, July 01, 2011     | 5:00:00 PM          | Sunday, July 03, 2011      | 8:00:00 PM                |              3060 |      5348119 |          73.27 |           2279 |           18.2 | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |            3060 | Rural   |
|   2014 |       5 | Minnesota    | Sunday, May 11, 2014      | 6:38:00 PM          | Sunday, May 11, 2014       | 6:39:00 PM                |                 1 |      5457125 |          73.27 |           2279 |           18.2 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |               1 | Rural   |
|   2010 |      10 | Minnesota    | Tuesday, October 26, 2010 | 8:00:00 PM          | Thursday, October 28, 2010 | 10:00:00 PM               |              3000 |      5310903 |          73.27 |           2279 |           18.2 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |            3000 | Rural   |
|   2012 |       6 | Minnesota    | Tuesday, June 19, 2012    | 4:30:00 AM          | Wednesday, June 20, 2012   | 11:00:00 PM               |              2550 |      5380443 |          73.27 |           2279 |           18.2 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |            2550 | Rural   |
|   2015 |       7 | Minnesota    | Saturday, July 18, 2015   | 2:00:00 AM          | Sunday, July 19, 2015      | 7:00:00 AM                |              1740 |      5489594 |          73.27 |           2279 |           18.2 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |            1740 | Rural   |

### Univariate Analysis

#### Plot Showing Distribution of Single Column
<iframe src='assets/outage-duration-histogram.html' width=800 height = 600 frameBorder=0></iframe>
#### Description of Plot
This histogram shows the distirbution of outages in our dataset. We can see that the majority of outages are very short, and there some enourmous outliers. We decided to remove the outliers:
<iframe src='assets/outage-duration-no-outliers-histogram.html' width = 800 height = 600 frameBorder=0></iframe>

###### X: OUTAGE_DURATION

After removing the outliers that are outside 2 standard deviations, we can see that not much of a difference was made to the overall distribution of outage durations. Therefore, we chose to continue using the duration with outliers since it these large outliers might still help us answer our question if they are evidence of slow repsonse times in rural areas.
### Bivariate Analysis

#### Plot Showing Relationship Between Two Columns
<iframe src='assets/duration-poppcturban-scatter-plot.html' width = 800 height = 600 frameBorder=0></iframe>

#### Interpert Plot
This plot is used to compare the data in the OUTAGE.DURATION and OUTAGE.LENGTH column and reveals that they are nearly identical. We plotted this relationship to ensure that we accurated calculated the length of the power outages since answering our question accurately depends on this value. 
### Interesting Aggregates

#### Grouped Table / Pivot Table

| urban   |   OUTAGE.DURATION |
|:--------|------------------:|
| Rural   |              1219 |
| Urban   |               480 |

####  Pivot Table Interpretation
This table has signifigant importance since it shows that power outages that occur in urban areas have a median value that is much higher than ones that occur in predominantely rural areas. We still need to run a hypothesis test to determine if this is due to random chance or not. 

#### Grouped Table / Pivot Table

| urban   |    1.0 |    2.0 |   3.0 |    4.0 |    5.0 |   6.0 |   7.0 |   8.0 |   9.0 |   10.0 |   11.0 |   12.0 |
|:--------|-------:|-------:|------:|-------:|-------:|------:|------:|------:|------:|-------:|-------:|-------:|
| Rural   | 1920   | 1181   |   251 | 1219   | 1260.5 | 547.5 | 960   |  1381 |  1528 |  833.5 |   2363 |   1898 |
| Urban   |  362.5 |  512.5 |   331 |  240.5 |  270   | 437   | 402.5 |   738 |  1423 |  960.5 |    193 |   1440 |

####  Pivot Table Interpretation
This table reveals that there is a large amount of variation between different months regarding the length of the power outage. However, there is not a consistent takeaway from this plot for it to alter our approach.

## Assessment of Missingnes

### NMAR Analysis

#### State Whether There Is A Column That Is NMAR

A column that may be NMAR is the 'CAUSE.CATEGORY.DETAIL' column, because this column is meant to give a detailed account regarding the cause of the power outage. The reason why this column is NMAR is because the NA entries may be due to negligence and laziness from the worker at hand during the outage. The worker's work ethic isn't measured in any other category/observed data but rather in unobserved data (work ethic/propensity to not follow protocol).The data isn't MAR or MD either because we can't predict the missingness based on any of the observed data we're collecting. It could be MCAR depending on how we frame it as we can either see the missingness as a result of negligence (NMAR) or we can see it as a result of system failure (MCAR) that wiped out results randomly. In our case, we are claiming negligence/laziness so that's NMAR. Some data that could help would be a column of prior history of successfully completed detailed reports of cause. This might possibly help predict and explain future missing data for the 'CAUSE.CATEGORY.DETAIL' column.


#### Results of Missingness Permutation Tests With Respect To Data

#### Plot Related To Missingness 
<iframe src='assets/kdeplot.html' width = 800 height = 600 frameBorder=0></iframe>
### Missingness Dependency

#### Present and Interperet Results of Missingness Permutation Tests
**Column selected: OUTAGE.DURATION

**Columns that selected doesn't depend on: POPULATION

First, we plotted the above plot with the population means when duration is missing vs not missing to see if the means had a similar shape and after we determined they were pretty close, we used a ks statistic test instead of a differences in means test. After running a ks-statistic test on 'OUTAGE.DURATION' and 'POPULATION' we got a p_value of 0.35, which is greater than our significance level of 0.05, so we fail to reject our null hypothesis that the distribution of population with duration missing is not the same as the distribution of population when duration is not missing.

**Columns that selected does depend on: POPDEN.RURAL

We plotted the means of POPDEN.RURAL when outage duration was missng vs not missing and decided we needed to run a ks-statistic test. After running a ks-statistic test on 'OUTAGE.DURATION' and 'POPDEN.RURAL' we got a p_value of 0.001103256120996406, which is less than our significance level of 0.05, so we are able to reject our null hypothesis that the distribution of the 'population density of the rural areas' with duration missing is not the same as the distribution of  the 'population density of the rural areas' when duration is not missing.

## Hypothesis Testing

### Hypothesis Testing
We are performing a Permutation Test-> Is the duration of power outages in rural areas equal to the duration of power outages in urban areas.
#### State Null and Alternative Hypotheses

#### Null Hypotheses: Whether or not the power outage occurred in a rural or urban area is not related to the duration of the power outage and any difference in this is due to chance.

#### Alternative Hypothesis: Whether or not the power outage occurred in a rural or urban area is related to the duration of the power outage and power outages in urban areas do last longer. 


#### Choice of Test Statistic

Our test statistic is the difference in means of the duration of the power outage between rural and urban outages. These accurately reflects the question we are trying to answer since it directly compares the numerical OUTAGE.DURATION columns.

#### P Value and Conclusion

We selected our p-value to be 0.05 since this is considered the industry standard and precise enough for our experiment. 

Since our p_value was much lower than our cutoff, we believe that there is an arguement to reject the null hypothesis. This leads us to believe that power outages in rural areas might last longer. However, we are not able to make a surefire statement since we are just performing statistical tests.

This result gives us insight into the areas we might want to prioritize with infastructure improvements in the United States. However, there are many other factors that could affect the duration in rural areas, such as difficult to reach equipment. 


# Power Outage Model

## Framing The Problem

### Our Prediction Problem/ Type:

Our prediction problem is determinging the duration of a power outage based on it's characteristics. This is a regression problem.

### Response Variable:
We are trying to predict the duration of a power outage given the information we have about the outage. 

### Metric:
We are using R^2 as our metric for model performance. We chose this metric since it accurately measures how well our model is fit to the data. R^2 is a also good metric for our problem since we are trying to predict a continuous variable.We also used this metric since it is built in the .score() method of sklearn. Other methods require additional methods and are not built into the library we are using. 

### Justify Information Known At "Time of Prediction"
We carefully selected features from the dataset that were only known when the power outage had just started. These are all variables such as the location of the power outage, the start date, and climate information of the power outage. These are all information that is readily available to us when the power outage has just started.


## Baseline Model

### Features and Description of Model
Our first model is very simple and does basic feature engineering and uses a limited amount of data. We used the following features:
Population:
- Population of the state the power outage is in
Cause Category: 
- The cause of the power outage
Population urban:
- The percentage of the population in the state that is urban

We have two quantitative features and one categorical feature that is nominal. 

We performed the neccessary encoding of the cause.category column. Obviously, this was a categorical column so we used the OneHotEncoder in our pipeline. 


### Model Performance

Our current model is very bad since it is very overfit. We need to add more features, perform feature engineering, and optimize our tree depth to improve our model. Our model would average a training R^2 of .70 and testing R^2 of .05. This model is very overfit and does not generalize well to new data.

## Final Model

### Features Added With Reasoning
We added the following features to our model:
- Month: The month the power outage started. This feature might of impropved our perfromance since winter months might have longer power outages than summer months. 
- Climate Category: The climate category of the state the power outage is in. This feature might have improved our performance since some climates might have longer lasting outages than others.
- Anomaly Level: This gives us more weather information about the power outage's location. We believe that worse weather might make it harder for power outages to be fixed, increasing the length of the outage.

### Model Chosen, Feature Engineering, and Hyperparameters
We chose to stick with the Decision Tree Regressor since it is a very simple model that is easy to interpret. We also continued to use the OneHotEncoder for our categorical features.
We also binarized the POPDEN_RURAL variable since it was a continuous variable. We determined the cutoff for this variable in project 3, and binarized it since you can seperate states into rural and urban based on this variable.
We also quantailized the Population feature since it was a continuous variable. This allows for the model to better understand the relationship between population and the duration of the power outage.

For our feature engineering, we used GridSearchCV to find the best hyperparameters for our model. We used the following hyperparameters: max_depth, min_samples_split. These are good hyperparameters since they prevent overfitting. The hyperparamters we ended up using are max_depth=3 and min_samples_split=20. These hyperparameters are good since they prevent overfitting and allow the model to generalize well to new data.

### Model Performance Compared to Baseline

Although are training R^2 is lower than our baseline model, our testing R^2 is higher. This means that our model is not overfit and is able to generalize well to new data. Our model would average a training R^2 of .25 and testing R^2 of .25. This model is better than our baseline model since the testing R^2 is higher.

## Fairness Analysis

#### Our choice for Group X:
Power outages that occur in the months 1-6.

#### Our choice for Group Y:
Power outages that occur in the months 7-12.

### Evaluation Metric

The evaluation metric that we are going to be using is the R^2.

#### Null Hypothesis:
Our model is fair. The R^2 for Group X and Group Y are roughly the same, and any differences are due to random chance.

#### Alternative Hypothesis:
Our model is unfair. The R^2 for Group X is higher than the R^2 for Group Y.

### Test Statistic, Signifigance Level, P-Value

For our test-statistic, we chose the difference between R^2 scores of Group X and Y.
Our alpha or significance level that we are going to be measuring our strength of evidence with is 0.05.
After running our permutation tests, we get a p-value of 0.322 which is greater than our significance level of 0.05.

<iframe src='assets/perm_test.html' width = 800 height = 600 frameBorder=0></iframe>

### Conclusion
Based on the evidence from our permutation test, we fail to reject the null because our p_value is higher than our significance level of 0.05. 
This evidence suggests that we cannot support the claim that the R^2 for Group X, our "early" months (1-6), 
is higher than the R^2 for Group Y, our "later" months (7-12).