# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 1: Exploring climate data of Singapore

### Overview

As this is our first EDA in the course, we will attempt as far as possible to demonstrate the following skills:
- Problem Statement
- Data Cleaning
- Exploratory Data Analysis/ Visualizations
- Conclusion


For our first project,we're going to analyze climate trends in Singapore. Climate refers to the general weather conditions prevailing over an area for a long period. There are several aspect to studying climate that includes rainfall, temperature, relative humidity, wet build temperature, sun shine duration etc.


### Problem Statement

Due to rising concerns about the impact of weather during the monsoon season in South East Asia, the Singapore Police Force believe more budget and man power should be allocated during the rainy season to proactively deal with potentially hazardous situations. Our goal is to explore the data and as far as possible, provide helpful insights on the Road Traffic Accident rates and their relation to seasonal changes. 

The following are some **key** questions we aim to answer:
> * Audience: Singapore Police Force (SPF)
> * What data will be used?: Monthly rainfall volume, Monthly rainfall frequency, Monthly Road Traffic Accidents, Monthly Mean amount of sunshine 
> * What value will this bring to your audience? Make decision on if SPF should proactively assign more manpower/invest in specialised equipment.

---

### Datasets


We will be using the two base datasets provided in:

* [`rainfall-monthly-number-of-rain-days.csv`](./data/rainfall-monthly-number-of-rain-days.csv): Monthly number of rain days from 1982 to 2022. A day is considered to have “rained” if the total rainfall for that day is 0.2mm or more.
* [`rainfall-monthly-total.csv`](./data/rainfall-monthly-total.csv): Monthly total rain recorded in mm(millimeters) from 1982 to 2022

