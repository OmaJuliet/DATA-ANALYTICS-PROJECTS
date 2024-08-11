# UDEMY COURSES ANALYSIS FOR 2023

## PROJECT OVERVIEW
Analysis carried using data source from Kaggle (https://www.kaggle.com/datasets/ankushbisht005/udemy-courses-data-2023) for reference.

## STEP 1: DATA COLLECTION
Data was collected from Kaggle website at (https://www.kaggle.com/datasets/ankushbisht005/udemy-courses-data-2023)


## STEP 2: DATA CLEANING
- Loaded the two datasets ('courses.csv' and 'instructors.csv')
- Dropped some unecessary columns in the two datasets
- Renamed some column names
- Checked for missing values and replaced missing values in columns to 'N/A'


## STEP 3: DATA VALIDATION AND EXPLORATION USING JUPYTER NOTEBOOK
- Checked for how many entries the datasets has
- Validated unique instructors
- Checked for total number of continents and countries
- Checked for top 10 courses by rating
- Checked for top 10 instructors by courses
- Checked for average courses rating
- Checked for courses created by year
- Identified datetime columns with timezone-aware datetimes
- Converted to Timezone-Aware Datetimes to Timezone-Naive Datetimes using the 'dt.tz_localize(None)' method to remove the timezone information in the "courses" dataset. This is because Jupyter Notebook can;t seem to merge datasets with columns containing timezones.
- Merged both datasets on 'instructors_id'
- Saved the cleaned and merged dataframe to an Excel file


## STEP 4: DATA VISUALIZATION USING POWER BI
- Imported cleaned data into Power BI
- New columns to convert `created` and `last_updated` columns back to date format and extract only the years
- 

- Saved report and publish to Power BI Service

![Report Dashboard](global-salary.png) 


## STEP 5: DATA VISUALIZATION USING TABLEAU
- Dataset too large to so I filtered only courses created from year 2019 - 2023 to visualize


## STEP 6: DATA STORYTELLING
At "Data Storytelling.md" file
