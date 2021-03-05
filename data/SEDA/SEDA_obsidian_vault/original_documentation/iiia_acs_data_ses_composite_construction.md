# ACS Data and SES Composite Construction 
For districts, counties, metropolitan areas and states we use data from the ACS to construct measures of:
	- median family income
	- proportion of adults with a bachelor's degree or higher 
	 - proportion of adults that are unemployed
	 - the household poverty rate
	 - the proportion of households receiving SNAP benefits
	 - the proportion of households with children that are headed by a single mother
**We also combine these measures to construct a single socioeconomic status composite.**

ACS data are available as 5-year pooled samples, from which we use samples from **2005-2009** through **2014-2018**.  The samples we use here reflect data for the total population of residents in each unit. In select years, district-level tabulations are also available for families who live in each school district in the U.S and who have children enrolled in public school. However, the most recent sample of this data that has all of the information we need is the **5-year 2007-2011** sample. We prefer to use the total population tabulation data from more recent years. We have compared measures constructed using the total population samples and the relevant children enrolled in public schools samples in years where both samples are available and the measures are highly correlated (r > 0.99) and not sensitive to which sample we use.

# Steps [^1][^2]

 ## Step 1 - Download/clean
 
 We download and clean the raw ACS data for each year and unit, saving the measures of interest along with their margins of error. We use data from the *2005-2009, 2006- 2010, 2007-2011, 2008-2012, 2009-2013, 2010-2014, 2011-2015, 2012-2016, 2013-2017,* and *2014-2018* samples. In *Appendix B1* we provide a list of the raw ACS data tables we downloaded and use to compute each derived measure.
 
 ## Step 2 - Combine fields
 
 Some of our derived measures require combining various fields from ACS in order to compute our desired metric *For example, in order to compute the proportion of adults with a bachelor's degree or higher we sum the number of men with a bachelor's degree, a master's degree or a professional degree with the number of women with a bachelor's degree, a master's degree or a professional degree and divide that sum by the total number of adults in the unit.* Each of these component measures is reported with its own margin of error in the raw ACS data. We use the margins of error from each component measure to generate a single standard error for the combined bachelor's degree attainment rate variable (and do the same for all 6 socioeconomic measures that make up the SES composite).  *Appendix B3* describes our methodology for computing the sampling variance of sums of ACS variables in detail.
 
 ## Step 3 - Impute missing data
 
 After constructing the 6 SES measures and their standard errors we impute some missing data **using Stata's -mi impute chainedroutine, which fills in missing values iteratively by using chained equations.** We reshape the data from long (one observation for each unit and race group (all, White, Black and Hispanic) in each year) to wide (one observation for each unit and a separate variable for each of the 6 SES by race measures in each year). We use both the 6 SES measures and their standard errors in the imputation model as well as the total population count in each unit. The imputation model, therefore, includes median income, proportion of adults with a bachelor's degree or higher, child poverty rate, SNAP receipt rate, single mother headed household rate, and unemployment rate for each race group (all, White, Black, Hispanic in each of 10-year spans for both the estimates and their standard errors. *We estimate the imputation model 5 times.* 
 
 ## Step 4 -  Compute SES composite
 
 Next, we use the imputed data to compute the SES composite. *This is done 5 times for each imputed data set and then we take the average.* This measure is computed as the first principal component score of the following measures (each standardized): median income, percent of adults ages 25 and older with a bachelor's degree or higher, child poverty rate, SNAP receipt rate, single mother headed household rate, and employment rate for adults ages 16-64. We use the logarithm of median income in these computations. 
 
 We calculate the component loadings by conducting the analysis in 2008-2012 at the [geographic school district](geographic_school_district.md) level and weighting by geographic school district enrollment. We then use the loadings from this principal component analysis to calculate SES composite values for different subgroups, years and units. Note that only observations without any imputed ACS data are used in the computation of the factor weights. *Table 15* shows the component loadings for the socioeconomic status composite as well as the mean and standard deviation of each measure it includes. 
 	
- The **"standardized loadings"** indicate the coefficients used to compute the overall geographic school district SES composite score from the 6 standardized indicator variables in 2008-2012, resulting in an SES composite that has an enrollment-weighted mean of 0 and standard deviation of 1 across all geographic school districts in 2008-2012 without any imputed data. 
- The "**unstandardized loadings"** are rescaled versions of the coefficients that are used to construct an SES composite score from the raw (unstandardized) indicator variables, but which is on the same scale as the standardized SES composite scores. To provide context for interpreting values of the SES composite, Table 16 reports average values of the indicator variables at different values of the SES composite.
 
## Step 5 - Construct standard Error

The next step is to construct a standard error of the SES composite. We discuss our methodology in detail in *Appendix B4.*
 
 ## Step 6 - Bayes shrinking
 
 The final step is to do the empirical Bayes shrinking for the SES composites as well as for each of the 6 SES measures that go into making the composite. In addition to the timevarying versions of the SES composite, we also create an SES composite that is the average of SES in the 2005-2009 and 2014-2018 ACS (i.e., using the first and last years' ACS samples). The shrinkage is done using a random effects meta-analysis regression model weighted by the standard error of each measure.

[^1]: Our derivation of these measures is complicated by the fact that we use the ACS-reported margins of error to compute empirical Bayes shrunken versions of our key ACS measures. The shrunken measures help account for attenuation bias that results from the fact that smaller units' measures include more measurement error due to smaller sample sizes. *Appendix B2* describes the problems of measurement error and attenuation bias in detail. 

[^2]:Note that we do not compute standard errors or empirical Bayes shrunken versions of state-level measures. State samples are sufficiently large as to not be concerned about measurement error due to small samples. Therefore, many of the steps described below only refer to district, county, and metropolitan area data.


#original