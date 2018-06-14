# West Nile Prevention: Pesticide recommendations using predictive modeling 



## Introduction

Chicago has a decades long history of West Nile, a virus spread to humans through infected mosquitoes. While many infected with the virus develop no symptoms, 20% develop mild symptoms such as fever and vomiting, and 0.7% develop serious symptoms that may result in death [1].  The first human cases of West Nile appeared in Chicago in 2002, with 225 cases and 22 fatalities [2]. In 2004, the City of Chicago started a surveillance and control program where mosquito traps were placed throughout the city and captured mosquitoes are tested for the virus every year from late spring to early fall [2]. The test results influence airborne pesticide usage to eradicate adult mosquitoes and prevent the spread of the virus. There is therefore value in accurately predicting West Nile outbreaks to help the city allocate resources for disease prevention.   

The goals for this project were to build a machine learning model to predict the presence of West Nile in mosquitoes in traps at specified locations in the city of Chicago. In addition, we conduct a pesticide cost-benefit analysis provide further recommendations to the City to maximize desired outcomes.   

## Data Analysis

We utilized 3 datasets:

1. NOAA weather conditions from 2007 – 2014
2. GIS data from 2011 and 2013 pesticide spray efforts by Vector International 
3. Mosquito trap West Nile surveillance test results from 2007 – 2014

Organizationally, the data was aggregated and cleaned in order to remove issues pertaining to missing information or incorrectly logged data. In order to use all the data in our models, some text features were converted to be represented numerically. A plot representing weather stations (airports), trap locations, and spray locations is shown in Figure 1.

Visualization was used extensively to uncover trends around spraying location, trap location and west nile outbreaks for each year. The dataset only included years that were odd (2013 for example), which meant it was not possible to determine year to year trends. Feature engineering was performed in order to calculate distances relative to the weather stations. Figure 2 shows the location and density of West Nile presence (in red) in our training data. The spray locations in 2011 and 2013 are represented in yellow. From this visualization, it becomes obvious that there are varying patterns of West Nile per year. 2007 and 2013 had the highest number of positive cases. Pesticide spray efforts in 2011 and 2013 targeted some outbreak areas, but the largest areas were not addressed, leaving room for improvement. 

Additionally, we wanted to verify the belief that West Nile virus thrives in hot and dry conditions. Figure 3 confirms this belief, as we see the most cases in the lower right corner of high average temperature and low precipitation. There are no cases in cold and wet conditions. We will use this insight to inform our feature selection when modeling, as there are many features which measure the hot and dry condition.

The data was found to be imbalanced with a ratio of 1:20 positives to negatives for our target variable of West Nile Virus. In order to deal with this, a combination of oversampling and undersampling were used in the form of the SMOTEENN method to reach a 1:1 ratio. With all the data thuroughly understood, it was time to develop a model to predict our target, West Nile Virus occurrence. 

<img src="https://git.generalassemb.ly/cstreams/Project_4/blob/master/images/spray-trap.png" width="40%" height="50%">
<img src="https://git.generalassemb.ly/cstreams/Project_4/blob/master/images/temp.png" width="40%" height="50%">
<img src="https://git.generalassemb.ly/cstreams/Project_4/blob/master/images/wnv-spray.png" width="150%" height="150%">
<img src="https://git.generalassemb.ly/cstreams/Project_4/blob/master/images/wnv-temp-precip.png" width="40%" height="40%">



## Modeling

While many of the best Kaggle submissions used highly engineered features (time-lagged weather statistics, polynomially-smoothed interaction terms, etc.) with excellent effects on their model scores, our time constraints prevented us from delving too deeply into feature engineering. We selected features that broadly related to temperature, moisture, and location.  We began with a broad feature set that we pruned down experimentally before settling on our final features.

For model selection, we began by identifying our data science problem as binary classification, yes/no West Nile virus present.  We considered a large family of classification models from sklearn. Including logistic regression, support vector classifiers, tree-based classifiers (decision trees, random forests, extra trees), and neural networks (pure dense and convolutional).

This data posed a peculiar difficulty: we had to predict even-year data but we could only train on odd-year data.  Since yearly trends in temperature and weather were idiosyncratic (some years very different than others), overfitting to these years was an early pitfall of our modeling process.  We mitigated this tendency by adapting our features to try to take year-to-year variance into account.  After tuning, our best model was logistic regression.

<img src="https://git.generalassemb.ly/cstreams/Project_4/blob/master/images/coefficients.png" width="50%" height="50%">
<img src = "https://git.generalassemb.ly/cstreams/Project_4/blob/master/images/roc-curve.png" width ="50%" height="50%">

Our final logistic model received an RoC-AuC score of 0.68869.  These features out-performed more broad weather and location features, vindicating our initial belief that these features were good predictors.


## Discussion

Given our model’s West Nile predictions, we can recommend target areas for the City to spray with pesticides. For 2014, our model predicts West Nile in the red areas shown in Figure X. This map shows a 120,000-acre area that includes neighborhoods such as Lincolnwood, Old Irving Park, North Mayfair, Portage Park, and Albany Park.  The estimated spray cost for the given acreage is $800,000. This value is based on an extensive study conducted in Sacramento, CA in 2005 for similar acreage and adjusted for 2014 inflation [3].

<img src="https://git.generalassemb.ly/cstreams/Project_4/blob/master/images/predictions.png" width="50%" height="50%">

Even with our predictive model, outbreaks in humans are hard to predict. The cost of West Nile in humans takes many forms, including medical costs and costs from economic loss when infected individuals are hospitalized and can’t work. From the Sacramento study described above, it was determined that for severe cases, inpatient costs average $40,000 and economic loss is estimated at $12,000 when adjusted for 2014 inflation [3].  A summary is shown in the figure below. 

<img src="https://git.generalassemb.ly/cstreams/Project_4/blob/master/images/medical.png" width="50%" height="50%">

Given our 2014 WNV predictions, only 15 cases of severe West Nile need to be prevented in order to make the $800,000 pesticide spray cost effective. We recommend to always spray, given the high medical costs, economic impact, and unpredictability of WNV outbreaks in people. Hyper vigilance helps lower the risk of a severe West Nile outbreak. 

## Next Steps

Our likely next steps would be to improve our predictive model by considering additional features, gathering more data, and building ensemble models. We would also like to examine the efficacy of spraying pesticides against West Nile virus and build models that predict outbreaks more reliably in humans rather than in mosquitoes. 

## References

[1] https://www.cdc.gov/westnile/ 

[2] https://www.cityofchicago.org/city/en/depts/cdph/provdrs/healthy_communities/news/2012/jul/city_to_spray_insecticidetonightandwednesdaytokillmosquitoes.html 

[3] https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011/
