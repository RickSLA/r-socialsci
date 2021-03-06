---
title: "R basics"
teaching: 0
exercises: 0
questions:
- "What data types are available in R?"
- "What is an object?"
- "How can values be initially assigned to variables of different data types?"
- "What arithmetic and logical operators can be used?"
- "How can subsets be extracted from vectors and data frames?"
- "How does R treat missing values?"
- "How can we deal with missing values in R"

objectives:
- "Define what an Object is"
- "Understand that everything in R is an Object"
- "Assign values to objects of different types"
- "Perform arithmetical and logical operations on variables of different types"
- "Extract subsets from vectors and data frames"
- "Recognise missing values in a data object"
- "Understand how R treats missing values"
- "Detect and replace missing values"

keypoints:
- "First key point."
---

## Preliminaries

In this lesson we we be using 3 data files;

SAFI_results.csv contains survey data related to farming in Mozambique and Tanzania. It can be downloaded from [here](../data/SAFI_results.csv)

SN7577.csv is a UKDS datset related to political engagement. It can be downloaded from [here](../data/SN7577.csv)

SN7577.tab is the same as above but in tab delimited format. It can be downloaded from [here](../data/SN7577.tab)

You may also want to download the data dictionary associated with the SN7577 dataset from [here](../data/audit_of_political_engagement_11_ukda_data_dictionary.rtf)

You should save the 3 data files in a folder where you intend to do your R work.

In order to follow along with the examples in this episode you should start RStudio and create a new RScript file either from
the File menu or the toolbar icon immediately below it.

If you intend to save the script you may wish to name it. If you click the save icon on the toolbar a standard save as
dialog will appear.

Although we will routinely refer to using the console in this episode, and you can in fact type all of the code directly into the console, we will assume that you will write all of your code to the script file first and then select the code and either click the `run` button on the toolbar or use the keyboard shortcut of `ctrl+Enter`.

