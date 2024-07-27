# Project: Analyzing Housing Market Trends

## Step 1: Scraping Data
Tools: Python (BeautifulSoup, Scrapy), Selenium

Task: Scrape housing listings from a real estate website.

1. Identify the website: Choose a real estate website like Zillow, Realtor.com, or Redfin.

2. Set up your environment: Install necessary libraries
`pip install requests beautifulsoup4 scrapy selenium`

3. Write the scraper
```python
import requests
from bs4 import BeautifulSoup
import pandas as pd

# Define the URL
url = 'https://www.realtor.com/realestateandhomes-search/San-Francisco_CA'
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36'
}

# Fetch the page content
response = requests.get(url, headers=headers)
soup = BeautifulSoup(response.content, 'html.parser')

# Extract data
listings = []
for listing in soup.find_all('div', class_='CardContent__StyledCardContent-rui__sc-7ptz1z-0 kDqsxy card-content card-content'):
    price = listing.find('div', class_='Pricestyles__StyledPrice-rui__btk3ge-0 kjbIiZ card-price')
    house_type = listing.find('div', class_='base__StyledType-rui__sc-108xfm0-0 hRTvWe message')
    # details = listing.find('ul', class_='PropertyMetastyles__StyledPropertyMeta-rui__sc-1g5rdjn-0 iQEvdK card-meta')
    bed = listing.find('li', class_='PropertyBedMetastyles__StyledPropertyBedMeta-rui__a4nnof-0 jkAoUn')
    bath = listing.find('li', class_='PropertyBathMetastyles__StyledPropertyBathMeta-rui__sc-67m6bo-0 hGQdFx') 
    squareFeet = listing.find('li', class_='PropertySqftMetastyles__StyledPropertySqftMeta-rui__sc-1gdau7i-0 cYyTDO')
    address = listing.find('div', class_='card-address truncate-line')

    # Check if elements are found and extract text
    price_amount = price.text.strip() if price else 'N/A'
    house_type_text = house_type.text.strip().split()[0] if house_type else 'N/A'
    bed_text = bed.text.strip().split('bed')[0] if bed else 'N/A'
    bath_text = bath.text.strip().split('bath')[0] if bath else 'N/A'
    squareFeet_text = squareFeet.text.strip().split('sqft')[0] if squareFeet else 'N/A'
    address_text = address.text.strip() if address else 'N/A'

    # Append data to listings
    listings.append({
        'Price': price_amount,
        'House type': house_type_text,
        'Number of bedrooms': bed_text,
        'Number of bathrooms': bath_text,
        'Square Feet': squareFeet_text,
        'Property Address': address_text
    })
```

## Step 2: Cleaning Data
Tools: Python (Pandas, NumPy)

Task: Clean the scraped data.

1. Load data into a DataFrame:
```python
# Print the listings to debug
print(listings)

# Create a DataFrame
df = pd.DataFrame(listings)
```

2. Save the DataFrame to Excel:
```python
# Save DataFrame to Excel
df.to_excel('housing_data.xlsx', index=False)
```

3. Clean data: Remove handle missing values, remove currency symbol in price column to perform calculations:
```python
# Clean the 'Price' column to convert the 'Price' column from a string to a numeric format by removing the currency ($) symbol and commas
def clean_price(price):
    if isinstance(price, str):
        price = price.replace('$', '').replace(',', '')
        try:
            return float(price)
        except ValueError:
            return None
    return None

df['Price'] = df['Price'].apply(clean_price)

# Drop rows with None values in the 'Price' column to remove rows where the 'Price' column couldn't be converted to a numeric value.
df = df.dropna(subset=['Price'])
```


## Step 3: Performing Analysis
Tools: Python (Pandas, Scikit-learn)

Task: Perform different types of analysis.

1. Descriptive Analysis:
`df.describe()`

2. Diagnostic Analysis: Identify patterns and anomalies.
```python
# Diagnostic analysis to identify patterns and anomalies.
df.groupby('Property Address')['Price'].mean().plot(kind='bar')
plt.xlabel('Property Address')
plt.ylabel('Mean Price')
plt.title('Mean Price per Property Address')
# plt.xticks(rotation=90)  # Rotate x-axis labels if needed
plt.show()
```

<!-- 3. Predictive Analysis: Predict future prices using machine learning.
```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

X = df[['beds', 'baths', 'sqft']]
y = df['price']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = LinearRegression()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
``` -->

4. Prescriptive Analysis: Recommend investment opportunities.
```python
# Calculate and print quantiles
quantile_25 = df['Price'].quantile(0.25)
print(f"25th percentile value: {quantile_25}")

# Perform prescriptive analysis to recommend investment opportunities.
recommendations = df[df['Price'] < quantile_25]

# Print the filtered recommendations
print(recommendations)
```