#### Additional Data
As well as incorporating the following:
* [Monthly mean sunshine hours](https://data.gov.sg/dataset/sunshine-duration-monthly-mean-daily-duration)
* ['RTA-monthly.csv'](./data/RTA-monthly.csv)

#### Overview of data
***rainfall-monthly-number-of-rain-days, rainfall-monthly-total and monthly mean sunshine hours***
rainfall-monthly-number-of-rain-days, rainfall-monthly-total and monthly mean sunshine hours are all double column data sets with the month and the aspect being measured such e.g. rainfall-monthly-number-of-rain-days is a year-month column and the number of rainy days in a the corresponding month.
The datasets go as far back as 1981.

***RTA-monthly***
RTA monthly information was much less clean at a shape of 30 rows x 167 columns. Columns were the Corresponding year-months and rows were various data points on the various different groups involved in Road Traffic Accidents such as Buses, Personal Mobility Devices, Vans etc.


### Data Dictionary
|Feature|Type|Dataset|Description|
|---|---|---|---|
|total_rainfall|float|rainfall-monthly-total|Total rainfall in mm| 
|no_of_rainy_days|float|rainfall-monthly-number-of-rain-days|Number of days it rained in a given month| 
|mean_sunshine_hrs|float|sunshine-duration-monthly-mean-daily-duration| Average number of sunshine hours for each day of a given month| 
|Total_Fatalities|float|RTA-monthly|Total number of fatalities per month that occur including all vehicles (such as buses,motorcycles and personal mobility devices), passengers and pedastrians| 
|Total_Injuries|float|RTA-monthly|Total number of Injuries per month that occur including all vehicles (such as buses,motorcycles and personal mobility devices), passengers and pedastrians| 
|year_month|int|All csvs|Year and month of recorded data|
|year|int|All csvs|Year of recorded data|
|month|int|All csvs|Month of recorded data|


---

### Data cleaning

* ***RTA Monthly***
RTA Monthly is a table with total injuries and fatalities as well as the vehicles/groups involved from 2009 to 2022 on a monthly basis. It was last updated

* ***Date standardisation**
RTA Monthly required a remapping of "Year month" _(2019 Jan)_ to its numerical form _(2019-11)_.

The dataset was then subsequently transposed to have the dates be the rows instead of the various groups as well as injuries/fatalities and joining.

* ***Joining***

By using the .join function, that joins datasets on the index, all the datasets were combined.
Columns such as "Total Casualties Fatalities" and "Total Casualties Injured" were renamed to "Total_Fatalities" and "'Total_Injuries" for aiding understanding. They were also combined into "Total_Fatalities_and_Injuries" for potential further analysis.

* ***Leveraging custom functions**

To the best of my ability, whenever code was repeated, there was an attempt to turn them into functions. This was done in a future focused mindset, in which when this project is revisited, increasing efficency.
Such functions are "index_handler", "outliers", "df_explorer" and "month_year_creator".

---

### EDA and visualisations
***inital prompts***


*Dictionary of standard deviations per column*


{'Total_Fatalities': 4.223083748567184,
 'Total_Injuries': 129.2273721472202,
 'Total_Fatalities_and_Injuries': 131.31354570584776,
 'mean_sunshine_hrs': 1.1896689935014488,
 'no_of_rainy_days': 5.327645982909785,
 'total_rainfall': 101.06617796756169}

*Which month have the highest and lowest total rainfall in 2009, 2010, 2015 and 2020? All values are in (mm).*

2009

June 2009 lowest at 21.8
Nov 2009 highest at 281.8

2010

Feb 2010 lowest at 6.3
July 2010 highest at 298.5

2015

Feb 2015 lowest at 18.8
Dec 2015 highest at 302.3

2020

Feb 2020 lowest at 65.0
May 2020 highest at 255.6


*Which year have the highest and lowest total rainfall in the date range of analysis*

Highest:

year: 2021

Rainfall: 2809.6 mm

Lowest:

year: 2015

Rainfall: 1267.1 mm


*Which year have the highest and lowest number of rainy days in the date range of analysis?*

Highest
2013 206 days

Lowest
2019 120 days

*Are there any outliers months in the dataset?*

Yes, refer to code notebook for details but outliers have been identified and printed as dataframes.

When taking a look at the dataset, we first did a correlation graphs, to guide our attempt to deep dive to specific interesting sections.

***EDA & Visualisations***

***Statistics observations***

Most of the data seems to be normally distributed, with few outliers. A loop and function to identify outlier information in the dataframes was written, however as the down stream proccessing had limited data, it was decided to leave this information in so as to not effect key takeaways.

***Correlations observations***
From the inital heatmap of **All** factors, we can see the following:
* ***Year negative correlation to Fatalities and injuries.***
it seemd there was a negative correlation between years and the number of injuries/fatalities, meaning as years went on, the total number of injuries/fatalities from Road Traffic Accidents decreased.

* ***Rainfall volume/frequency negative correlation to sunshine hours.***
this result is unsurprising as usually during rainfall, generally speaking, sunshine would be more sparse.

* ***Rainfall volume/frequency negative correlation to sunshine hours.***
this result is unsurprising as usually during rainfall, generally speaking, sunshine would be more sparse.

* ***Month strongly correlated with total rainfall and weakly correlated with rainy days.***
At 0.68 correlation coefficient, total rainfall is strongly correlated with the month, meaning the later in the year it is, the higher the rainfall volume. Interestingly, the number of rainy days althought slightly correlated is not nearly as strongly affected at 0.43.  

### Investigation: Years and Months observations from bar and scatterplots.
***Year and month observations***
The next steps for this investigation were to plot the Year and Road Traffic values to get a better visual on the meaning behind the strong correlation coefficent.
To add robustness into the investigation of our dataset, various bar charts of the frequency of rainfall, total volume of rainfall and number of road traffic accidents were plotted grouped by both years and months individually.
We used scatterplots to visualise stronger correlations, such as the strong negative relationship between Year and Road Traffic Accidents, while also reconfirming weaker relationships, such as month and total rainfall, to ensure that there was no trend that was missed it our observations.

In totality,the two observation that could be made in terms of trends, was
1. an increase in rainfall during the end of year period, mainly during the November and December.
2. mean sunlight hours per month had a strong negative correlation to the volume and number of raindays per month.(We discuss this point, in its own section below)

However, we are unable to observe any trend or relation to an increase in Road Traffic Accidents occuring.

A summarised version of our findings are:
1. There was no clear relation between the number of Road Traffic Accidents, both Fatalities and Injuries and rainfall.
2. There was no clear relation between the number of Road Traffic Accidents, both Fatalities and Injuries and mean sunlight hours per month.
3. On a annual basis, from 2009 to 2022 there is a clear drop in the number of Road Traffic Accidents.
4. There is no clear relation between month and Road Traffic Accidents.

### Further Investigation: Mean sunlight hours impact
***Mean sunlight hours***
One of the speculations when considering impact of weather on Road Traffic Accidents, was the impact on visbility. As there have been studies conducted that prove visibility to be associated with likelihood of crashes.
https://www.sciencedirect.com/science/article/pii/S0386111216300681

The base assumption was that there would be a negative relationship between sunlight and rainfall, as generally when it rains, there is less visible natural sunlight due to blocking by dense clouds and the rainfall itself.(https://education.nationalgeographic.org/resource/cloud)

This assumption was somehwat validated, as both the number of rainy days and the rain volume had a high negative of correlation with the mean sunlight hours of a given month.

We can try and interpret the lack of correlation between mean sunlight hours and the rate of Road Traffic Accidents as Singapore, due to the country having systems inplace to be relatively well lit when natural sunlight is not avaliable. 

***Limitation***
However, as the above is all speculation, perhaps this requires further study, of which our reccomendations will be covered during our reccomendations section.

### Further Investigation: splitting data by Year
***Further investigation: splitting data by Year***
With no clear trend between rainfall frequency/volume and the number of Road Traffic Accidents as well as with mean sunlight hours and the number of Road Traffic Accidents, we attempt as far as possible to look at the data on a more granular level. This was done through grouping the dataset by year and plotting the correlations between each factor as a heatmap. This resulted in strong correlation between rainfall frequency/volume, as well as mean sunlight hours to Road Traffic Accidents, reaching upwards of ranges from 0.6 to 0.79 correlation coefficent. 

***Further Investigation: Limitation***
It is important to acknowledge that although the results look promising, a clear flaw is that the data set only has 164 rows, meaning roughly 8-12 data points per correlation graph. Thus although interesting would require more granular data or perhaps additional factors to improve the accuracy of our correlations.


### Conclusion and Reccomendations
Due to rising concerns about the impact of weather during the monsoon season in South East Asia, the Singapore Police Force believe more budget and man power should be allocated during the rainy season to proactively deal with potentially hazardous situations. Our goal is to explore the data and as far as possible, provide helpful insights on the Road Traffic Accident rates and their relation to seasonal changes. 

***Conclusions***
When observing monthly data from 2009 to 2022, there was no impact of rainfall frequency and volume on the number of road traffic accidents per month. 

When looking at mean sunlight hours, an indirectly caused impact by weather on potential road conditions, there was little to no impact on the rate of road traffic accidents from monthly data from 2009 to 2022.

***Reccomendations***
Although there was no direct relationship that could be shown from the inital study, when splitting the data by years, there was an observed potential for correlation between rainfall frequency/volume as well as mean sunlight hours per month on the number of road traffic accidents. A further study perhaps both incorporating more factors such as traffic volume, road conditions and specific sections of singapore (e.g. perhaps highways might have a higher frequency of road traffic accidents than a commercial business area) and more granular time data such as records of the day or time of Road Traffic Accidents/rainfall might prove promising in the future.

***Limitations***
In our datasets, we worked mainly with monthly data. As the dataset was not as granular as weekly or daily data, we are unable to conclude if slicing by year does holds merit. i.e. are the strong correlations between years and rainfall/sunlight due to a small sample size or are there more factors that we could potentially observe.