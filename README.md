# EXECUTIVE SUMMARY
***
## Project 4: West Nile Virus Prediction for the City of Chicago

### Background & Problem Statement

West Nile virus is most commonly spread to humans through infected mosquitos. Around 20% of people who become infected with the virus develop symptoms ranging from a persistent fever, to serious neurological illnesses that can result in death.

In 2002, the first human cases of West Nile virus were reported in Chicago. No vaccine or specific medicines are available for the West Nile virus infection. Due to the serious health implications and cost of seeking treatment,it became a pressing concern that required intervention by the state. By 2004 the City of Chicago and the Chicago Department of Public Health (CDPH) had established a comprehensive surveillance and control program that is still in effect today.

Every week from late spring through the fall, mosquitos in traps across the city are tested for the virus. The results of these tests influence when and where the city will spray airborne pesticides to control adult mosquito populations.

However, it is a huge cost to the state to carry out the planned sprays so as a result, spraying where it has the highest impact and spread is of utmost importance. The Centre of Disease Control and Prevention (CDC) hired our team of data scientists to better understand and evaluate where and when the pesticide should be sprayed given a number of variables.

Given weather, trap locations and spray data, we were tasked to predict when and where different species of mosquitos will test positive for West Nile virus. We aimed to develop a model that could accurately predict outbreaks of West Nile virus in mosquitos such that it would help the City of Chicago and CPHD more efficiently and effectively in allocating resources towards preventing transmission of this potentially deadly virus. 


### Data processing

We were provided with weather, trap(train/test datasets) and spray data from 2007,2009, 2011 and 2013. Duplicates from each dataset were dropped. With the trap dataset, only the dates, mosquito species, trap number, trap spatial coordinates, number of mosquitoes in each trap and presence of West Nile virus were retained. In the weather dataset, variables with 50% or more missing values were dropped while other columns with missing values were tranformed through forward filling imputations. The trap and weather dataset were merged and a weather station and its associated weather data was allocated to each trap based on their proximity to it. Next, new features such as the Year, Month, Week, Day of week were created to generate better insight. While the spray dataset was not used in our modelling process, it provided a fair amount of insight during our preliminary exploration of the data. With our datatsets cleaned, we first used the Principle Component Analysis to identify features of importance that we wished to run on our chosen models. The baseline model was generated using Logistic regression. The following classification models were used and compared - AdaBoost,XGboost, SVC, Gradient Boost, Logistic Regression.

As we hoped to correctly predict whether West Nile virus is present and minimise the risk of false negatives (has west nile virus but was incorrectly identified) we focused on using the Area under Curve (AUC), F2 and Recall scores a metrics to guage the efficacy of our models.


### Results & Insights
The test set was ran on each model and the following results were produced:

| Model | AUC Score | Recall score | Accuracy score |
| --- | --- | --- | --- |
| Logistic Regression(Baseline)| 0.53 | 0.06 | 0.95 |
| Logistic Regression | 0.74 | 0.57 | 0.90 |
| SVC | 0.74 | 0.59 | 0.88 |
| Ada Boost | 0.79 | 0.70 | 0.88 |
| Gradient Boost | 0.77 | 0.65 | 0.89 |
| XGBoost| 0.83 | 0.81 | 0.85 |

XGBoost was identified as the best model.

The top features predicting the presence of the virus were identified as follows:
- NumMosquitos
- Sunset
- Sunrise
- Average Temperature
- Precipitation
- Average Wind Speed

----

## Cost-Benefit Analysis

Due to the lack of updated, Chicago-specific data, we’ve used previous studies on the West Nile Virus in California. As they are quite dated, we have made inflation adjustments where necessary.

While more accurate cost estimates can be obtained once we have access to better data, this analysis should give a good enough sense of whether the spraying effort, supported by our model, is worth the cost

#### Costs
1. Cost of spray = \\$245/km2 [1]

2. Cost of survellience = \\$82/trap (per weekly surveillence) [2]

3. Ecological costs = While Ultra Low Volume products can minimize risks to humans and the environment, sprays have been known to kill other harmless(maybe even beneficial i.e. honeybees) animals
 

Resources used:<br>
[1] In 2005, aerial spray was utilised in Sacremento County, 6 times over an area of 477km2. The total cost was USD700k, including labour costs [(Source: US National Library of Medicine)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011/). While this is arguably a more expensive method vs truck spraying, it was also 16 years ago. With inflation considered, we think this can be a good proxy.

[2] Cost of surveilling and testing each mosquito trap from 2004-2012 in California was estimated to be \\$72 [(Source: US National Library of Medicine)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4340646/). It is difficult to estimate the impact of inflation accurately, but assuming 1.5 percent inflation/year from 2012-2021, the estimated cost in 2021 terms would be \\$82. 


#### Benefits
1. Reduce sufffering = 1% of those affected experience sevre symptoms and fatality, 20% expereience moderate symptoms and 79% experience mild symptoms. Alleviating the presence of West Nile virus could reduce the number of people who have to face severe or moderate symptoms or even prevent people from facing that possibility [1]

2. Prevent loss of income = \\$500/infected person [2]

