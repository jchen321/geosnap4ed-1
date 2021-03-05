# Common Core of Data Imputation 
School-level data from the CCD are available from **Fall 1987 until Fall 2018**. There is some missing data on racial composition and free/reduced price lunch receipt for some schools in some years. We therefore impute missing data on race/ethnicity and free/reduced priced lunch counts at the school level prior to aggregating data to the geographic school district, county, metropolitan area, or state level


##  Imputation model

The imputation model includes school-level data from the 1991-92 through 2018-19 school years and measures of total enrollment, enrollments by race (Black, Hispanic, White, Asian, and Native American), enrollments by free and reduced-priced lunch receipt *(note that reduced-priced lunch is only available in 1998 and later)*, an indicator for whether the school is located in an urban area, and state fixed effects. To improve the imputation of free and reduced-priced lunch in more recent years we also use the proportion of students at each school that are classified as economically disadvantaged in the EDFacts data for 2008-09 through 2017-18 in the imputation model. Different states use different definitions of economically disadvantaged but these measures are highly correlated with free lunch rates from the CCD (r=.90). The imputations are estimated using predictive **mean matching in Stata's -mi impute chainedroutine**, which fills in missing values iteratively by using chained equations. The idea behind this method is to impute variables iteratively using a sequence of univariate imputation models, one for each imputation variable, with all variables except the one being included in the prediction equation on the right-hand side. This method is flexible for imputing data of different types. For more information, see [the Stata documentation](https://www.stata.com/manuals/mi.pdf)

## Changes prior to imputation

Prior to the imputation, we make three changes to the reported raw CCD data. 

### Search for alternate (state) data sources in high FRPM states

First, for states with especially high levels of missing free and reduced-price lunch data in recent years, we searched state department of education websites for alternative sources of data. We were only able to locate the appropriate data for Oregon and Ohio. For these states we replace CCD counts of free and reduced-price lunch receipt with the counts reported in state department of education data for 2008-09 through 2017-18.In Ohio, 8% of schools were missing CCD free lunch data in 4 or more of the EDFacts years. In Oregon, 5% of schools were missing CCD free lunch data in 4 or more of the EDFacts years. Other states with high rates of missing free lunch data in the CCD during the EDFacts years are Alaska, Arizona, Montana, Texas, and Idaho. **Unfortunately, we were unable to locate alternative data sources for these states, and rely on the imputation model to fill in missing data.**

### impute FRPM levels in "community eligable (100% FRPM) schools"

Second, starting in the 2011-12 school year some states began using community eligibility for the delivery of school meals whereby all students attending schools in low-income areas would have access to free meals regardless of their individual household income. Free lunch counts in schools in the community eligibility program are not reported in the same way nation-wide in the CCD. In community eligible schools, some schools report that all of their students are eligible for free lunch while others report counts that are presumably based on the individual student-level eligibility. Because reported free lunch eligible rates of 100 percent in community eligible schools may not accurately reflect the number of children from poor families in the school, we impute free lunch eligible rates in these schools. We replace free and reduced priced lunch counts as equal to missing if the school is a community eligible program school in a given year and their reported CCD free lunch rate is 100 percent. We then impute their free lunch eligible rate as described above.

### Replace 0 FRPM as missing and impute 

Third, and finally, prior to imputation we replaced free and reduced-price lunch counts as missing if the count was equal to 0. Anomalies in the CCD data led some cases to be reported as zeros when they should have been missing so we preferred to delete these 0 values and impute them using other years of data from that school.

## Format
The structure of the data prior to imputation is wide - that is, there is one variable for each year for any given measure (i.e., total enrollment 1991, total enrollment 1992, total enrollment 1993, ..., total enrollment 2018) for all the measures described above. The exception are time invariant measures - urbanicity and state. We impute 6 datasets and use the average of the 6 imputed values for each school in each year. We then aggregate this imputed school by year data file to different geographic levels, computing our desired measures.

#original