# Detailed Construction Overview
## Step 1: Creating the Crosswalk
The primary purpose of the [crosswalk](crosswalk.md) is to create stable school identifiers and assign schools to larger geographic units such as geographically-defined school districts, counties, metropolitan areas, commuting zones, and states. We use the **CCD**'s Public Elementary/Secondary School Universe Survey Data (Directory and School Characteristics files) and the Local Education Agency (School District) Universe Survey Data (Directory files) as the basis for the crosswalk. 

### Stable School IDs
Since we want to be able to track schools as they change districts (district changes could be due to districts splitting, merging or some other administrative change), we create stable school IDs using the CCD's Longitudinal ID Crosswalks. According to the CCD documentation, 

> "Schools are uniquely identified in CCD by the 12-digit variable **ncessch.** This variable is a combination of the state code (the first two digits or FIPST), the Local Education Agency (LEA) ID (the first seven digits or leaid) and the last five digits (schid).  It was always intended that the schid should be unique within the state so that a school could be tracked from year-to-year even if a re-organization caused it to change LEAs.  However, a system error created some duplicate schids within some states."

Because of some schools changed school IDs during the 2008-09 to 2017-18 time period, we use the CCD's longitudinal ID crosswalks12 from the [CCD's Reference Library](https://nces.ed.gov/ccd/reference_library.asp) to uniquely identify schools. These stable school IDs became the last 5 digits of the **sedasch** IDs. The final sedasch is comprised of 12 digits just like the NCES school ID, ncessch. Similar to ncessch, the sedasch ID's first 2 digits correspond to the state FIPS code, first 7 digits correspond to a stable district ID (sedalea), and the last 5 digits correspond to the school ID within the state. The next section describes how schools were assigned into geographic school districts. This assignment determines the 7-digit stable district ID that will be the first part of the sedasch ID.

### Assignment of Schools to Geographically Defined Districts in SEDA.
Most public school districts in the U.S. are geographically defined. In SEDA we use the 2019 EDGE Unified and Elementary School District Boundaries to define districts used in SEDA which we call [geographic school districts](geographic_school_district.md). Commonly, public school districts have administrative control over the traditional public schools that fall within their specific geographic boundaries. However, there may be some schools physically located within the geographic boundary of a school district that are not under its administrative control. For example, there may be **charter schools** or **magnet schools** located within the boundaries of a school district that are operated by a different school district or a charter school network (which may have no geographic boundary). 

In SEDA we have several rules around what schools are placed or excluded from geographic school districts based on location (latitude & longitude coordinates), school type information, and school status information. **The aim is for the district test score estimates in SEDA to reflect most of the public school students living within the geographic boundaries of the school district.** The motivation for this assignment is to better align the test scores for students living within school district boundaries with the demographic and socioeconomic data that we retrieve from other sources that report data by geographic school district boundaries. 

We use a school's most recently observed CCD information on school status, charter status, magnet status, coordinates, and county ID to create *time-invariant information* for schools in SEDA.  Below are the geographic district assignment rules in SEDA based on these time-invariant characteristics: 
  + **Charter schools**: All (except for special education) charter schools are geolocated and reassigned to the Elementary or Unified District in which they physically reside. 

  +  **Magnet schools**: All (except for special education) magnet schools are geolocated and reassigned to the Elementary or Unified District in which they physically reside.
 
  +   **Schools operated by secondary districts**: All schools with LEAIDS corresponding to secondary school districts in the Secondary School District Boundary file are geolocated to the Elementary or Unified geographic district in which they physically reside. This is because the EDFacts data we use is for grades 3-8. 
  
  +   **Virtual schools**: By their nature, most virtual schools do not draw students from within district geographic boundaries. We identify schools as virtual using CCD data from 2013-14 through 2017-18 Public Elementary/Secondary School Universe Survey Data. The virtual school identifier did not exist in earlier years of data, so we flag schools as virtual in all years of our data if they were ever identified as virtual by any later year CCD indicators. *Virtual schools are excluded from SEDA*. 
  
  +   **Special Education Schools**: We classify schools as Special Education schools if they are ever classified as "Special Education" in the school-type variable in the CCD data between 2009 and 2018. We exclude these schools from their geographic districts, counties, commuting zones, and metropolitan areas and instead assign them to a statewide "SEDA special education district." This ensures that their test scores are not used in estimating the test score distributions in any geographic unit (because many special education schools enroll students who take alternative assessments, their test scores are not comparable to those in other schools. We do not report test scores for such schools*. 
  
  +   **BIE Controlled Schools**: Schools controlled by the **Bureau of Indian Education (BIE)** are placed in the Elementary or Unified District in which they are physically located. Note that the current version of SEDA 4.0 does not include individual school estimates for BIE schools, they are included in the geographic district estimates as well as counties, metropolitan areas, and commuting zones. BIE school estimates will be included in a future release of SEDA. Schools operated by supervisory unions: We place all (except for special education) schools that are part of supervisory unions in their supervisory union LEAs. This rule mostly affects schools in Vermont and New York. For example, New York City School District (LEA 3620580) is a supervisory union comprised of 33 subordinate school districts. 
  
  +   **Closed Schools**: We geolocated all closed schools (except for special education schools) to the Elementary or Unified Districts in which they physically reside. 
  
  +   **District of Columbia Schools**: All schools within Washington, DC are given DC's geographic district ID (1100030).
  
  +   **Hawaii Schools**: All schools within Hawaii are given Hawaii's geographic district ID (1500030).
  
  +   **Puerto Rico Schools**: All schools within Puerto Rico are given Puerto Rico's geographic district ID (720003). 

All students in a school that is assigned to a particular geographically defined school district will be reflected in that district's estimate. School districts (most are geographically defined) used in SEDA are identifiable by their **sedalea**. You can identify a given school's assigned district by looking at the first 7 digits of the sedasch ID, which will be the sedalea ID. School Assignment to Higher Aggregations. 

For each school, we use the county code provided in CCD in the most recent year the school was observed. This county code (sedacounty) is stable over time. The county code is then used to merge on the 2013 metropolitan areas and 2010 commuting zones. Therefore, all schools in SEDA also have the same metropolitan area (sedametro) and commuting zone (sedacz), and state (fips) over time.

## Step 2: Data Cleaning 
In this step, we first merge the EDFacts data (described under II.A. Source Data) by NCES school ID (ncessch) and year with the crosswalk developed in Step 1. With this merge, the EDFacts data now have stable unit IDs (**sedasch**, **sedalea**, **sedacounty**, **sedametro**, **sedacz**, and **fips)** which will be used throughout the SEDA process. 

We then create flags for schools (by state, grade, year, and subject) that we intend to drop before estimation. The flags we create are listed below: 
 + **State participation is less than 95% in the tested subject-grade-year:** Using the EDFacts data, we are able to estimate a participation rate for all state-subject-grade-year cases in the 2012-13 through 2017-18 school years. This state-level suppression is important because *both the quality of the estimates and the linkage process depends on having the full population of student test scores for that state-subject-grade-year*. State participation may be low due to a number of factors, including student opt out or pilot testing. Note that we do not suppress any entire state-subject-grade-year cases prior to the 2012-13 school year as enrollment data are not available in EDFacts. However, opt out was low in 2012-13 (no state was excluded based on this threshold), which suggests states met 95% threshold in prior years when data are not available. A full list of the states, grades, years, and subjects this affects is in Table 4.
 
 + **Duplicate BIE or EDFacts IDs**: We remove a handful of places from the data that report data under both BIE school IDs and regular school IDs. These were identified by the NCES. According to the CCD documentation, "There is a possibility that some schools are reported in CCD by both the BIE and the state in which the schools are located, leading to a double counting of students and staff.  (NCES allows for the possibility of co-located schools, so a double-counting of schools is not an issue.)  *This arises from situations where both the state and BIE share operational or financial responsibilities for a school.* In order for SEDA to also avoid double counting, we remove the schools from the list and retain their counterparts listed in Table 5.
 
 + **Virtual schools**: We flag all virtual schools in EDFacts based on the crosswalk and remove them from SEDA.
 
 + **Not all students took the same content tests within the state-subject-grade-year**: There are two common ways this appears within the data. First, there are cases where districts were permitted to administer locally selected assessments. Second, in some cases students take end-of-course rather than end-of-grade assessments. When test scores measure different content and are reported on different scales using different cut scores, proficiency counts cannot be compared across districts or schools within these state-subject-grade-year cases. All of these flagged places are removed from SEDA.
 
 + **Insufficient data was reported to EDFacts: Some states reported no data in certain years**: These places are all flagged and removed from SEDA. See full list reported in Table 4 (marked as manual removals).
 
 + **NAEP data was not reported in any years or grades for a state-equivalent and subject.** Puerto Rico does not take the NAEP assessment in Reading Language Arts, so linking the Puerto Rico RLA estimates to a common national RLA scale is not possible.

### Alternate Assessments 
In 2008-09 through 2011-12, EDFacts does not distinguish students taking regular from alternate assessments; these counts were combined in the reported data. Therefore, for consistency in all years, *we combine the performance data for regular and alternate assessments as reported in EDFacts*. To ensure that all assessment's proficiency levels match the regular assessment's proficiency levels, we collapse the top categories for any places who have one higher proficiency level than the regular assessment. 
- The affected state, subject, grade, and year cases include: Arkansas, math and RLA, grades 3-8, years 2012, 2013, and 2014; Colorado, math and RLA, grades 3-8, years 2012, 2013, and 2014; Iowa, math and RLA, grades 3 through 8, years 2015 and 2018.

## Step 3: Cutscore Estimation and Linking 
In this step, we use **HETOP** models and the all-student geographic school district proficiency count data to estimate state-subject-grade-year cutscores on a common scale linked to NAEP after dropping the flagged places in the previous step and also removing any BIE schools for the creation of cutscores. 

To address practical challenges that can arise in linking and the HETOP estimation framework, within a specific state-subject-grade-year we:
  + Rearrange geographic school districts. We reconfigure geographic school districts that meet certain criteria within a state-subject-grade-year in order to improve the HETOP estimation process. This reconfiguration allows us to retain the maximum possible number of test scores in the estimation sample for the cutscores. This is important as the linking methods we use later in this step rely on having information about the full population in each state-grade-year-subject. 
  
	  + *First*, we combine vectors of counts that have fewer than 20 students into **"overflow" groups** because estimates based on small sample sizes can be inaccurate.
	  
	  + *Second*, in some vectors with more than 20 students the pattern of counts does not provide enough information to estimate a mean or a standard deviation; we also place these count vectors into the "overflow" group. *If the resulting overflow groups have parameters that cannot be estimated via maximum likelihood, they are removed from the data*.
	  
  + Constrain geographic school districts. For groups not in the "overflow" group, we always estimate a unique mean. But we can sometimes obtain more precise and identifiable estimates by placing additional constraints on group standard deviation parameters in the HETOP model. We constrain standard deviation parameter estimates for groups that meet the following conditions during estimation:
  
	  + There are fewer than 50 student assessment outcomes in a geographic school district.
	  
	  + There are not sufficient data to estimate both a mean and standard deviation (all student assessment outcomes fall in only two adjacent performance level categories; all student assessment outcomes fall in the top and bottom performance categories; or all student assessment outcomes fall in a single performance level category)
	  
**After these data processing steps, we estimate a separate HETOP model for each statesubject-grade-year and save the cutscore estimates**. For state-grade-year-subjects with only two proficiency categories, we cannot estimate unique geographic school district standard deviations and instead we use the model with a single, fixed standard deviation parameter (the HOMOP model).These cutscores are expressed in units of their respective state-year-grade-subject student-level standardized distribution. See **Reardon et al. (2017)** and the description in Step 5 below for additional details about the HETOP model.

**To place these cutscores on a common scale across states, grades, and years we use data from the *National Assessment of Educational Progress (NAEP)***. Because NAEP is administered only in 4th and 8th grades in odd-numbered years, we interpolate and extrapolate linearly to obtain estimates of these parameters for grades (3, 5, 6, and 7) and years (2010, 2012, 2014, 2016, and 2018) in which NAEP was not administered The reported national NAEP means and standard deviations, along with interpolated values, by year and grade, are shown in Table 6.

We then use these state-specific NAEP estimates to place each state's cutscores on the NAEP scale. The methods we use—as well as a set of empirical analyses demonstrating the validity of this approach—are described in more detail by **Reardon, Kalogrides, and Ho (2019)**

**Finally, we standardize the NAEP-linked cutscores relative to a *reference cohort of students***. This standardization is accomplished by subtracting the national grade-subject-specific mean and dividing by the national grade-subject-specific standard deviation for a reference cohort. We use the average of the four national cohorts that were in 4th grade in 2009, 2011, 2013, and 2015. We rescale at this step such that all means recovered in Step 5 will be interpretable as an effect size relative to the average of the four national cohorts that were in 4th grade in 2009, 2011, 2013, and 2015.


The resulting cutscores are on the [cohort standardized (CS)](cohort_standardized_cs.md) scale, standardized to this nationally averaged reference cohort within subject, grade, and year.

### ARCC & SBAC Cutscores for BIE Waiver Schools.
Once we have scaled cutscores, we take all states, years, and subjects that took the PARCC and SBAC and average their cuts together to get the appropriate PARCC cuts and SBAC cuts to apply to BIE waiver schools. The table of states, years, subjects averaged for this cut creation is in **Table 7**.

### Applying Cuts to BIE schools.
BIE Schools submitted data in different proficiency categories than the proficiency categories reported by the states in which the BIEs reside. In addition, the Navajo Nation had test waivers for the PARCC beginning in SY 2015-16 and Miccosukee Indian School which had a waiver for the SBAC starting in SY 2014-15. For these waiver schools, we use the averaged waiver cuts discussed above. For non-waiver BIE schools, we realign the BIE cuts to match the cuts for the states in which they were located. A few schools whose cuts we could not determine were omitted from SEDA. See Table 8. **Data for BIE schools will be included in a future SEDA release**.

## Step 4: Selecting Data for Mean Estimation
In Step 5, we estimate a model separately for each unit-subgroup that draws only on the subject-grade-year data for that unit-subgroup. In some subjects, grades, and years, we are less confident in the quality of the unit-subgroup data and do not want to include these in the estimation as it may bias the parameter estimates. *We create flags for dropping these cases*, which are described below:

  + **The participation rate is less than 95%**. If the participation rate for "all students" is less than 95%, we do not report any estimates for demographic subgroups regardless of whether the subgroup-specific participation rate was greater than 95% because we are concerned about data quality in cases with low overall participation.
  
  + **Incomplete data reported by student demographic subgroups**. There are a small number of cases where the total number of test scores reported by race or gender is less than 95% of the total reported test scores for all students. For example, there may be 50 test scores reported for all students, but only 20 test scores for male students and 20 test scores for female students. In this case, we would not report the male or female test score means because insufficient test scores were reported by gender. This measure can be constructed in all years. **We flag and remove any places that do not have at least a 95% representation rate for gender or race from SEDA**.
  
  + **More than 40% of students take alternate assessments**.  Measurement error may affect unit-subgroup-subject-grade-year cases where over 40% of the students take alternate assessments. These assessments typically differ from the regular assessment and have different proficiency thresholds. This flag can be constructed only in the 2012-13 through 2017-18 school years, so we do not apply this rule in earlier years. We flag and remove places that meet this criterion from SEDA at this step.
  + Students scored only in the top or only in the bottom proficiency category. We cannot obtain maximum likelihood estimates of unique means for these cases and therefore remove them prior to estimation. This flag can be constructed in every year.
 
*We next flag units-subgroup-subject-grade-year cells that do not meet the minimum statistical estimation requirements*, described below. 
  + **First**, we create a "type flag" for each unitsubgroup-subject-grade-year case. It is considered "insufficient" if the case meets one of the following conditions: 
  
	  a) has all observations in a single (middle) category 
	  b) has all observations in only 2 adjacent categories 
	  c) has only 2 proficiency categories (one cut score)
	  d) has all observations in only the top and bottom categories (e.g., X-0-0-X or X-0-X).
	  
	  Otherwise, cases are flagged as "sufficient." Constraints on the parameter estimates for "insufficient" cases are needed during estimation because they do not provide sufficient data to freely estimate both a mean and a standard deviation. 
