---
title: "Introducing dplyr and tidyr"
teaching: 0
exercises: 0
questions:
- "How can I select specific columns from a data frame using dplyr?"
- "How can I select specific rows from a data frame using dplyr?"
- "What is ‘piping’ and what are the advantages of using it?"
- "How can create new columns or remove existing columns from a data frame?"
- "How can I change the format of a data frame from wide to long and vice versa?"

objectives:
- "Use select to choose columns from a data frame" 
- "Use filter to choose rows from a data frame"
- "Use the pipe (%>%) operator to combine commands"
- "Use mutate to create new columns in a data frame"
- "Use group_by and summarise commands to aggregate data in a data frame"
- "Use spread to change the data format from long to wide"
- "Use gather to change the data format from wide to long"
- "Use write.csv to save a data frame to a csv file"

keypoints:
- "First key point."
---


## What are 'dplyr' and 'tidyr'?

`dplyr` is a package, part of the 'tidyverse' package, for
making tabular data manipulation easier. It pairs nicely with `tidyr` which enables you to conveniently convert between different data formats which can be useful for plotting and some types of analysis.

The package `dplyr` provides easy tools for the most common data manipulation
tasks. It is built to work directly with data frames, with many common tasks
optimized for faster execution. An additional feature is the
ability to work directly with data stored in an external database. We will look at this in a later episode.


The package **`tidyr`** addresses the common problem of wanting to reshape your data for plotting and use by different R functions. Sometimes we want data sets where we have one row per measurement. Sometimes we want a data frame where each measurement type has its own column, and rows are instead more aggregated groups - like plots or aquaria. Moving back and forth between these formats is nontrivial, and **`tidyr`** gives you tools for this and more sophisticated  data manipulation.

