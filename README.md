# Exploring Airbnb Market Trends
![]()![image](https://github.com/LeoDSaint/Exploring-Airbnb-Market-Trends/assets/134188212/1045a992-06e2-4d0c-a3a8-1b2a18a575ec)

## Project Description

New York City has a variety of Airbnb listings to meet the high demand for temporary lodging for travelers, with several different price levels, room types, and locations. As a consultant working for a real estate start-up, I have collected Airbnb listing data from various sources to investigate the short-term rental market in New York. I'll analyze this data to provide insights on private rooms to the real estate company.

**_Disclaimer_**: _The dataset used in this project is obtained from (DataCamp.com) to demonstrate the capabilities of Python. You can access the dataset [here](https://app.datacamp.com/learn/projects/exploring-airbnb-market-trends/guided/Python)._

## About Dataset
There are three files in the data folder: **_airbnb_price.csv_**, **_airbnb_room_type.xlsx_**, **_airbnb_last_review.tsv_**.

### data/airbnb_price.csv 
This is a CSV file containing data on Airbnb listing prices and locations.

1. listing_id: unique identifier of listing
2. price: nightly listing price in USD
3. nbhood_full: name of borough and neighborhood where listing is located

### data/airbnb_room_type.xlsx 

This is an Excel file containing data on Airbnb listing descriptions and room types.

1. listing_id: unique identifier of listing
2. description: listing description
3. room_type: Airbnb has three types of rooms: shared rooms, private rooms, and entire homes/apartments

### data/airbnb_last_review.tsv 

This is a TSV file containing data on Airbnb host names and review dates.

1. listing_id: unique identifier of listing
2. host_name: name of listing host
3. last_review: date when the listing was last reviewed

## Problem Statemnt
1. What are the dates of the earliest and most recent reviews? Store these values as two separate variables with your preferred names.
2. How many of the listings are private rooms? Save this into any variable.
3. What is the average listing price? Round to the nearest two decimal places and save into a variable.
4. Combine the new variables into one DataFrame called review_dates with four columns in the following order: first_reviewed, last_reviewed, nb_private_rooms, and avg_price. The DataFrame should only contain one row of values.

## Skills Demonstrated
1. Importing and cleaning data
2. Data manipulation
3. Report insights to a real estate start-up!



## Data Cleaning

```python
# Import necessary packages
import pandas as pd
import numpy as np
import re
# importing data/airbnb_price.csv
airbnb_price= pd.read_csv(r'C:/Users/USER\Downloads/airbnb_price.csv')
```
#### airbnb price dataframe head
![](datainspection2.png)

After inspecting the airbnb_price table and seeing that it consists of 3 columns:
1. listing_id 
2. price
3. nbhood_full
We need to make sure that the columns  are in the correct format and cleaned
### Checking for NaN and Duplicated Values
!()[Checking_airbnb_price_for_missing_and_duplicated_values.png]
###
```python
# Convert 'listing_id' column to text
df1['listing_id'] = df1['listing_id'].astype(str)
#splitting the nbhooh_full column into borough and neighborhood 

# Define a regex pattern to capture borough and neighborhood
regex_pattern = r'(?P<Borough>\w+),\s(?P<Neighborhood>.*)'
# Extract borough and neighborhood into new columns using regex
df_extracted = df1['nbhood_full'].str.extract(regex_pattern)

# Assign the extracted columns back to the oriinal DataFrame
df1[['borough', 'nbhood']] = df_extracted

#Remove Trailing Whitespaces
df1['nbhood'] = df1['nbhood'].str.strip()
df1['borough'] = df1['borough'].str.strip()
df1['nbhood_full'] = df1['nbhood_full'].str.strip()

# Remove non-numeric characters from the "price" column
df1['price'] = df1['price'].apply(lambda x: re.sub(r'\D', '', x))

# Convert 'price' column to integer (assuming 'price' values are whole numbers)
df1['price'] = df1['price'].astype(float)
# Display the updated data types
print(airbnb_price.head())

```
![](cleaned_airbnb_dataframe)


Now moving on to the airbnb room type

#### airbnb room_type dataframe head
![](airbnb_room_type_inspection)
After inspecting the airbnb_room_type table and seeing that it consists of 3 columns:
1. listing_id
2. description
3. room_type
We need to make sure that the columns  are in the correct format and cleaned

### Checking for NaN and Duplicated Values

![Checking_airbnb__room_type_for_missing_and_duplicated_values.png](Checking_airbnb__room_type_for_missing_and_duplicated_values.png)

Upon inspection, it was observed that the **description** column contains **10** missing values. To gain further insights into the nature of this missingness, a closer examination was conducted.

![missingness](missingness.png)

It was found that the missing values in the **description** column are exclusively associated with listings categorized as "Private Room" in the **room_type** column. This suggests that certain clients opt not to provide additional information when renting out private rooms, possibly indicating a preference for privacy and discretion in such arrangements.

so now we deal with the missing values and convert the listing_id column to text 
```python
# Replace missing values in the "description" column with "not specified"
airbnb_room_type['description'].fillna("not specified", inplace=True)
# Convert the listing_id to text
airbnb_room_type['listing_id'] = airbnb_room_type['listing_id'].astype(str)

```
### inspecting the room_type column 
```python
print(airbnb_room_type['room_type'].unique())
```
### converting it to categorical column and capitlzing everything to ensure consistency 
*It seems like there are inconsistencies in the values of the "room_type" column, with variations in capitalization. To address this, I Captilized everything to ensure consistency.*
```python
# Inspecting the room_type column
print(airbnb_room_type['room_type'].unique())

# Convert the room_type  to categorical
airbnb_room_type['room_type']= airbnb_room_type['room_type'].astype('category')

# Capitalize values in the 'room_type' column 
airbnb_room_type['room_type'] = airbnb_room_type['room_type'].str.capitalize()

# Print the unique values in the 'room_type' column again
print(airbnb_room_type['room_type'].unique())
#Displaying clean airbnb_room_type dataframe
print(airbnb_room_type.head())
```
![](cleaned)