+ **Second**, we construct a "size flag." We flag unit-subgroupsubject-grade-year cases as "small" if they have fewer than 100 test scores; otherwise, cases are flagged as "large". Each unit-subgroup-subject-grade-year, then, has two associated flags.

Prior to estimation, these flags are used to remove cases and unit-subgroups from the data. If a unit-subgroup has *only one* "insufficient" or "small" unit-subgroup-subject-grade-year case, then that case is dropped from the data. We also drop entire unit-subgroups that have no "sufficient" unit-subgroup-subject-grade-year cases. Our estimation methods, described in the next step, cannot produce a standard deviation estimate when all subject-grade-year cases for a given unit when these conditions are met. To see how many cases all of these drops affect and what is excluded from mean estimation, please see Table 9.

During estimation, these flags are used again to place constraints on the standard deviation estimates for individual unit-subgroup-subject-grade-year cases that are insufficient or small.

## Step 5: Estimating Test Score Means 
The goal of this step is to estimate the mean and standard deviation of test scores for each subgroup in each unit (schools, geographic school districts, counties, metropolitan areas, commuting zones, or states) across subjects, grades, and years.

### Pooled HETOP estimation. 
In the prior steps, we have prepared two pieces of information that we use in estimation: the observed proficiency counts for each unit-subgroup-grade-year-subject from **Step 4** and the estimated cutscores separating the proficiency categories in the associated state-grade-year-subject from **Step 3**. All schools are affiliated with a single state and, thus, a single test and a single set of cutscores. While larger units (districts, metropolitan areas, commuting zones, and states) are also typically affiliated with a single state, test, and set of cutscores, there are a few notable exceptions:

 - **Units that contain BIE schools**: As noted above, BIE schools often have different cut scores than the other schools assigned to the geographic school district, metropolitan area, commuting zone, or state. In these cases, the unit is split into two or more components where the schools assigned to each component take the same test and use the same cutscores. For example, we might split a unit into two pieces: unit-regular schools and unit-BIE waiver schools. 
 - **Metropolitan Areas or Commuting Zones that cross state lines**: A subset of metropolitan areas and commuting zones cross state lines and therefore can be affiliated with several state's cutscores. We split these units into metropolitan area-by-state or commuting zone-by-state components, where the schools assigned to each component took the same test and used the same cutscores. 
 
 For both of these cases, we estimate pooled HETOP using data for each subcomponent and the appropriate cut scores, we then aggregate the components after estimation into overall unit estimates

