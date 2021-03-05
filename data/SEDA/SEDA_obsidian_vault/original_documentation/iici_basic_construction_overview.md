# Basic Construction Overview

## Step 1: Creating the [Crosswalk](crosswalk.md)
 This step links each public school to a unique stable school ID, geographic school district, county, commuting zone, metropolitan area, and state.

## Step 2: Data Cleaning  
This step removes data not used in SEDA 4.0.

## Step 3: Estimating and Linking [Cutscores](cutscores.md)
 This step uses Heteroskedastic Ordered Probit (HETOP) models to estimate the state-grade-subject-year cutscores, link the estimated cutscores to the NAEP scale, and standardize the linked cutscores to the [cohort standardized (CS)](cohort_standardized_cs.md) the resulting cutscores are comparable across states and years.

## Step 4: Selecting Data for Mean Estimation 
This step selects data for unit-subgroupsubject-grade-year cases that will be used in estimation. We exclude cases with low participation in the assessment or high percentages of students taking alternate assessments. We also exclude cases for which we have insufficient data to produce an estimate.

## Step 5: Estimating Means
 This step uses the pooled HETOP model to estimate school, district, county, commuting zone, metropolitan area, and state subgroup-subject-gradeyear means and standard deviations, along with their standard errors, based on the cutscores from Step 3 and the data prepared in Step 4." 

## Step 6: Creating Additional Reporting Scales
 This step creates [grade cohort standardized (GCS)](grade_cohort_standardized_gcs.md) estimates for all units, such that each unit is interpreted as 1 grade level. From this point onward, we have two scales of the data for all units: CS and GCS. Subsequent steps are equivalent for both scales unless otherwise noted.

## Step 7: Calculating Achievement Gaps
This step estimates White-Black, White-Hispanic, White-Asian, White-Native American, White-Multiracial, male-female, and nonpoor9- poor10 achievement gaps for districts, counties, metropolitan areas, commuting zones, and states in each subject-grade-year where there is sufficient data.

## Step 8: Pooling Mean and Gap Estimates 
 This step estimates the average achievement, learning rate, and trend in test scores by subject and overall for each unit and scale. From this point onward, we have three types data for districts, counties, metropolitan areas, commuting zones, and states: long (not pooled by grade, year, or subject), pooled by subject (poolsub), and pooled overall (pool). For schools, we only report the pooled overall (pool) estimates.

## Step 9: Suppressing Data for Release 
The step suppresses estimates that are too imprecise to be useful or do not reflect the performance of at least 20 unique students in both long and pooled files for all units and scales. For estimates reported in the long files, this step adds a small amount of random noise to meet the reporting requirements of the US Department of Education.

#original 