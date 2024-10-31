# WildLand Fire Analysis for Stockton, CA
#### DATA-512 | Homework 4 | Common Analysis

## Background
More and more frequently summers in the western US have been characterized by wildfires with smoke billowing across multiple western states. There are many proposed causes for this: climate change, US Forestry policy, growing awareness, just to name a few. Regardless of the cause, the impact of wildland fires is widespread as wildfire smoke reduces the air quality of many cities. There is a growing body of work pointing to the negative impacts of smoke on health, tourism, property, and other aspects of society.

## Project Goal

This project aims to investigate the effects of wildfires on Stockton, CA. We will specifically analyze different features of wildfires like total acreas burnt, disatnce from city, etc. We combine this information with Air Quality Index (AQI) estimates and analyse the impact over the last 60 years. Additionally, we seek to enhance reproducibility and develop professional skills necessary for conducting data-driven analyses, as part of the Autumn 2024 DATA 512 course at the University of Washington.

## License

### Code for Data Aquisition
Snippets of the code used in this project were taken from examples was developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.1 - August 16, 2024

A copy of the examples referred is also available in this repository under the folder named `reference_code`.

### Source Data

##### WildLand Fire Data

The wildfire data utilized in this project was sourced from the [Combined wildland fire datasets for the United States and certain territories, 1800s-Present (combined wildland fire polygons](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81). This dataset includes a "combined" dataset that is free of duplicates, containing information of different types of wildland fires. For our analysis, we use the JSON file named `USGS_Wildland_Fire_Combined_Dataset.json`. This file is not committed to the repository since its size exceeds the allowed limits of git.

##### Air Quality Index Data

The air quality index data used in this project was retrieved from the US Environmental Protection Agency (EPA) Air Quality Service (AQS) API. The [documentation](https://aqs.epa.gov/aqsweb/documents/data_api.html) outlines the available call parameters and includes sample requests.

For fetching the AQI data from monitoring stations near Stockton,CA, we utilised an API by passing the Federal Information Processing Series (FIPS) codes for the target city, county, and state, which we obtained [here](https://www.census.gov/library/reference/code-lists/ansi.html).

## Documentation

#### API

*  US Environmental Protection Agency (EPA) Air Quality Service (AQS) [API](https://aqs.epa.gov/aqsweb/documents/data_api.html): These APIs are used to fetch details about air pollutants, monitoring stations, and the AQI readings for each station.

#### Packages

* [geojson](https://pypi.org/project/geojson/), [pyproj](https://pyproj4.github.io/pyproj/stable/): These packages are used to process and handle geographical data
* [requests](https://requests.readthedocs.io/en/latest/api/#): This package is used to make requests to APIs
* [pandas](https://pandas.pydata.org/docs/reference/index.html), [polars](https://docs.pola.rs/api/python/stable/reference/index.html): These packages are used for loading data from files, processing different columns and writing data into a file
* [sklearn](https://scikit-learn.org/stable/api/index.html), [statsmodels](https://www.statsmodels.org/stable/api.html), [scipy](https://docs.scipy.org/doc/scipy/reference/): These packages are used in building a prediction model for smoke estimates.

## How to Run

The complete project code is organized into the following 5 Jupyter notebooks:

* [1_data_aquisition_wildfire.ipynb](1_data_aquisition_wildfire.ipynb): The goal of this notebook is to process the raw information on wildland fires and calculate the shortest distance of each wildland fire from Stockton, CA
* [2_data_aquisition_aqi.ipynb](2_data_aquisition_aqi.ipynb) : The goal of this notebook is to fetch AQI information from monitoring stations near Stockton,CA. 
* [3_data_cleaning.ipynb](3_data_cleaning.ipynb): The goal of this notebook is to aggregate the highly granual AQI data from multiple monitoring stations to an yearly AQI estimate at Stockton, CA
* [4_smoke_estimates.ipynb](4_smoke_estimates.ipynb): The goal of this notebook is to calculate smoke estimates by using the wildland fire information like total acres burnt, distance from Stockton, CA, etc 
* [5_data_visualization.ipynb](5_data_visualization.ipynb): The goal of this project is to create plots for an easy understanding of the trends in data.

Steps to run:

1. Download the combined dataset named `USGS_Wildland_Fire_Combined_Dataset.json` from [Combined wildland fire datasets for the United States and certain territories, 1800s-Present (combined wildland fire polygons](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) and place it in `./resources` folder. The [1_data_aquisition_wildfire.ipynb](1_data_aquisition_wildfire.ipynb) reads the json from this location and processes the data.
2. We need an API key to call the EPA AQS API. The code to raise a sign up request for API key is present in [2_data_aquisition_aqi.ipynb](2_data_aquisition_aqi.ipynb), with the invocation commented out. Generate an API key for yourself and update the code to use that API key. Note that this AQI data aquisition can take time, and it is suggested to use processed file present in the repository
3. Run the notebooks in the above sequence. All the required dependencies are installed in the inital cell of the notebook. If you come across a missing module error while executing the notebook, please run `pip install <module_name>`

## Generated Files

Each notebook generates some intermediary files that are used by the later notebooks. Below is a list of the generated intermediary files:

1. [wildfires_with_distances.csv](generated_files/intermediate/wildfires_with_distances.csv): Contains a list of wildland fires, with an added column representing the shortest distance of the wildfire from Stockton, CA. This is created by [1_data_aquisition_wildfire.ipynb](1_data_aquisition_wildfire.ipynb)
2. [gaseous_AQI_1964-2024.csv](generated_files/intermediate/gaseous_AQI_1964-2024.csv): Contains the response from EPA AQI API call for fetching daily summaries of gaseous pollutants. This is created by [2_data_aquisition_aqi.ipynb](2_data_aquisition_aqi.ipynb)
3. [particulate_AQI_1964-2024.csv](generated_files/intermediate/particulate_AQI_1964-2024.csv): Contains the response from EPA AQI API call for fetching daily summaries of particulate pollutants. This is created by [3_data_cleaning.ipynb](3_data_cleaning.ipynb)
4. [yearly_weighted_aqi_1964-2024.csv](generated_files/intermediate/yearly_weighted_aqi_1964-2024.csv): Contains the yearly AQI estimates at Stockton, CA that was calculated by aggregating daily summaries received from multiple monitoring stations. This is created by [4_smoke_estimates.ipynb](4_smoke_estimates.ipynb)
5. [smoke_estimates_1964-2024.csv](generated_files/intermediate/smoke_estimates_1964-2024.csv): Contains the yearly smoke estimates at Stockton, CA that were calculated using the wildland fire information like area burnt, distance from city, etc.

## Generated Plots

The notebook [5_data_visualization.ipynb](5_data_visualization.ipynb) generates 3 plots and saves them in the `generated_plots` folder.

Description of the generated visualisations are written in [Part1_Reflection.pdf](Part1_Reflection.pdf)

## Considerations and Limitations

1. Missing data
    - While fetching the AQI data, we noticed that the particulate data was missing for a few years, but gaseous data was present. This might skew our AQI estimates that were calculated by aggregating the AQI values per pollutant.
    - In the daily summary fetched from AQI API, we notice that about 20% of the records had missing AQI. We proceeded with the analysis by dropping these values, but we might want to look at options to infer the AQI values by some other means
    - Though we wanted to analyse wildfire data till 2024, I could only find wildland fires till 2020. We might want to look for other sources to get information about wildfires for these years
2. Error handling
    - The shortest distance calculation failed for a few wildfires. Since the number was very small in comparision to the actual dataset, these wildfires were dropped. We might want to look at ways in which the calculation can be performed for these wildland fires.
3. Smoke estimate calculation
    - The formula used for calculating smoke estimates was created based on generic understanding of what might cause smoke. There are a lot of other nuanced factors that can be considered like wind direction, shape of the wildland fire, duration of the fire, etc. I decided to go ahead with my estimate since I saw a decent correlation between smoke estimate and AQI. That being said, someone with more domain knowledge might be able to come up with better formula for smoke estimate.