We use these data and a pooled HETOP model (**Shear & Reardon, 2021**; **Reardon et al., 2017**) to estimate the mean and standard deviation of achievement on the CS scale for *unit* (school, geographic school district, county, commuting zone-by-state, metropolitan area-by-state, or state), *subgroup*, *year*, *grade*, and *subject*. As described below, the pooled HETOP model is run separately for each unit-subgroup-subject, but combines data across grades and years when estimating these parameters. Combining data across grades and years allows us to get better estimates for years and grades in which sample sizes are small or where the proficiency count data are limited.

We use a pooled HETOP model in order to overcome three practical challenges. The challenges are: 1) in some states, years, and grades, *K* = 2 and there is not sufficient information to estimate both a mean and a standard deviation for each unit-subgroup-gradeyear-subject; 2) if *K* ≥ 3 but there are sampling zeros because test scores were not observed in all *K* categories for a particular grade and year, there may not be sufficient information to estimate both a mean and a standard deviation; and 3) when the sample size is small, prior simulations (e.g., **Reardon et al., 2017**; **Shear & Reardon, 2021**) have shown that estimates of standard deviations can be biased and contain excessive sampling error.

We estimate a pooled HETOP model (**Shear & Reardon, 2021**) for each unit, separately for each subject and subgroup, by "pooling" data across all available grades and years, and maximizing the joint log likelihood function given by:

> **Scientific notation not extracted - See original PDF**

The cutscores are treated as fixed here, using the values estimated in Step 3. We have replaced 휎푢푟푦푔푏 in the above equation with exp(ℎ푢푟푏(푔,푦)), where ℎ푢푟푏(푔,푦) is a unit-specific function used to model the natural log of the standard deviations as a function of grade and year: ℎ푢푟푏(푔,푦) = ln(휎푢푟푦푔푏) = 훾푢푟푦푔푏 . We do this for two reasons. First, estimating 훾푢푟푦푔푏 = ln(휎푢푟푦푔푏) rather than 휎푢푟푦푔푏 directly ensures that the ML estimate will be positive. Second, defining 훾푢푟푦푔푏 as a function of grade and year, rather than allowing this value to be unique in each grade and year, defines the pooled HETOP model. If we place no constraints on the model and allow ℎ푢푟푏(푔,푦) = 훾푢푟푏푔푦 to take on  a unique value in each grade and year, maximization of this likelihood will result in identical estimates to those obtained by maximizing the likelihood separately for each grade and year.

To leverage the data available across multiple grades and years and overcome the limitations noted above, we define ℎ푢푟푏(푔,푦) in the following way. First, we allow 훾푢푟푦푔푏 to be freely estimated in each grade-year cell that is both "sufficient" and "large", by the flags defined above. For all other grade-year cells, we constrain ℎ푢푟푏(푔,푦) such that the estimate of 훾푢푟푦푔푏 is equal to the mean of the 훾̂푢푟푦푔푏 estimates across the freely estimated cells. That is, we estimate a common "pooled" standard deviation across the grades and years in which there are "insufficient" data and/or "small" cell sizes. More formally, for all years and grades in which 푛푢푟푦푔푏 < 100 and/or in which there are insufficient data to estimate both a mean and a standard deviation, we constrain ℎ푢푟푏(푔,푦) = 훾푢푟푏 to be equal, while allowing ℎ푢푟푏(푔,푦) = 훾푢푟푦푔푏 to be freely estimated in grades and years with at least 100 test scores and sufficient data to estimate both a mean and standard deviation. We further constrain the model such that the "pooled" log standard deviation is equal to the (unweighted) mean of the unconstrained log standard deviations by defining the constraint: 훾푢푟푏 = ∑ ∑ (퐼푢푟푦푔푏 ∙퐼푢푟푦푔푏 ∙훾푢푟푦푔푏) ∑ ∑ (퐼푢푟푦푔푏 ∙퐼푢푟푦푔푏) , where 퐼 푢푟푦푔푏 100  is the size indicator flag (equal to 1 if size is "large") and 퐼푢푟푦푔푏 is the sufficient data indicator flag (equal to 1 if there are sufficient data). If 퐼푢푟푦푔푏 and 퐼푢푟푦푔푏 are equal to 1 for all cells in a unit, then we estimate a unique mean and standard deviation for each cell. For all other units, there will be a mix of freely estimated and constrained standard deviation parameters. Recall in Step 4 that we removed unit-subgroups where 퐼푢푟푦푔푏 = 0 for all cells because we are unable to estimate a standard deviation parameter.