There are shortcut alternatives to many of the RStudio menu and toolbar items. The RStudio article [Editing and Executing Code](https://support.rstudio.com/hc/en-us/articles/200484448-Editing-and-Executing-Code) explains many of them.

When you select code for execution, be careful that you either select the whole line or lines. If you are just running a single line, simply place the cursor in the line. If you select part of a line, only that part will be executed.


## Using the console in RStudio

If you type simple arithmetic expressions into the console it will be evaluated and the result displayed immedediately below.

In order to save the result for further use it is necessary to assign the value of the expression to a variable.


## Creating variables and assigning values

Varaibles are usually given short but meaningful names. You can use uppercase characters if you want, but remember that R is case sensitive, so that you must be consistent when calling the variable later. You can also use numbers as part of a variable name but you cannot start the name with a number. There are some words which cannot be used as variable names such as 'if' or 'function'. You can get a complete list of these words using the `help("reserved")` or `?reserved` commands from the console.

You can use the `.` caharacter as part of the name but this is not recommended. If you wish to make longer variable names more readable, using the `_` character helps. 

~~~
Age <- 60
age <- 60
~~~
{: .language-r}

will create two separate variables.

The  `<-` symbol is used in R to denote assignment to a variable. You can read the above assignment as 'Age is assigned the value of 60'.


The `=` symbol can also be used in place of `<-`.

If you type the above two lines in at the console or run them (one at a time) from a script, you will just recieve the standard `>` prompt as a response.

If you want to know what the value of a variable is, you can just type the name of the variable.

~~~
age
~~~
{: .language-r}

~~~
[1] 60
~~~
{: .output}

In which case the current value of the `age` variable will be printed to the console.
Once a variable has been created, you can give it different values either directly or by some calculation. Often you will give a varaible a value based on the value of other variables.

~~~
age <- 55
age
~~~
{: .language-r}

~~~
[1] 55
~~~
{: .output}

~~~
area_hectares <- 1.0
area_acres <- area_hectares * 2.5
area_acres
~~~
{: .language-r}

~~~
[1] 2.5
~~~
{: .output}


If the value assigned to `area_hectares` is changed, it will not effect the value of `area_acres`.

## Comments

All programming languages allow the programmer to include comments in their code. To do this in R we use the `#` character.
Anything to the right of the `#` sign up to the end of the line is treated as a comment and is ignored by R. You can start lines with comments or include them after code on the line.

~~~
area_hectares <- 1.0			# land area in hectares
area_acres <- area_hectares * 2.5	# convert to acres
area_acres				# print land area in acres.
~~~
{: .language-r}

~~~
[1] 2.5
~~~
{: .output}

> ## Exercise
> Create two variables `length` and `width` and assign them the values `2.5` and `3.2` respectively. 
> Create a third variable `area` and assign it a value based on the current values of `length` and `width`. 
> Show that changing the values of `length` and `width` does not effect the value of `area`.
> 
> > ## Solution
> >
> > ~~~
> > length <- 2.5       
> > width <- 3.2      
> > area <- length * width       
> > area
> > ~~~
> > {: .language-r}
> >
> >
> > ~~~
> > [1] 8
> > ~~~
> > {: .output}
> >
> >
> > ~~~      
> > # change the values of  length and width       
> > length <- 7.0     
> > width <- 6.5      
> > # the value of area isn't changed      
> > area       
> > ~~~
> > {: .language-r}
> >
> > ~~~
> > [1] 8
> > ~~~
> > {: .output}
> {: .solution}
{: .challenge}


## Using functions

A function is a pre-defined piece of R code. It allows more complicated or repetited sections of code to be pre-built and then subsequently used and reused wherever needed. As a programmer you can write your own functions but much of the time you rely on functions which are built into the R environment as part of base R or are added in from a 3rd party R package.

Simple repetition of code can be useful, but it is more likely that you wish to perform the same actions using a different set of variables.
Because of this, most functions allow the user of the function to provide parameters representing variables, which you can change each time you call the function.

You call a function by providing the name of the function and a set of parameters - possibly only one, in brackets (also called parentheses) following the name.
In most cases, the function call will be on the right hand side of the assignment. The value(s) returned by the function are assigned to the variable on the left hand side.

For example there is a builtin function which calculates the (positive) square root of a number.

~~~
a <- sqrt(4)
a
~~~
{: .language-r}

~~~
[1] 2
~~~
{: .output}

The square root of 4 is calculated and assigned to the variable `a`

The sqrt function only take a single parameter. It doesn't have to be a literal value like '4' it could be a variable name or

~~~
x <- 12
a <- sqrt(x)
a
~~~
{: .language-r}

~~~
[1] 3.464102
~~~
{: .output}


~~~
y <- 12.5
a <- sqrt(y)
a
~~~
{: .language-r}

~~~
[1] 3.535534
~~~
{: .output}

~~~
y <- 12.5
a <- sqrt(y - 0.5)
a
~~~
{: .language-r}

~~~
[1] 3.464102
~~~
{: .output}

~~~
y <- 12.5
a <- sqrt(round(y))
a
~~~
{: .language-r}

~~~
[1] 3.464102
~~~
{: .output}

> ## Exercise
> Based on the answers to the last two code segments, what do you think the `round` function does?
> 
> 
> > ## Solution
> > 
> > It rounds down a number to the nearest integer value with 0.5 being rounded up.
> > 
> > 
> {: .solution}
{: .challenge}

So far we have used `sqrt` and `round` functions, and we have only provided a single parameter in each case.  The `sqrt` function only takes a single parameter and `round` can take one or two parameters. Other functions can take more than two parameters. 
If you are using a function that you are not familiar with, you can find out what parameters can be used with the function by using the `args` function. The `args` function takes the name of another function as a parameter and provides you with a list of all of the parameters associated with the function.

~~~
args(sqrt)
~~~
{: .language-r}

~~~
function (x)
NULL
~~~
{: .output}

~~~
args(round)
~~~
{: .language-r}

~~~
function (x, digits = 0)
NULL
~~~
{: .output}

~~~
args(args)
~~~
{: .language-r}

~~~
function (name)
NULL
~~~
{: .output}


You can see from this that 'sqrt' only has the one parameter, but 'round' has two. The second parameter is called 'digits'. It has been given a default value of 0. It is because a default value has been given (when the function was created) and therefore we are not required to provide a value when we call the function, and in fact we didn't. The first parameter, 'x' has no default value and therefore has to be specified when the function is called. This makes sense because whenever we use 'round' we want to round some number of our choice.

> ## Exercise
>
> type in '?round' at the console and then look at the output in the Help pane.
> What other rounding like functions are there?
> How do you use the 'digits' parameter in the round function?
>
{: .challenge}

## Variables and Objects 
We have been happily creating things which we have called variables by assigning values to them. Variables are used in almost all programming languages. In R the more accurate term is 'Object'. Everything in R is an object. There are situations where the difference between an Object and a Variable is significant, but for the majority of the work that we will be doing in this lesson, you can treat the terms as synonymous.

You can find what an object is [here](https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Objects)



## R datatypes

In R you do not have to specify the type associated with your variables it is assumed by the values you assign to the variable although R may not assume what you expect.

The simple types avaialbe in R are

Numeric,
Character,
Logical,
Integer,
Complex and
Raw

We will not be using the Complex (for complex numbers) or Raw (for binary datastreams, like audio or video) in this lesson.

You can find out the type of a variable (or the class of an object) by using the 'class' function which takes tha single parameter of an variable/object name)