## Step 4: Visualizing Data using Python
Tools: Python (Matplotlib, Seaborn), Plotly

Task: Create visualizations to tell a story.

1. Price Distribution:
```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.histplot(df['Price'])
plt.title('Price Distribution')
plt.xlabel('Price')
plt.ylabel('Frequency')
plt.show()
```

2. Geographical Distribution (using Plotly):
```python
import plotly.express as px
import pandas as pd

# Sample data
df = pd.DataFrame({
    'latitude': [37.7749, 37.8049],
    'longitude': [-122.4194, -122.2711],
    'price': [1000000, 800000]
})

# Create a scatter mapbox plot
fig = px.scatter_mapbox(df, lat='latitude', lon='longitude', color='price', size='price',
                        color_continuous_scale=px.colors.cyclical.IceFire, size_max=15, zoom=10)
fig.update_layout(mapbox_style="carto-positron")
# fig.update_layout(mapbox_style="open-street-map")
fig.update_layout(mapbox=dict(center=dict(lat=df['latitude'].mean(), lon=df['longitude'].mean()), zoom=10))
fig.show()
```


## Step 4: Visualizing Data using Power BI
### Data Cleaning and Transformation (if needed)
1. Get Data: Click on the "Home" tab and select "Get Data" -> "Text/CSV" if you have the data in a CSV file, or choose another appropriate option if your data is in a different format.

2. Transform Data: Click on the "Transform Data" button to open the Power Query Editor.

3. Clean Data: Use Power Query to clean and transform your data. This can include:
- Removing unnecessary columns
- Changing data types
- Handling missing values
- Creating calculated columns

4. Apply Changes: Once the data is clean, click "Close & Apply" to load the cleaned data back into Power BI.


### Creating Visualizations
1. Create a New Report: In the main Power BI window, you’ll see a blank canvas where you can start creating your report.

2. Add Visualizations:
- Bar Chart for Price Distribution: showing price distributions using histograms and bar charts.
1. Click on the "Clustered Bar Chart" icon from the Visualizations pane.
2. Drag the "Price" field to the "Values" area and any other relevant field (e.g., "Address") to the "Axis" area.

- Map for Geographical Distribution: showing the geographical distribution of properties.
1. Click on the "Map" icon from the Visualizations pane.
2. Drag the "Latitude" and "Longitude" fields to the appropriate areas.
3. Drag the "Price" field to the "Size" area to adjust bubble size based on price.

- Line Chart for Price Trends Over Time:
1. Click on the "Line Chart" icon.
2. Drag the "Date" field to the "Axis" area and the "Price" field to the "Values" area.

- Table for Detailed Listings:
1. Click on the "Table" icon.
2. Drag relevant fields (e.g., "Address," "Price," "Beds," "Baths," "Sqft") to the "Values" area.


### Enhancing Visualizations
1. Formatting: Click on the visualizations to format them. You can change colors, labels, titles, and other properties in the "Format" pane.

2. Filters: Use the Filters pane to add slicers and filters to your report. This allows users to interact with the data and drill down into specific subsets.

3. Calculated Fields: Create calculated fields or measures using DAX (Data Analysis Expressions) to add more insights. For example:
- Average Price: `AVERAGE('Table'[Price])`
- Total Listings: `COUNTROWS('Table')`


### Sharing Your Report
1. Publish to Power BI Service:
- Click on the "Publish" button in Power BI Desktop.
- Sign in to your Power BI account and select a workspace to publish your report.

2. Create a Dashboard in Power BI Service:
- Go to the Power BI Service (web version).
- Create a new dashboard and pin tiles from your report to the dashboard.

3. Sharing: Share the dashboard with others by granting access or generating a shareable link.



## Step 5: Communicating Insights
Tools: Jupyter Notebook, PowerPoint, Google Slides

Task: Document your findings and create a presentation.

1. Jupyter Notebook: Document each step, include code, visualizations, and explanations.

2. Presentation: Summarize key findings, insights, and recommendations.


### Data Sources
- Web Scraping: Real estate websites like Zillow, Realtor.com, or Redfin.
- Open Data Sets:
- - Kaggle Housing Data
- - UCI Machine Learning Repository
- - Data.gov


### Project Structure
- Introduction: Project objectives and questions to be answered.
- Data Collection: Methods and tools used for scraping data.
- Data Cleaning: Steps taken to clean and preprocess the data.
- Analysis: Different types of analysis performed and their results.
- Visualization: Visualizations created to illustrate findings using Python and Power BI.
- Conclusion: Insights, recommendations, and future work.