# JamToApple - Airbag Deployment Rate Analysis

## Project Overview
Our project analyzes various factors in car accidents to determine which factors are the most valuable for identifying successful airbag deployments. More specifically, we want to find out under what circumstances fail to deploy in potentially fatal accidents and what types of vehicles are more prone to airbag failures. This is especially valuable given how important driving is to most people's everyday lives and how airbags can save lives in an accident. If an airbag does not deploy under some crash conditions when they are supposed to, innocent lives could be ruined due to a manufacturing flaw. These findings aim to support safer vehicle designs. 

## Team 
- Jada Giddens
- Austin Dinh
- Tyler Yu

## Datasets
### Sources
The datasets used are collected annually from the National Highway Traffic Safety Administration (NHTSA) in their [Crash Report Sampling System (CRSS)](https://www.nhtsa.gov/file-downloads?p=nhtsa/downloads/CRSS/). This system collects every piece of information about every (reported) accident in a given year, with advanced information about every vehicle and person involved. More specifically, only the *accident*, *person*, and *vehicle*  sources were used. These all provide different information about airbag deployments in an accident. For this project, we chose to only use the 2023 data.

The *accident* dataset  provides a high level overview of an accident, such as when and where it occurred, what type of an accident (head on collision, tree, pedestrian, etc). Only one row is provided per accident.

The *person* dataset provides information about every person involved in an accident, including passengers of each vehicle. It lists basic details such as age and gender, and more advanced information such as seat position and if their airbag deployed.

The *vehicle* dataset gives details of every vehicle involved in an accident. This includes the make/model, how fast it was travelling at time of the accident, etc. 

### Dataset Details
| Dataset Name| Rows    | Features |
| ----------- | ------- |----------| 
| accident    | 50103   | 80       |
| person      | 122388  | 112      |
| vehicle     | 87461   | 169      |
| merged      | 79630   | 61       |
| final       | 36402   | 66       |

### Data Dictionary
A more detailed dictionary can be found officially [here](https://crashstats.nhtsa.dot.gov/Api/Public/ViewPublication/813707). 

The CRSS datasets provide information through two ways: a numeric column mapping an index to a key (found in data manual above) or a typed column indicating the key. In addition, missing values are expressed through specific indexes (often 9, 98, 99) or "Unknown". 

For example: 
| REGION | REGIONNAME | NUM_INJ | NUM_INJNAME                                  |
| ------ | ---------- | ------- | -------------------------------------------- |
| 2      | Midwest    | 0       | No person injured/property damage only crash |
| 3      | South      | 1       | 1                                            | 
| 1      | Northeast  | 99      | All persons in crash are unknown if injured  |

### Important Variables
While more variables were used for data visualizations, these are the variables used for our final model:

#### Included in Dataset:
Pages are listed to the above manual if encoded with an index (maps string type to integer)
- *MAKE* - manufacturer of vehicle (pg. 99)
- *MOD_YEAR* - vehicle model year
- *BODY_TYP* - type of car i.e pickup, sedan, etc (pg. 105)
- *IMPACT1* - initial impact point (pg. 135)
- *FRONTAL_IMPACT* - if the initial impact was in the front (0/1)
- *ROLLOVER* - if vehicle rolled over (pg. 133)
- *VSURCOND* - roadway conditions (pg. 159)
- *MAN_COLL* - type of collision i.e t-bone, rear-end, etc (pg. 67)
- *LGT_COND* - light condition (pg. 75)
- *WEATHER* - type of weather (pg. 76)

#### Engineered Features
- *TOTAL_OCCUPANTS* - total number of people in vehicle
- *AVG_AGE* - average age of people in vehicle
- *ANY_DRINKING* - if anyone had a BAC > 0.0
- *ANY_UNRESTRAINED* - if anyone had no seat belt on 
- *HIGH_SPEED_CRASH* - travel speed >= 30 mph
- *MIN_AIRBAG_SPEED* - travel speed >= 20 (when most airbags deploy)
- *CRASH_SEVERITY_SCORE* - calculated score between 0-100 based on speed and impact location

### Data Cleaning and Preprocessing
- Removed implict missing values (marked as Unknown) only from variables used in final model
- No explicit missing values / NaNs in dataset
- Removed motorcycle data 

### Methodology
#### Exploratory Data Analysis
- Stacked bar plots to visualize when and where accidents occur / airbag deployment correlations
- Histogram of airbag deployment rates by make/models

#### Models
- Logistic regression (baseline model)
- Random Forest
- XGBoost
    - Hyperparameter tuning (GridSearchCV)

### Evaluation Metrics
- Accuracy
- F1-score
- Confusion matrix 

### Key Findings
- Random forest and XGBoost models performed relatively the same in terms of accuracy
- Identified the initial impact point and if the vehicle was travelling over 20 miles per hour (average speed for airbags to deploy) to be some of the most important features for deciding if an airbag properly deployed
- Industry average for airbags not deploying is 18%  whereas our model identified 30% of cases where an airbag was not deployed but should have

### Future Work
- Include data from previous years (2016-2022) with a script to improve model reliability
- Evaluate airbag deployment rates by seat positions (driver, front passenger, back seats, etc)
- More features identifying the extent to which the vehicle was damaged
