# Introduction
The purpose of this lab is to find the 3 zip codes that provide the best investment opportunity for the Syracuse Real Estate Investment Trust. The challenge lies in narrowing down all the zip codes in the US down to some areas of focus by examining median housing value trends.

# Methodology
The time series data set for this lab was given in .csv format, it contains monthly median housing prices from Zillow, for all the zip codes in the US, from January 1996 up to June 2018. Additional data regarding business patterns at the county level was brought in to help narrow down the search.

## The Data:
* Zillow Dataset: The base dataset, it contains a monthly time series that includes:
  * Zipcode
  * City
  * State
  * Metro area
  * County
  * SizeRank
  * Date - This feature came in as multiple features (one per month)
  * MedianHhValue - this was the value of the date features

* County Business Patterns (CBP) is an annual series that provides economic data by industry at the U.S., State, County and Metropolitan Area levels. This data was only available until 2016.
  * EMP - Total Number of Employees
  * PAYANN - Total Annual Payroll
  
## Data exploration and munging
The original dataset was transformed using Pandas’s melt function to put into a tall format. Additional features were created for Year, Month and the additional data from the US Census API.

### The relationship between annual payroll, total employees, and median HH price
There is a small correlation between the variables, especially annual payroll, and the median house price:

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig1.jpg)

This data did come in on an annual basis and by county. At that level, the correlations improve slightly:

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig2.jpg)

These correlations vary a lot by zip code, but annual payroll shows this correlation a lot more than employee count.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig3.jpg)

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig4.jpg)

### Narrowing down the search
In order to down sample the data I wanted to identify the potential of each state, since this is the broadest category in the dataset it would still yield a large number of zip codes. This graph illustrates the idea:

Average median housing value by state for 2000, 2009 and 2017:

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig5.jpg)

Percentage change was used to establish growth so the more expensive states would not dwarf the less expensive ones. For example, from 2009 to 2017, the top 5 growing states by percentage change in yearly average median housing value were:
1.	California		0.525145
2.	North Dakota		0.471845
3.	District of Columbia	0.412503
4.	Colorado		0.369003
5.	Florida			0.361417

In order to avoid the bias from selecting an arbitrary year like 2009, the 2018 average annual median housing price was compared against every year from 2008 and 2017, saving the top 10 states from every comparison. The final list includes 20 states: AZ, WA, MA, CO, UT, OR, HI, FL, CA, DC, TN, ID, GA, ND, RI, NE, SD, MI, TX and NV (a total of 6433 zip codes).

### Modeling
Time series models were built using prophet, a tool released by Facebook that utilizes a Bayesian based curve fitting method to forecast time series data (Hayashi, 2017).
The additive model was used to project all the selected zip codes forward until Dec 2022.

# Results and Findings
In order to select the best investment opportunities for the SREIT, I used a similar method as I did to narrow down the data. The avg. 2018 median HH value of each zip code was compared to its projected value 5 years from now.
The top 100 zip codes according to this method concentrate a lot around Florida and California.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig7.jpg)

The top 5 zip codes per this method are:
1.	Luna Pier MI, 48157
2.	Atlanta GA, 30315
3.	Hastings FL, 32145
4.	Crockett CA, 94525
5.	Sacramento CA, 95815

## Validating the top 3 choices
In order to validate the choices, each of the top 3 zip codes was examined individually to see if there are any concerning trends.

#### 1 - Luna Pier MI, 48157
A relatively new community, it has experienced a lot of growth since 2014. However, it shows indications that it might be plateauing.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig8.jpg)

It seems like the original model interpreted some of the missing data as 0 so the projection was too positive. In reality, eliminating the null values prior to 2014, the forecast becomes much more uncertain and neutral.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig9.jpg)

I will discard Luna Pier from the recommendations.

### 2 - Atlanta GA, 30315
Another newer community but seems to have much steadier growth than Luna Pier.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig10.jpg)

The prophet model shows the median HH value will be positive in the future. RMSE of the predictions when 2017 and 2018 values were held out was 4711.81, the second lowest in the top 10.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig11.jpg)

### 3 - Hastings FL, 32145
A similar pattern to the Atlanta zip code, a newer community with a lot of growth over the past few years

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig12.jpg)

The prophet model for Hastings is also very positive, the uncertainty band continues into the positive and is very narrow until 2020. The RMSE for 2017-2018 values was also relatively low 8357.93 compared to other zip codes in the top 10.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig13.jpg)

### 4 - Crockett CA, 94525
Crockett is also similar to the other 2 recommendations, a newer community with rapid growth over the past few years. Median prices in this zip code don’t vary very much by month since 2017.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig14.jpg)

The prophet model fit for the dataset is not as good as the other two zip codes, but the projections are generally positive, and it’s in California which is one of the most desirable states.

![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig15.jpg)

The forecast for Crockett, however, has much less uncertainty than the next two on the top 10 list:
  * Sacramento ![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig16.jpg)
  * Denver ![Alt Text](https://github.com/pmb06d/forecasting_home_values/blob/master/graphs/Fig17.jpg)

# Discussion and Assignment Questions
* The three recommended zip codes for investment are:
  * Atlanta GA, 30315
  * Hastings FL, 32145
  * Crockett CA, 94525

* According to the prophet models, the median housing value in these zip codes will increase by the largest percentage out of all the zip codes in selected 20 states.

* Down sampling was achieved by looking at the top 10 states in percentage growth from 2008 to 2017 to 2018. 

* As an additional indicator for investment, a relationship was established between total annual payroll and median housing value, although it varies significantly by zip code.
