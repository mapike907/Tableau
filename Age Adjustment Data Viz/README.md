# Data-Visualization Utilizing Tableau

This code produces outputs that are used in Tableau to create data visualization for Vaccine Breakthrough. This file shows the outputs created from this code and Tableau manipulation. Requires `12.21_VB%20Age%20Adjustment.sas` program to get outputs for Tableau.

![](Images/VB_DataViz_Slide1.png)

# Cases:

![](Images/VB_DataViz_Slide2.png)

Age-Adjusted rate is a measure that controls for the effect of age differences on health event rates. Populations were taken from the mid-point (15th) of each month.
Vaccination Status: A fully vaccinated person had a SARS-CoV-2 RNA or antigen detected on a respiratory specimen collected > 14 days after completing the primary series of an FDA-authorized or approved COVID-19 vaccine. A fully vaccinated person with one booster dose has received one additional dose of an FDA-authorized or approved COVID-19 vaccine on or after August 13, 2021, and had a SARS-CoV-2 RNA or antigen detected on a respiratory specimen > 14 days after receiving an additional dose. Additional dose can be Pfizer, Moderna, or J&J. 

Persons with partial vaccination status, who received one FDA-authorized vaccine dose but did not complete a primary series > 14 days before collection of a specimen where SARS-CoV-2 RNA or antigen was detected are considered unvaccinated. 

Data analysis performed on January 11, 2022 from cedrs_dashboardconstrained, filtering on variables: earliest_collectiondate, breakthrough, and booster cases by earliest_collectiondate >= 14-days following ‘vax_date’. Vax_date is third dose. 

# Hospitalizations:

![](Images/VB_DataViz_Slide3.png)

Age-Adjusted rate is a measure that controls for the effect of age differences on health event rates. Populations were taken from the mid-point (15th) of each month.
Vaccination Status: A fully vaccinated person had a SARS-CoV-2 RNA or antigen detected on a respiratory specimen collected > 14 days after completing the primary series of an FDA-authorized or approved COVID-19 vaccine. A fully vaccinated person with one booster dose has received one additional dose of an FDA-authorized or approved COVID-19 vaccine on or after August 13, 2021, and had a SARS-CoV-2 RNA or antigen detected on a respiratory specimen > 14 days after receiving an additional dose. Additional dose can be Pfizer, Moderna, or J&J. Hospitalization data obtained from COPHS and is lagged by two-weeks. November data is incomplete.

Persons with partial vaccination status, who received one FDA-authorized vaccine dose but did not complete a primary series > 14 days before collection of a specimen where SARS-CoV-2 RNA or antigen was detected are considered unvaccinated. 

Data analysis performed on January 11, 2022 from cedrs_dashboardcontrained filtering on variables: earliest_collectiondate, cophs_admissiondate, breakthrough and booster cases by earliest_collectiondate >= 14-days following vax_date.

# Deaths:

![](Images/VB_DataViz_Slide4.png)

Age-Adjusted rate is a measure that controls for the effect of age differences on health event rates. Populations were taken from the mid-point (15th) of each month. Death data is lagged by 1-month. December 2021 data is incomplete.

Vaccination Status: A fully vaccinated person had a SARS-CoV-2 RNA or antigen detected on a respiratory specimen collected > 14 days after completing the primary series of an FDA-authorized or approved COVID-19 vaccine. A fully vaccinated person with one booster dose has received one additional dose of an FDA-authorized or approved COVID-19 vaccine on or after August 13, 2021, and had a SARS-CoV-2 RNA or antigen detected on a respiratory specimen > 14 days after receiving an additional dose. Additional dose can be Pfizer, Moderna, or J&J. Death data is obtained from vital statistics and is lagged by four weeks. November data is incomplete.

Persons with partial vaccination status, who received one FDA-authorized vaccine dose but did not complete a primary series > 14 days before collection of a specimen where SARS-CoV-2 RNA or antigen was detected are considered unvaccinated. 

Data analysis performed on January 20, 2022 from cedrs_view, filtering on variables: earliest_collectiondate >= 08/13/2021, deathvsdeceased, and booster cases by earliest_collectiondate >= 14-days following vax_date.

# Data Analysis Walkthrough:

| Data | Description |
| --- | --- |
| Cases, Vaccine Breakthrough (VB) and Vaccine Breakthrough with Booster (VBB) | Pulled data subset from cedrs_view / cedrs_dashboardconstrained |
| Variables Pulled |  eventid, age_at_reported, collectiondate, earliest_collectiondate,vax_firstdose, vax_utd, vaccine_received, breakthrough, vax_booster, hospitalized_cophs, cophs_admissiondate, datevsdeceased, deathdueto_vs_u071 |
| Variables Created | Create variable vb_day_duration: find the difference between the test date (earliest_collectiondate) and date boosted (vax_booster), which needs to be >= 14 days to meet breakthrough case definition. |
| | Vax_booster date after CDC booster recommendation, 2021-08-13.|
| | Created variable “boosted_case” to distinguish between VB and VBB cases. Boosted_case = 0 is not boosted, VB. |
| | If breakthrough = 1 and vax_booster = ‘’ (missing) then boosted_case = 0 else if breakthrough = 1 and vax_booster < ‘2021-08-13’ then boosted_case = 0 |
| | If breakthrough = 0 then boosted_case = 0 (these are unvaccinated) |
| | Boosted_case = 1 is boosted, VBB. If breakthrough = 1 and vax_booster >= ‘2021-08-13’ and vb_day_duration >=14 then boosted_case = 1. |
| | Cases, Unvaccinated: Breakthrough = 0 and earliest_collectiondate >= ‘2021-01-01’ and earliest_collectiondate <= ‘2021-12-31’. This gives us only 2021 data. |
| Cases, Hospitalizations | hospitalized_cophs = 1 |
| Cases, Deaths | deathdueto_vs_u071 = 1 |
| Populations: VB, VBB and Unvaccinated (denominator data) | VB: Obtained monthly population data from ciis.vaxunvax_nobooster utilizing a summation of variable “cumulative_fullyvaxed_nobooster” for each age group (agegrp) by month. |
| | VBB: Obtained monthly population data from ciis.vaxunvax_aug13booster utilizing a summation of variable “cumulative_received_booster” for each each age group (agegrp) by month. |
| | Unvaccinated: obtained monthly population data from ciis.vaxunvax_population_byage utilizing a summation of variable “total_not_fully_vaccinated” for each age group (agegrp) by month.
| Age Adjustment Calcuations, VB, VBB Unvaccinated | Obtained cases_per100k for Cases & Hospitalizations
||Obtained cases_per1M for Deaths |
||Assign age groups (agegrp) a weight (weight) value based upon CDC Population data. |
||Cases_wt = cases_per100k*weight or cases_wt = cases_per1M*weight |
||Summation of all age group case_wt per month yields the Age Adjustment Rate. |






