## Subsetting Vectors

If we want to subset (or extract) one or several values from a vector, we must provide one or several indices in square brackets. For this example, we will use the state data, which is built into R and includes data related to the 50 states of the U.S.A. Type ?state to see the included datasets. state.name is a built in vector in R of all U.S. states:


```R
state.name
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Alabama'</li><li>'Alaska'</li><li>'Arizona'</li><li>'Arkansas'</li><li>'California'</li><li>'Colorado'</li><li>'Connecticut'</li><li>'Delaware'</li><li>'Florida'</li><li>'Georgia'</li><li>'Hawaii'</li><li>'Idaho'</li><li>'Illinois'</li><li>'Indiana'</li><li>'Iowa'</li><li>'Kansas'</li><li>'Kentucky'</li><li>'Louisiana'</li><li>'Maine'</li><li>'Maryland'</li><li>'Massachusetts'</li><li>'Michigan'</li><li>'Minnesota'</li><li>'Mississippi'</li><li>'Missouri'</li><li>'Montana'</li><li>'Nebraska'</li><li>'Nevada'</li><li>'New Hampshire'</li><li>'New Jersey'</li><li>'New Mexico'</li><li>'New York'</li><li>'North Carolina'</li><li>'North Dakota'</li><li>'Ohio'</li><li>'Oklahoma'</li><li>'Oregon'</li><li>'Pennsylvania'</li><li>'Rhode Island'</li><li>'South Carolina'</li><li>'South Dakota'</li><li>'Tennessee'</li><li>'Texas'</li><li>'Utah'</li><li>'Vermont'</li><li>'Virginia'</li><li>'Washington'</li><li>'West Virginia'</li><li>'Wisconsin'</li><li>'Wyoming'</li></ol>





```R
state.name[1]     #first element
state.name[13]    #13th element
```




'Alabama'






'Illinois'



What would happen if your index was out of bounds?


```R
state.name[51]
```




NA



You can use the : colon to create a vector of consecutive numbers.


```R
state.name[1:5]     #first 5 elements
state.name[6:20]    #elements 6-20 
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Alabama'</li><li>'Alaska'</li><li>'Arizona'</li><li>'Arkansas'</li><li>'California'</li></ol>







<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Colorado'</li><li>'Connecticut'</li><li>'Delaware'</li><li>'Florida'</li><li>'Georgia'</li><li>'Hawaii'</li><li>'Idaho'</li><li>'Illinois'</li><li>'Indiana'</li><li>'Iowa'</li><li>'Kansas'</li><li>'Kentucky'</li><li>'Louisiana'</li><li>'Maine'</li><li>'Maryland'</li></ol>




If the numbers are not consecutive, you must use the c() function:


```R
state.name[c(1, 10, 20)]
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Alabama'</li><li>'Georgia'</li><li>'Maryland'</li></ol>




We can also repeat the indices to create an object with more elements than the original one:


```R
state.name[c(1, 2, 3, 2, 1, 3)]
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Alabama'</li><li>'Alaska'</li><li>'Arizona'</li><li>'Alaska'</li><li>'Alabama'</li><li>'Arizona'</li></ol>




> NOTE : R indices start at 1. Programming languages like Fortran, MATLAB, Julia, and R start counting at 1, because that’s what human beings typically do. Languages in the C family (including C++, Java, Perl, and Python) count from 0 because that’s simpler for computers to do.

## Conditional subsetting
Another common way of subsetting is by using a logical vector. TRUE will select the element with the same index, while FALSE will not:

The example below is greating a vector that contains the first 5 states ("Alabama","Alaska","Arizona","Arkansas","California"). The line below has changed the logical


```R
five_states <- state.name[1:5]
five_states[c(TRUE, FALSE, TRUE, FALSE, TRUE)]
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Alabama'</li><li>'Arizona'</li><li>'California'</li></ol>




Typically, these logical vectors are not typed by hand, but are the output of other functions or logical tests. state.area is a vector of state areas in square miles. We can use the < operator to return a logical vector with TRUE for the indices that meet the condition:


```R
state.area < 10000
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>TRUE</li><li>TRUE</li><li>FALSE</li><li>FALSE</li><li>TRUE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>TRUE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>TRUE</li><li>TRUE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>TRUE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>TRUE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li><li>FALSE</li></ol>





