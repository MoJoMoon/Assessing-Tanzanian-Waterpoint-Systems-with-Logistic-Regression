
# Module 3 Final Project
***
Morgan Jones

<img src='img\water image.jpg'>

## Introduction

We have been commissioned by the United Republic of Tanzania Ministry of Water to aid in their ongoing Mission: To improve access to safe drinking water and sanitation services to all, and manage water resources so as to ensure national food security and sustainable industrial based economic development. In honor of Maji Week 2020, the Ministry announced intentions to start a service which will monitor and service the functionality of various waterpoints in Tanzania. In order to operate efficiently, the service will need to predict the operation status of the several thousand waterpoints in the country. Our task is to assist the members of this service with accurate predictions for the waterpoints that are functioning as intended, and those which need repair.

As data scientists, we will fulfill this goal by conducting data analysis on the Tanzanian waterpoints and generating classification models which predict with a high accuracy the operating condition of waterpoints within the Tanzanian borders. Areas of focus for the breadth of this work will be:

 - How do water source characteristics effect the functionality of a waterpoint?

 - How does the surrounding geography impact functionality of a waterpoint?

 - How do years and seasonality of operation effect the functionality of a waterpoint?

## Objectives

For this notebook, we will build a **Classifier** model to ***predict*** the ***functionality*** of water wells in Tanzania as accuratly as we can. In order to achieve this objective, we will clean, explore, and model the dataset for classification. As such we will need to complete the following tasks:

 - Understand the Data: Construct a unique business case around the model. Analyze the dataset from various points of view.
 - Preprocess the Data: Import the data and preprocess the data through cleaning, scrubbing, handling missing values, and exploring different methods with benchmarking.
 - Describe the Data: Conduct EDA. Create novel distributions, compare multiple distributions, and find insights in the data.
 - Fit models and conduct Hypothesis Testing: Compare multiple models and give detailed numerical and visual analysis of models.
 - Gather insights: Give a conclusion with recommendations that are business relevant and are driven by analysis

## Metrics for Evaluation

Access to clean water is a basic human right, and an aim of this work is to widen its availability to all citizens of Tanzania. With this in mind, we value a model which has high accuracy. Our primary evaluation metric is the Classification Rate defined as:


$$ 
\begin{align}
 Classification Rate = \frac1N \sum_{i=0}^n I(y_i = \hat{y_i})
\end{align}
$$ 

 - Yields the percentage of rows where the predicted class $\hat{y}$ is equal to the actual class $y$ in the test dataset.
 - The maximum value for $Classification Rate$ is 1 and the minimum value is 0.
 - Our goal is to maximize the $Classification Rate$, with a minimum benchmark of:
 
$$ 
\begin{align}
Classification Rate = 0.80
\end{align}
$$ 

This translates to our model **predicting the correct status of a waterpoint 80 times out of 100 times**. 

Our model will be a **ternary classifier**, meaning that there are three potential classes a waterpoint can be placed. In the event our model improperly predicts the status of a waterpoint, we place a **higher importance on false positives more than false negatives**. Our model predicting that a waterpoint is functional when it is not functional can put members of a community at risk of not having access to clean drinking water for a period of time. However, when our model predicts that a waterpoint is non-functional, or functional but in need of repair when it is actually functional, the ministry may allocate resources to an issue that is currently non-existant, but these resources could potentially be used for another waterpoint or for that waterpoint at a later time. Therefore for the purposes of this work we will **value a low false positive rate more than a low false negative rate**. Precision is more important where False Positives are more costly than False Negatives, when defined as:

$$ \text{Precision} = \frac{\text{Number of True Positives}}{\text{Number of Predicted Positives}} $$

  

$$ \text{Recall} = \frac{\text{Number of True Positives}}{\text{Number of Actual Total Positives}} $$

Therefore we aim to generate a final model where:

$$ \text{Precision} < \text{Recall} $$


## Target Variable

**Status_Group** | **Description** 
:---------|:-------------
`functional` | The waterpoint is operational and there are no repairs needed
`functional needs repair` | The waterpoint is operational, but needs repairs
`non functional` | The waterpoint is not operational, and needs repair/replacement


## Exploration


### Water Source Observations

<img src='img\function distribution.png'>

From our explorations into the various water source dimensions of our dataset, we have been able to find certain patterns that may be indicators for predicting the functioning status of a waterpoint. We can see from our visualizations that:

 - **Amount_tsh**
     - High levels of water available to a waterpoint appears correlated to the waterpoint being functional
     - Low levels of amount_tsh appears related to non-functionality. This could indicate that the waterpoint is dry with no water to offer
 
 - **Quantity**
     - Enough level of water quantity is linked to a waterpoint being functional
     - Dry level of water quantity has a high relation to non-functional waterpoints further indicating low amounts of water could cause a waterpoint to stop functioning.
     
 - **Quality**
     - Most waterpoints connected to large amounts of water have Fluoride and Good quality. These quality levels have a high relationship with functional waterpoints.
     
 - **Extraction**
     - Most wells are gravity and handpump wells, with these types having the highest amount of functioning waterpoints.
     - The functioning wells connected to the largest sources of water are using a motorpump.
     
 - **Source**
     - The source type with the most functioning wells is Spring Well. 
     - The source type with the most non-functioning wells is the Shallow Well, which again could indicate that shallow wells may dry faster.
     
