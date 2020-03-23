
# Hackathon-Good-Fast-Cheap  

teammates (Scott Rosengrants , Megha Zavar)
### Organization
This repositories contains data folder and 2 jupyter notebooksone with EDA and another one for buiding the model.


# Goal

The project was completed as part of a 1 day (8hour) Hackathon ,with a goal to create the best performing predictive model 
on a hold-out sample of data to predict whether income exceeds $50K/yr based on census data. Also known as "Census Income" dataset.
Our team was presented with following contraint .More details on project statement can be found ![here](https://git.generalassemb.ly/mzavar/Hackathon-Good-Fast-Cheap)

### Team Algorithm Constraint
Must use a Random Forest
Your choice of features
Your choice of samples

# About data
The dataset has 32561 rows and 14 columns as features, 7 categorical, 6 numerical. The target column being predicted is
called 'wage' with 2 values <=50k and  >50k.
Data can be found ![here](https://archive.ics.uci.edu/ml/datasets/adult) 

# Exploratory Data Analysis
### Missing data
The seems clean with no missing values with exeption of leading spaces and few '?' on few of the cartorical data.
We replaced '?' with 'unknown' with lack of any better imputing strategy and removed spaces.

### Handling categorical data
We converted the **target** column, wage to a numerical equivalent where  0 meant  wage<=50k and 1 meant wage>50k.

We analysed the categorical columns, identified them as nominal or ordinal columns and converted them to numerical or dropped them if they were repetative , contained non-relevant information to predict the target , as part of EDA

|  column |Categories|Type   | description  | categories|  
|---|---|---|---|--- |
| wage  |2   |binary |  Converted to 0 as < 50k and 1  > |   |   |
| workclass|9 |nominal| dummified  |{Private,Local-gov,?,State-gov,Self-emp-inc,Federal-gov,Without-pay, Never-worked}
| education | 60  | ordinal | Dropped as education-num has same information in numeric form  |   |   |
| marital-status  | 5  | nominal  | dummified  | {Married-civ-spouse,Never-married,Divorced,Separated,Widowed,Married-spouse-absent,Married-AF-spouse} |
|occupation |15 |noimnal | dummifies |
|relationship| 6|nominal | dropping it as it might be same as marital_status, might introduce muti-collinearity|Husband,Not-in-family,Own-child,Unmarried,Wife,Other-relative
|sex |2 |nominal |  | 0 - Male , 1 - Female
|native-country|42 |nominal |buecketed in 2 categores as USA as 50% of records and other 41  categories comprised  remaining half |United-States= 1 , Other -0 


By the end of EDA , we had  36 numerical features which was way below the maximum recomended threshold of 80 sqrt(total rows), so we decided to throw in all features into the model as a starting place.

# Modelling approach and Results

We were constrianed to using the 'Random forest machine' algorithm. The objective was to classify if a person's wages were less than 50k or more than 50k given their profile information. The required delirable however was the probability of the outcome ( >50k). We started with a 'RandomForestClassifier' model from Sklearn set with all of its defaults with training data
which was created by spliiting the  dataset in training and  test data with a 75 /25 split.
While the First model itself was good and has accuracy of 96 % on training data  it suffered from very high variance.
The next was to tune the model  for various hyperparameters for decision tree and random forest.
The following table shows the results summary and iteration we experimented with to tune the model. 


| Model  | train accuracy  | test accuracy|  description  |  |
|---|---|---|---|---|
| Baseline  |75.9%   |75.9%  | Model would be right 75 percent of time If it were to predict every incoming record less than 50k, |   |   |
| Random forest 1  | 96.40%  | 82.20%  |   |   |
|  gs1 | 82.28%  |  84.12 |max_depth=10,n_Estimators =200  |
|gs2-7 | no change | no-chage | criterion , min_samples_split ,min_sample_leaf |
|gs1 +fnlwgt| 84.28% | 84.42% | included fnlwgt
|gs1 + feature selection| 84.19 | 84.76 | We simplified model by choosing feature with coeff > 0.1 and < -0.1


# Conclusion

The new simpler and smaller features list of (just 12 features) when paired with the optimized Random Forrest performed the best and had the highest accuracy score out of all the attempts. Its test accuracy was also higher than the train accuracy indicating the Random Forrest is no longer overfit. This model was used to create the predictions included in our submission_sm.csv for the Hackathon challenge.