~~~
name <- "peter"
class(name)
~~~
{: .language-r}

~~~
[1] "character"
~~~
{: .output}

~~~
length <- 2.5
class(length)
~~~
{: .language-r}

~~~
[1] "numeric"
~~~
{: .output}

~~~
width <- 2
class(width)
~~~
{: .language-r}

~~~
[1] "numeric"
~~~
{: .output}

~~~
boolean_value <- TRUE
class(boolean_value)
~~~
{: .language-r}

~~~
[1] "logical"
~~~
{: .output}

Notice that the quotes are needed for the Character type as without them R would assume that 'peter' was a variable. You can use single or double quotes.

Notice that the length and width are both considered to be numeric. By default R does not distinguish between Real and Integers.
You can indicate that you want an Integer by suffixing a value with 'L'

~~~
int_val2 <- 4L
class(int_val2)
~~~
{: .language-r}

~~~
[1] "integer"
~~~
{: .output}

## Vectors

A vector is the most common and basic data type in R. A vector is composed by a series of values, which can be either numbers or characters. We can assign a series of values to a vector using the 'c' function.

~~~
# a vector of ages
ages <- c(50, 60, 65, 82)
class(ages)
~~~
{: .language-r}

~~~
[1] "numeric"
~~~
{: .output}

~~~
ages
~~~
{: .language-r}

~~~
[1] 50 60 65 82
~~~
{: .output}

~~~
# A vector of possessions
possessions <- c("bicycle", "radio", "television")
class(possessions)
~~~
{: .language-r}

~~~
[1] "character"
~~~
{: .output}

~~~
possessions
~~~
{: .language-r}

~~~
[1] "bicycle"    "radio"      "television"
~~~
{: .output}

When we look at the class of the vector it is the type of the individual elements which is returned.

> ## Exercise
>
> What happens if we try to create a vector with a mixture of character and Numeric elements
>
> > ## Solution
> >
> > ~~~
> > mix <- c("bicycle", 22, "television")    
> > mix
> > ~~~
> > {: .language-r}
> >
> > ~~~
> > [1] "bicycle"    "22"         "television"
> > ~~~
> > {: .output}  
> >
> > ~~~  
> > class(mix)    
> > ~~~
> > {: .language-r}
> >
> > ~~~
> > [1] "character"
> > ~~~
> > {: .output}
> >
> > The numeric values are converted to character values
> >
> {: .solution}
{: .challenge}

> ## Exercise
> Here are some more 'mixtures' to try.
> Can you explain the results
>
> > ## Solution
> >
> > ~~~
> > num_char1 <- c(1, 2, 3, "a")
> > class(num_char1)
> > num_char2 <- c(1, 2, 3, "4")
> > class(num_char2)
> > num_logical <- c(1, 2, 3, TRUE)
> > class(num_logical)
> > char_logical <- c("a", "b", "c", TRUE)
> > class(char_logical)
> > ~~~
> >
> {: .solution}
{: .challenge}


You can append or prepend additional elements to the vector using the 'c' function

~~~
# a vector of ages
ages <- c(50, 60, 65, 82)
ages
~~~
{: .language-r}

~~~
[1] 50 60 65 82
~~~
{: .output}

~~~
# adding to the end of the vector
ages <- c(ages, 55, 63, 25, 72, 56, 57, 78, 89, 21, 100 )
ages
~~~
{: .language-r}

~~~
ages
~~~
{: .language-r}

~~~
[1]  50  60  65  82  55  63  25  72  56  57  78  89  21 100
~~~
{: .output}

~~~
# Adding new possessions at the beginning
possessions <- c("washing machine", possessions )
possessions
~~~
{: .language-r}

~~~
[1] "washing machine" "bicycle"         "radio"           "television"
~~~
{:. output}

You can find the structure of a vector by using the `str` function

~~~
str(posessions)
~~~
{: .language-r}

~~~
chr [1:4] "washing machine" "bicycle" "radio" "television"
~~~
{: .output}

~~~
str(ages)
~~~
{: .language-r}