In sum, the models described here are used to produce ML estimates of 휇푢푟푦푔푏 and 휎푢푟푦푔푏 (where 휎푢푟푦푔푏 may be constrained to be equal in some grades and years), as well as estimated standard errors 푠푒(휇푢푟푦푔푏) and 푠푒(휎푢푟푦푔푏) and the estimated sampling covariances 푐표푣(휇푢푟푦푔푏 ,휎푢푟푦푔푏), where unit can be a school, geographic school district, county, commuting zone-by-state, metropolitan area-by-state, or state. The estimates are produced on the CS scale described elsewhere, and can be transformed to other scales, as described in Step 6.

**Aggregating unit components**. For the subset of units where we need to split the unit into components for pooled HETOP estimation, we need to "re-aggregate" the components into complete unit estimates. The following summary is written for metropolitan areas that cross state lines; however, the same logic can be applied to units serving BIE schools or commuting zones that cross state lines." ([Manson, Steven et al 2019:29](zotero://open-pdf/library/items/Q5HJYYT3?page=29))

## Step 6: Creating Additional Reporting Scales
As described in **Step 3**, we standardize the [cutscores](cutscores.md) prior to estimation such that all mean estimates are produced on the [CS](cohort_standardized_cs.md) scale. In this step, we establish a second scale: The [Grade Cohort Standardized (GCS)](grade_cohort_standardized_gcs.md) scale. We recommend CS-scaled estimates for research purposes and the GCS-scaled estimates for low-stakes reporting to non-research audiences.

