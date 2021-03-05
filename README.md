# Geosnap4ed

## [Project planning notes](https://hackmd.io/CVR4gbd8TsW872VG23OxEA)

This project will build on geosnap's neighborhood construction capabilities to
include additional educational data parameters in neighborhood construction.

# Urban Institute data ([website](educationdata.urban.org))
### questions to consider
    
    - what are key attributes per layer/sublayer
    - how complete is the data (check different years)
    - what are ways to link the data together?
    - note unique identifiers for joining data
     
## School layer [api docs](https://educationdata.urban.org/documentation/schools.html)

## School-Districts layer [api docs](https://educationdata.urban.org/documentation/school-districts.html)
     

## Colleges layer [api docs](https://educationdata.urban.org/documentation/colleges.html)

# Stanford Education Data Archive [website](https://edopportunity.org/)

[google scholar page for Sean F.
Reardon](https://scholar.google.com/citations?hl=en&user=LKx7rDsAAAAJ&view_op=list_works&sortby=pubdate)

# Directory descriptions


data_markdown/SEDA4_docs/annotations_obsidian <-- annotated [SEDA4 Documentation](https://stacks.stanford.edu/file/druid:db586ns4974/seda_documentation_4.0.pdf) can opened as an obsidian vault.
    
census_reference_chart/ : per [issue 30](https://github.com/spatialucr/geosnap4ed/issues/30)<br>

# Issues 
## Outstanding

1) Interface with the UI to find specific parameters for each endpoint, such as
   "max results".
    
2) the [api documentation for common core directory](https://educationdata.urban.org/documentation/school-districts.html#ccd_directory) list certain variables (`leaid`, `year`, `fips`, and `state_leaid`) as filters, but unsure if you can bake this into the URL. Other datasets, such as the the [Colleges IPEDS](https://educationdata.urban.org/documentation/colleges.html#ipeds_directory), feature additional filters seemingly not explicitly exploitable by the API URL.
    
3) examine 'count' and 'next' fields in returned UI JSONs

## Solved
1) python example request on urban institute school layer uses ```from urllib
   import urlopen``` on my machine, urllib will import alone, but urlopen will
   not. calling urlopen as urllib returns attribute error. error continues after
   updating to latest version of urllib. **fix** = In python3, urllib2 has been split into `urllib.request` and
`urllib.error`. Use `urllib.request` to request data from the API.
