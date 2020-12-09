# Geosnap4ed
This project will build on geosnap's neighborhood construction capabilities to
include data from the Urban Institute and the Stanford Educational
Data Archive (SEDA) to include additional educational data parameters in neighborhood
construction.

# Urban Institute data
Notes summarizing the contents of the UI rest API data

split into three 'layers'

    - schools
    - school districts
    - colleges

**todo**: describe the contents of each sublayer and conceptualize linkages
    
    - what are key attributes per layer/sublayer
    - how complete is the data (check different years)
    - what are ways to link the data together?
    - pull data for specific area
    - note unique identifiers for joining data
     
## School layer [api docs](https://educationdata.urban.org/documentation/schools.html)

## School-Districts layer [api docs](https://educationdata.urban.org/documentation/school-districts.html)
    
#### notes
pinging the directory layer of school districts for 2019 returns no results, but
2018 does - 2018 should be considered the most recent data year in terms of
educational data available. 

## Colleges layer [api docs](https://educationdata.urban.org/documentation/colleges.html)

# Stanford Education Data Archive
SEDA notes

no api?

[nice GIS app on their website](https://edopportunity.org/)

from the
[codebook](https://stacks.stanford.edu/file/druid:db586ns4974/SEDA_documentation_v30_09212019.pdf):

> "The SEDA 3.0 achievement data is constructed using data from the EDFacts data system
housed by the U.S. Department of Education (USEd), which collects aggregated test score data
from each state’s standardized testing program as required by federal law." (pg 7)

> "(covariate data) included in the district, county, and metro covariates files come primarily from two sources. The
first is the American Community Survey (ACS) detailed tables which we obtained from the
National Historical Geographic Information System (NHGIS) web portal.1 These data include
demographic and socioeconomic characteristics of individuals and households residing in each
unit. The second is the Common Core of Data (CCD)...The measures included in the school covariates file come from the CCD as well
as the Civil Rights Data Collection (CRDC)" (pg 5)

[google scholar page for Sean F.
Reardon](https://scholar.google.com/citations?hl=en&user=LKx7rDsAAAAJ&view_op=list_works&sortby=pubdate)

# Issues 
## Outstanding

1) UI API seems to only pull 1000 records, resulting in incomplete datasets; for example only pulling records of school districts from Alabama - Arkansas, when surely other states have school districts as well. We discussed interfacing with the UI to find specific parameters for each endpoint, such as "max results".
    
2) the [api documentation for common core directory](https://educationdata.urban.org/documentation/school-districts.html#ccd_directory) list certain variables (`leaid`, `year`, `fips`, and `state_leaid`) as filters, but unsure if you can bake this into the URL. Other datasets, such as the the [Colleges IPEDS](https://educationdata.urban.org/documentation/colleges.html#ipeds_directory), feature additional filters seemingly not explicitly exploitable by the API URL.
    
3) examine 'count' and 'next' fields in returned UI JSONs

## Solved
1) python example request on urban institute school layer uses ```from urllib
   import urlopen``` on my machine, urllib will import alone, but urlopen will
   not. calling urlopen as urllib returns attribute error. error continues after
   updating to latest version of urllib. **fix** = In python3, urllib2 has been split into `urllib.request` and
`urllib.error`. Use `urllib.request` to request data from the API.