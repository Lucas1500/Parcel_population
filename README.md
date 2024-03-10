# Estimating Parcel Level Population in the Puget Sound Region
## Erika Fox, Kaizhi Lu, Gavin Pierce

This report presents our efforts supporting Public Health - Seattle and King County (PHSKC) in developing an advanced model for generating parcel-level population estimates in King County. More details of the report can be found [here](https://github.com/nrs02004/Jiujitsu-ELO/blob/main/writeup/cond-logit.pdf).

***
### Background & Problem Introduction
PHSKC consists of 1,200 employees serving over 40 different sites and operating with a biennial budget of $1 billion. With service to almost 2.53 million residents, PHSKC is one of the largest public health departments in the United States. The population of King County is diverse, with over 100 languages spoken in the region, and the public health arena reflects this, with over 7,000 medical professionals serving across 19 acute care hospitals and beyond. PHSKC provides countless essential services in support of these populations and medical efforts. For a variety of applications such as resource allocation, disease surveillance and healthcare planning, PHSKC required small area population estimates. Given a small geographical area, the department would like a precise estimate of the number of people living in that area.

Unfortunately, for many applications, the desired data was not available. In most cases, the best publicly available data is U.S. Census Bureau decennial data. However, this data is aggregated spatially and injected with noise before it is released to the public to protect individual privacy. The Census Bureau advises against the use of data published on the smallest aggregate regions due to the large proportion of injected noise. However, Census tracts typically contained between 1,200 and 8,000 people3 and thus are far too large for PHSKC’s desired use. 

In order to make predictions and display results on regions familiar to the public, such as neighborhoods or ZIP codes, PHSKC desired parcel-level estimates of population which could subsequently be aggregated to the desired regions. A parcel is defined as an identifiable unit of land that is treated as separate for valuation or zoning purposes4. In our setting, a parcel is a plot of land that contains either a single-family home or a group of apartment or condominium units.

<img src="/image/tract_parcel.jpg" alt="drawing" width="600"/>

***
### Objective
Our primary objective was to develop a model which accurately predicts the population of regions of interest (parcels) for a single year. Our aim was to build a regression model using known population data, then apply the fitted model to predict the population of parcels for which covariate data is available but population data is not.

***
### Dataset Description
Data for our primary analysis were sourced from the American Community Survey (ACS) and the King County Assessor’s office, both of which are publicly available sources. We were primarily interested in variables that existed in both data sources and had the potential to lend predictive value at the parcel level, such as number of bedrooms, geographical location, property tax assessed value, housing type, units per structure, and year structure was built.

#### Training Data – American Community Survey (ACS)
The American Community Survey is an ongoing survey conducted annually by the U.S. Census Bureau to act as a supplemental estimation tool to the decennial census. The ACS provides vital information about communities across the United States, including demographic, social, economic, and housing characteristics. It samples approximately 5% of households in a region on a yearly basis and offers household-level statistics, including our outcome of interest, persons-per-household.

#### Prediction Data – King County Assessor (Assessor)
The King County Assessor data contained property assessment information maintained by the King County Assessor's Office, including detailed information as residential homes, commercial buildings, vacant land, and other real estate assets. The King County Assessor’s Office collected from all taxable properties within King County’s jurisdiction. The data collected offered many of the same or similar characteristics to that of the ACS data, but did not provide data on persons-per-household. It provided data regarding the distribution of parcels by zip code and neighborhood, allowing for the potential to apply a fitted model to data from the desired region to predict population. The Assessor’s Office offers data ranging from 1982-2023.

#### Common Predictor Variables
After careful consideration, we decided to use the following four variables. Some of them were continuous variables such as Number of Bedrooms in the Unit and Tax Assessed Value of the Unit; some of them were categorical ones such as Unit type and Location. For the location variable, we used the specific PUMA, which was Public Use Microdata Areas, in which each household was located. PUMAs were defined by the US Census Bureau as non-overlapping, statistical geographic areas that partition each state or equivalent entity into geographic areas containing no fewer than 100,000 people each. A graph illustrated the relationship between PUMA and census tract had been shown below. For consistency, the right census tracts graph was the same as the previous tract vs. parcel graph.

<img src="/image/tract_puma.jpg" alt="drawing" width="600"/>

A glimpse of the two datasets had been provided below. The variables in the black box were common predictor variables. The variables in the blue box were particular variable needed for training or prediction purpose. There were also some variables in the dataset that were not in the box used for weighting or other purpose.

<img src="/image/data_preview.jpg" alt="drawing" width="600"/>

***
### Method
**1. Built predictive models using the ACS training data:**

* Accounted for survey weights in all regression models

* Fit several linear regression models using different subsets of the available predictors
  * Modeled number of people as a function of type of unit (house, apartment, or condo), number of bedrooms, tax value, PUMA
    
* Similarly, fit several Poisson regression models on different predictor sets
  * Modeled occupancy rate (number of people living per bedroom in a home) as a function of type of unit, tax value, PUMA

**2. Utilized cross-fold validation to evaluate model performance on unseen ACS training data:**

* Utilized 5-fold cross-validation in order to compare model performance on unseen (ACS) data
  * For each model, computed a weighted mean squared error for each fold and took average, where $I$ indicates the set of observations in hold-out set $j$:
    
$$
RMSE_{i} = \frac{1}{5} \sum_{j=1}^{5} \left( \frac{\sum_{i \in I} w_i * (y_i - \hat{y_i})^2}{\sum_{i=1} w_i^2} \right)
$$





