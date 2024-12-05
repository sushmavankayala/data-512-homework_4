# WildLand Fire Analysis for Stockton, CA
#### DATA-512 | Homework 4 

## Background
More and more frequently summers in the western US have been characterized by wildfires with smoke billowing across multiple western states. There are many proposed causes for this: climate change, US Forestry policy, growing awareness, just to name a few. Regardless of the cause, the impact of wildland fires is widespread as wildfire smoke reduces the air quality of many cities. There is a growing body of work pointing to the negative impacts of smoke on health, tourism, property, and other aspects of society.

## Project Goal

### Part 1 - Common Analysis
Part 1 of this project aims to investigate the effects of wildfires on Stockton, CA. We will specifically analyze different features of wildfires like total acreas burnt, disatnce from city, etc. We combine this information with Air Quality Index (AQI) estimates and analyse the impact over the last 60 years. We aim to come up with a metric to meaingfully represent smoke estimates in the city, and forcast the smoke estimates till the year 2050.

### Part 2 - Extension

**Project Goal:**

Part 2 of this project explores the health impacts of wildfire smoke, focusing on respiratory diseases and cancer. Wildfire smoke, particularly fine particulate matter (PM2.5), is linked to conditions like asthma, COPD, lung infections, and certain cancers. By analyzing health data and wildfire smoke estimates, the goal is to provide insights for public health officials in Stockton. We also aim to utilize the forecasted smoke estimates from **Part 1** to estimate how the health metrics are likely to change in the future.  Key questions include:
- How does wildfire smoke exposure affect mortality rates?
- Is there a correlation between wildfire smoke and hospitalizations?

Additionally, we seek to enhance reproducibility and develop professional skills necessary for conducting data-driven analyses, as part of the Autumn 2024 DATA 512 course at the University of Washington.

## License

### Code for Data Aquisition
Snippets of the code used in this project were taken from examples was developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.1 - August 16, 2024

A copy of the examples referred is also available in this repository under the folder named `reference_code`.

## Source Data

### Part 1 - Common Analysis

#### Wildland Fire Data  

The wildfire data for this project was sourced from the [Combined Wildland Fire Datasets for the United States and Certain Territories, 1800s-Present](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81). This dataset includes a "combined" dataset free of duplicates and containing information about various wildland fires. For our analysis, we used the JSON file named `USGS_Wildland_Fire_Combined_Dataset.json`. Due to its size exceeding the limits of Git, the file is not committed to the repository.

**Key variables in this dataset**:  
- `Fire Name` *(String)*: Identifier or name of the wildfire.  
- `Fire Start Date` *(Date)*: The starting date of the fire.  
- `Fire End Date` *(Date)*: The date when the fire was extinguished or contained.  
- `Fire Type` *(String)*: The category of fire, such as vegetation or forest fire.  
- `Burned Area` *(Float)*: The total area affected by the fire, typically measured in acres.  
- `Location` *(Float, Float)*: Latitude and longitude coordinates indicating the geographic location of the wildfire.  

#### Air Quality Index Data  

