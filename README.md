# **Purpose**

In this notebook session, I’ll be exploring different methods of
recoding variables in the R language. I intend to explore solutions in
base R, dplyr, and plyr packages.

------------------------------------------------------------------------

# **Recoding with dplyr**

One of the most used packages for data transformations in R is
[dplyr](https://dplyr.tidyverse.org/) from the
[tidyverse](https://www.tidyverse.org/). We can load in `dplyr` and the
`magrittr` (for the `%>%` operator) packages individually, or we can
chose to load the `tidyverse` packages to load them both in with other
packages.

<br>

``` r
#Load in tidyverse.
library(tidyverse)
```

<br>

## **Example Data-1**

To practice recoding with dplyr, let’s create a dataframe to practice on
and call it `example_data_1`. We’ll make it a
[tibble](https://tibble.tidyverse.org/) since we’re using dplyr.

<br>

``` r
#Setting a seed for reproducibility.
set.seed(99)

#Creating the example data frame.
example_data_1 <- tibble("ID" = c(1:10),
                         "Gender" = sample(0:3, 10, replace = TRUE),
                         "Levels" = sample(c("Low","Moderate","High"), 10, replace = TRUE),
                         "Medication Type" = sample(1:5, 10, replace = TRUE))
```

<br>

Which gives us…

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
</tbody>
</table>

<br>

## **case_when() function**

The dyplr package has a [`case_when`
function](https://dplyr.tidyverse.org/reference/case_when.html) that is
a “go-to” for most situations you’ll encounter! Let’s use it to recode
our `Medication Type` variable with the following codes:

<br>

<table style="width:30%; margin-left: auto; margin-right: auto;" class="table table-striped table-hover">
<thead>
<tr>
<th style="text-align:center;">
Medication Category
</th>
<th style="text-align:center;">
Code
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
Antidepressants
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
Anxiolytics
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
Stimulants
</td>
<td style="text-align:center;">
3
</td>
</tr>
<tr>
<td style="text-align:center;">
Antipsychotics
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
Mood Stabilizers
</td>
<td style="text-align:center;">
5
</td>
</tr>
</tbody>
</table>

<br>

We can apply the function by directly mutating the variable in the data
set with tidy code that utilizes magrittr’s pipe operator. You can read
more about the tidyverse style of coding like this
[here](https://style.tidyverse.org/pipes.html).

<br>

``` r
#Using tidyverse-styled code to mutate the "Medication Type" variable to recode the numbers into strings.
example_data_1 <- example_data_1 %>%
  mutate(`Medication Type` = case_when(`Medication Type` == 1 ~ "Antidepressants",
                                       `Medication Type` == 2 ~ "Anxiolytics",
                                       `Medication Type` == 3 ~ "Stimulants",
                                       `Medication Type` == 4 ~ "Antipsychotics",
                                       `Medication Type` == 5 ~ "Mood Stabilizers"))
```

<br>

In this block, we’re using the [`mutate`
function](https://dplyr.tidyverse.org/reference/mutate.html?q=mutate#useful-mutate-functions)
to change the `Medication Type` variable. It’s here that we apply the
`case_when` function to the variable to recode our numeric codes to
strings.

<br>

The result… <br>

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antidepressants
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Mood Stabilizers
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Mood Stabilizers
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Antidepressants
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
</tbody>
</table>

<br>

We can also use `case_when` to convert existing strings to numeric
values as well. We can test this out by recoding the `Medication Type`
variable back to numeric code…

<br>

``` r
#Using tidyverse-styled code to mutate the "Medication Type" variable back into numeric values.
example_data_1 <- example_data_1 %>%
  mutate(`Medication Type` = case_when(`Medication Type` == "Antidepressants" ~ 1,
                                       `Medication Type` == "Anxiolytics" ~ 2,
                                       `Medication Type` == "Stimulants" ~ 3,
                                       `Medication Type` == "Antipsychotics" ~ 4,
                                       `Medication Type` == "Mood Stabilizers" ~ 5))
```

<br>

Which gives us our original table back:

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
</tbody>
</table>

<br>

The `case_when()` function is great to use for more complex/conditional
recoding as well. Check out the tidyverse [reference
page](https://dplyr.tidyverse.org/reference/case_when.html) for more
possibilities/limitations!

<br>

------------------------------------------------------------------------

## **recode() function**

The dyplr package also has a [`recode()`
function](https://dplyr.tidyverse.org/reference/recode.html) that can be
useful. Let’s use it to recode our `Levels` variable with the following
codes:

<br>

<table style="width:30%; margin-left: auto; margin-right: auto;" class="table table-striped table-hover">
<thead>
<tr>
<th style="text-align:center;">
Levels Category
</th>
<th style="text-align:center;">
Code
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
3
</td>
</tr>
</tbody>
</table>

<br>

To do this, we can directly apply and assign the function to the
variable we wish to recode…

<br>

``` r
#Applying the recode function to the "Gender" variable in the data set.
example_data_1$Levels <- recode(example_data_1$Levels, 
                                "Low" = 1,
                                "Moderate" = 2,
                                "High" = 3)
```

``` r
#Similarly, we can use the tidyverse-style as well...
example_data_1 <- example_data_1 %>%
  mutate(Levels = recode(Levels,
                         "Low" = 1,
                         "Moderate" = 2,
                         "High" = 3))
```

<br>

Either way, we get the same result…

<br>

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
4
</td>
</tr>
</tbody>
</table>

<br>

> <img src="C:/Users/meghansa/Desktop/Website%20Stuff/TTscripts/Recoding_in_R/images/Be%20Careful.png" width="200" />
> <font color = "#040669">*This function only works when trying to
> convert **character/string** values*</font>

<br>

Note that if we try to use the `recode()` function to convert numeric
data types, and error will be thrown. We can see this by trying to
convert the `Levels` variable back into it’s string categories.

<br>

``` r
#Attempting to convert numeric data types with the "recode" function...
example_data_1 <- example_data_1 %>%
  mutate(Levels = recode(Levels,
                         1 = "Low",
                         2 = "Moderate",
                         3 = "High"))
```

    ## Error: <text>:4:28: unexpected '='
    ## 3:   mutate(Levels = recode(Levels,
    ## 4:                          1 =
    ##                               ^

<br>

We get an error telling us that ‘=’ was unexpected. This is because the
function is expecting a string input. If you really wanted to use the
`recode()` function with numeric types, you’d have to convert the
numbers to strings first and then run the function…

<br>

``` r
#Attempting to convert numeric data types with the "recode" function...
example_data_1 <- example_data_1 %>%
  mutate(Levels = recode(as.character(Levels),
                         "1" = "Low",
                         "2" = "Moderate",
                         "3" = "High"))
```

<br> Which gives us… <br>

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
</tbody>
</table>

<br> It’s important to note that as of 04/26/2021 the
[lifecycle](https://lifecycle.r-lib.org/articles/stages.html) for the
`recode()` function has a [**questioning**
status](https://lifecycle.r-lib.org/articles/stages.html#questioning)
because of the order in which the function takes in values. There is a
possibility a new function will be created to replace this one.
Additionally, there is a specific function for recoding factors as well
called `recode_factor()` that can be read about
[here](https://dplyr.tidyverse.org/reference/recode.html), although the
[`forcats` package](https://forcats.tidyverse.org/) could be used for
easier factor processing.

------------------------------------------------------------------------

## **if_else() function**

Although you can use `case_when` for cleaner code that conditionally
recodes variables, maybe you want to try dplyr’s version of
[`if_else()`](https://dplyr.tidyverse.org/reference/if_else.html). This
is a function that is similar to base R’s
[`ifelse()`](https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/ifelse)
but can be a bit faster and is more strict with the data types allowed
in the function. We can use it to recode variables as well. Let’s use it
recode the `ID` variable in our set with the following codes…

<br>

<table style="width:30%; margin-left: auto; margin-right: auto;" class="table table-striped table-hover">
<thead>
<tr>
<th style="text-align:center;">
IDs Category
</th>
<th style="text-align:center;">
Code
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
South Wing
</td>
<td style="text-align:center;">
1-3
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
4-7
</td>
</tr>
<tr>
<td style="text-align:center;">
North Wing
</td>
<td style="text-align:center;">
8-10
</td>
</tr>
</tbody>
</table>

<br>

We can use the `mutate()` function to conditionally recode the `ID`
variable with the following…

<br>

``` r
#Using the if_else function with mutate for conditional recoding.
example_data_1 <- example_data_1 %>%
  mutate(ID = if_else(ID <= 3, "South Wing",
                      if_else(ID > 3 & ID <= 7, "East Wing","North Wing")))
```

<br>

Which gives us this…

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
South Wing
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
South Wing
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
South Wing
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
5
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
North Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
North Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
North Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
4
</td>
</tr>
</tbody>
</table>

<br>

Note that conditional recoding like this can also be done in the
`case_when()` function we reviewed previously.

<br>

------------------------------------------------------------------------

# **Recoding with plyr **

<br>

## **mapvalues() function**

If we have vectors full of set values we want to use to recode with, we
can use the `mapvalues` function from the [plyr
package](https://www.rdocumentation.org/packages/plyr/versions/1.8.6).
Note that this method only works when the vector of the old values and
new values are of the same length. Let’s explore this by repeating the
recoding for the `Medication Type` variable. Let’s look at our codes
again for a reminder:

<br>

<table style="width:30%; margin-left: auto; margin-right: auto;" class="table table-striped table-hover">
<thead>
<tr>
<th style="text-align:center;">
Medication Category
</th>
<th style="text-align:center;">
Code
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
Antidepressants
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
Anxiolytics
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
Stimulants
</td>
<td style="text-align:center;">
3
</td>
</tr>
<tr>
<td style="text-align:center;">
Antipsychotics
</td>
<td style="text-align:center;">
4
</td>
</tr>
<tr>
<td style="text-align:center;">
Mood Stabilizers
</td>
<td style="text-align:center;">
5
</td>
</tr>
</tbody>
</table>

<br>

Let’s say we have a vector of the string medication categories. We can
remap the values in our original data set with the following…

<br>

``` r
#Creating a vector of medication categories.
med_cats <- c("Antidepressants","Anxiolytics","Stimulants","Antipsychotics","Mood Stabilizers")
med_codes <- 1:5

#If you're solely using plyr, you can load in the plyr package. Note that if you have dplyr loaded as well, you will get a warning that plyr is masking alot of functions in dplyr. In this case, it's best to use plyr functions by directly calling it's namespace. This is shown below:

#Using the mapvalues function for recoding.
example_data_1$`Medication Type` <- plyr::mapvalues(example_data_1$`Medication Type`,
                                                    from = med_codes,
                                                    to = med_cats)
```

    ## The following `from` values were not present in `x`: 3

<br>

Which results in…

<br>

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
South Wing
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antidepressants
</td>
</tr>
<tr>
<td style="text-align:center;">
South Wing
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
South Wing
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Mood Stabilizers
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Mood Stabilizers
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
<tr>
<td style="text-align:center;">
East Wing
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
North Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Antidepressants
</td>
</tr>
<tr>
<td style="text-align:center;">
North Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
<tr>
<td style="text-align:center;">
North Wing
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
</tbody>
</table>

<br>

Note that the first argument is the object you wish to change. The
second (`from=`) is a set of values that you wish to find within your
object to change, and the last argument (`to =`) is the set of values
you wish to replace with.

<br>

Let’s try to recode the `ID` variable back into numbers to test if we
can just use a vector with duplicate values as an input for the
`mapvalues` function. Because the values are duplicated, we can use the
`warn_missing` argument to prevent any warnings from printing to the
console. Let’s try this out by attempting to replace these values with a
range of integers…

<br>

``` r
#Using mapvalues with existing dataframe columns and number ranges.
example_data_1$ID <- plyr::mapvalues(example_data_1$ID, 
                                     from = example_data_1$ID,
                                     to = 1:10,
                                     warn_missing = FALSE)
```

<br>

These code ran without error, but let’s see the dataframe…

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antidepressants
</td>
</tr>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Mood Stabilizers
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Mood Stabilizers
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Antidepressants
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
</tbody>
</table>

<br>

Not what we were expecting. The `mapvalues()` function did recode our
variables, but it only applied the numeric values to each unique index
because we had duplicates. This is definitely a limitation of the
`mapvalues()` function. Regardless, it seems the `mapvalues` function
can be really convenient if you have a lot of values to recode as this
only requires creating a vector once to be used. It’s also important to
note that you can’t do conditional recoding with this unless you
transform your values first in the `from=` vector. Because of this,
`mapvalues` is good for quick basic recoding when vectors of unique
values are present or created for the purpose of recoding.

<br>

For instances like this when we just want to recode something into a
range of numbers it can simply be applied as such…

<br>

``` r
#Recoding ID variable simply with desired number ranges.
example_data_1$ID <- 1:10
```

## **revalue() function**

<br>

Another function that can be used from the plyr package is the
[`revalue()`
function.](https://www.rdocumentation.org/packages/plyr/versions/1.8.6/topics/revalue)
This function works to recode **character and factor vectors only**.
Because of this limitation, most using the plyr package for recoding
will opt for the `mapvalues()` function. The `revalue` function can be
useful if you’d like to incorporate a level of data validation if you
want to be sure that the data in question is in fact characters or
factors.

<br>

As an example, let’s try to convert the `Gender` variable with the
`revalue` function to it’s appropriate categories with the following
codes:

<br>

<table style="width:30%; margin-left: auto; margin-right: auto;" class="table table-striped table-hover">
<thead>
<tr>
<th style="text-align:center;">
Gender Category
</th>
<th style="text-align:center;">
Code
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
Transgender
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
3
</td>
</tr>
</tbody>
</table>

Because the input needs to be a character or factor, we can coerce the
`Gender` variable to fit this requirement. Let’s change it into a
character vector with the `as.character()` function:

<br>

``` r
#Using the revalues function for recoding.
example_data_1$Gender <- plyr::revalue(as.character(example_data_1$Gender),
                                       replace = c("0" = "Female", "1" = "Male", "2" = "Transgender", "3" = "Non-Binary"))
```

<br>

Which gives us…

<br>

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Levels
</th>
<th style="text-align:center;">
Medication Type
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antidepressants
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Mood Stabilizers
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Mood Stabilizers
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
Transgender
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Anxiolytics
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Antidepressants
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
Antipsychotics
</td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

# **Recoding with Base R**

Maybe you want to stay in base R and don’t want to deal with alternative
packages. Although the previously mentioned packages can help make
recoding efficient, they aren’t the only way.

<br>

## **Example Data-2**

We’ll create a second example dataframe for the rest of the notebook…

``` r
#Setting a seed for reproducibility.
set.seed(1234)

#Creating the example data frame.
example_data_2 <- data.frame("ID" = c(1:10), 
                           "Gender" = sample(0:3, 10, replace = TRUE),
                           "Illness" = sample(1:3, 10, replace = TRUE),
                           "Severity" = sample(c("Low","Moderate","High"), 10, replace = TRUE),
                           "Medications" = sample(0:1, 10, replace = TRUE))
```

<br>

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Illness
</th>
<th style="text-align:center;">
Severity
</th>
<th style="text-align:center;">
Medications
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
0
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
</tbody>
</table>

<br>

More often than not, we’ll see data like this where categorical
variables will be numerically coded. Depending on the analyses, we may
need to switch back and forth. Let’s recode the gender variable into
categories. In this example our codes are the following:

<br>

<table style="width:30%; margin-left: auto; margin-right: auto;" class="table table-striped table-hover">
<thead>
<tr>
<th style="text-align:center;">
Gender Category
</th>
<th style="text-align:center;">
Code
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
Transgender
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
3
</td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

## **Named Vectors**

We can recode our `Gender` variable with a named vector where we
directly give names to the values that are already present in our data
frame. Let’s call ours `Gender_Codes` and then directly apply it to our
`Gender` variable in our `example_data_2` data set…

<br>

``` r
#Creating the named vector for gender.
gender_codes <- c("Female" = 0, 
                  "Male" = 1,
                  "Transgender" = 2,
                  "Non-Binary" = 3)

#Applying the named vector to the gender variable in the original data set. Note how we convert the gender variable to a factor and then wrap the "names" function around everything.
example_data_2$Gender <- names(gender_codes[as.factor(example_data_2$Gender)])
```

Which gives us….

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Illness
</th>
<th style="text-align:center;">
Severity
</th>
<th style="text-align:center;">
Medications
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
Transgender
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
</tbody>
</table>

<br>

We’re able to do this by converting our original `Gender` variable to a
factor, subsetting it inside of our `gender_codes` vector and applying
the resulting names into the `Gender` variable.

<br> <br>

------------------------------------------------------------------------

## **Vector Indexing**

In base R we can recode variables with vector indexing. This approach
can be used if you have a few values that need to be recoded. For this
example, let’s recode the `Illness` variable with the following codes:

<table style="width:30%; margin-left: auto; margin-right: auto;" class="table table-striped table-hover">
<thead>
<tr>
<th style="text-align:center;">
Illness Category
</th>
<th style="text-align:center;">
Code
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
Bipolar I
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
2
</td>
</tr>
<tr>
<td style="text-align:center;">
Cyclothymia
</td>
<td style="text-align:center;">
3
</td>
</tr>
</tbody>
</table>

<br>

When looking at our data set, we actually see that we have no
observations with the value of `1` or “Bipolar 1” present in the set.
With that knowledge, we know that we only have to recode values `2` and
`3`…

<br>

``` r
#Accessing the "Illness" vector to convert 2's into "Bipolar II".
example_data_2$Illness[example_data_2$Illness == 2] <- "Bipolar II"

#Accessing the "Illness" vector to convert 3's into "Cyclothymia".
example_data_2$Illness[example_data_2$Illness == 3] <- "Cyclothymia"

#Note that trying to recode the value "1" will not result in any errors, even though there aren't any 1s present. This code will run.  
example_data_2$Illness[example_data_2$Illness == 1] <- "Bipolar I"
```

<br>

Our result…

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Illness
</th>
<th style="text-align:center;">
Severity
</th>
<th style="text-align:center;">
Medications
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Cyclothymia
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
1
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Cyclothymia
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
Transgender
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
1
</td>
</tr>
</tbody>
</table>

Vector indexing can be great in a pinch, but can get a bit messy the
more values you have. This approach also won’t let you know if any
values you’ve declared is not present in your data which could lead to
potential issues at some point.

<br>

------------------------------------------------------------------------

## **If-Else Statements**

In base R, we can also use if-else chains to recode variables. This code
can get messier the more values you have to recode. If this method is
used for recoding, it might be best to limit it to recoding two or three
values. For this example, let’s recode the `Medications` variable. Our
codes for this variable is the following:

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
Medications Category
</th>
<th style="text-align:center;">
Code
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
No
</td>
<td style="text-align:center;">
0
</td>
</tr>
<tr>
<td style="text-align:center;">
Yes
</td>
<td style="text-align:center;">
1
</td>
</tr>
</tbody>
</table>

<br>

To recode this variable, we can use an ifelse statement…

``` r
#Applying the if-else statement to the Medications variable.
example_data_2$Medications <- ifelse(example_data_2$Medications == 0,"No","Yes")
```

<br>

Which gives us….

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Illness
</th>
<th style="text-align:center;">
Severity
</th>
<th style="text-align:center;">
Medications
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
1
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
<tr>
<td style="text-align:center;">
2
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Cyclothymia
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
3
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
<tr>
<td style="text-align:center;">
4
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
<tr>
<td style="text-align:center;">
5
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
<tr>
<td style="text-align:center;">
6
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Cyclothymia
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
7
</td>
<td style="text-align:center;">
Transgender
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
8
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
9
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
10
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
</tbody>
</table>

<br>

The if-else statement here evaluates the `Medications` variable in the
`example_data_2` data set. For each `Medications` value that is `0`, R
will replace the value with “No”, otherwise, it will replace it with the
other values we’ve supplied, “Yes”. Theoretically, we can make if-else
chains as big as we want to account for more than two values, but this
isn’t recommended for a large amount of values as it can get messy.

Let’s use an if-else chain to recode the `ID` column’s numerical values
into spelled out characters of each number.

<br>

``` r
#Applying the if-else statement to the Medications variable.
example_data_2$ID <- ifelse(example_data_2$ID == 1,"one",
                     ifelse(example_data_2$ID == 2,"two",
                       ifelse(example_data_2$ID == 3,"three",
                         ifelse(example_data_2$ID == 4,"four",
                           ifelse(example_data_2$ID == 5,"five",
                             ifelse(example_data_2$ID == 6,"six",
                               ifelse(example_data_2$ID == 7,"seven",
                                 ifelse(example_data_2$ID == 8,"eight",
                                   ifelse(example_data_2$ID == 9,"nine","ten")))))))))
```

Which gives us….

<table class="table table-striped table-hover" style="width: auto !important; margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:center;">
ID
</th>
<th style="text-align:center;">
Gender
</th>
<th style="text-align:center;">
Illness
</th>
<th style="text-align:center;">
Severity
</th>
<th style="text-align:center;">
Medications
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center;">
one
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
<tr>
<td style="text-align:center;">
two
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Cyclothymia
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
three
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
<tr>
<td style="text-align:center;">
four
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
<tr>
<td style="text-align:center;">
five
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
<tr>
<td style="text-align:center;">
six
</td>
<td style="text-align:center;">
Non-Binary
</td>
<td style="text-align:center;">
Cyclothymia
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
seven
</td>
<td style="text-align:center;">
Transgender
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Low
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
eight
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
nine
</td>
<td style="text-align:center;">
Female
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
Moderate
</td>
<td style="text-align:center;">
No
</td>
</tr>
<tr>
<td style="text-align:center;">
ten
</td>
<td style="text-align:center;">
Male
</td>
<td style="text-align:center;">
Bipolar II
</td>
<td style="text-align:center;">
High
</td>
<td style="text-align:center;">
Yes
</td>
</tr>
</tbody>
</table>

<br> While something like this may work in a pinch, it’s not really
efficient to recode this way. This approach requires that you have
knowledge of what your data contains beforehand. If we had an `ID` value
of `11`, it would not have been caught by this if-else chain/ladder. You
can always add statements that would help you catch unknown values, but
there are more efficient ways to recode multiple variables. Some of
which have already been presented in this notebook.

<br>

> Fun Fact: If you ever need to convert numbers to words like this you
> can use the
> [`numbers_to_words`](https://rdrr.io/cran/xfun/man/numbers_to_words.html)
> function from the [`xfun`
> package](https://cran.r-project.org/web/packages/xfun/vignettes/xfun.html).
> Alternatively, if you ever want to convert numbers into words, you can
> try out the [`wordstonumbers`
> package](https://github.com/fsingletonthorn/words_to_numbers) by
> [fsingletonthorn over on Github](https://github.com/fsingletonthorn)!