**Recall that the CS scale is standardized** within subject and grade, relative to the average of the four cohorts in our data who were in 4th grade in 2009, 2011, 2013, and 2015. We use the average of four cohorts as our reference group because they provide a stable baseline for comparison. This metric is interpretable as an effect size, relative to the grade-specific standard deviation of student-level scores in this common, average cohort. 

*For example*, a district mean of 0.5 on the CS scale indicates that the average student scored approximately one half of a standard deviation higher than the average national reference cohort scored in that same grade. Means reported on the CS scale have an overall average near 0 as expected. Note that this scale retains information about absolute changes over time by relying on the stability of the NAEP scale over time. This scale does not enable absolute comparisons across grades, however.

The GCS scale standardizes the unit means relative to the average difference in NAEP scores between students one grade level apart. The average grade-level difference in national NAEP scores is estimated as the within-cohort grade-level change for the average of four cohorts of students in *4th grade in 2009, 2011, 2013, and 2015.* See details on calculations in **Step 3**. 

We then identify the linear transformation that sets the grade 4 and 8 averages for this cohort at the "grade level" values of 4 and 8 respectively. Then transform unit means, standard deviations, and their variances accordingly.

Then, the coefficient can be interpreted as the estimated average national "grade-level performance" of students in unit *u*, subgroup *r*, year *y*, grade *g*, and subject *b*.

Means reported on the GCS scale have an overall average near 5.5 (midway between grades 3 and 8) as expected. This metric enables absolute comparisons across grades and over time, but *it does so by relying not only on the assumption that the NAEP scale is stable over time but also that it is vertically linked across grades 4 and 8 and linear between grades*. This metric is a simple linear transformation of the NAEP scale, intended to render the NAEP scale more interpretable.

