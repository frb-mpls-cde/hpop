# 👋 About the data 
*This page describes the goals of the homeowners-to-population ratio (HPOP) datasets and how they are assembled. For further technical details, please download the data and see the `data_dictionary` tab.*

---
# 🎯 Purpose
The [HPOP datasets](https://github.com/frb-mpls-cde/hpop) are a public data product from the [Community Development and Engagement](https://www.minneapolisfed.org/community-development-and-engagement) team at the Federal Reserve Bank of Minneapolis. 

This data product provides estimates of the *homeowners-to-population ratio* (HPOP), which is defined as the share of the adult population that owns their home. The HPOP is calculated using American Community Survey data from 2006 through 2024, and estimates of it are made available for various geographies (national, state, and core-based statistical area) and person-level characteristics (age group, race, and marital status). 

These homeownership estimates are meant to inform researchers, analysts, decision-makers, and others about current and historical homeownership conditions in the United States. 

The current version of the HPOP datasets reflects information (see below) as of **April 2026**. 

---
# 📰 Related publications
Read our article, “[New homeownership measure puts people first](minneapolisfed.org/article/2026/new-homeownership-measure-puts-people-first),” which explores how the HPOP more accurately captures the share of adults who own homes compared to the traditional owner-occupancy rate.

---
# 🌍 Scope 

These datasets include estimated HPOPs and owner-occupancy rates for a variety of geographic areas and demographic characteristics. This includes estimates at the national, state, and metropolitan-statistical-area levels. It also includes estimates by age, marital status, and race/ethnicity.

---
# 🏗️ Structure 

Each dataset contains four variables: 

- `year` = Year of estimate 

- `hpop` = HPOP (share of the adult population that owns their home) 

- `ownocc` = Owner-occupancy rate (share of occupied housing units where the owner is a resident) 

- `gq` = Share of the adult population living in group quarters

Geography levels are indicated where applicable.

When a dataset includes subgroups based on person-level characteristics, a numeric category (`*group`) and a corresponding description (`*label`) are defined. See the data dictionary for these mappings.

---
# 🌱 Sources 

## Primary data

The HPOP datasets are derived from American Community Survey (ACS) data. Additional data are used to translate ACS Public Use Microdata Area (PUMA) geographies into Core-Based Statistical Areas (CBSAs). 

Sources of this information are: 

- [U.S. Census Bureau American Community Survey](https://www2.census.gov/programs-surveys/acs/data/pums/)

- Geographic crosswalk files, from the [Missouri Census Data Center Geocorr Applications (2022)](https://mcdc.missouri.edu/applications/geocorr2022.html), using population weights and the U.S. Census Bureau [PUMA  relationship files](https://usa.ipums.org/usa/volii/pumas10.shtml)

---
# 📝 Technical notes 

## The HPOP

We define the HPOP (homeowners-to-population ratio) as the share of the adult population, $`n`$, that owns their primary residence. This ratio, in time period $`t`$, can be represented as: 

$$
HPOP_{t} = \frac{n_{h,t}}{N_{t}}
$$

where our population sample in time period $`t`$, denoted $`N_{t}`$, includes all adults (people ages 18 and older). The number of homeowners, denoted $`n_{h,t}`$, counts all adults who are homeowners ($`h=1`$) in time period $`t`$.   

We count an adult as a homeowner if two conditions are met. The first condition is that the adult lives in an owner-occupied housing unit. A housing unit is owner-occupied if the `ten` variable, representing housing tenure, is reported as either “Owned with a mortgage or loan” or “Owned free and clear.” Any adults living in non-owner-occupied housing units, including rental housing, housing “occupied without payment of rent,” or group quarters (which include college dormitories, nursing homes, correctional facilities, mental hospitals, military barracks, group homes, missions, and shelters) will not be counted.  

The second condition for counting the adult as a homeowner is that the adult is either the household reference person, or the spouse or unmarried partner of the reference person (who lives with the reference person). This relationship is determined by the `relationship` variable in the ACS. The reference person is assigned based on the person in the household in whose name “the house or apartment is owned, being bought, or rented.”<sup>1</sup> We weight observations based on the person-level weights (`pwgtp`) to reflect population-level statistics, and we report the HPOP as a percent, so we multiply $`HPOP_{t}`$ by 100. 

Given this definition, the HPOP can be interpreted directly as the share of the adult population that owns their home, if the assumption that spouses/unmarried partners are also owners in owner-occupied housing holds true. Alternatively, the HPOP could be interpreted as the share of the adult population that either owns or lives with a partner that owns their primary residence. 

## The owner-occupancy rate

In these data we compare the HPOP to what is often referred to as the homeownership rate but is technically the *owner-occupancy rate*. We follow the standard definition of the owner-occupancy rate as the share of occupied homes where the owner resides: 

$$
OwnerOccupied_{t} = \frac{h_{o,t}}{H_{t}}
$$

Where the sample of housing units, denoted by $`H_{t}`$, includes all occupied non-group-quarters units in time period $`t`$. The number of owner-occupied units is represented by $`h_{o,t}`$. We calculate owner-occupancy rates using household-level weights (`wgtp`) and similarly report owner-occupancy rates as a percentage.  

## Subnational-level statistics

We calculate the HPOP for a variety of subnational-level populations of interest. To calculate state-level estimates, we use values from the `state` variable. To calculate estimates at the CBSA (Core-Based Statistical Area) level, we translate PUMA (Public Use Microdata Area) geographies into CBSAs.<sup>2</sup> To do this, we crosswalk PUMAs to counties and then counties to CBSAs. Since PUMA geographies are modified with each decennial census, we base our PUMA-to-county crosswalk on 2020 PUMA geographies. We first translate all PUMAs into 2020 PUMAs.<sup>3</sup> After counties have been assigned to PUMAs, we then crosswalk counties to CBSAs.   

To calculate homeownership rates by age, we use the person-level `agep` variable and split it into seven categories: “18–24,” “25–34,” “35–44,” “45–54,” “55–64,” “65–74,” and “75 and older.” For our race/ethnicity estimates, we utilize the `rac1p` and the `hisp` variables. We create 10 mutually exclusive race/ethnicity categories by counting all Hispanic or Latino adults as a single category and then utilize the nine racial categories for all non-Hispanic or non-Latino adults. For our marital status estimates, we utilize the `mar` variable and its five categories: “Married,” “Widowed,” “Divorced,” “Separated,” and “Never married.” 

## Footnotes

1. The 2005–2007 relationship variable only identifies “In-laws,” while the files from 2008 and beyond separate this classification into “Parent-in-law” or “Son-in-law or daughter-in-law.” For 2005–2007, we categorize people who are in-laws and under the age of 55 as “Son-in-law or daughter-in-law,” and those 55 and older as “Parent-in-law,” based on the observed distribution of this variable by age in 2008 data. 

2. For definitions of CBSAs, see the U.S. Census Bureau’s [Delineation Files](https://www.census.gov/geographies/reference-files/time-series/demo/metro-micro/delineation-files.html) page. 

3. For PUMA crosswalk files, see IPUMs’ [2010 PUMAs](https://usa.ipums.org/usa/volii/pumas10.shtml) and the U.S. Census Bureau's [Relationship Files](https://www.census.gov/geographies/reference-files/time-series/geo/relationship-files.2020.html#puma_comp) pages.

---
# 📫 Contact 

The Community Development and Engagement team welcomes questions and feedback: [mpls.communitydevelopment@mpls.frb.org](mailto:mpls.communitydevelopment@mpls.frb.org).
