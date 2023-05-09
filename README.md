# Drought_Impact
Drought Impact Prediction - Pixel-wise approach \
Selene Ledain

## Repository Structure

The following is a description of the structure of this template repository.

```buildoutcfg
    .
    ├── README.md                           > This file contains a description of what is the repository used for and how to use it.
    ├── Data Cleaning and Processing
    |   ├── Cleaning and Processing.ipynb   > Renaming, structring folders, etc... of data downloaded from PAIRS
    |   ├── Dataset debug.ipynb             > Debugging custom PyTorch dataset creator
    ├── Data Downloading                    > Scripts for downloading (PAIRS) data and automating download
    |   ├── swiss_dem                       > Download, process and upload swisstopo DEM to PAIRS
    ├── Data Exploration Notebooks          > Initial exploration and testing with PAIRS
    ├── Data Uploading                      > Upload external data to PAIRS
    ├── EDA                                 > Exploratory data analaysis notebooks
    ├── Feature Engineering                 > Generate topographic features from the DEM
    ├── Impact Prediction                   > Contains dataset creation, model architecture, model training and testing
    |   ├── cleaning                        > Cloud removal scripts for Sentinel-2 data
    |   ├── LSTM                            > LSTM models
    |   ├── RF                              > RF models
    |   ├── data_generation_scripts         > create datasets using sampler and dataset class
    |   ├── utils                           > config files, util functions
    ├── NDVI_analysis                       > NDVI and drought signal analysis
```


## 1. Data downloading
- Move scripts to main (root) folder 
- Need PAIRS key
- For the swisstopo DEM, downloading is done by using the URLs
- Data will be downloaded to folders seperated by data source (ERA5, Sentinel2...)

## 2. Data uploading
- Need to create a dataset in PAIRS and have editing rights
- Use scripts in `Data uploading` and in `Data downloading > swiss_dem`

Query examples: https://pairs.res.ibm.com/apidoc/#/datalayers/post_v2_datasets__dataset_id__datalayers
Uploading tutorial: https://ibm.box.com/s/14c07fdeuedex6uzoyii6ve997xas5m5


## 3. Cleaning and Processing
- Rename files and remove useless files downloaded from PAIRS

## 4. Feature engineering
- Call `create_dem_feat.py` in terminal
- Created features will be added to downloaded files 

## 5. Dataset creation
The "nofilter" data was used for the final models, meaning that the raw data is collected and cloud removal is done next.
- Use .pj scripts in `Impact prediction > data_generation_scripts`
- Ensure that corresponding config files (stored in utils) has correct information (move the config file to same directory as .pj file)
- Once data is created, get training set stats (max and min of each feature) by using the `get_stats` scripts (both .pj in utils and .py in Impact Prediction)
- Get indexes (date and coordinates) of created samples using the `get_called_samples` scripts  (both .pj in utils and .py in Impact Prediction)

## 6. Cloud removal
The last version is the `cleaning > correct_ndvi_filter.py` script. It filters data using the cloud probability and blue bands from Sentinel-2, then performs a linear interpolation on remaining data.

The NDVI signal was analysed, and notebooks are in `NDVI_analysis`.

## 7. Model training
LSTM and RF models.
- "simple" refers to models using only 6 selected features
- "all" means all features were used
- "cp" means all raw data was used and evaluation was only done on cloud free data

For baseline models (`baseline_*.py`), the MSE and R2 is calculated on different datasets.

## 8. Inference and feature analysis
Predictions can be analysed with `preds.ipynb`.\
Model can be tested with `Impact prediction > LSTM > test_model.py`
