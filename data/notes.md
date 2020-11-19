# Urban Institute data
split into three layers(?)
    - schools
    - school districts
    - colleges

## School layer [api docs](https://educationdata.urban.org/documentation/schools.html)
sources:

- Common Core of Data

    *The Common Core of Data is the US Department of Education's primary
    database on public elementary and secondary education. It provides directory
    and enrollment information at the school level and directory, enrollment,
    and finance data at the school district level.*
    
- The Civil Rights Data Collection

    *The Civil Rights Data Collection (CRDC) is a biennial survey required by
    the US Department of Education's Office for Civil Rights. The CRDC features
    data about enrollment, math and science courses, Advanced Placement courses,
    discipline, school expenditures, and teacher experiences.*
    
- EDFacts

    *The US Department of Education’s EDFacts initiative collects, analyzes, and
    centralizes data from state education agencies on various topics. Currently
    included in the explorer and portal are assessment data for reading and math
    for grades 3–12 at both the school and school district level.*
    
- National Historical Geographic Information System

    *Provided through IPUMS, the National Historical Geographic Information
    System contains population, housing, agriculture, and economic data for all
    census geographies from 1790 through the present.*
    
# Issues
1) python example request on urban institute school layer uses 
    ```from urllib import urlopen```
    on my machine, urllib will import alone, but urlopen will not. calling urlopen as urllib
    returns attribute error. error continues after updating to latest version of
    urllib
    FIX = In python3, urllib2 has been split into urllib.request and urllib.error