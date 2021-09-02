Geog4/6300: Lab 1
================

## Loading data into R, data transformation, and summary statistics

**Overview:**

This lab is intended to assess your ability to use R to load data and to
generate basic descriptive statistics. You’ll be using monthly weather
data from the Daymet climate database (<http://daymet.ornl.gov>) for all
counties in the United States over a 10 year period (2005-2015). These
data are available on the Github repo for our course. The following
variables are provided:

-   gisjn\_cty: Code for joining to census data
-   year: Year of observation
-   month: Month of observation
-   dayl: Mean length of daylight (in seconds)
-   srad: Mean solar radiation per day
-   tmax: Mean maximum recorded temperature (Celsius)
-   tmin: Mean minimum recorded temperature (Celsius)
-   vap\_pres: Mean vapor pressure (indicative of humidity)
-   prcp: Total recorded prcpitation (mm)
-   cty\_name: Name of the county
-   state: state of the county
-   region: Census region (map:
    <https://www2.census.gov/geo/pdfs/maps-data/maps/reference/us_regdiv.pdf>)
-   division: Census division
-   lon: Longitude of the point
-   lat: Latitude of the point

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
daymet_data <- read_csv("data/Daymet_Cty_Summary_2005_2015.csv")
```

We can look at the first few rows of the dataset using the *head*
function. We also use *kable* to format this nicely.

``` r
kable(head(daymet_data))
```

| gisjn\_cty | year | month |     dayl |     srad |     tmax |       tmin | vap\_pres | prcp | CTY\_NAME | State   | Region       | Division                    |       Lon |    Lat |
|:-----------|-----:|:------|---------:|---------:|---------:|-----------:|----------:|-----:|:----------|:--------|:-------------|:----------------------------|----------:|-------:|
| G01001     | 2005 | Apr   | 46137.60 | 420.6933 | 23.13333 |  9.4666667 | 1210.6667 |  195 | Autauga   | Alabama | South Region | East South Central Division | -86.64257 | 32.535 |
| G01001     | 2005 | Aug   | 47380.65 | 364.3871 | 31.58065 | 22.0322581 | 2649.0323 |  150 | Autauga   | Alabama | South Region | East South Central Division | -86.64257 | 32.535 |
| G01001     | 2005 | Dec   | 35663.77 | 258.5806 | 13.33871 |  0.1774194 |  637.4194 |   69 | Autauga   | Alabama | South Region | East South Central Division | -86.64257 | 32.535 |
| G01001     | 2005 | Feb   | 39028.11 | 297.2571 | 16.60714 |  5.4107143 |  935.7143 |  152 | Autauga   | Alabama | South Region | East South Central Division | -86.64257 | 32.535 |
| G01001     | 2005 | Jan   | 36444.06 | 261.5742 | 15.75806 |  3.5483871 |  855.4839 |   75 | Autauga   | Alabama | South Region | East South Central Division | -86.64257 | 32.535 |
| G01001     | 2005 | July  | 50045.13 | 350.2452 | 31.37097 | 22.0483871 | 2649.0323 |  284 | Autauga   | Alabama | South Region | East South Central Division | -86.64257 | 32.535 |

***Question 1:** After loading the file into R, pick TWO variables and
determine whether they are nominal, ordinal, interval, and ratio data.
Justify the classification you chose in a sentence or two for each one.*

{Your response goes here.}

There are a lot of observations here, 413,820 to be exact. To get a
better grasp on it, we can use group\_by and summarise in the tidyverse
package, which we covered in class. This will allow us to identify the
mean value for each year by county across the study period.

***Question 2:** Use group\_by and summarise to calculate the mean
minimum temperature for each year by county, also including State and
Region as grouping variables. Your resulting dataset should show the
value of tmin for each county in each year. Use the kable and head
functions as shown above to call the resulting table.*

``` r
#Your code goes here.
```

***Question 3:** What if we were only interested in the South Region?
Filter the original data frame (daymet\_data) to just include counties
in this region. Then use group\_by and summarise again to calculate the
mean minimum temperature by year *for each state.\* For 1 point extra
credit, use the round function to include only 1 decimal point. You can
type ?round in the console to find documentation on this function. Use
kable and head to call the first few lines of the resulting table\*

``` r
#Your code goes here.
```

***Question 4:** To visualize the trends, we could use ggplot to
visualize change in mean temperature over time. Create a line plot
(geom\_line) showing the state means you calculated in question 3. Use
the color parameter to show separate colors for each state.*

``` r
#Your code goes here.
```

***Question 5:** If you wanted to look at these data as a table, you’d
need to have it in wide format. Use the pivot\_wider function to create
a wide format version of the data frame you created in question 3. In
this case, the rows should be states, the columns should be the years,
and the data in those columns should be mean minimum temperatures. Then
call the whole table using kable.*

``` r
#Your code goes here.
```

***Question 6:** Returning to the original dataset, create a data frame
that shows the mean *maximum\* temperature and mean precipitation for
*all* states in 2015, also grouping by region. Then use ggplot to create
a scatterplot (geom\_point) for these two variables, coloring the points
using the region variable.\*

``` r
#Your code goes here.
```

\***Question 7** In the space below, explain what each function in your
code for question 6 does to the dataset in plain English.

{Your response here.}

\***Lab reflection** How do you feel about the work you did on this lab?
Was it easy, moderate, or hard? What were the biggest things you learned
by completing it?
