# Pharmaceutical Drug Spending by Countries
Data Science Institute - Cohort 4 - Team 22 Project

## Members
* Adrienne Lloren ([amlloren](https://github.com/amlloren))
* Anna Karlova ([anna-karl](https://github.com/Anna-Karl))
* Danica Leung ([ldanica](https://github.com/ldanica))
* Ferial Vahmiyan ([ferialvahmiyan](https://github.com/Ferialvahmiyan))
* Nusrat Khan([nusikhan](https://github.com/NusiKhan))

## Business Case

Our team has selected the Pharmaceutical Drug Spending by Countries dataset. Our goal is to identify the Top 10 and Bottom 10 spending countries (ranked by the average of Health Spending per Capita) to gain insights on these two country groupings. To start, we explored the general characteristics of the raw data and came up with a strategy to handle missing values. We implemented an aggregation to the year range 2005 and 2014 to clean the data from missing values and outliers. Next, we explored the dataset using Linear Regression, Clustering, Forecasting and Data Visualization using python. Optionally, we also explored data visualization using Tableau Public. 

![screenshot of Pharmaceutical Drug Spending dashboard](images/pharma_spending_dashboard.png)
Screenshot of Pharmaceutical Drug Spending dashboard ([Tableau Public Link](https://public.tableau.com/app/profile/anna.karlova/viz/Pharmaspendingbycountries1970-2015/Dashboard1))

We sourced our raw dataset by downloading them from the links below:
* https://datahub.io/core/pharmaceutical-drug-spending
* https://github.com/datasets/pharmaceutical-drug-spending

## Project Overview

* [Requirements](#requirements)
* [Exploratory Data Analysis](exploratory-data-analysis)
* [Understanding the Raw Data](#understanding-the-raw-data)
* [Data Cleaning and Missing Values](#data-cleaning-and-missing-values)
* [Identifying Top 10 and Bottom 10 Spenders](#identifying-top-10-and-bottom-10-spenders)
* [Data Analysis on Python](data-analysis-on-python)
    * [Linear Regression](#linear-regression)
    * [Clustering](#clustering)
    * [Forecasting](#forecasting)
* [Data Visualization on Tableau](data-visualization-on-tableau)
    * [Global Health Spend](global-health-spend)
    * [Top 10 Countries Dashboard](top-10-countries-dashboard)
    * [Bottom 10 Countries Dashboard](bottom-10-countries-dashboard)
* [Business Case](business-case)
    * Insights
* [Conclusion](#conclusion)

### Requirements

This project uses the following Python libraries

* NumPy : For fast matrix operations.
* pandas : For analysing and getting insights from datasets.
* matplotlib : For creating graphs and plots.
* seaborn : For enhancing the style of matplotlib plots.
* sklearn : For linear regression analysis.

## Exploratory Data Analysis

INSERT IMAGE OF ANNA'S EDA dynamic dashboard.

**To explore the dynamic features download the 1_EDA.ipynb file in our repo.**

## Understanding the Raw Data

### Schema 

| name | type | description |
|---|---|---|
| LOCATION | string (object) | Country code |
| TIME | number (int64) | Date in the form of %Y |
| PC_HEALTHXP | number (float64) | Percentage of health spending |
| PC_GDP | number (float64) | Percentage of GDP |
| USD_CAP | number (float64) | in USD per capita (using economy-wide PPPs) |
| FLAG_CODES | string (object) | Flag codes |
| TOTAL_SPEND | number (float64) | Total spending in millions |

### Understanding the Features

| column name | feature | description |
|------|-------|-------------|
| PC_HEALTHXP | Percentage of Health Expenditure | This is the percentage of a country's total health expenditure specifically spent on pharmaceuticals, including prescription medicines and over-the-counter products. |
| PC_GDP | Percentage of GDP | This is the percentage of the country's Gross Domestic Product (GDP) that is spent on pharmaceuticals. |
| USD_CAP | Health Expenditure per Capita in USD | This is the average amount of money spent on pharmaceuticals per person, calculated in US dollars. |
| TOTAL_SPEND | Total Spending | This is the total amount of money spent on pharmaceuticals by the country in a given year, typically in millions or billions of dollars. |
 
### Summarizations found in the dataset.

The following table presents key summarizations derived from the Pharmaceutical Drug Spending by Countries dataset. These summarizations provide a foundational understanding of the dataset's scope, including the number of countries, the time span covered, and the completeness of the data.

| question | analysis |
|----------|----------|
| How many countries are in this data set? | There are 36 countries in this data set. |
| How many years are in this data set? | There are 47 years in this data set. |
| What is the year range of this data set? | The data ranges from the years 1970 to 2016. |
| What is the total number of observations in the dataset? | There are 1036 observations in this data set |
| What is the total number of possible observations? | There are 1,692 possible observations |
| How many values are missing? | There are 656 missing values |

**The supporting python code can be viewed in the 2_summarizations.ipynb file in our repo.**

## Data Cleaning and Handling Missing Values

Through preprocessing we are able to identify missing values and outliers. To assist in visualizing the missing values in our dataset we created a heatmap, where blue indicated available data and yellow indicated missing data.

![Heatmap of Missing Data](images/missing_values.png)

### Our Considerations: 

* Impact on Analysis:
    * More years generally provide a better trend analysis but come with the trade-off of more missing data.
    * Fewer years provide more complete data but may not capture long-term trends and changes effectively.

* Number of Countries:
    * Dropping fewer countries is preferable to maintain a larger sample size for more robust comparative analysis.
    * However, if the number of dropped countries does not significantly impact the representation, it may be acceptable to drop them for a more complete dataset.

* Business Case:
    * The focus on this project requires reliable data to identify high and low spenders accurately.
    * Data completeness is critical to avoid biased conclusions and ensure accurate identification of best practices.

Based on this heatmap visualization and our aggregation considerations, our group decided to aggregate the dataset to the year range 2005 to 2014 and drop 4 countries (New Zealand, Russia, Turkey, and United Kingdom). 

| Start Date | End Date | # of Years | # Dropped Countries | List of Dropped Countries |
|:----:|:----:|:---:|:---:|:---|
| 2005 | 2014 | 10 | 4 | New Zealand, Russia, Turkey, United Kingdom |

### Correlation between Features

To explore the relationships between different variables we created a heatmap to visualize the correlation between each feature where a correlation closer to 1.00 means more correlated.

![Heatmap of correlations between features](images/correlation_heatmap.png)

The following table shows the correlations between features.

| feature 1 | feature 2 | correlation |
|:---------:|:---------:|:-----------:|
| USD_CAP | TIME | 0.84 |
| PC_HEALTHXP |  PC_GDP | 0.72 |
| PC_GDP | USD_CAP | 0.67 |
| PC_GDP | TIME | 0.61 |

Since the correlation between the Health Spending per Capita vs Time produced a high correlation compared to the other variables, our team focused our efforts on this correlation when exploring the Top 10 and Bottom 10 health expenditure per country. Additionally, working with the Health Expenditure per Capita standardized our data exploration to already incorporate the other factors such as population.

**The supporting python code can be viewed in the 3_data_cleaning.ipynb file in our repo.**

## Identifying Top 10 and Bottom 10 Spenders
The ranking is based off the average health spend per capita for the year range 2005 to 2014. Due to the time restrainst of this team project, we opted to focus on the Top 10 and Bottom 10 countries and ommitted exploring the countries that fell in the mid-level category. By doing so our analysis is not completely comprehensive but it allowed us to explore the polar ends of the dataset more thoroughly.

### Top 10 Highest Spending Countries (Average Per Capita)

This table ranks the top 10 highest spending countries by their average spending per capita in USD between the 2005 and 2014 year range.

| Rank | Country Code | Country Name     | Avg Spend Per Capita ($ USD) |
|:----:|:------------:|:----------------:|:-----------------------------------:|
| 1    | USA          | United States    | 937.895500                          |
| 2    | CAN          | Canada           | 720.361100                          |
| 3    | CHE          | Switzerland      | 700.745600                          |
| 4    | GRC          | Greece           | 655.150889                          |
| 5    | JPN          | Japan            | 646.764700                          |
| 6    | IRL          | Ireland          | 635.788600                          |
| 7    | DEU          | Germany          | 627.021300                          |
| 8    | FRA          | France           | 617.524100                          |
| 9    | BEL          | Belgium          | 595.256000                          |
| 10   | LUX          | Luxembourg       | 573.441300                          |

![bar chart of the top 10 highest spending countries based on the average spend per capita ](images/top_barchart.png)

### Bottom 10 Lowest Spending Countries (Average Per Capita)

This table ranks the countries by their average spending per capita in USD, starting from the lowest between the 2005 and 2014 year range.

| Rank | Country Code | Country Name     | Avg Spend Per Capita ($ USD) |
|:----:|:------------:|:----------------:|:-----------------------------------:|
| 1    | LVA          | Latvia           | 269.196400                          |
| 2    | EST          | Estonia          | 269.575900                          |
| 3    | ISR          | Israel           | 274.791375                          |
| 4    | MEX          | Mexico           | 277.979500                          |
| 5    | POL          | Poland           | 304.320700                          |
| 6    | DNK          | Denmark          | 324.135200                          |
| 7    | LTU          | Lithuania        | 373.809400                          |
| 8    | CZE          | Czech Republic   | 395.318600                          |
| 9    | NOR          | Norway           | 412.787900                          |
| 10   | NLD          | Netherlands      | 428.909700                          |

![bar chart of the bottom 10 lowest spending countries based on the average spend per capita ](images/bottom_barchart.png)

**The supporting python code can be viewed in the 4_top_bottom.ipynb file in our repo.**

## Data Analysis on Python

### Linear Regression
NOTE: Isreal was dropped because it was missing values for 2014, 2015 and 2016.


**The supporting python code can be viewed in the 5_linear_regression.ipynb file in our repo.**

### Clustering

**The supporting python code can be viewed in the 6_clustering.ipynb file in our repo.**

### Forecasting

* Use Ferial's Dynamic plotly

**The supporting python code can be viewed in the 7_forecasting.ipynb file in our repo.**

## Data Visualization on Tableau (Optional)

* Global Health Spend ([Tableau Public Link](https://public.tableau.com/app/profile/anna.karlova/viz/Pharmaspendingbycountries1970-2015/Dashboard1))
* Top 10 Countries Dashboard ([Tableau Public Link](https://public.tableau.com/app/profile/adrienne.lloren/viz/PharmaceuticalDrugSpendingbyCountries_17333346970600/TOP10))
* Bottom 10 Countries Dashboard ([Tableau Public Link](https://public.tableau.com/app/profile/adrienne.lloren/viz/PharmaceuticalDrugSpendingbyCountries_17333346970600/BOTTOM10))

## Conclusion
* How does our data exploration and analysis support the business case?
* How did we incorporate the skills we learned from this certificate program?
* What we would explore if we had more time?

## Team Videos:
* Adrienne Lloren
* Anna Karlova
* Danica Leung
* Ferial Vahmiyan
* Nusrat Khan

# PART 1:

## Business Case

Our team has selected the Pharmaceutical Drug Spending by Countries dataset and will be investigating "Cost Optimization." By identifying high and low spenders, countries can adopt best practices to optimize their pharmaceutical spending. 

* https://datahub.io/core/pharmaceutical-drug-spending
* https://github.com/datasets/pharmaceutical-drug-spending

## Team Work Agreement

* We all agreed that we will independently work on all parts of the project and then during the 2nd team project week, we will compare, contrast and compile our findings into our Main branch.
* We will meet up on at 1pm during our next few Friday work periods and allocate 30 mins to an hour discussing our team project.
* In the meantime, we will each create our own branches to the dsi_team_22 repo where we will commit and push the research, code and analysis we independently work on.
* The focus for the upcoming weeks will be on data cleaning (How to decide what to do with missing data) as well as categorizing the dataset by the Top 10 and Bottom 10 spenders based on the countries' Average Health Expenditure per Capita. Explore these categories and document your findings and analysis.
* As we progress with the next few modules, we will revisit our tasks and deliverables for our team project.

## Initial Exploratory Data Analysis

To start we explored line plot visualizations of the features in the dataset. Below are the data visualizations produced during this EDA.

![line plot of the percentage of GDP per country over the year range 1970 to 2016 ](images/pc_gdp_v_time.png)
![line plot of the percentage of health expenditure per country over the year range 1970 to 2016 ](images/pc_healthxp_v_time.png)