3. Avoid medical costs = \\$2,300/infected person [3]

Resources used:<br>
[1] Proportion of infected persons that suffer mild, moderate and severe symptoms are obtained from Centre of Disease Control.[(Source)](https://www.cdc.gov/westnile/symptoms/index.html)


[2] Median per capita income  in Chicago is \\$37,100year. [(Source: US Census)](https://www.census.gov/quickfacts/fact/table/chicagocityillinois/LND110210) Assuming 5 days of sick leave on average (same assumption as [(Source: US National Library of Medicine)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011/)

[3] In 2005, medical costs  for mild and moderate cases (WNF) cost \\$300 per patient, moderate cases cost \\$6,317 while serious cases (WNND)cost \\$33,143. Applying these costs with inflation rate of 1.5 percent per year, it would be 
\\$380, \\$8016  and \\$42000 respectively in 2021 terms. Applying to the ratios of mild/moderate : severe, the average medical cost per infected person works out to be 
$800 [(Source: US National Library of Medicine)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011/)

#### Balancing the costs & benefits
* Annual cost of surveillance <br>
Propose weekly surveillance from Jun-sep, with existing 80 traps.
<br>\\$82 x 80 traps x 16 = **\\$105,000**

* Annual cost of spray
    * Model recall score: 81% for positive, 85% for negative
    * Actual % of traps positive: ~30% in Jul-Aug 2013, to be used as an estimate
    * %  of traps that model will identify as positive in Jul-Aug period: <br>81% 30% + 15% * 70% = 35%
    * Cost of weekly spray over jul-aug = 35%  x 606km2  x \\$245 x 8 = **\\$416,000**

Total Annual Cost = **\\$521,000**

* Benefits per person who avoids wnv = \\$500 income + $2,300 medical = **\\$2800/pax**

<br>Putting the estimated costs and benefits per pax together, we need to **avoid 185 potential cases** from our spraying efforts, to make this whole exercise worth it

#### Is spraying worth it?
If ~100 of the positive mosquitos can be eliminated by the spraying program, it is reasonable to assume that at least 185 people can be “saved” from the virus (i.e. assuming each mosquito potentially infects **1.85** people)


* Rationale behind aforementioned theory<br>

Let’s use 2013 as an example:<br> 
About 200 samples in 2013 were tested Wnv positive [1]

Assume that spraying is effective in reducing mosquito counts by about 50% [2] 

[1] Number of mosquitos that are positive in each positive sample could actually be more than 1, but we assume just one positive mosquito to be conservative

[2] The daily count of mosquitos appeared to have reduced by about 58% after the sprays in 2013, although it is unclear if the spray is the direct cause of it. Other studies [(Source)](https://www.cmmcp.org/sites/g/files/vyhlif2966/f/uploads/tjz083.pdf) have also show about 54% of reduction in mosquito count in treated areas vs an increase in untreated areas


#### How does our model fit in?
An indiscriminate spraying over all locations over the jul-aug period will increase cost by **~3x** vs using our model to guide on where to spray

This would mean we need to prevent 555 people from potentially contracting the virus, to make the indiscriminate spraying worth it. 

Even in 2002 when the virus first appeared and number of cases were at its peak, chicago only recorded 225 cases 

Given how expensive the spray and surveillance cost, as well as possible ecological concerns, other inexpensive preventive measures should also be considered

E.g. Homeowners can help to check for hotspots within their houses, where still water can accumulate

## Conclusions 

With our goal to predict when and where different species of mosquitoes will test positive for WNV, we created several classification models to predict the presence of the virus. Among all the regression algorithms that we used, we found that XGBoost Classification performed well compared than other model and the baseline. Given that it seems to have a lower accuracy score as compared to Gradient Boost and Logistic Regression models, it scored the best on the testing data with the ROCAUC score of 0.831 and thus we will consider this model to be the most useful for this study.

In essence our model suggests that weather parameters are very useful in predicting the presence of west nile virus and as are the number of mosquitoes caught per trap. This warrants further research into establishing better associations between mosquito life cycles and weather patterns so as to better characterize how and when they become vectors of West Nile Virus. Additionally, we can see that the more mosquitos caught, the better we are able to characterize the presence of the virus reiterating that larger sampling sizes inherently hold better predictive power.

## Recommendations

There are other "cheaper" ways for the government to help reduce the spread of WNV. The Chicago budget in 2013 showed $9,000,000 dedicated to community engaged care. This covers promoting health through education, policy and service. The government can ramp up education on West Nile Virus and how to prevent mosquito breeding. They can educate the public on preventing stagnant water, applying insect repellent, wearing the proper attire to reduce the chances of getting bitten by mosquitos.

Other measures which they have already undertaken but can place more emphasis on is the reporting of dead birds (a key in transmission of WNV), reporting on tall grass and weeds.

In future reiterations of this project, Deep Neural Networks like Keras could be used as they may have better prediction results. Rather than sampling mosquito population and having a binary classification for WnvPresent, an improvement may be to show the number of mosquitos carrying WNV so that a rate can be calculated. Additionally, more robust and complete datasets provided on an annual basis as opposed to alternate years would provide better insights.








