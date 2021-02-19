# Overview of the Test Score Data Files 

## SEDA 4.0 contains test score data files for:
  - schools
 - geographically defined school districts
 - counties
 - commuting zones
 - metropolitan statistical areas
 - states 
 
 Test score data files contain information about the average academic achievement as measured by standardized test scores administered in 3rd through 8th grade in [Mathematics test scores](mathematics_test_scores) and [Reading Language Arts (RLA (test scores)](reading_language_arts_rla_test_scores.md) over the 2008-09 through 2017-18 school years.
 
## School Files 
 There are two school-level test score data files, corresponding to the two different metrics in which the data are released: the [cohort standardized (CS)](cohort_standardized_cs) scale and the [grade cohort standardized (GCS)](grade_cohort_standardized_gcs) scale.
 
 In each file there are variables corresponding to the average test score in the middle grade of the data, the average "learning rate" across grades (grade slope), the "trend" in the test scores across cohorts (cohort slope), and the difference between math and RLA test scores (math slope). Each measure is included along with its respective standard error. Estimates are reported for all students; no estimates are provided by demographic subgroup.
 
## Geographic School District, County, Commuting Zone, Metropolitan Statistical Area, and State Files.
Thirty test score files are released corresponding to:
 - The five units (geographic school districts, counties, metropolitan areas, commuting zones, and states) 
 - Two scales (CS and GCS) 
 - Three pooling levels: 
	 - "Long" files contain estimates for each grade and year separately
	 - "Pooled by Subject" (or poolsub) files contain estimates that are averaged across grades and years within subjects
	 - "Pooled Overall" (or pool) files contain estimates that are averaged across grades, years, and subjects

## Pooling level differences 
In the long files there are variables corresponding to test score means by subgroup and their respective standard errors in each grade, year, and subject. 

In the poolsub files, there are variables corresponding to the average test score mean in math and in RLA (averaged across grades and years), the average "learning rate" across grades in math and in RLA, and the average "trend" in the test scores across cohorts in math and in RLA, along with their standard errors. 

In the pooled overall file, there are variables corresponding to the average test score mean (averaged across grades, years, and subjects), the average "learning rate" across grades, the average "trend" in the test scores across cohorts, and the average difference between math and RLA test scores, along with their standard errors. Estimates are reported for all students and by demographic subgroups.

#original