For reference, *1 CS scale standard deviation is approximately 3 grade levels*. As such, this metric is useful for presenting descriptive research to broad audiences not familiar with interpreting standard deviation units. However, we do not advise it for analyses where the vertical linking across grades and the linear interpolation assumptions are not required nor defensible.

## Step 7: Calculating Achievement Gaps
We calculate achievement gap estimates in SEDA 4.0 for all units *except schools*. Gaps are estimated as the difference in average achievement between subgroups, using the mean estimates from **Steps 5** and **6**. We calculate White-Black (*wbg*), White-Hispanic (*whg*), WhiteAsian (*wag*), White-Native American (*wng*), White-Multiracial (*wmg*), male-female (*mfg*), and nonECD-ECD (*neg*) achievement.

The gaps can be interpreted similarly to the means in the units defined by the [CS](cohort_standardized_cs.md) and [GCS](grade_cohort_standardized_gcs.md) scales. If one or both of the subgroup means needed for the calculation is not available or excluded in a given unit-subject-grade-year, the gap estimate will be missing.

## Step 8: Pooled Mean and Gap Estimates 
**Pooled Mean Estimates**. For each unit-subgroup, we have up to 120 subject-grade-year mean estimates (10 years, 6 grades, 2 subjects). We pool the estimates within a unit using precision-weighted random-coefficient models. These models provide more precise estimates of:
 - average test scores in a unit (across grades and cohorts)
 
 -  estimates of the grade slope (the "learning rate" at which scores change across grades, within a cohort)
 
 -  cohort slope (the "trend" or rate at which scores change across student cohorts, within a grade). For

For geographic school districts, counties, metropolitan areas, commuting zones, and states, we provide both subject-specific and overall pooled estimates. For schools we provide only overall pooled estimates.

*For each of the following model-types, we estimate a single model drawing on data for all 50 states plus DC to recover pooled estimates*. Separate models are estimated for Puerto Rico because only math data is included in SEDA. These models are described at the end of the section
 - **Subject-Specific Pooled Estimates**. This model allows each unit-subgroup to have a subject-specific intercept (average test score), a subject-specific linear grade slope (the "learning rate"), and a subject-specific cohort trend (the "trend").
 
 - **Overall Pooled Estimates**. This model pools data across grades, years, and subjects to produce overall unit estimates. This model allows each unit to have a unit-specific intercept (average test score, pooled over subjects), linear grade slope (the "learning rate" at which scores change across grades, within a cohort, pooled over subjects), cohort trend (the "trend," or rate at which scores change across student cohorts, within a grade, pooled over subjects), and the math-RLA difference.

   - *Tables 11a-e* and *12a-e* report the variance and covariance terms from the matrices and the estimated reliabilities from estimated by the pooling models for all units.

 **OLS and EB Estimates from HLM Pooling Models**. SEDA 4.0 contains two sets of estimates derived from the pooling models described in Equations (8.1) and (8.2). First are what we refer to as the *OLS estimates*. Second are the *Empirical Bayes (EB)* shrunken estimates. 
   - The OLS estimates are the estimates that we would get if we took the fitted values from Equation (8.1) or (8.2) and added in the residuals These are unbiased estimates but they may be noisy in small units. We obtain standard errors of these as described in *Appendix A2*. 
   
   - The EB estimates are based on the fitted model as well, but they include the EB shrunken residual. The EB estimates are biased toward *y hat 00*, but have statistical properties that make them suited for inclusion as predictor variables or when one is interested in identifying outlier units. We report the square root of the posterior variance of the EB estimates as the standard error of the EB estimate.

 - In general, the EB estimates (marked as "eb" in the data files) should be used for descriptive purposes and as predictor variables on the right-hand side of a regression model; they are the estimates shown on the [edopportunity website](https://edopportunity.org). They **should not** be used as outcome variables in a regression model because they are shrunken estimates. Doing so may lead to biased parameter estimates in fitted regression models. The OLS estimates (marked as "ol" in the data files) are appropriate for use as outcome variables in a regression model. When using the OLS estimates as outcome variables, we recommend fitting precision-weighted models that account for the known error variance of the OLS estimates.

**Puerto Rico Pooled Estimates**. For schools, counties, and metropolitan areas in Puerto Rico, we use pool the subgroup-subject-grade-year estimates using a model similar to that in Equation 8.2, but omitting the centered math term (*Mb* −.5). We need to remove this term because we only produce estimates in Puerto Rico in math. The estimates produced in this model are reported as both the pooled overall and pooled subject estimates. Notes that this model only produces OLS estimates (not EB shrunken estimates, as discussed in the prior section).
	
**Pooled Gap Estimates**. We use the models in Equations 8.1 and 8.2 to pool gaps in districts, counties, metropolitan areas, commuting zones, and states. For example, the pooled White-Black gap parameter estimates in unit *u* are obtained by 1) computing the gap (the difference in mean White and Black scores) in each unit-grade-year-subject; and, 2) fitting model *8.1* or *8.2* above using these gaps on the left-hand side. However, notably the interpretation of the estimated pooling model coefficients differs. These models recover the average test score gap across grades and years, the rate of the gap changes over grades within cohorts, and the trend in the gap across cohorts within grades. 
 
 *For users interested in analyzing pooled achievement gaps*, it is important to use the pooled gap estimates (described above) rather than taking the difference between pooled estimates of group-specific means. *For example*, taking the difference of pooled White and Black mean scores will not yield the same White-Black pooled gap estimates as the above approach because the difference in the EB shrunken means is not generally equal to the EB shrunken mean of the gaps. The latter (using the pooled gaps) is preferred.

