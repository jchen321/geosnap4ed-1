# Covariate Data
## Extent
SEDA 4.0 also provides estimates of [Socioeconomic Estimates](socioeconomic_estimates.md), [Demographic Estimates](demographic_estimates.md), and [Segregation Estimates](segregation_estimates.md) characteristics of schools, districts, counties, metropolitan areas, and states. 

## Sources
The measures included in the district, county, metropolitan area, and state covariates files come primarily from two sources. 
 - The first is the American Community Survey (ACS) detailed tables which we obtained from the National Historical Geographic Information System (NHGIS) web portal. These data include demographic and socioeconomic characteristics of individuals and households residing in each unit. 
 - The second is the Common Core of Data (CCD) which is an annual survey of all public elementary and secondary schools and school districts in the United States.

## File structure and methods
Twelve files (three per aggregation) in SEDA 4.0 contain CCD and ACS that data have been curated for use with the geographic school district-level, county-level, metropolitan arealevel, and state-level achievement data. 

These data include raw measures as well derived measures (e.g., a composite socioeconomic status measure, segregation measures). Each of the three covariate files we construct for each unit contain the same variables but differ based on whether they report these variables separately for each grade and year, average across grades (providing a single value per unit per year), or average across grades and years (providing a single value per unit).

Two data files are provided for schools: 
	- one that includes an observation for each school in each year 
	- one that averages data across years and reports a single record for each school with these averages. 

School level data from the CCD is used to aggregate various measures to the geographic school district, county, and metropolitan statistical area levels. The measures from the ACS are downloaded separately at the school district, county, metropolitan area, and state levels of aggregation and are not available at the school level.

#original