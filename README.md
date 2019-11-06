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