~~~
num [1:14] 50 60 65 82 55 63 25 72 56 57 ...
~~~
{: .output}

The output will tell you the type of the elements, how many elements there are and will list up to the first 10 element values.


## Extracting subsets from vectors

If we want to extract a specific element from a vector we use an indexing system. In R indexing always start from 1. In some other languages like Python, they alway s start from 0.

We specify an index by use of square brackets ('[]').

~~~
# extract 2nd element from the ages vector
ages_2 <- ages[2]
ages_2
~~~
{: .language-r}

~~~
[1] 60
~~~
{: .output}

~~~
# extract 3rd elecment from the posessions vector
posessions
poss_3 <- posessions[3]
poss_3
~~~
{: .language-r}

~~~
[1] "radio"
~~~
{: .output}

~~~
# we can also re-assign specific elecments
possessions[3] <- "Mobile Phone"
possessions
~~~
{: .language-r}

~~~
[1] "radio"
~~~
{: .output}


We can extract more than one element at a time by providing a vector of the indexes we want. The index values don't have to be in order, and we can repeat indexes if we want. We provide the vector by using the 'c' function.

~~~
ages_765 <- ages[c(7, 6, 5)]
ages_765
~~~
{: .language-r}

~~~
[1] 25 63 55
~~~
{: .output}

~~~
class(ages_765)
~~~
{: .language-r}

~~~
[1] "numeric"
~~~
{: .output}

~~~
ages_77765136 <- ages[c(7, 7, 7, 6, 5, 1, 3, 6)]
ages_77765136
~~~
{: .language-r}

~~~
[1] 25 25 25 63 55 50 65 63
~~~
{: .output}

~~~
class(ages_77765136)
~~~
{: .language-r}

~~~
[1] "numeric"
~~~
{: .output}

## Conditional subsetting
Another common way of subsetting is by using a logical vector. TRUE will select the element with the same index, while FALSE will not:

~~~
some_possessions <- possessions[c(TRUE, FALSE, TRUE, FALSE)]
some_possessions
~~~
{: .language-r}

~~~
[1] "washing machine" "radio"  
~~~
{: .output}

Rather than manually typing in the logical vector values of TRUE and FALSE, the logical vector is most commonly generated automatically by using a conditional expression.

~~~
logical_vector <- ages > 50
logical_vector
~~~
{: .language-r}

~~~
[1] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
[14]  TRUE
~~~
{: .output}

The expression 'ages > 50' applies the test '> 50' to each element of the ages vector in turn.
The expression 'ages > 50' doesn't return the values of elements in the ages vector which are greater than fifty. Instead it returns a logical vector of the same size(number of elements) as the ages vector, with a value of TRUE to indicate condition has been met, and FALSE to indicate that it has not.

So now that we know that 'ages > 50' returns or creates a logical vector we can use the expression directly to obtain a vector of all of the values which match the indices of the TRUE values in the logical vector.

~~~
ages_gt_50 <- ages[ages > 50]
ages_gt_50
~~~
{: .language-r}

~~~
[1]  60  65  82  55  63  72  56  57  78  89 100
~~~
{: .output}

The expression does not have to be as simple as the one we have used. All of the usual comparison operators are available.

>, <, >=, <= , ==, !=  

Notice the test for equality is '==' not judt '='

You can combine multiple tests using & (both conditions are true, AND) or | (at least one of the conditions is true, OR)

~~~
ages_60_to_90 <- ages[ages >= 60 & ages <= 90]
ages_60_to_90
~~~
{: .language-r}

~~~
[1] 60 65 82 63 72 78 89
~~~
{: .output}

A common task is to search for certain strings in a vector. One could use the “or” operator | to test for equality to multiple values, but this can quickly become tedious. The %in% operator (it is really a function called match) allows you to test if any of the elements of a search vector are found

~~~
possessions
~~~
{: .language-r}

~~~
[1] "washing machine" "bicycle"         "radio"           "television"    
~~~
{: .output}

~~~
electrical <- c('dish washer', 'hair dryer', 'Mobile Phone','television','washing machine')
electrical_possessions <- possessions[possessions %in% electrical]
electrical_possessions
~~~
{: .language-r}

~~~
[1] "washing machine" "television"
~~~
{: .output}

## Missing data

It is very common for datasets to have data missing. As R a language is designed for data analysis, it has to have ways of dealing with missing data.

In a vector Missing data is represented by 'NA' (Not Available)

