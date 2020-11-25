# Urban Institute data
split into three layers(?)

    - schools

    - school districts
    
    - colleges

## School layer [api docs](https://educationdata.urban.org/documentation/schools.html)
Sources:
- Common Core of Data<br>
    *The Common Core of Data is the US Department of Education's primary
    database on public elementary and secondary education. It provides directory
    and enrollment information at the school level and directory, enrollment,
    and finance data at the school district level.*
    
- The Civil Rights Data Collection<br>
    *The Civil Rights Data Collection (CRDC) is a biennial survey required by
    the US Department of Education's Office for Civil Rights. The CRDC features
    data about enrollment, math and science courses, Advanced Placement courses,
    discipline, school expenditures, and teacher experiences.*
    
- EDFacts<br>
    *The US Department of Education’s EDFacts initiative collects, analyzes, and
    centralizes data from state education agencies on various topics. Currently
    included in the explorer and portal are assessment data for reading and math
    for grades 3–12 at both the school and school district level.*
    
- National Historical Geographic Information System<br>
    *Provided through IPUMS, the National Historical Geographic Information
    System contains population, housing, agriculture, and economic data for all
    census geographies from 1790 through the present.*

## School-Districts layer [api docs](https://educationdata.urban.org/documentation/school-districts.html)
Sources:
- Common Core of Data<br>
    *The Common Core of Data is the US Department of Education's primary database on public elementary and secondary education. It provides directory and enrollment information at the school level and directory, enrollment, and finance data at the school district level.*

- SAIPE (Small Area Income and Poverty Estimates)<br>
    *The US Census Bureau's Small Area Income and Poverty Estimates 
    program provides estimates of income and poverty for every state and county.
    SAIPE also provides estimates of the number of school-age children in
    poverty for all school districts.*
    
- EDFacts<br>
    *The US Department of Education’s EDFacts initiative collects, analyzes, and
    centralizes data from state education agencies on various topics. Currently
    included in the explorer and portal are assessment data for reading and math
    for grades 3–12 at both the school and school district level.*
    
### WIP notes
pinging the directory layer of school districts for 2019 returns no results, but
2018 does - 2018 is most recent data year.

## Colleges layer [api docs](https://educationdata.urban.org/documentation/colleges.html)
Sources:
- Integrated Postsecondary Education Data System (IPEDS)<br>
    *The Integrated Postsecondary Education Data System (IPEDS) is built from
    interrelated surveys conducted annually by the US Department of Education's
    National Center for Education Statistics. IPEDS features data on enrollment,
    program completion, graduation rates, faculty and staff, finances,
    institutional prices, and student financial aid for every postsecondary
    institution that participates in the federal student aid program. The
    following endpoints are currently available:*
    
    Directory<br>
    Institutional characteristics<br>
    Admissions<br>
    Enrollment<br>
    Fall retention<br>
    Student-faculty ratios<br>
    Graduation rates<br>
    Outcome measures<br>
    Completers<br>
    Awards<br>
    Academic libraries<br>
    Finance<br>
    Student financial aid<br>
    Academic year charges<br>
    Program year charges<br>


- College Scorecard<br>
    *Maintained by the Department of Education, the College Scorecard contains data
    on costs, completion, financial aid, debt, repayment, and earnings for all
    degree-granting institutions.*

- National Historical Geographic Information System<br>
    *Provided through IPUMS, the National Historical Geographic Information System
    contains population, housing, agriculture, and economic data for all census
    geographies from 1790 through the present.*

- Federal Student Aid<br>
    *The US Department of Education's Office of Federal Student Aid provides data
    about federal financial assistance programs, including student aid data (Title
    IV loans, grants, and campus-based programs), and financial responsibility
    composite scores.*

## Issues (outstanding)
    1) API seems to only pull 1000 records, resulting in incomplete datasets; for example only pulling records of school districts from Alabama - Arkansas, when surely other states have school districts as well.
    
    2) the [api documentation for common core directory]https://educationdata.urban.org/documentation/school-districts.html#ccd_directory list certain variables (`leaid`, `year`, `fips`, and `state_leaid`) as filters, but unsure if you can bake this into the URL. Other datasets, such as the the [Colleges IPEDS](https://educationdata.urban.org/documentation/colleges.html#ipeds_directory), feature additional filters seemingly not explicitly exploitable by the API URL.
    
    3) examine 'count' and 'next' fields in returned JSONs

## Issues (fixed)
    1) python example request on urban institute school layer uses 
    ```from urllib import urlopen```
    on my machine, urllib will import alone, but urlopen will not. calling urlopen as urllib
    returns attribute error. error continues after updating to latest version of
    urllib
    FIX = In python3, urllib2 has been split into `urllib.request` and `urllib.error` for some reason

# Stanford Education Data Archive

no api?

[nice GIS app on their website](https://edopportunity.org/)

from the
[codebook]https://stacks.stanford.edu/file/druid:db586ns4974/SEDA_documentation_v30_09212019.pdf):

> "The SEDA 3.0 achievement data is constructed using data from the EDFacts data system
housed by the U.S. Department of Education (USEd), which collects aggregated test score data
from each state’s standardized testing program as required by federal law." (pg 7)

> "(covariate data) included in the district, county, and metro covariates files come primarily from two sources. The
first is the American Community Survey (ACS) detailed tables which we obtained from the
National Historical Geographic Information System (NHGIS) web portal.1 These data include
demographic and socioeconomic characteristics of individuals and households residing in each
unit. The second is the Common Core of Data (CCD)...The measures included in the school covariates file come from the CCD as well
as the Civil Rights Data Collection (CRDC)" (pg 5)

only data source unique to SEDA vs. Urban Institute is the ACS. UI contains
Edfacts, CCD, CRDC, NHGIS, wrapped in an API. 

One advantage SEDA might have is the variety of geographic units/analysis

[google scholar page for Sean F.
Reardon](https://scholar.google.com/citations?hl=en&user=LKx7rDsAAAAJ&view_op=list_works&sortby=pubdate)