The air quality index (AQI) data for this project was retrieved from the [EPA Air Quality Service (AQS) API](https://aqs.epa.gov/aqsweb/documents/data_api.html). This dataset contains daily AQI measurements from monitoring stations. To fetch AQI data near Stockton, CA, we used the Federal Information Processing Standards (FIPS) codes for city, county, and state, obtained from the [Census Bureau](https://www.census.gov/library/reference/code-lists/ansi.html).

**Key variables in this dataset**:  
- `Date` *(Date)*: The date of air quality measurement.  
- `Station ID` *(String)*: A unique identifier for each monitoring station.  
- `AQI Value` *(Integer)*: The calculated AQI score for the given date and location.  
- `Pollutant` *(String)*: The specific pollutant measured, such as PM2.5 or Ozone.  
- `Latitude/Longitude` *(Float, Float)*: Coordinates of the monitoring station.  
- `State/County/City FIPS Codes` *(String)*: Numeric codes representing the geographic location of the measurements.  

### Part 2 - Extension

#### Multiple Causes of Death - CDC  

The [Multiple Cause of Death database](https://wonder.cdc.gov/mcd.html) provides mortality and population statistics for all U.S. counties based on death certificates. Each certificate includes the primary cause of death, up to twenty contributing causes, and demographic details.

**Key variables in this dataset**:  
- `Year` *(Integer)*: The year of death.  
- `County` *(String)*: The county where the death occurred.  
- `Cause of Death` *(String)*: The primary and contributing causes of death, categorized by ICD-10 codes.  

#### Healthcare Cost and Utilization Project - AHRQ  

The [Healthcare Cost and Utilization Project (HCUP)](https://hcupnet.ahrq.gov/) is a resource by the Agency for Healthcare Research and Quality (AHRQ) that provides insights into U.S. hospitalizations, healthcare utilization, and costs. This dataset helps analyze trends in care access, outcomes, and costs.

**Key variables in this dataset**:  
- `Hospitalization Year` *(Integer)*: The year of admission.  
- `Hospital ID` *(String)*: Unique identifier for hospitals.  
- `Diagnosis Codes` *(String)*: ICD-10 codes representing primary and secondary diagnoses.  
- `Procedure Codes` *(String)*: ICD-10 codes for procedures performed.  
- `Discharge Status` *(String)*: Patient discharge outcome (e.g., routine, transferred, deceased).  
- `Length of Stay` *(Integer)*: Duration of hospitalization in days.  
- `Total Charges` *(Float)*: Total cost of hospitalization.  

#### California Health and Human Services (CalHHS)  

The [California Health and Human Services Open Data Portal](https://data.chhs.ca.gov/) offers access to aggregated, non-confidential health data. For this analysis, hospital discharge data, which includes aggregated insights from the HCUP dataset, was utilized.

**Key variables in this dataset**:  
- `Year` *(Integer)*: The year of discharge.  
- `County` *(String)*: The county where the hospital discharge occurred.  
- `Diagnosis Codes (ICD-10)` *(String)*: Codes for primary and secondary diagnoses.  
- `Procedure Codes (ICD-10)` *(String)*: Codes for medical procedures performed.  
- `Discharge Status` *(String)*: Patient outcome post-discharge.  
- `Hospital Type` *(String)*: Classification of the facility (e.g., general, specialty).  
- `Race and Ethnicity` *(String)*: Demographics of the patients.  
  
*Note: Per my initial extension plan, and I intended to use data from CDC and AHRQ for my analysis. I recently found the third source CalHHS, which directly provides aggregated data that includes the information from the AHRQ dataset. Hence, I will use the CalHHS data in place of AHRQ data for this analysis. I included the details about AHRQ nonethless, since it might be useful for someone who would want to extend my analysis to include length of stay*

## Documentation

#### API

*  US Environmental Protection Agency (EPA) Air Quality Service (AQS) [API](https://aqs.epa.gov/aqsweb/documents/data_api.html): These APIs are used to fetch details about air pollutants, monitoring stations, and the AQI readings for each station.

#### Packages

* [geojson](https://pypi.org/project/geojson/), [pyproj](https://pyproj4.github.io/pyproj/stable/): These packages are used to process and handle geographical data
* [requests](https://requests.readthedocs.io/en/latest/api/#): This package is used to make requests to APIs
* [pandas](https://pandas.pydata.org/docs/reference/index.html), [polars](https://docs.pola.rs/api/python/stable/reference/index.html): These packages are used for loading data from files, processing different columns and writing data into a file
* [matplotlib](https://matplotlib.org/stable/api/index.html): This package is used to plot graphs for visual analysis of the data.
* [sklearn](https://scikit-learn.org/stable/api/index.html), [statsmodels](https://www.statsmodels.org/stable/api.html), [scipy](https://docs.scipy.org/doc/scipy/reference/): These packages are used in building a prediction model for smoke estimates.


## How to Run

The complete project code is organized into the following 5 Jupyter notebooks:

* [1_data_aquisition_wildfire.ipynb](1_data_aquisition_wildfire.ipynb): The goal of this notebook is to process the raw information on wildland fires and calculate the shortest distance of each wildland fire from Stockton, CA
* [2_data_aquisition_aqi.ipynb](2_data_aquisition_aqi.ipynb) : The goal of this notebook is to fetch AQI information from monitoring stations near Stockton,CA. 
* [3_data_cleaning.ipynb](3_data_cleaning.ipynb): The goal of this notebook is to aggregate the highly granual AQI data from multiple monitoring stations to an yearly AQI estimate at Stockton, CA
* [4_smoke_estimates.ipynb](4_smoke_estimates.ipynb): The goal of this notebook is to calculate smoke estimates by using the wildland fire information like total acres burnt, distance from Stockton, CA, etc 
* [5_data_visualization.ipynb](5_data_visualization.ipynb): The goal of this notebook is to create plots for an easy understanding of the trends in data.
* [6_health_data.ipynb](6_health_data.ipynb): The goal of this notebook is to clean, process and merge all the health related data to enable analysis.
* [7_analysis_and_extended_model.ipynb](7_analysis_and_extended_model.ipynb): The goal of this notebook is to analyse correlations between heath related metrics and smoke estimates. It also forecasts the metrics closely correlated to the smoke estimate, upto year 2050.

Steps to run:

1. Download the combined dataset named `USGS_Wildland_Fire_Combined_Dataset.json` from [Combined wildland fire datasets for the United States and certain territories, 1800s-Present (combined wildland fire polygons](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) and place it in `./resources` folder. The [1_data_aquisition_wildfire.ipynb](1_data_aquisition_wildfire.ipynb) reads the json from this location and processes the data.
2. We need an API key to call the EPA AQS API. The code to raise a sign up request for API key is present in [2_data_aquisition_aqi.ipynb](2_data_aquisition_aqi.ipynb), with the invocation commented out. Generate an API key for yourself and update the code to use that API key. Note that this AQI data aquisition can take time, and it is suggested to use processed file present in the repository
3. Run the notebooks in the above sequence. All the required dependencies are installed in the inital cell of the notebook. If you come across a missing module error while executing the notebook, please run `pip install <module_name>`

## Generated Files

Each notebook generates some intermediary files that are used by the later notebooks. Below is a list of the generated intermediary files:

1. [wildfires_with_distances.csv](generated_files/intermediate/wildfires_with_distances.csv): Contains a list of wildland fires, with an added column representing the shortest distance of the wildfire from Stockton, CA. This is created by [1_data_aquisition_wildfire.ipynb](1_data_aquisition_wildfire.ipynb)
2. [gaseous_AQI_1964-2024.csv](generated_files/intermediate/gaseous_AQI_1964-2024.csv): Contains the response from EPA AQI API call for fetching daily summaries of gaseous pollutants. This is created by [2_data_aquisition_aqi.ipynb](2_data_aquisition_aqi.ipynb)
    Fields:
    - `state_code`: String – Code for the state.
    - `county_code`: String – Code for the county.
    - `site_number`: Integer – Unique site identifier.
    - `parameter_code`: String – Code for the measured pollutant.
    - `poc`: Integer – Parameter occurrence code.
    - `latitude`: Float – Latitude of the monitoring station.
    - `longitude`: Float – Longitude of the monitoring station.
    - `datum`: String – Geodetic datum.
    - `parameter`: String – Name of the pollutant measured.
    - `sample_duration_code`: String – Code for sample duration.
    - `sample_duration`: String – Duration of the sample.
    - `pollutant_standard`: String – Standard used to measure the pollutant.
    - `date_local`: Date – Date of the measurement.
    - `units_of_measure`: String – Units used to measure AQI.
    - `event_type`: String – Type of event (e.g., "normal", "alert").
    - `observation_count`: Integer – Number of observations.
    - `observation_percent`: Float – Percentage of valid observations.
    - `validity_indicator`: String – Indicator of data validity.
    - `arithmetic_mean`: Float – Mean of the AQI values.
    - `first_max_value`: Float – First maximum AQI value.
    - `first_max_hour`: Integer – Hour of the first maximum AQI value.
    - `aqi`: Integer – The AQI value.
    - `method_code`: String – Code for the measurement method.
    - `method`: String – Description of the measurement method.
    - `local_site_name`: String – Local name of the monitoring site.
    - `site_address`: String – Address of the monitoring site.
    - `state`: String – Name of the state.
    - `county`: String – Name of the county.
    - `city`: String – Name of the city.
    - `cbsa_code`: String – Code for the Core-Based Statistical Area.
    - `cbsa`: String – Name of the CBSA.
    - `date_of_last_change`: Date – Last date the data was updated.
3. [particulate_AQI_1964-2024.csv](generated_files/intermediate/particulate_AQI_1964-2024.csv): Contains the response from EPA AQI API call for fetching daily summaries of particulate pollutants. This is created by [3_data_cleaning.ipynb](3_data_cleaning.ipynb)
    Fields: (Same as gaseous_AQI_1964-2024.csv, as the structure is identical)
4. [yearly_weighted_aqi_1964-2024.csv](generated_files/intermediate/yearly_weighted_aqi_1964-2024.csv): Contains the yearly AQI estimates at Stockton, CA that was calculated by aggregating daily summaries received from multiple monitoring stations. This is created by [4_smoke_estimates.ipynb](4_smoke_estimates.ipynb)
    Fields:
    - `year`: Integer – Year of the data.
    - `weighted_avg_aqi`: Float – Weighted average AQI for the year.
5. [smoke_estimates_1964-2024.csv](generated_files/intermediate/smoke_estimates_1964-2024.csv): Contains the yearly smoke estimates at Stockton, CA that were calculated using the wildland fire information like area burnt, distance from city, etc. This is created by [4_smoke_estimates.ipynb](4_smoke_estimates.ipynb)
    Fields:
    - `Fire_Year`: Integer – Year of the wildfire event.
    - `smoke_estimate`: Float – Estimated smoke impact (based on area burned, distance, etc.)
6. [discharge_and_death_data_1999_2020.csv](generated_files/intermediate/discharge_and_death_data_1999_2020.csv): Contains the yearly health related metrics, i.e, deaths per each of the 5 PM2.5 related diseases, and hospital discharges related to respiratory diseases. This is created by [6_health_data.ipynb](6_health_data.ipynb)

    Fields:
    - `Year`: Integer – Year of the data.
    - `Asthma`: Integer – Number of asthma-related deaths.
    - `Lung cancer`: Integer – Number of lung cancer-related deaths.
    - `Pneumonia`: Integer – Number of pneumonia-related deaths.
    - `Colon cancer`: Integer – Number of colon cancer-related deaths.
    - `COPD`: Integer – Number of COPD-related deaths.
    - `Respiratory Diseases`: Integer – Total deaths related to respiratory diseases.
    - `Cancer`: Integer – Total cancer-related deaths.
    - `Total Deaths`: Integer – Total deaths from all causes.
    - `Year_right`: Integer – Year corresponding to the hospital discharge data.
    - `Discharges`: Integer – Number of hospital discharges related to respiratory diseases.

## Generated Plots

### Common Anaysis
The notebook [5_data_visualization.ipynb](5_data_visualization.ipynb) generates 3 plots and saves them in the `generated_plots` folder.

Description of the generated visualisations are written in [Part1_Reflection.pdf](Part1_Reflection.pdf)

### Extension Plan

As part of my analysis to understand how the number of deaths and hospital discharges is correlated to smoke estimates, I generated the below plots. Note that all these plots are related to PM2.5 related diseases considered for the analysis. More details on the exact diseases can be found in [6_health_data.ipynb](6_health_data.ipynb)

- **[total_deaths_per_year.png](generated_plots/total_deaths_per_year.png)**: This plot shows the total number of deaths per year in San Joaquin County from 1999 to 2020.
- **[categorical_deaths_per_year.png](generated_plots/categorical_deaths_per_year.png)**: This plot displays the deaths categorized by major disease types (e.g., respiratory diseases, cancer) per year.
- **[deaths_per_disease_per_year.png](generated_plots/deaths_per_disease_per_year.png)**: This plot shows the number of deaths per disease category (respiratory, cancer, etc.) on a yearly basis.
- **[respiratory_discharges_and_deaths_per_year.png](generated_plots/respiratory_discharges_and_deaths_per_year.png)**: This plot compares the annual number of hospital discharges and deaths due to respiratory diseases.
- **[smokeestimate_death_discharge.png](generated_plots/smokeestimate_death_discharge.png)**: This plot shows the relationship between smoke estimates and respiratory-related deaths and discharges over the years.
- **[forecasted_deaths_Respiratory Diseases.png](<generated_plots/forecasted_deaths_Respiratory Diseases.png>)**: This plot presents the forecasted number of deaths due to respiratory diseases from 2020 to 2050 based on predicted smoke estimates.
- **[forecasted_deaths_Asthma.png](generated_plots/forecasted_deaths_Asthma.png)**: This plot illustrates the forecasted number of deaths due to asthma from 2020 to 2050, based on the predicted smoke estimates.
- **Smoke Estimate_<disease>**: These plots illustrate the trends between smoke estimates and specific diseases over the years. 

### Considerations and Limitations

1. **Missing Data**  
   - **Incomplete AQI Data:** During AQI data retrieval, particulate matter data (PM2.5) was missing for some years, while gaseous pollutant data was available. This could skew our aggregated AQI estimates since PM2.5 is a critical component of wildfire smoke analysis.  
   - **Daily AQI Gaps:** Approximately 20% of daily AQI records fetched from the API had missing values. While we proceeded by dropping these records, imputation techniques or alternative estimation methods could be explored to address these gaps.  
   - **Wildfire Data Limitation:** The wildfire dataset only extended up to 2020, despite our interest in analyzing data through 2024. Additional sources or updated datasets may be required to fill this gap.
2. **Error Handling**  
   - **Distance Calculations:** A small number of wildfires were excluded due to failures in shortest distance calculations. While the number was negligible compared to the dataset size, refining or debugging this process could ensure that no valid records are excluded.  
3. **Smoke Estimate Calculation**  
   - **Simplistic Formula:** The smoke estimate was calculated using a simplified formula based on general assumptions about fire size, duration, and proximity. While the formula showed decent correlation with AQI, it does not consider factors like wind patterns, fire shape, or atmospheric dispersion. A more sophisticated model, possibly involving domain experts, could improve accuracy.  
4. **County-to-City Generalization**  
   - The mortality and hospitalization data was only available at the county level (San Joaquin County), whereas the analysis and forecasting targeted Stockton city. This generalization may introduce inconsistencies or inaccuracies in city-level insights.  
5. **Correlation vs. Causation**  
   - Although we found significant correlations between smoke estimates and health outcomes (e.g., mortality, hospitalization), these relationships do not establish causation. Further statistical or experimental methods are necessary to confirm causal links.
6. **Limited Hospitalization Data**  
   - The CalHHS dataset provided hospitalization data only from 2012 onward. This limited range might reduce the reliability of trends or correlations observed. Expanding the dataset or supplementing with historical records could strengthen the analysis.  
7. **COVID-19 Confounding Effect**  
   - The spike in deaths and hospitalizations during 2020 due to COVID-19 may confound the analysis of wildfire smoke impacts. Isolating this effect or excluding 2020 from some analyses was necessary but may affect trend continuity.  
8. **Modeling Limitations**  
   - **Forecasting Smoke Estimates:** The predictive models rely on assumptions from part 1 of the project. Errors or biases in those smoke estimate forecasts can propagate to the predictions of health impacts.  
   - **Asthma Model:** While asthma was strongly correlated with smoke estimates, the forecast assumes past trends will hold, which may not account for changes in healthcare access, wildfire prevention, or community preparedness.  
9. **Data Aggregation Bias**  
   - Aggregating data by year may obscure short-term trends or outliers, such as days with extreme smoke levels or seasonal variations in respiratory diseases. Granular analysis, such as monthly or seasonal data, could provide additional insights.  
10. **Data Source Reliability**  
    - The health and AQI data come from reputable sources (CDC, CalHHS, EPA), but any inaccuracies or biases in their collection and reporting processes will impact the analysis.  

### Conclusion  

This analysis aimed to uncover the health impacts of wildfire smoke exposure in Stockton, California, by integrating and analyzing diverse datasets, including wildfire smoke estimates, air quality metrics, mortality records, and hospitalization data. Key findings indicate significant correlations between smoke estimates and respiratory health outcomes, particularly deaths related to asthma. Using predictive models, we forecasted future respiratory mortality trends based on projected smoke estimates, providing a glimpse into the potential long-term effects of wildfire smoke exposure. These insights serve as a foundation for addressing public health challenges, emphasizing the need for proactive measures to mitigate smoke-related health risks in Stockton's population.  

While the findings shed light on the link between wildfire smoke and health outcomes, they also highlight the complexity of establishing causal relationships and the need for deeper investigation. Future research could focus on expanding datasets, employing advanced models for smoke dispersion and health forecasting, and collaborating with domain experts to refine methodologies. By building on this study, public health officials and policymakers can develop targeted interventions to reduce the adverse health effects of wildfire smoke and improve community resilience.