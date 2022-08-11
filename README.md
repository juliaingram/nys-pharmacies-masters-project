# As PBMs put pressure on independent pharmacies, areas in New York facing physician shortages lose them 
## Master's Project, Columbia Journalism School

This repo contains the code for the data collection and analysis behind my master's project on independent pharmacies in New York State. For this project, I scraped data on pharmacies registered with the state and categorized them as independent or part of a larger chain. I then analyzed change in the number of open pharmacies over time in areas designated to be health professional shortage areas, a federal designation indicating a lack of primary care services. I found that the number of independent pharmacies outside of New York City in a shortage area has declined over the last five and ten years. Within the city, independent pharmacies have grown, but that increase is higher in areas not facing a shortage. 
 
I also estimated the population outside of NYC living more than a 10-minute drive from a pharmacy in 2017 and 2022, finding that that population has increased from 15% of the state to 23% over the last five years. 

Below is the methodology for the data collection and analysis:

## Data Collection
### State Pharmacy Data
Data on all pharmacies in New York State was scraped from the New York Department of Education’s Office of the Professions online verification search engine on June 21, 2022. All pharmacy owners must register their pharmacy with the Office of the Professions, which oversees the state’s Board of Pharmacy. They are required to renew their registration every three years, and notify the state when they close. 

Each pharmacy is given a unique six-digit registration number, associated with an individual web page with information on their legal name, the name they do business as, their street address, the date they first registered and the date their registration ends. The date a pharmacy was first registered was assumed to be the day it opened and the day a pharmacy’s registration ends was assumed to be the date it closed. 

The data is stored as chunks of text on the state’s website, so regular expressions were used to turn it into structured data. The HERE API was used to geocode the pharmacies from their street address, identifying the latitude and longitude of each one.

### Health Professional Shortage Area Data
Federal data on primary care health professional shortage areas was downloaded from Health Resources and Services Administration (HRSA). It can be accessed on this page, under “Shortage Areas.”

## Analysis
### Classifying Pharmacies as Independent or Chains
Pharmacies established after 2004 — after modern PBMs started to form — were grouped by legal name to identify the largest chains. A pharmacy’s legal name is the parent company name they registered under, sometimes separate from the name they do business as or post on their storefront. Pharmacies with more than 10 locations in the state over that timeframe were classified as major chains, along with any of these companies’ subsidiaries. This list included CVS, Walgreens, Rite Aid, Duane Reade, Walmart and Stop & Shop, among others. 

Pharmacies associated with hospitals or clinics were excluded using keyword searches and manual checks. Publicly owned pharmacies, such as those associated with state universities or correctional facilities were also excluded. 

The remaining pharmacies with fewer than 10 locations under common ownership were classified as independent. Pharmacies with more than three locations under common ownership were manually checked to ensure they were not part of a minor chain that wasn’t captured earlier, such as Gristedes, a New York City based supermarket. The Medicine Shoppe pharmacies, a franchise where each location is independently owned, was also classified as independent. 

### Counting Pharmacies in Health Professional Shortage Areas
The HRSA data was filtered to only include shortage areas in New York State. Both geographic and population shortage areas were included, which reflect whether the Medicaid-eligible or whole population of a geographic area face a shortage. Designations proposed for withdrawal were excluded. 

From the larger pharmacy dataset, now categorized by pharmacy type, three smaller datasets were extracted: pharmacies open on June 21 in 2012, 2017, and 2022. This date was chosen because June 21, 2022 is the date the data was acquired and is most recent of. 

The three pharmacy data slices were spatially joined with the geographic outlines of the latest available census tracts for New York State to identify which census tract each pharmacy lied in. 

A list of currently designated health professional shortage areas extracted from the HRSA data was then used to label pharmacies as being in a shortage area or not. Shortage areas can either be census tracts, full counties, or county subdivisions. Full county shortage areas were broken down into census tracts. Shortage area county subdivisions, which account for 5% of the population facing a shortage area in New York, were excluded from the analysis because they do not always line up with census tracts. 

Finally, the labeled data allowed for percent change calculations of the total number of pharmacies in shortage area tracts vs. those without the designation over the five and ten year periods.

### Calculating the Population More than a 10-minute Drive from a Pharmacy
Using OpenRouteService, an API that will generate isochrones from a set of points, spatial files containing the total area within a 10-minute drive surrounding open pharmacies on June 21, 2017 and June 21, 2022 were generated. Isochrones are the area accessible from a given point within a specified timespan. 

The population living within that area was estimated using census tract level population estimates from the most recent American Community Survey. 