**Replicating the Pooled Estimates**. Notably, we pooled non-noised long-form estimates prior to data suppression, described in **Step 9**. Users will not be able to identically replicate our pooled estimates given two differences between the public long files and the ones used to create the pooled estimates: added noise and fewer estimates (described in more detail below). However, the results should be similar.

## Step 9: Suppressing Data for Release
**Long Form Files**. For the geographic school district, county, commuting zone, metropolitan area, and state long-form files, our agreement with the US Department of Education requires that (1) all reported cells reflect at least 20 students; and (2) that a *small amount of random noise is added to each estimate in proportion to the sampling variance of the respective estimate*. The added noise is roughly equivalent to randomly removing one student's score from each unit-subgroup-subject-grade-year estimate. These measures are taken to ensure that the raw counts of students in each proficiency category cannot be recovered from published estimates. 

The random error added to each to unit-subgroup estimate is drawn from a normal distribution. The SEs of the mean are adjusted to account for the additional error.

In addition, we remove any imprecise individual estimates where the [CS](cohort_standardized_cs) scale standard error is greater than 2. Any individual estimate with such a large standard error is too imprecise to use in analysis. We also remove all estimates associated with units that are based on more than 20% alternate assessments across the grades and years in the EDFacts data. **Table 13** summarizes the cases removed in the district, county, commuting zone, metropolitan area, and state long files.

**Pooled Files**. For a small number of units, there is insufficient data to recover an OLS estimate or SE for a given parameter. While we are able to recover EB estimates for these parameters, we do not release them. Moreover, in the interest of discouraging the overinterpretation of imprecisely estimated parameters, SEDA 4.0 does not report EB or OLS parameter estimates (the average test score, learning rate, trend in average test score) for a unit when the OLS reliabilities of the individual parameter are below 0.7.

We also remove all estimates associated with units that are based on more than 20% alternate assessments across the grades and years in the EDFacts data.  **Table 14** summarizes the cases removed in the school, district, county, commuting zone, metropolitan area, and state pooled files.

# Additional Notes

**Gender Mean and Gap Estimates**. Recent research reported by Reardon, Kalogrides, et al. (2019) suggests that the magnitude of gender achievement gaps can be impacted by the proportion of test items that are multiple-choice versus constructed-response. As a result, differences in gender gaps across states (or across time when a state changes the format of its test) may confound true differences in achievement with differences in the format of the state test used to measure achievement. See **Reardon, Fahle, et al. (2019)** for a description of an analytic strategy that can be used to adjust for these potential effects.

**Charter School Estimates**. Research reported in Reardon, Papay, Kilbride, et al. (2019) shows that estimates of student learning rates (the coefficient on the "grade" term in the pooling models in Step 8) are generally unbiased and reliable, except when student mobility in and out of schools is high. In the three states' data they used, student mobility was higher, on average, in small schools and districts, schools with long grade spans, and in charter schools. In addition, in very small schools and charter schools, the estimated grade slope is biased upwards, as a result of differential mobility (more lower-achieving students leave schools than enter). As a result, **we recommend that users interpret the school level grade slopes with some caution**, particularly for small schools, schools that span 4 or more of the grades from 3 to 8, and charter schools. Moreover, **users should be cautious in comparing grade slopes in charter schools to those of traditional public schools**given the evidence of systematic upward bias in the charter sector estimates.

#original