### Geographical Observations

<img src='img\pop.png'>

From our explorations into the various geographical dimensions of our dataset, we have been able to find certain patterns that may be indicators for predicting the functioning status of a waterpoint. We can see from our visualizations that:

 - **Region**
     - Ininga & Kilmanjaro have some of the highest percentages of functional waterpoints and the highest counts of wells, but contain some of the lowest average populations. This could indicate that a factor for keeping wells functioning is having a high ratio of waterpoints to population.
     - Mara. Lindi, Rukwa, and Mtwara have some of the highest average populations near waterpoints, but have more non-functioning wells than functioning wells.
     - Kigoma has the highest ratio of wells that are functional but in need of repair.
 
 - **Basin**
     - Lake Victoria, Pangani, Rufiji, have the most waterpoints by basin.
     - Rufiji has the best percentage of functional waterpoints.
     - Ruvuma/ Southern Coast have the worst ratio of functional to non-functional wells.

 - **Elevation**
     - Tanga, Shinyanga, Mwanza have the highest elevations for functional but in need of repair waterpoints
     - There is no elevation difference between functional and non-functional wells for most regions.
 - **Population by Region**
     - Certain levels of population, such as 499247, 878501, and 463070 there is a high ratio of functioning wells to non-functioning wells. For population levels 462674, 563370, 661359, and 1,060,886 there is more likely to be a non-functioning well than a functioning well.
     
     
### Years & Seasonality Observations

<img src='img\seasonality.png'>

From our explorations into the various temporal dimensions of our dataset, we have been able to find certain patterns that may be indicators for predicting the functioning status of a waterpoint. We can see from our visualizations that:

 - **Report Year**
     - Most reports are made in 2011 with 48% and 2013 with 40% of the reports.
     - The best ratio of functional reports is 2011, with the worst being 2012.
 - **Construction Year**
     - Most functional reports were made on wells constructed after 1990 indicating that age could be a factor of functionality.
     - More wells were created after 1990, with the most being constructed in 2000.
 - **Month Recorded**
     - Most reporting is done in March with 31% and February with 20% of total reports. 
     - January, July, October, and November have the worst ratios for functioning wells.
     - There appears to be a regional relationship with the month that a report is generated, this could have a correlation on whether a well is functioning.
     
 - **Season Recorded**
     - Most reports are made in the long rain season with 38% of reports.
     - The least amount of reports are made in the short rain season with 3% of total reports.
     - The long rain has the highest ratio of functionality of the seasons.
     - The short rain season has the lowest ratio of functionality.
     - There is a regional relationship with the season that a report was made. 
         - Most long dry reports are made in the North West.
         - Most long rain reports are made in the East.
         - Most short rain reports are made in the Mid West.
         
 - **Day Recorded** 
     - The day with highest percentage of reports is Wednesday at 15%.
     - The day with the lowest percentage of reports is Sunday with 12%.
     - There appears to be no correlation between the day a report was made and the functionality of the well.
     - There is no apparent regional relationship between the day of the week a report was made.
     
 - **Age**
     - The most common age for a well is 13 years.
     - For age range between -7 and 19 years, the number of functional wells is higher than non-functional wells.
     - For age range between 20 and 53 years, the number of functional wells is lower than non-functional wells.

## Model Interpretation

After carefully cleaning and exploring our data, we were able to run a Sci-Kit learn pipeline on several models, having the following accuracy results after running Stratified K-Fold Cross Validation:

<img src= 'img\models.PNG'>

<img src= 'img\scores1.PNG'>

The results indicated that a Random Forest Classifier would be the most accurate in predicting the functionality of a well. After hyperparameter tuning with GridSearchCV, and tuning for accuracy, and recall we created a Random Forest Classifier which achieved an accuracy score of 0.8063485448357297, thus successfully reaching the goal set in the metrics for evaluation section of our work. We also managed to keep a relatively low false positive rate. The resulting confusion matrix and classification report are as follows:

<img src= 'img\confusion_matrix.PNG'>

In regard to feature importance, the Random Forest Classifier deduced that geographical coordinates and elevation measures were the most vital features for predicting functionality in a well. `construction_year` was also a valuable feature, which can confirm that the linear regression model to predict missing year values was beneficial. Two features that were engineered were also considered as most important, those being `age`, and `funder_counts`. Our Random Forest feature importances are as follows:

<img src= 'img\top10_importances.PNG'>