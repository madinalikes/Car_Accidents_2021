# TeamProject
Jack Miller, Madina Zhaksylyk, Juan Marin, Glen Dagger

<br>

# Project Description

This project analyzes car accident data from the United States in 2020 and 2021 in an attempt to identify possible relationships with various factors. The data used was primarily contained within a csv file found on Kaggle called ["US Accidents (2016-2021)"](https://www.kaggle.com/datasets/sobhanmoosavi/us-accidents), as well as through API calls to the [Census API](https://www.census.gov/data/developers/data-sets.html). 

The most recent census data available was from 2020, so all questions requiring census information (population, income, and age) were explored using the 2020 car accident data.

Due to missing information in the dataset that was discovered later (when observing unrealistically low accident counts during the summer months, with a notable lack of any accident data for July 4th in particular), questions concerning weather and time were explored using the more complete 2021 car accident data.

In order to clean and organize the data, the original dataset (consisting of data from over 2.8 million accidents between 2016 and 2021) was read into the jupyter notebook using Pandas. We then each filtered the data by isolating the rows from the particular year of interest and removing irrelevant columns. All census information was retrieved using Census.gov API calls. The dataframe was grouped by "County" and "State" to get a count of the number of accidents within each county. To merge county data from the census with the filtered data from the original csv file, we had to use a dictionary of all U.S. state names to convert the names to abbreviations. All weather data used was included in the original dataset. 

To visualize the data, we used the following libraries:
- [Matplotlib](https://matplotlib.org/stable/index.html)
- [Jupyter Gmaps](https://jupyter-gmaps.readthedocs.io/en/latest/)
- [Plotly Express](https://plotly.com/python/plotly-express/)


The following are our areas of focus and questions to guide our research:

1. [Median Income by County (2020)](#median-income-by-county)
    - Does a county's median income correlate with the number of car accidents?
2. [Median Age by County (2020)](#median-age-by-county)
    - Does the median age of a county correlate with the number of car accidents?
3. [Time of Year (2021)](#time-of-dayyear)
    - Is the time of year related to the amount of car accidents in the United States?
    - Do certain holidays have higher amounts of car accidents?
4. [Weather (2021)](#weather)
    - What weather categories have the highest amount of car accidents?
    - Do more car accidents happen at night or during the day?

<br>

---
<br>

## Median Income by County
> *Does a county's median income correlate with the number of car accidents?*

The number of accidents per 1000 people was calculated by dividing the accident count of each county (dataframe grouped by "County") by its population (from Census API) and multiplying that value by 1000. Once that was done, we were able to create a heat map that shows areas with a higher number of accidents per capita by county. 

<br>
![image](https://user-images.githubusercontent.com/111404552/204955955-ad2ac625-3091-4a9d-a96c-b6305633711b.png)


Interestingly, the regions that showed the highest intensity were southern/central Minnesota (triangle from Twin Cities down to Rochester and west almost to Sioux Falls), central/eastern California (in the vicinity of Sacramento), northern Oregon (east of Portland), and to a lesser extent, in Virginia (greater Richmond area).

There does not appear to be a relationship between these areas as they each vary significantly in region and population, though they are all in the general vicinity of cities with similar metro population sizes  (metro populations of between 1 and 4 million ). 

<br>

In order to get a more accurate understanding of the dataset in terms of accidents in each county per capita, we used the Plotly Express library to create a chloropleth map to display the United States map with county boundaries shown. Each county was then coded on a gradient scale according to the number of accidents per 1000 people. In order to do this, we needed to find a GeoJson file with precise county boundary information. This map ultimately showed similar patterns as the heat map, as far as identifying areas in the United States with higher per capita accident rates.

It also showed, however, that there are a significant number of counties that either had no accidents all year or were not represented within the data source (particularly in the middle of the country and most of Texas/Louisiana). We do not have a sufficient explanation for these gaps in data. This suggests that our dataset has limitations and we need to be cautious in generalizing our observations and conclusions.


![image](https://user-images.githubusercontent.com/111404552/204956163-4afa0aa4-bd7f-4258-b67a-e8f283aa7fb8.png)


<br>

In order to answer the research question, we created a scatter plot of car accidents per 1000 people vs. the county median household income in 2020. We calculated the linear regression and r value, and found no correlation between the median household income of a county and its accidents per capita (r value of 0.04). This shows no evidence of a relationship between the amount of wealth in a county and its relative frequency of car accidents.

![image](https://user-images.githubusercontent.com/111404552/204956202-ba18cbc5-aff0-482a-86c8-814a86bea1f8.png)

___
## Median Age by County
> *Does the median age of a county correlate with the number of car accidents?*

To see if age had any impact, the median age per county was used as multiplier to the number of accidents per 1000 people in each county. A heat map was created to see how it visually changed compared to the heat map the shows the number of accidents per 1000 people. The idea behind using median age as a multiplier was that if the median age was would exaggerate higher aged counties, potentially changing the visualization. The results smoothed out the heatmap showing only counties in Oregon and California reached the higher ends of the range for this data, with the majority of counties falling into lower percentiles.

![image](https://user-images.githubusercontent.com/111404552/204956221-662f28ef-563a-4cbf-97a1-7fe51ec0f1ea.png)

For further context on the multiplier, a bar graph was developed to look at the number of counties in each age bracket. Approximately 80% of the counties fell into age brackets between 35 - 50, which included 3 of the 7 total age brackets.

![image](https://user-images.githubusercontent.com/111404552/204956249-ba024524-cc17-4f7e-ac1c-0d98395df280.png)
___
## Time of Day/Year

> *Is the time of year related to the amount of car accidents in the United States?*

We started with the Kaggle car accident data from 2016-2021. Originally, we tried using 2020 to match the same year that was used for the income and age questions. But after further investigation, we noticed that the 2020 data had some limitations in the amount of car accidents for certain months, specifically the fourth of July had no car accidents. Thus, we chose 2021 to answer this specific question. 

In order to get the dataset ready for the analysis we added a months column that would take into account the month of the accident. To create this column, only the two numbers that represented the month  from the “start time” column were taken out in order to create this new column. Afterwards, I was able to create a line graph that represented the trend of car accidents throughout the year according to months. 


![image](https://user-images.githubusercontent.com/111404552/204956286-237fcbd8-64c4-44c8-8a78-b4c546c439d8.png)

<br>

The graph showed that the spring time months see the least amount of accidents and then they steadily keep rising since May and peak during the winter months. The peak in winter months could maybe be explained by the winter weather conditions that make driving harder for people. This made us think of the next question, if certain holidays have more car accidents than others.

<br>

> *Do certain holidays have higher amounts of car accidents?*

In order to answer this question we had to create buckets for each holiday. Any accident that occurred on 12/24 was Christmas, any accident that occurred on 10/31 was halloween, and etc. So after those buckets were created, we created a new dataframe that only had car accidents by holidays and that was then graphs into a bar graph in order to accurately compare the results between the 6 holidays. 

![image](https://user-images.githubusercontent.com/111404552/204956329-db4c8cdf-6887-45db-9215-681b6f1d4674.png)

This graph was able to show us that Christmas had the most amount of car accidents compared to all the other holidays. It doubled the amount of car accidents for all the other holidays. Therefore, US citizens should be most careful when driving during winter months and especially during Christmas. 
___
## Weather
> *What weather categories have the highest amount of car accidents?*

In order to answer this question we used car accident data from Kaggle for the years 2016-2021. We narrowed the data down to the year 2021 to examine this question more closely. First to analize weather categories that have more car accidents we cleaned our data, grouping the weather into 7 categories: fair, cloudy, rainy, foggy, snowy, windy and other. From the bar chart that we created we can tell that most car accidents happen in fair weather conditions. This is likely due to the fact that most days in the year have fair weather conditions. However, there are bad weather conditions that cause car accidents. The leading bad weather condition that causes car accidents is cloudy with a steep drop off to rainy, then snowy, then windy and lastly others with few car accidents. This indicates that avoiding travel when the weather is cloudy, when possible, is the best way to avoid a car accident when the weather conditions are not fair.

![image](https://user-images.githubusercontent.com/111404552/204956352-636584e4-befd-4319-9d90-edcac6e04d9a.png)

Further, we analyzed the relationship between car accidents and temperature(F). From the bar chart provided below, we can tell that most car accidents occur between 60F to 90F. There are more accidents at temperatures 30-60F and fewer at other temperatures.

![image](https://user-images.githubusercontent.com/111404552/204956381-ff869f4b-9541-40d6-946f-53c72973b821.png)

<br>

> *Do more car accidents happen at night or during the day?*

To analyze this question we used our data on car accidents in 2021 and visualized it in a pie chart. From the graph shown below, we can tell that most car accidents happen in day time 65.8%. The other 34.2% of car accidents happen at night. This might be due to there being more cars on the road during the daytime than at night. Most people are driving to work during the daytime as well and may feel more rushed than people who drive at night.


![image](https://user-images.githubusercontent.com/111404552/204956396-eb94efe4-3f7d-4e31-9a51-3d76f21fbeb7.png)
