# Analyzing Neighborhood Characteristics of Affordable Housing Under 421a

The City of New York is facing a housing crisis, one that disproportionately affects those in the lowest income brackets. 421a is one of the largest affordable housing programs under the City's Housing New York initiative, which aims to provide an additional 300,000 affordable housing units by 2026. This analysis considers characteristics of neighborhoods where new developments are taking place, and looks to identify any factors that may predict whether or not a development will recive a tax abatement under 421a. These factors, if any, may indicate bias in the application of 421a and help to inform future iterations.

## Introduction

In the past decade, New York City's population has grown by 500,000 residents, reaching 8.4 million by 2019. During this same time period, however, housing only increased by 100,000 units, further exacerbating a long standing issue for the City - shortage of affordable housing ([*Office of the New York City Comptroller*](https://comptroller.nyc.gov/reports/nyc-for-all-the-housing-we-need/)). The shortage of affordable housing is particularly acute for New Yorkers in the lowest income brackets, with over 1 million New Yorkers falling in the categories of Extremely Low or Very Low income categories (up to 30% or 50% of the Area Median Income, respectively) and only 425,000 housing units of correpsonding affordability as of 2011.

In response, the City oversees a number of programs in support of afforable housing, most recently guided by the Housing New York (HNY) plan established by then Mayor Bill de Blasio in 2016. The goal at the outset of HNY was to provide an additional 200,000 affordable housing units (later raised to 300,000 units) over the next decade, through building new affordable units, converting currently non affordable units, and conserving and promoting the habitability of existing affordable units ([*The City of New York*](http://www.nyc.gov/html/housing/assets/downloads/pdf/housing_plan.pdf)).

On such program under HNY is the 421a tax incentive, which, in its current iteration, grants property tax abatements for new housing developments that include a certain number of affordable units. When 421a was first established in 1971, it did not have any affordable unit requirements and existed solely to encourage development on underutilized lots in an economically declining city. It was not until 1985 that the first affordability requirement was enacted for developments between 14th and 96th Street in Manhattan, known as the Geographic Exclusion Area, though developers could meet these requirements by providing affordable units off site. This initial requirement called for at least 20% of all units to be affordable for eligible projects. In 2006, this requirement was expanded to include developemtns across all five city boroughs. During this phase, off site affordable units would still be eligible for a 10 or 15-year tax exemption, depending on location, but 20 or 25 year exemptions required on site affordable units. In 2015, the 421a program set forth a higher required level of affordable units, from 20% to 25-30%, a longer maximum duration of the incentive, from 25 years to 35 years, and introduced an additional requirement that construction works be paid union level wages on all eligible projects ([*Observer*](https://observer.com/2015/05/a-taxing-matter-looking-back-on-the-history-of-421-a/)). 

The current iteration cost the City $1.7 billion in lost tax revenue in 2021, making it the most expensive tax break in the City. This is largely due to the scale of the incentive, which applied to 56% of all multifamily residential projects from 2013 to 2021 ([*The City*](https://www.thecity.nyc/housing/2022/2/10/22925482/a-tenants-guide-to-421-a-the-citys-biggest-tax-break-for-developers-and-landlords)). The level of investment and the potential for positive impact from the incentive make it a highly contested topic, with opponents arguing the incentive is inefficient and ineffective, and funds would be better allocated towards other housing initiatives. 

Terms of the incentive are renegotiated during each renewal of the incentive, and are only applicable for a finite number of years. The current iteration is set to expire in June 2022, though current New York Govenor Kath Hochul has already set forth a proposed revision.

Analysing ongoing housing developments in the city and what neighborhoods are taking advantage of the incentive allows for a more informed and refined bill in future iterations, maximizing the benefit to New Yorkers seeking affordable housing. This project looks at all new housing developments taking place and the neighborhood characteristics, to determine if there is any identifiable pattern in which developments qualified for the 421a tax abatement. The significance of this pattern, if any, would help to confirm the incentive is equitably available to neighborhoods of varying characteristics, or help inform any potential reforms towards a more equitably incentive.


## Executive Summary

### 01 Data Import and Cleaning
The data utilized for this project comes from a number of discrete sources, as follows.
<br /><br />
[<strong>NYU Furman Center Neighborhood Indicators </strong>](https://furmancenter.org/coredata/userguide/data-downloads)<br />
    This data set contains available neighborhood indicator at City, borough, subborough, and community district levels, from 2006 to 2020. Data types of each column were reformatted as necessary, then rows were seperated based on the type of region they represented. [Community District](https://furmancenter.org/files/sotc/SOC2007_IndexofCommunityDistricts_000.pdf) and [Subborough](https://www.census.gov/geographies/reference-maps/2017/demo/nychvs/sub-bourough-maps.html) region id values were reassigned to align with one another, so observations from each category could be combined into a more complete data set for each specific region. Values that were missing in both Community District and Subborough level observations were then further supplemented with more generalized Borough level observations. Any remaining null values underwent a final review to ensure randomness of distribution. The data was recombined into one data frame and saved to the data folder as clean_neighborhood_data.csv.
<br /><br />
[<strong>NYC Department of Finance Property Exemption Detail</strong>](https://data.cityofnewyork.us/City-Government/Property-Exemption-Detail/muvi-b6kx)<br />
    This data set contains records on tax abatements and exemptions filed for all buildings in the city, applicable to tax years 2019 to 2022. The corresponding [Exemption Codes](https://data.cityofnewyork.us/City-Government/Exemption-Classification-Codes/myn9-hwsy) data frame was utilized to narrow down all building level observations to those under various forms of 421a. From the Borough, Block, and Lot columns, a BBL code was generated for each observation to align with building specific ids seen in other data sets.
<br /><br />
[<strong>NYC Department of City Planning Housing Database</strong>](https://www1.nyc.gov/site/planning/data-maps/open-data/dwn-housing-database.page)<br />
    This data set contains building level observations for all permitting construction and demolition projects in the city from 2010 to 2020. The Community District ids for each observation for found to be erroneous, so the [PUMA 2010](https://www1.nyc.gov/assets/planning/download/pdf/data-maps/nyc-population/census2010/puma_cd_map.pdf) values were utilized instead and converted to their respective Community District ids. From there, observations were narrowed down to new construction of residential projects with 3 or more units - those relevant to the 421a incentive. The housing data set was saved to the data folder as clean_housing_data.csv.
<br /><br />
<strong>Final Data Set</strong><br />
Observations from the Housing Database and Property Exemption Detail data sets were matched based on the BBL id of each property, and a column of dummy values was added to the housing data set to indicate whether each property had recieved an abatement under 421a (value of 1). The neighborhood characteristics were merged with the housing data set based on the region id and year of each observation. Columns with remaining excessive null values were elimated from the final data set before dropping rows to preserve the maximum amount of data. The final cleaned and merged data set was saved to the data folder as final_data.csv.
<br /><br />

### 02 EDA and Feature Selection
The final data set was imported and a series of general overview tables produced. Visualizations focused on comparing the distribution of individual variables between 421a and non 421a observations, to see whether any trends were visually apparent. Correlation between variables was narrowed down to features above a threshold absolute value of .05, listed as high_correlation_features. This list was then further analyzed to limit collinearity between variables, based on relation to 421a and context of each variable. From this, a final list of features to include in the model was produced.

<br />

### 03 Production Model
After importing libraries and the final data frame were imported, X and y were defined as the model features and target variable, respectively. They were split into train and test sets, and X was scaled using Standard Scalar. This set up served as the foundation for a series of classification models: Logistic Regression, K-Nearest Neighbors, Decision Tree, Random Forest, AdaBoost, snf GradientBoost. The performance metrics for each model were saved into a DataFrame for comparison, then GradientBoost was selected to proceed with based on a combination of CrossVal Score and Test Accuracy. The GradientBoost parameters were further refined through the use of GridSearchCV. Final visualizations and a confusion matrix illustrated the performance of the final production model.


<br /><br />
## Data Dictionary
|Feature|Type|Description|Source|
|---|---|---|---|
|**units**|*float*|Number of units permitted for property (sum of pre construction and post construction units).|NYC Department of City Planning|
|**bbl**|*int*|Nine digit value indicating borough, block, and lot of property specific observation.|NYC Department of City Planning|
|**year_filed**|*float*|Year construction permit was filed with the City.|NYC Department of City Planning|
|**floors**|*float*|Number of floors permitted for property (sum of pre construction and post construction floors).|NYC Department of City Planning|
|**region_id**|*float*|Subborough region code.|NYC Department of City Planning|
|**latitude**|*float*|Latitude of property specific observation.|NYC Department of City Planning|
|**longitude**|*float*|Latitude of property specific observation.|NYC Department of City Planning|
|**421a**|*int*|Dummy value indicating whether a property recieved a 421a tax abatement.|NYC Department of Finance|
|**Region Name**|*object*|Neighborhood name corresponding with subborough id.|US Census Bureau|
|**gross_rent_0_1beds**|*float*|The median gross rent among studios and 1-bedroom units, which includes the amount agreed to or specified in the lease regardless of whether furnishings, utilities, or services are included; and estimated monthly electricity and heating fuel costs paid by the renter.|NYU Furman Center|
|**gross_rent_2_3beds**|*float*|The median gross rent among 2-3 bedroom units, which includes the amount agreed to or specified in the lease regardless of whether furnishings, utilities, or services are included; and estimated monthly electricity and heating fuel costs paid by the renter.|NYU Furman Center|
|**hpi_1f**|*float*|The average price changes in repeated sales of the same 1 family properties (index=100 in 2000).|NYU Furman Center|
|**hpi_4f**|*float*|The average price changes in repeated sales of the same 2-4 family properties (index=100 in 2000).|NYU Furman Center|
|**hpi_al**|*float*|The average price changes in repeated sales of the same properties of any type (index=100 in 2000).|NYU Furman Center|
|**hpi_cn**|*float*|The average price changes in repeated sales of the same condominiums (index=100 in 2000).|NYU Furman Center|
|**hpi_ot**|*float*|The average price changes in repeated sales of the same 5+ family properties (index=100 in 2000).|NYU Furman Center|
|**lp_all**|*float*|The total number of residential properties that had mortgage foreclosure actions initiated against them.|NYU Furman Center|
|**lp_fam14condo_initial**|*float*|The total number of 1-4 family properties and condominiums that had a new filing of mortgage foreclosure actions initiated against them after at least six years without having any such filings.|NYU Furman Center|
|**lp_fam14condo_rate**|*float*|The rate of mortgage foreclosure actions initiated per 1,000 1-4 family properties and condominium units.|NYU Furman Center|
|**lp_fam14condo_repeat**|*float*|The total number of 1-4 family properties and condominiums that had repeat filing of mortgage foreclosure actions initiated against them after already have such a filing in the previous six years.|NYU Furman Center|
|**med_r_1f**|*float*|The median price per unit for one unit buildings.|NYU Furman Center|
|**med_r_4f**|*float*|The median price per unit for two- to four-unit buildings.|NYU Furman Center|
|**med_r_cn**|*float*|The median price per unit for condominiums.|NYU Furman Center|
|**med_r_ot**|*float*|The median price per unit for multifamily buildings.|NYU Furman Center|
|**nb_permit_res_units**|*float*|The number of units authorized by new residential building permits is derived from the building permit and jobs reports of the DOB.|NYU Furman Center|
|**reo**|*float*|The total number of 1-4 family buildings that completed the foreclosure process and were acquired by the foreclosing lender.|NYU Furman Center|
|**serious_viol_rate**|*float*||NYU Furman Center|
|**total_viol_rate**|*float*|All violations that HPD defines as class C ('immediately hazardous') that were opened in a given time period, regardless of their current status. NOTE: In 2019 some violation types relating to pests were reclassified from B to C class resulting in a shift in the number of violations from B to C.|NYU Furman Center|
|**units_cert**|*float*|The number of residential units in buildings issued new certificates of occupancy issued by the DOB each year.|NYU Furman Center|
|**volume_1f**|*float*|The number of transactions of 1 family residential properties that have a non-trivial price and the sale must not be marked as 'insignificant' by the Department of Finance.|NYU Furman Center|
|**volume_4f**|*float*|The number of transactions of 2-4 family residential properties that have a non-trivial price and the sale must not be marked as 'insignificant' by the Department of Finance.|NYU Furman Center|
|**volume_al**|*float*|The number of transactions of all residential properties that have a non-trivial price and the sale must not be marked as 'insignificant' by the Department of Finance.|NYU Furman Center|
|**volume_cn**|*float*|The number of transactions of condominiums that have a non-trivial price and the sale must not be marked as 'insignificant' by the Department of Finance.|NYU Furman Center|
|**volume_ot**|*float*|The number of transactions of 5+ family residential properties that have a non-trivial price and the sale must not be marked as 'insignificant' by the Department of Finance.|NYU Furman Center|
|**hh_inc_med_adj**|*float*|The median household's total income of all members of the household aged 15 years or older.|NYU Furman Center|
|**hh_inc_own_med_adj**|*float*|The median owner-occupied household's total income of all members of the household aged 15 years or older.|NYU Furman Center|
|**hh_inc_rent_med_adj**|*float*|The median renter household's total income of all members of the household aged 15 years or older.|NYU Furman Center|
|**hp_first_fhava_pct**|*float*|The percentage of households living with children under 18.|NYU Furman Center|
|**hp_first_hi_pct**|*float*|The percentage of all first-lien loan originations, for the purchase of an owner-occupied home, condominium, or cooperative apartment that were insured or guaranteed by the FHA or VA, as reported by HMDA.|NYU Furman Center|
|**hp_first_orig**|*float*|The percentage of all first-lien loan originations, for the purchase of an owner-occupied 1-4 family home, that were reported as 'higher cost' under HMDA.|NYU Furman Center|
|**hp_first_orig_lmi_app_pct**|*float*|The number of Home purchase loan origination|NYU Furman Center|
|**hp_first_orig_lmi_nbhd_pct**|*float*|The share of all first-lien loan originations, for the purchase of an owner-occupied 1-4 family building, condominium, or cooperative apartment, that were made to low- to moderate-income borrowers.|NYU Furman Center|
|**hp_first_orig_rt**|*float*|The share of all first-lien loans, for the purchase of an owner-occupied 1-4 family building, condominium, or cooperative apartments, that were originated for homes in low- to moderate-income census tracts.|NYU Furman Center|
|**population_density**|*float*|The number of first-lien home purchase loan originations for owner-occupied 1-4 family buildings, condominiums, or cooperative apartments divided by the total number of 1-4 family buildings, condominiums, and cooperative apartments.|NYU Furman Center|
|**refi_hi_pct**|*float*|The percentage of loan originations, for the refinancing of an owner-occupied 1-4 family home, that were reported as 'higher cost' under HMDA.|NYU Furman Center|
|**refi_orig_rt**|*float*|The number of refinance loans for owner-occupied 1-4 family buildings, condominiums, and cooperative apartments divided by the total number of 1-4 family buildings, condominiums, and cooperative apartments in the given geographic.|NYU Furman Center|
|**rent_gross_med_adj**|*float*|The median gross rent, which includes the amount agreed to or specified in the lease regardless of whether furnishings, utilities, or services are included; and estimated monthly electricity and heating fuel costs paid by the renter.|NYU Furman Center|
|**unit_occ_own_pct**|*float*|The number of owner-occupied units divided by the total number of occupied housing units.|NYU Furman Center|


## Conclusion
While there were some low level correlations with neighborhood characteristics, no single feature displayed a strong enough relationship to make predictive statements about the likelihood of a development receiving a 421a tax abatement. This could be due to an insufficient amount of data or due to the model not capturing complex elements related to development decisions. As is, the model did not prove there are any features that influence the distribution of 421a incentives in a biased nature. Further research could be done to compare 421a to other tax incentive programs or isolate specific periods of 421a related policy.