```R
state.area[state.area < 10000]
```

The first expression gives us a logical vector of length 50, where TRUE represents those states with areas less than 10,000 square miles. The second expression subsets state.name to include only those names where the value is TRUE.

You can also specify character values. state.region gives the region that each state belongs to:


```R
state.region == "Northeast"
```


```R
state.name[state.region == "Northeast"]
```

Again, a TRUE/FALSE index of all 50 states where the region is the Northeast, followed by a subset of state.name to return only those TRUE values.

Sometimes you need to do multiple logical tests (think Boolean logic). You can combine multiple tests using | (at least one of the conditions is true, OR) or & (both conditions are true, AND). Use help(Logic) to read the help file.


```R
state.name[state.area < 10000 | state.region == "Northeast"]
```


```R
state.name[state.area < 10000 & state.region == "Northeast"]
```

The first result includes both states with fewer than 10,000 sq. mi. and all states in the Northeast. New York, Pennsylvania, Delaware and Maine have areas with greater than 10,000 square miles, but are in the Northeastern U.S. Hawaii is not in the Northeast, but it has fewer than 10,000 square miles. The second result includes only states that are in the Northeast and have fewer than 10,000 sq. mi.

R contains a number of operators you can use to compare values. Use help(Comparison) to read the R help file. Note that two equal signs (==) are used for evaluating equality (because one equals sign (=) is used for assigning variables).

A common task is to search for certain strings in a vector. One could use the “or” operator | to test for equality to multiple values, but this can quickly become tedious. The function %in% allows you to test if any of the elements of a search vector are found:


```R
west_coast <- c("California", "Oregon", "Washington")
state.name[state.name == "California" | state.name == "Oregon" | state.name == "Washington"]
```


```R
state.name %in% west_coast
```


```R
state.name[state.name %in% west_coast]
```

## Missing Data

As R was designed to analyze datasets, it includes the concept of missing data (which is uncommon in other programming languages). Missing data are represented in vectors as NA. R functions have special actions when they encounter NA.

When doing operations on numbers, most functions will return NA if the data you are working with include missing values. This feature makes it harder to overlook the cases where you are dealing with missing data. As we saw above, you can add the argument na.rm=TRUE to calculate the result while ignoring the missing values.


```R
rooms <- c(2, 1, 1, NA, 4)
mean(rooms)
```


```R
max(rooms)
```


```R
mean(rooms, na.rm = TRUE)
```


```R
max(rooms, na.rm = TRUE)
```

If your data include missing values, you may want to become familiar with the functions is.na(), na.omit(), and complete.cases(). See below for examples.


```R
## Use any() to check if any values are missing
any(is.na(rooms))
```


```R
## Use table() to tell you how many are missing vs. not missing
table(is.na(rooms))
```


```R
## Identify those elements that are not missing values.
complete.cases(rooms)
```


```R
## Identify those elements that are missing values.
is.na(rooms)
```


```R
## Extract those elements that are not missing values.
rooms[complete.cases(rooms)]
```

You can also use !is.na(rooms), which is exactly the same as complete.cases(rooms). The exclamation mark indicates logical negation.


```R
!c(TRUE, FALSE)
```

How you deal with missing data in your analysis is a decision you will have to make–do you remove it entirely? Do you replace it with zeros? That will depend on your own methodological questions.

## Key Points
    - Use the assignment operator <- to assign values to objects. You can now manipulate that object in R
    - R contains a number of functions you use to do something with your data. Functions automate more complicated sets of commands. Many functions are predefined, or can be made available by importing R packages
    - A vector is a sequence of elements of the same type. All data in a vector must be of the same type–character, numeric (or double), integer, and logical. Create vectors with c(). Use [ ] to subset values from vectors.


```R

```
