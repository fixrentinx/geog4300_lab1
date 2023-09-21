Geog4/6300: Lab 1
================

## Loading data into R, data transformation, and summary statistics

Your name: Isabella Fiorentino

**Overview:**

This lab is intended to assess your ability to use R to load data and to
generate basic descriptive statistics. You’ll be using monthly weather
data from the Daymet climate database (<http://daymet.ornl.gov>) for all
counties in the United States over a 11 year period (2010-2021). These
data are available on the Github repo for our course. The following
variables are provided:

- cty_txt: Code for joining to census data
- year: Year of observation (with an initial “Y” to make it a character)
- month: Month of observation (1=Jan, 2=Feb, etc.)
- median_tmax: Median maximum recorded temperature (Celsius)
- median_tmin: Median minimum recorded temperature (Celsius)
- sum_prcp: Total recorded prcpitation for the month (mm)
- cty_name: Name of the county
- state: state of the county
- region: Census region (map:
  <https://www2.census.gov/geo/pdfs/maps-data/maps/reference/us_regdiv.pdf>)
- division: Census division
- X: Longitude of the county centroid
- Y: Latitude of the county centroid

These labs are meant to be done collaboratively, but your final
submission should demonstrate your own original thought (don’t just copy
your classmate’s work or turn in identical assignments). Your answers to
the lab questions should be typed in the provided RMarkdown template.
You’ll then “knit” this to an Github document and upload it to your
class Github repo.

**Procedure:**

Load the tidyverse package and import the data:

``` r
library(tidyverse)
daymet_data <- read_csv("data/daymet_monthly_median_2010-2021.csv") %>% 
  separate(date, into = c("year","month"), sep = "-")
```

We can look at the first few rows of the dataset using the *head*
function. We also use *kable* to format this nicely.

``` r
kable(head(daymet_data))
```

| cty_txt | year  | month | median_tmax | median_tmin | sum_prcp | cty_name          | state  | region      | division         |         x |        y |
|:--------|:------|:------|------------:|------------:|---------:|:------------------|:-------|:------------|:-----------------|----------:|---------:|
| G02060  | Y2010 | 1     |       -4.27 |      -10.83 |    10.04 | Bristol Bay       | Alaska | West Region | Pacific Division | -156.7011 | 58.74213 |
| G02185  | Y2010 | 1     |      -20.73 |      -28.20 |     0.00 | North Slope       | Alaska | West Region | Pacific Division | -153.4411 | 69.30696 |
| G02180  | Y2010 | 1     |      -16.50 |      -23.72 |     5.75 | Nome              | Alaska | West Region | Pacific Division | -163.9703 | 64.89492 |
| G02050  | Y2010 | 1     |      -11.20 |      -18.90 |    24.55 | Bethel            | Alaska | West Region | Pacific Division | -159.7678 | 60.92187 |
| G02261  | Y2010 | 1     |      -13.93 |      -20.03 |    15.84 | Valdez-Cordova    | Alaska | West Region | Pacific Division | -144.4573 | 61.57080 |
| G02170  | Y2010 | 1     |       -5.10 |      -12.42 |    35.84 | Matanuska-Susitna | Alaska | West Region | Pacific Division | -149.5702 | 62.31653 |

**Question 1:** After loading the file into R, pick TWO variables and
determine whether they are nominal, ordinal, interval, and ratio data
using the course readings and lectures as reference. Justify each
classification in a sentence or two.

The County Name, State and Region are all examples of Nominal Data. This
is because nominal data are label variables without any quantitative
value.

The total recorded precipitation for the month is an example of ratio
data. This is because a sum of zero has no value and you cannot have
negative quantities of rain.

There are a lot of observations here, 452,448, to be exact. To get a
better grasp on it, we can use group_by and summarise in the tidyverse
package, which we covered in class. This will allow us to identify the
mean value for each year by county across the study period.

**Question 2:** Use group_by and summarise to calculate the mean minimum
temperature for each year *by county* across all months, also including
State and Region as grouping variables. Your resulting dataset should
show the value of tmin for each county in each year. Use the kable and
head functions as shown above to call the resulting table.

``` r
#group data by county and summarize
daymet_sum<-daymet_data %>%
  group_by(cty_name,year,month,state,region) %>%
  summarise(mean_temp=mean(median_tmin))
```

    ## `summarise()` has grouped output by 'cty_name', 'year', 'month', 'state'. You
    ## can override using the `.groups` argument.

``` r
kable(head(daymet_sum))
```

| cty_name  | year  | month | state          | region       | mean_temp |
|:----------|:------|:------|:---------------|:-------------|----------:|
| Abbeville | Y2010 | 1     | South Carolina | South Region |    -2.400 |
| Abbeville | Y2010 | 10    | South Carolina | South Region |     9.220 |
| Abbeville | Y2010 | 11    | South Carolina | South Region |     4.275 |
| Abbeville | Y2010 | 12    | South Carolina | South Region |    -2.800 |
| Abbeville | Y2010 | 2     | South Carolina | South Region |    -2.360 |
| Abbeville | Y2010 | 3     | South Carolina | South Region |     4.200 |

**Question 3:** What if we were only interested in the South Region?
Filter the original data frame (daymet_data) to just include counties in
this region. Then calculate the mean minimum temperature by year *for
each state.* For an optional extra challenge, use the round function to
include only 1 decimal point. You can type ?round in the console to find
documentation on this function. Use kable and head to call the first few
lines of the resulting table

``` r
#filter daymet_data
daymet_SR<-daymet_data %>%
  filter(region=="South Region")

#group by year and state and summarize
SR_sum<-daymet_SR %>%
  group_by(state,year) %>%
  summarise(mean_temp=mean(median_tmin)) %>%
  mutate(mean_temp = round(mean_temp, digits = 1))
```

    ## `summarise()` has grouped output by 'state'. You can override using the
    ## `.groups` argument.

``` r
kable(head(SR_sum))
```

| state   | year  | mean_temp |
|:--------|:------|----------:|
| Alabama | Y2010 |      10.2 |
| Alabama | Y2011 |      10.7 |
| Alabama | Y2012 |      11.8 |
| Alabama | Y2013 |      10.8 |
| Alabama | Y2014 |      10.2 |
| Alabama | Y2015 |      12.5 |

**Question 4:** To visualize the trends, we could use ggplot to
visualize change in mean temperature over time. Create a line plot
(geom_line) showing the state means you calculated in question 3. Use
the color parameter to show separate colors for each state. You may also
need to define the state as a group in the aesthetic parameter.

``` r
#GGplot
ggplot(SR_sum, aes(x = year, y = mean_temp, color = state, group = state)) +
  geom_line(size = 1) +
  labs(x = "Year",y = "Mean Minimum Temperature",title = "Mean Minimum Temperature by Year for Each State") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

![](Lab1_Loading_data_and_basic_stats_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

**Question 5:** If you wanted to look at these data as a table, you’d
need to have it in wide format. Use the pivot_wider function to create a
wide format version of the data frame you created in question 3. In this
case, the rows should be states, the columns should be the years, and
the data in those columns should be mean minimum temperatures. Then call
the *whole table* using kable.

``` r
#table
SR_wide<- SR_sum %>%
  pivot_wider(names_from = year, values_from = mean_temp)
kable(SR_wide)
```

| state                | Y2010 | Y2011 | Y2012 | Y2013 | Y2014 | Y2015 | Y2016 | Y2017 | Y2018 | Y2019 | Y2020 | Y2021 |
|:---------------------|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|------:|
| Alabama              |  10.2 |  10.7 |  11.8 |  10.8 |  10.2 |  12.5 |  11.9 |  12.4 |  12.0 |  12.3 |  12.3 |  11.8 |
| Arkansas             |  10.1 |  10.2 |  11.1 |   9.2 |   9.1 |  10.8 |  10.9 |  11.0 |  10.3 |  10.6 |  10.5 |  10.7 |
| Delaware             |   8.7 |   8.8 |   9.2 |   8.3 |   7.6 |   8.7 |   8.8 |   9.3 |   8.8 |   9.3 |   9.6 |   8.9 |
| District of Columbia |   9.2 |   9.3 |   9.5 |   8.5 |   8.0 |   8.9 |   9.1 |  10.0 |   9.1 |   9.8 |  10.0 |   9.6 |
| Florida              |  14.2 |  15.8 |  16.4 |  16.2 |  15.4 |  17.5 |  16.7 |  17.2 |  16.7 |  17.0 |  17.2 |  16.6 |
| Georgia              |  10.3 |  11.2 |  12.4 |  11.2 |  10.8 |  12.8 |  12.1 |  12.7 |  12.2 |  12.7 |  12.7 |  12.0 |
| Kentucky             |   7.3 |   7.9 |   8.2 |   6.7 |   6.7 |   8.0 |   8.4 |   8.5 |   8.1 |   8.4 |   8.2 |   8.1 |
| Louisiana            |  13.2 |  13.9 |  14.7 |  13.2 |  12.7 |  14.8 |  15.0 |  15.4 |  14.5 |  14.4 |  14.8 |  14.7 |
| Maryland             |   8.3 |   8.6 |   8.7 |   7.8 |   7.0 |   8.1 |   8.4 |   8.9 |   8.4 |   8.9 |   9.2 |   8.7 |
| Mississippi          |  11.0 |  11.4 |  12.2 |  11.0 |  10.5 |  12.8 |  12.4 |  12.9 |  12.2 |  12.5 |  12.6 |  12.4 |
| North Carolina       |   8.7 |   9.1 |   9.7 |   8.7 |   8.6 |  10.2 |   9.8 |  10.0 |   9.9 |  10.5 |  10.1 |   9.6 |
| Oklahoma             |   9.6 |   9.5 |  10.4 |   8.5 |   8.8 |   9.8 |  10.2 |  10.0 |   9.2 |   9.3 |   9.4 |   9.9 |
| South Carolina       |  10.2 |  11.0 |  11.8 |  10.6 |  10.4 |  12.3 |  11.8 |  12.1 |  11.8 |  12.2 |  12.2 |  11.5 |
| Tennessee            |   8.3 |   8.6 |   9.4 |   7.9 |   7.6 |   9.4 |   9.1 |   9.3 |   9.2 |   9.6 |   9.3 |   9.1 |
| Texas                |  11.5 |  12.2 |  12.8 |  11.5 |  11.5 |  12.4 |  12.9 |  12.9 |  12.1 |  12.0 |  12.4 |  12.5 |
| Virginia             |   7.4 |   7.9 |   8.2 |   7.3 |   6.8 |   8.2 |   8.2 |   8.4 |   8.2 |   8.7 |   8.6 |   8.1 |
| West Virginia        |   4.9 |   5.8 |   5.7 |   4.8 |   4.2 |   5.6 |   5.9 |   6.1 |   5.8 |   6.2 |   6.2 |   5.8 |

**Question 6:** Let’s assess the relationship of heat and precipitation
by region. Returning to the original dataset, create a data frame that
shows the mean *maximum temperature* and *mean precipitation* for *all*
states in 2015, also including region as a subgroup. Then use ggplot to
create a scatterplot (geom_point) for these two variables, coloring the
points using the region variable.

``` r
#filter by year and group by state
daymet_2015<-daymet_data %>%
  filter(year=="Y2015") %>%
  group_by(state,year,region) %>%
  summarise(mean_max_temp=mean(median_tmax), mean_precipitation=mean(sum_prcp)) %>%
  mutate(mean_max_temp = round(mean_max_temp, digits = 1), mean_precipitation = round(mean_precipitation, digits = 1))
```

    ## `summarise()` has grouped output by 'state', 'year'. You can override using the
    ## `.groups` argument.

``` r
#GG scatter plot for mean values colored by region.
ggplot(daymet_2015, aes(x = mean_max_temp, y = mean_precipitation, color = region, shape = region, group = region)) +
  geom_point(size = 2) +
  scale_shape_discrete()
```

![](Lab1_Loading_data_and_basic_stats_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

``` r
  labs(x = "Mean Maximum Temperature",y = "Mean Precipitation",title = "Mean Precipitation versus Mean Maximum Temperature for Each Region") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```

    ## NULL

**Question 7:** In the space below, explain what each function in your
code for question 6 does to the dataset in plain English.

First, I wanted to store my data table in a new environment (line116).
The filter function (line 117) pulls out all of the data pertaining to
2015 from the original data set (daymet_data). I then wanted to
calculate the mean for the max temperature and precipitation using the
states as a qualitative grouping variable. Line 118 is grouping the data
by state with year and region as subgroups. Line 119 summarizes the mean
maximum temperature and mean precipitation for each state in 2015. As an
extra step, I wanted to round the mean values to one decimal place using
the mutate and round function in line 120.

For the ggplot, I first needed to define what my x and y variables were
in the daymet_2015 data set (line 123). I also needed to specify how I
wanted the points to appear grouped by region with a different color and
shape representing each region. Then, I needed to specify what kind of
ggplot I wanted. Line 124 shows I want a scatter plot with each point
being size 2. Line 125 changed the shape of the plot points based on its
corresponding region. In line 126 I defined the axis labels for x and y
and gave the plot a title. In lines 127 and 128, I chose a color scheme
and positioned the x axis titles at a 45 degree angle for aesthetic
purposes.

**Lab extension:** In script 1-3, we covered ways of working with the
Daymet API. Create a script below that uses that daymetr package to
download data from Daymet for a place (or places) of your choosing. Then
visualize the temporal pattern for a variable of your choosing in this
place, for example, similar to what you did in question 4. Use a dplyr
function (e.g., mutate, summarise, filter, etc.) to do any needed data
wrangling and create a visual using ggplot. In addition to this code,
write a short summary of a pattern that’s evident in the data you
visualized.

``` r
#Your code goes here
```

{Explanation goes here.}

**Lab reflection:** How do you feel about the work you did on this lab?
Was it easy, moderate, or hard? What were the biggest things you learned
by completing it?

I would say this lab was moderately hard for me to complete. The
questions challenged me to piece different functions together that we
have been learning, and not just regurgitate the in class practice. I
noticed I ran into a lot less errors and overall I felt more confident
in my ability to work through each question. I liked the optional
challenge to use the rounding function! I am trying to be better about
using comments and organizing my code so I can easily read what I am
working on. Using GGPlot in class was a little overwhelming but I feel
more comfortable using it after this lab.