To learn more about **`dplyr`** and **`tidyr`** after the workshop, you may want to check out this
[handy data transformation with **`dplyr`** cheatsheet](https://github.com/rstudio/cheatsheets/raw/master/data-transformation.pdf) and this [one about **`tidyr`**](https://github.com/rstudio/cheatsheets/raw/master/data-import.pdf).


We will start by loading our SN7577 dataset

~~~
SN7577 <- read_csv("SN7577.csv")

## inspect the data
str(SN7577)
~~~

Previously we noted that the SN7577 variable appeared to be in 3 different classes. WE were happy to think of it as a data frame as that was what we were talking about.  Now it is convenient to think of it as having a class of  `tbl_df`.
This is referred to as a "tibble".
Tibbles are almost identical to R's standard data frames, but they tweak some of the old behaviors of data frames. For our purposes the only differences between data frames and tibbles are that:

1. When you print a tibble, R displays the data type of each column under its name; it prints only the first few rows of data and only as many columns as fit
on one screen.
2. Columns of class `character` are never automatically converted into factors.

We have in fact already noticed these effects when we have previously used 'str' and had to explicitly convert our categorical variables into Factors.


## Selecting columns and filtering rows

We're going to learn some of the most common **`dplyr`** functions: `select()`,
`filter()`, `mutate()`, `group_by()`, and `summarize()`. 
To select columns of a data frame, use `select()`. The first argument to this function is the data
frame (`SN7577`), and the subsequent arguments are the columns to keep.

~~~
select(SN7577, sex, age, numage)
~~~

To choose rows based on a specific criteria, use `filter()`:

~~~
filter(SN7577, numage == 90)
~~~



## Pipes

What if you want to select and filter at the same time? There are three
ways to do this: use intermediate steps, nested functions, or pipes.

With intermediate steps, you create a temporary data frame and use
that as input to the next function, like this:'

~~~
SN7577_a <- filter(SN7577, numage == 90)
SN7577_b <- select(SN7577_a, sex, age, numage)
SN7577_b
~~~

This is readable, but can clutter up your workspace with lots of objects that you have to name individually. With multiple steps, that can be hard to keep track of.

You can also nest functions (i.e. one function inside of another), like this:

~~~
SN7577_b <- select(filter(SN7577, numage == 90), sex, age, numage)
SN7577_b
~~~

This is handy, but can be difficult to read if too many functions are nested, as
R evaluates the expression from the inside out (in this case, filtering, then selecting). So if you want to work what is going on you have to start in the middle and work your way out to the side.



The last option, *pipes*, are a recent addition to R. Pipes let you take
the output of one function and send it directly to the next, which is useful
when you need to do many things to the same dataset.  Pipes in R look like
`%>%` and are made available via the `magrittr` package, installed automatically
with `dplyr`. 


~~~
SN7577 %>%
  filter(numage == 90) %>%
  select( sex, age, numage)
~~~

In the above code, we use the pipe to send the `SN7577` dataset first through
`filter()` to keep rows where `numage` equals 90, then through `select()`
to keep only the `sex`, `age` and `numage` columns. Since `%>%` takes
the object on its left and passes it as the first argument to the function on
its right, we don't need to explicitly include the data frame as an argument
to the `filter()` and `select()` functions any more.

Some may find it helpful to read the pipe like the word "then". For instance,
in the above example, we took the data frame `SN7577`, *then* we `filter`ed
for rows with `numage == 90`, *then* we `select`ed columns `sex`, `age`
and `numage`. The `dplyr` functions by themselves are somewhat simple,
but by combining them into linear workflows with the pipe, we can accomplish
more complex manipulations of data frames.

If we want to create a new object with this smaller version of the data, we
can assign it a new name:

~~~
SN7577_90 <- SN7577 %>%
  filter(numage == 90) %>%
  select( sex, age, numage)

SN7577_90
~~~

Note that the final data frame is the leftmost part of this expression.

> ## Exercise
> 
>  Using pipes, subset the `SN7577` data to include rows where the numage is  less than 65
>  and retain only the columns `sex`, `age` and `numage.
> 
> > ## Solution
> > 
> > ~~~
> > SN7577_lt65 <- SN7577 %>%
> >   filter(numage < 65) %>%
> >   select( sex, age, numage)
> > 
> > SN7577_lt65
> > ~~~
> > 
> {: .solution}
{: .challenge}
 

### Mutate

Frequently you'll want to create new columns based on the values in existing
columns, for example to do unit conversions, or to find the ratio of values in two
columns. For this we'll use `mutate()`.

To create a new column of weight in kg:

~~~
SN7577 %>%
  mutate(income_000 = income * 1000) %>%
  select( income_000, income)
~~~

You can also create a second new column based on the first new column within the same call of `mutate()`:

~~~
tax_rate <- 0.2
SN7577 %>%
  mutate(income_000 = income * 1000, income_taxed = income_000 * ( 1 - tax_rate)) %>%
  select( income_000, income, income_taxed)
~~~


If this runs off your screen and you just want to see the first few rows, you
can use a pipe to view the `head()` of the data. (Pipes work with non-`dplyr`
functions, too, as long as the `dplyr` or `magrittr` package is loaded).

~~~
SN7577 %>%
  mutate(income_000 = income * 1000) %>%
  select( income_000, income) %>%
  head()
~~~

> ## Exercise
>
> 
>  Create a new data frame from the `SN7577` data that meets the following
>  criteria: contains only the `numage` column and a new column called
>  `voting years` containing values that are the `numage ` values minus 18.
>  Ensure that the `voting years` column has no -ve values. 
>  Return only the voting_years and numage columns.
> 
> > ## Solution
> > 
> > ~~~
> > SN7577 %>%
> >  filter(numage >= 18) %>%
> >  mutate(voting_years = numage - 18) %>%
> >  select(voting_years, numage)
> > ~~~
> > 
> {: .solution}
{: .challenge}

### Split-apply-combine data analysis and the summarize() function

Many data analysis tasks can be approached using the *split-apply-combine*
paradigm: split the data into groups, apply some analysis to each group, and
then combine the results. `dplyr` makes this very easy through the use of the
`group_by()` function.


#### The `summarize()` function

`group_by()` is often used together with `summarize()`, which collapses each
group into a single-row summary of that group.  `group_by()` takes as arguments
the column names that contain the **categorical** variables for which you want
to calculate the summary statistics. So to compute the average `numage` by `Q1` (voting intentions):

~~~
SN7577 %>%
  group_by(Q1) %>%
  summarize(avg_age = mean(numage, na.rm = TRUE))
~~~


> ## Exercise
>
> The 'na.rm = TRUE' in the summarize clause in the mean function isn't really needed for the SN7577 data as there are no NA values. 
> 1. Try removing it and running the code again and confirm that the answers are the same.
> 2. There ar however 6 rows with 'numage' = 0. These are effectively missing data. Change these values to NA and run the 
>  code again, both with and without the 'na.rm = True' parameter
> 
> > ## Solution
> > 
> > ~~~
> > SN7577 %>%
> >   group_by(Q1) %>%
> >   summarize(avg_age = mean(numage, na.rm = TRUE))
> > 
> > SN7577 %>%
> >   group_by(Q1) %>%
> >   summarize(avg_age = mean(numage))
> > 
> > SN7577$numage[SN7577$numage == 0] <- NA
> > 
> > SN7577 %>%
> >   group_by(Q1) %>%
> >   summarize(avg_age = mean(numage, na.rm = TRUE))
> > 
> > SN7577 %>%
> >   group_by(Q1) %>%
> >   summarize(avg_age = mean(numage)) 
> > ~~~
> > 
> {: .solution}
{: .challenge}

If we know that there are 'NA's in a column we could filter them out in advance of any other processing.

~~~
# Art there any NAs in numage?
SN7577 %>%
  filter(is.na(numage))

# use '!' to exclude them
SN7577 %>%
  filter(!is.na(numage)) %>%
  group_by(Q1) %>%
  summarize(avg_age = mean(numage))
~~~


You can also group by multiple columns. 'age' is an age grouping variable from 1 to 7, 'sex' uses integers 1 and 2 to denote Male and Female.

~~~
SN7577 %>%
  group_by(sex, age) %>%
  summarize(avg_age = mean(numage, na.rm = TRUE))
~~~

If we ran a similar piece of code, but this time going back to usoing 'Q1'

~~~
SN7577 %>%
  filter(!is.na(numage)) %>%
  group_by(sex, Q1) %>%
  summarize(avg_age = mean(numage))
~~~

All of the results are not displayed. This is because 'Q1' can have 11 different values and as we noted earlier, 'dplyr' restrict the output.

We can get round this by explictly adding a call to the 'print' function at the end of our pipeline.

~~~

SN7577 %>%
  filter(!is.na(numage)) %>%
  group_by(sex, Q1) %>%
  summarize(avg_age = mean(numage)) %>%
  print(n =22)
~~~


We can also have more than one summary at a time.

~~~

SN7577 %>%
  filter(!is.na(numage)) %>%
  group_by(sex, Q1) %>%
  summarize(avg_age = mean(numage), 
            count_age = n())    %>%
  print(n = 22)
~~~

the 'n' function counts the number of rows in each group. This is something which is very commonly done that it has its own funtion'tally' in 'dplyr'

If we wanted to know how many of each age the respondents were we could use:

~~~
SN7577 %>%
  group_by(numage) %>%
  tally() %>%
  print(n = 100)
~~~

If you go done to the bottom of the listing you will see that the 6 NAs are also accounted for.

> ## Exercise
>
> Q2 in SN7577 is an indication of who the respondent is inclined to vote for. Missing data is denoted by a value
> of -1. Find the average age and number of respondents in each of the Q2 categories.
> 
> > ## Solution
> > 
> > ~~~
> > SN7577 %>%
> >   group_by(Q2) %>%
> >   summarize(avg_age = mean(numage, na.rm = TRUE),
> >             count_age = n())
> > ~~~
> > 
> {: .solution}
{: .challenge}


## Reshaping with gather and spread

To look at the 'gather' and 'spread' functions we will use the SAFI_results data.

~~~
library(readr)
SAFI_results <- read_csv("SAFI_results.csv")
~~~

Within the SAFI_results data isa column, C02_respondent_wall_type, which records the material used make the walls of the houses. We can see from the following that there are only 4 different values.

~~~
SAFI_results %>%
  select (C02_respondent_wall_type) %>%
  group_by(C02_respondent_wall_type) %>%
  tally()
~~~


Rather than having the single column, we may prefer to have individual columns for each type of wall and an indication if a particular house (= observation) has that type of wall material.

We can immediately see that as a house can only use one type of material, then for any given observation, there is only data for one of the four possible columns. The other three will be NA.

This is a ntural consequence of converting from 'long' to 'wide' format.

To perform this action we are going to use the `spread` function.

`spread()` takes three principal arguments:

1. the data 
2. the *key* column variable whose values will become new column names.  
3. the *value* column variable whose values will fill the new column variables.

Further arguments include `fill` which, if set, fills in missing values with 
the value provided.

~~~
\# Spread, create dummy variable to use as values
SAFI_wall_spread <- SAFI_results %>%
  mutate(mycount = 1) %>%
  spread(key = C02_respondent_wall_type, value = mycount, fill = 0)
~~~

In the code above a new variable called mycount is created with a value of 1 for each observation. This is only done to use as a column for the 'value' parameter. In the `spread` function the Key is the existing column containing the wall types which will become the new columns and they will be given a value od either 1 if the observation has tat walltype and 0 otherwise (the fill value).

The newly created dataframe has 4 newly created column all with values of 1 or 0. the key column has been removed automatically.


If you have data which is in this wide format and need to convert it to 'long' format (i.e. what we sttarted with), then you can use the `gather` function.

`gather()` takes four principal arguments:

1. the data
2. the *key* column variable we wish to create from column names.
3. the *values* column variable we wish to create and fill with values 
associated with the key.
4. the names of the columns we use to fill the key variable (or to drop).

For our key we will re-create the original column name of C02_respondent_wall_type, we will create a dummy variable called mycount to pass to the `value` parameter. 
We also need to passin the list of column names to be used to populate the key. As they are contiguous we can use the the ':' operator to specify the start and last column names.

~~~
SAFI_gather <- SAFI_wall_spread %>%
  gather(key = C02_respondent_wall_type, value = C02_count, burntbricks:sunbricks) %>%
  filter(C02_count == 1) %>%
  select(-C02_count)
~~~

After the gather, as we are re-creating our original dataframe we are only interested in rows the C02_count value is 1. The others were created as a result of the fill=0 option in the spread function. Having filtered on CO2_count, we can now delete it as it wasn't in the original dataframe.

Both SAFI_results and SAFI_gather now have 131 observations and 55 variables.

> ## Exercise
>
> 1. Use the `spread` function to create new columns for all of the different roof types (C01_respondent_roof_type)
> 
>  2. Reverse the operation using the `gather` function
>
> > ## Solution
> > 
> > ~~~
> > \# Spread, create dummy variable to use as values
> > SAFI_roof_spread <- SAFI_results %>%
> >   mutate(mycount = 1) %>%
> >   spread(key = C01_respondent_roof_type, value = mycount, fill = 0)
> > SAFI_roof_spread
> > 
> > \# Gather + filter out excess rows and unneeded colum
> > SAFI_gather <- SAFI_roof_spread %>%
> >   gather(key = C01_respondent_roof_type, value = C01_count, grass:mabatisloping) %>%
> >   filter(C01_count == 1) %>%
> >   select(-C01_count)
> > ~~~
> > 
> {: .solution}
{: .challenge}


## Exporting data

Now that you have learned how to use `dplyr` to extract information from
or summarize your raw data, you may want to export these new data sets for future use.

Similar to the `read_csv()` function used for reading CSV files into R, the tidyverse includes a `write_csv()` function that generates CSV files from data frames.


Use of 'write_csv' is straightforward you onlty need to provide the name of the dataframe you wish to write out and the name of the file that you wish to use.

We will save our SN7577_papers dataframe.

~~~
write_csv(SN7577_papers, path = "SN7577_papers.csv")
~~~

The filename is passed to the path parameter, as the name suggests you could specify a full path + the filename. If you do not , then the current working directory will be assumed.

~~~