Although the 'NA' are normally inserted automatically by R when we read data from a dataset, for demonstration purposes we can deliberately create a vector with missing values. This can be done directly using the 'c' function and specifying 'NA' (without the quotes) as an element value or in our
case we will modify and element of the ages vector

~~~
ages
~~~
{: .language-r}

~~~
[1]  50  60  65  82  55  63  25  72  56  57  78  89  21 100
~~~
{: .output}

~~~
ages[4] <- NA
ages
~~~
{: .language-r}

~~~
[1]  50  60  65  NA  55  63  25  72  56  57  78  89  21 100
~~~
{: .output}

Some numerical operations will effectively ignore missing values

~~~
ages <- ages + 1
ages
~~~
{: .language-r}

~~~
[1]  51  61  66  NA  56  64  26  73  57  58  79  90  22 101
~~~
{: .output}

When adding 1 to each element of the vector the results (i.e the values of the elements) are not effected by the NA element (adding 1 to NA is still NA)

However there are many other statistical functions where the overall result is affected by missing values.

~~~
mean(ages)
~~~
{: .language-r}

~~~
[1] NA
~~~
{: .output}

~~~
max(ages)
~~~
{: .language-r}

~~~
[1] NA
~~~
{: .output}

By default if any of the elements are NA then the result will be NA
We can overide this by setting the 'na.rm' parameter to TRUE

~~~
mean(ages, na.rm = TRUE)
~~~
{: .language-r}

~~~
[1] 61.84615
~~~
{: .output}

~~~
max(ages, na.rm = TRUE)
~~~
{: .language-r}

~~~
[1] 101
~~~
{: .language-r}

Now results are returned. `na.rm = TRUE` removes any NA values before the calculation is performed so the result of the mean calculation is based on 13 values in the vector not 14.

If your data include missing values, you may want to become familiar with the functions is.na(), na.omit(), and complete.cases(). See below for examples.

~~~
# Extract those elements which are not missing values.
ages[!is.na(ages)]
~~~
{: .language-r}

~~~
[1]  51  61  66  56  64  26  73  57  58  79  90  22 101
~~~
{: .output}

~~~
# Returns the object with incomplete cases removed. The returned object is an atomic vector of type `"numeric"` (or `"double"`).
na.omit(ages)
~~~
{: .language-r}

~~~
[1]  51  61  66  56  64  26  73  57  58  79  90  22 101
attr(,"na.action")
[1] 4
attr(,"class")
[1] "omit"
~~~
{: .output}

~~~
# Extract those elements which are complete cases. The returned object is an atomic vector of type `"numeric"` (or `"double"`).
ages[complete.cases(ages)]
~~~

~~~
[1]  51  61  66  56  64  26  73  57  58  79  90  22 101
~~~
{: .output}

All of the above will remove missing data. You need to take care when using them because in a dataset of data with many rows, only the rows without any missing data will be retained. It could bethat the missing data was only in element that you were not hoping to analyse anyway, so their presenece wouldn't be a problem.


## Replacing missing values

You can replace missing values in much the same way as we set the NA value. If you do this you need to be aware what future an analysis might be affected.

~~~
# create some missing values
ages[c(2,4,6)] <- NA
ages
~~~
{: .language-r}

~~~
[1]  51  NA  66  NA  56  NA  26  73  57  58  79  90  22 101
~~~
{: .output}

~~~
# get the mean of the non missing values
mean(ages, na.rm = TRUE)
~~~
{: .language-r}

~~~
[1] 61.72727
~~~
{: .output}

~~~
# set the missing values to 0
ages[is.na(ages)] <- 0
ages
~~~
{: .language-r}

~~~
[1]  51   0  66   0  56   0  26  73  57  58  79  90  22 101
~~~
{: .output}

~~~
# get the mean - the result is now based on 14 values 3 of which have been set to 0
mean(ages)
~~~
{: .language-r}

~~~
[1] 48.5
~~~
{: .output}

In some datasets you wish to analyse you may find the genuinely missing values have been replaced with values such as -99 or -999. In such cases you want to deliberately change these to NA and ten use the `na.rm = TRUE` option for you calculations.

> ## Exercise
> for the vector c(12,-99, 21, 34, -99, -99, 53, 32, 90, -99) calculate the mean of the 6 non-Missing values
>
>
> > ## Solution
> >
> > ~~~
> > test <- c(12,-99, 21, 34, -99, -99, 53, 32, 90, -99)    
> > test[test == -99] <- NA     
> > test     
> > mean(test,na.rm = TRUE)     
> > ~~~
> > {: .language-r}
> >
> {: .solution}
{: .challenge}
