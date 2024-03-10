# Estimating Parcel Level Population in the Puget Sound Region
## Erika Fox, Kaizhi Lu, Gavin Pierce

This report presents our efforts supporting Public Health - Seattle and King County (PHSKC) in developing an advanced model for generating parcel-level population estimates in King County.

### Background & Problem Introduction
PHSKC consists of 1,200 employees serving over 40 different sites and operating with a biennial budget of $1 billion. With service to almost 2.53 million residents, PHSKC is one of the largest public health departments in the United States. The population of King County is diverse, with over 100 languages spoken in the region, and the public health arena reflects this, with over 7,000 medical professionals serving across 19 acute care hospitals and beyond. PHSKC provides countless essential services in support of these populations and medical efforts. For a variety of applications such as resource allocation, disease surveillance and healthcare planning, PHSKC required small area population estimates. Given a small geographical area, the department would like a precise estimate of the number of people living in that area.

Unfortunately, for many applications, the desired data was not available. In most cases, the best publicly available data is U.S. Census Bureau decennial data. However, this data is aggregated spatially and injected with noise before it is released to the public to protect individual privacy. The Census Bureau advises against the use of data published on the smallest aggregate regions due to the large proportion of injected noise. However, Census tracts typically contained between 1,200 and 8,000 people3 and thus are far too large for PHSKC’s desired use. 

In order to make predictions and display results on regions familiar to the public, such as neighborhoods or ZIP codes, PHSKC desired parcel-level estimates of population which could subsequently be aggregated to the desired regions. A parcel is defined as an identifiable unit of land that is treated as separate for valuation or zoning purposes4. In our setting, a parcel is a plot of land that contains either a single-family home or a group of apartment or condominium units.

<img src="/output/tract_parcel.jpg" alt="drawing" width="600"/>
