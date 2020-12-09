# Urban Institute: Website Data Source Notes

## Readme 
Urban Institute Website: Full API 
- Website Breakdown
    - 3 Topics
        - Schools 
        - School District
        - Colleges
    - 8 Integrated Data Sources 
        - Common Core of Data (CCD)
        - The Civil Rights Data Collection (CRDC)
        - Small Area Income and Poverty Estimates (SAIPE)
        - EDFacts 
        - Integrated Postsecondary Education Data System (IPEDS)
        - College Scorecard (Scorecard)
        - National Historical Geographic Information System (NHGIS)
        - Federal Student Aid (FSA)
    - Management 
        - **US Department of Education**, The Common Core of Data (CCD)
            - primary database on public elementary and secondary education 
        - **US Department of Education's Office for Civil Rights**, The Civil Rights Data Collection (CRDC)
            - biennial survey
        - **US Census Bureau's**, Small Area Income and Poverty Estimates (SAIPE)
            - program provides estimates of income and poverty 
        - **US Department of Education's**, EDFacts
            - intiative collects, analyzes, and centralizes 
        - **US Department of Education's**, National Center for Education Statistics, Integrated Postsecondary Education Data System (IPEDS)
            - interrelated survey
        - **Department of Education**, College Scorecard
            - maintained, contains data 
        - **IPUMS**, National Historical Geographic Information System (NHGIS)
            - provided through IPUMS 
        - **US Department of Education's**, Office of Federal Student Aid, Federal Student Aid (FSA)
            - data about federal finacial assistance programs
    - 3 Topics 
        - Schools (4)
            - CCD 
            - CRDC
            - EDFacts
            - NHGIS
        - School District (3)
            - CCD
            - SAIPE
            - EDFacts 
        - College (4)
            - IPEDS
            - Scorecard
            - NHGIS 
            - FSA 
    - Data sources vailable in 
        - Stata
        - R
        - Python
        - Javascript
        - direct download 

## Schools: Data Source
- Overview  
    - School-level database contains data from the following sources
        - Common Core of Data (CCD)
        - the Civil Rights Data Collection (CRDC)
        - EDFacts
        - National Historical Geographical Information (NHGIS)
    - Information about the data sources
        - CCD vs. CRDC  
            - CCD includes information on schools over a **long period** 
            - CRDC topics covered are **more limited**         
    - Data Elements 
        - CCD
            - Basic information about the school
                - location
                - grade offerings
                - charter status 
                - student enrollment demographics 
        - CRDC
            - Contains three years of data on every public school
                - student discipline
                - retention
                - course enrollment 
                - chronic absenteeism
                - SAT or ACT testtaking
        - EdFacts 
            - provides assessment data broken down by various subgroups 
        - NGHIS
            - provides census geographies for each school 
                - tract 
                - block group 

## School District: Data Source
- Overview 
    - district-level database contains data on school districts from 
        - the National Center for Education Statistics' Common Core of Data (CCD)
        - the US Census Small Area Income and Poverty Estimates (SAIPE)
    - Data Elements 
        - CCD 
            - contains information on
                - district characteristics
                - revenue
                - spending
        - SAIPE 
            - reports the number and share of
                - children ages 5 to 17 in poverty in each school district
## Colleges: Data Source
- Overview 
    - this institution-level database contains data on colleges and universities 
    - Data Elements
        - IPEDS
            - contains information on 
                - institution characteristics
                - admissions
                - enrollment
                - completions
                - graduation rates
                - program charges
                - other topics
        - Scorecard
            - is a collection of data from difference sources
                - including IPEDS
            - exclude the scorecard's data from IPEDS
            - contains information on 
                - student loan repayment
                - earnings 
        - NHGIS
            - for each institution 
                - tract 
                - block group 
            
        



            






