# Introduction to R

**Questions**
- What is an object?
- What is a function and how can we pass arguments to functions?
- How can values be initially assigned to variables of different data types?
- How can a vector be created What are the available data types?
- How can subsets be extracted from vectors?
- How does R treat missing values?
- How can we deal with missing values in R?

**Objectives**
- Assign values to objects in R.
- Learn how to name objects.
- Use comments to inform script.
- Solve simple arithmetic operations in R.
- Call functions and use arguments to change their default options.
- Inspect the content of vectors and manipulate their content.
- Subset and extract values from vectors.
- Analyze vectors with missing data.
- Define the following terms as they relate to R: object, vector, assign, call, function.

## Variable

In R, information is stored as variables. These variables can be numerical, sets, text, dates, and can be more complex like graphs and datasets (which will be covered later)

To store something as a variable, we can use either "<-" or "=". But most commonly used is the "<-", in general its good practice.

> NOTE: "<-" are not the same thing "->"


```R
temperature <- 27
```

"<-" is an assignment operator. It assigns the value(s) on the right to the object(s) on the left. The example above creates the symbol called temperature  and assigns it the numeric value of 27. Some R users would say “temperature  gets 27.” time_minutes is now a numeric vector with one element. Or you could say temperature  is a numeric vector, and the first element is the number 27.

When assigning a value to an object, R does not print anything to the console. You can force R to print the value by using parentheses OR by typing the object name


```R
#method 1
(temperature <- 27 )

#method 2
temperature
```




27






27



> NOTE: You can give your objects any name, but it is recommended to that the names be explicit and not too long. Here are some tips for assigning values:
    
    -Do not use names of functions that already exist in R: There are some names that cannot be used because they are the names of fundamental   functions in R (e.g., if, else, for, see here for a complete list. In general, even if it’s allowed, it’s best to not use other function names (e.g., c, T, mean, data, df, weights). If in doubt, check the help to see if the name is already in use.
      
    -R is case sensitive: age is different from Age and y is different from Y.

    -No blank spaces or symbols other than underscores: R users get around this in a couple of ways, either through capitalization (e.g. myData) or underscores (e.g. my_data). It’s also best to avoid dots (.) within an object name as in my.dataset. There are many functions in R with dots in their names for historical reasons, but dots have a special meaning in R (for methods) and other programming languages.

    -Do not begin with numbers or symbols: 2x is not valid, but x2 is.

    -Be descriptive, but make your variable names short: It’s good practice to be descriptive with your variable names. If you’re loading in a lot of data, choosing myData or x as a name may not be as helpful as, say, ebookUsage. Finally, keep your variable names short, since you will likely be typing them in frequently.

We can store strings and text as variables. This is done by using "". Store your first and last name as well as your favourit colour below in between the quotes.


```R
nameFirst <- " "
nameLast <- " "
colour <- " "
```

We may include variables in arithmetic expressions.  These variables need to have a value that was assigned previously by some assignment operation. R will substitute the variable name by the respective value when solving the expression that contain these variables.  If you include a variable that does not exist (e.g.  you mispelled the name),R will generate an error saying that it does not know the value of that variable


```R
tempFarh <- temperature * 9/5 +32
```


```R
#incorrect call
tempfarh
```


    Error in eval(expr, envir, enclos): object 'tempfarh' not found
    Traceback:



If you sometimes forget what variables you already used, you can use the ls() function to list all what is currently stored in memory. It will only give you the variable names and not the values stored. 


```R
ls()
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'colour'</li><li>'nameFirst'</li><li>'nameLast'</li><li>'temperature'</li><li>'tempFarh'</li></ol>




We can also change an object’s value by assigning it a new one. This overwrites the previous value without prompting you, so be careful! Also, assigning a value to one object does not change the values of other objects. 

Lets take a look at an example


```R
#first temperature set to 27 degrees Celsius
(temperature <- 27)

#convert to farhenheit
(tempFarh <- temperature * 9/5 +32)

#changing temperture to 88 degrees Celsius
(temperature <- 88)

```




27






80.6






88



If we run tempFarh, what will we get?
- 80.6
- 190.4
- Error
- No idea


```R
tempFarh
```




80.6



To correct this, we re-run the equation for the conversion. 


```R
(tempFarh <- temperature * 9/5 +32)
```




190.4



## Commenting

All programming languages allow the programmer to include comments in their code. To do this in R we use the # character. Anything to the right of the # sign and up to the end of the line is treated as a comment and will not be evaluated by R. You can start lines with comments or include them after any code on the line.

Comments are essential to helping you remember what your code does, and explaining it to others. Commenting code, along with documenting how data is collected and explaining what each variable represents, is essential to reproducible research


```R
temperatureMorning <- 23       #morning temperature taken at 7AM
temperatureAfternoon <- 33     #afternoon temperature taken at 1PM
temperatureEvening <-20        #evening temperature taken at 5PM
tempertureNight <- 5           #night temperature taken at 9PM

tempFarh <- temperature * 9/5 +32   #convert a Celcius temp to Fahrenhait
```

RStudio makes it easy to comment or uncomment a paragraph: after selecting the lines you want to comment, press at the same time on your keyboard Ctrl + Shift + C. If you only want to comment out one line, you can put the cursor at any location of that line (i.e. no need to select the whole line), then press Ctrl + Shift + C.


```R
temperatureMorning <- 23       #morning temperature taken at 7AM
temperatureAfternoon <- 33     #afternoon temperature taken at 1PM
temperatureEvening <-20        #evening temperature taken at 5PM
tempertureNight <- 5           #night temperature taken at 9PM

tempFarh <- temperature * 9/5 +32   #convert a Celcius temp to Fahrenhait
```

#### EXERCISE 2.1

Create two variables length and width and assign them any values. Create a third variable area and give it a value based on the current values of length and width.


```R
#Exercise 2.1
length <- 2.5
width <- 3.2
area <- length * width
area
```




8



What happens when you change the values of length and width with the code below?


```R
length <- 7
width <- 6
area
```




8



What happened here is that you did not update the area variable. You need to run the area equation again to reassign area to a new length and width.

## Functions and their arguments

R is a “functional programming language,” meaning it contains a number of functions you use to do something with your data. Functions are “canned scripts” that automate more complicated sets of commands. Many functions are predefined, or can be made available by importing R packages as we saw in the “Before We Start” lesson.

Call a function on a variable by entering the function into the console, followed by parentheses and the variables. A function usually gets one or more inputs called arguments. For example, if you want to take the sum of 3 and 4, you can type in sum(3, 4). In this case, the arguments must be a number, and the return value (the output) is the sum of those numbers. An example of a function call is:




```R
sum(3,4)
```




7



The function is.function() will check if an argument is a function in R. If it is a function, it will print TRUE to the console.

Functions can be nested within each other. For example, sqrt() takes the square root of the number provided in the function call. Therefore you can run sum(sqrt(9), 4) to take the sum of the square root of 9 and add it to 4.

Typing a question mark before a function will pull the help page up in the Navigation Pane in the lower right. Type ?sum to view the help page for the sum function. You can also call help(sum). This will provide the description of the function, how it is to be used, and the arguments.

In the case of sum(), the ellipses . . . represent an unlimited number of numeric elements.


```R
#?sum()
```


```R
is.function(sum)        # check to see if sum() is a function
sum(3, 4, 5, 6, 7)      # sum takes an unlimited number (. . .) of numeric elements

```




TRUE






25



Here are examples of some other frequently used functions:


```R
#sqrt(x) : Returns the square root of x
sqrt(543)

#log(x, base=y) : Takes the logarithm of x with base y; if base is not specified, returns the natural logarithm
log(12.3)
log(8, base=2)

#sin(x) :  returns sine of an angle NOTE: angles are in radians
sin(0.2)

```




23.3023603954621






2.50959926237837






3






0.198669330795061



Lets complicate things a little. You can use functions within functions, but be careful of your brackets! When possible declare your variable externally, then call on them in the funtion. This function here uses the unit circle to find the angle. We are using the coordinates

( $\frac{\sqrt{3}}{2}$ , $\frac{1}{2}$ ) 
This will return an result close to $\frac {\pi}{6}$ or 30$^{\circ}$

The point here is not to quiz you on the unit circle, but rather to show that it is best to break down complicated functions. 


```R
#method 1
a=0.5
b=sqrt(3)/2

sin( a/b  )

#method 2
sin( (0.5) / (sqrt(3)/2) )
```




0.545805614673243






0.545805614673243



## Arguments
Some functions take arguments which may either be specified by the user, or, if left out, take on a default value. However, if you want something specific, you can specify a value of your choice which will be used instead of the default. This is called passing an argument to the function.

For example, sum() takes the argument option na.rm. If you check the help page for sum (call ?sum), you can see that na.rm requires a logical (TRUE/FALSE) value specifying whether NA values (missing data) should be removed when the argument is evaluated.

By default, na.rm is set to FALSE, so evaluating a sum with missing values will return NA:


```R
sum(3, 4, NA)
```




&lt;NA&gt;



Even though we do not see the argument here, it is operating in the background, as the NA value remains. 3 + 4 + NA is NA.

But setting the argument na.rm to TRUE will remove the NA:


```R
sum(3, 4, NA, na.rm = TRUE)
```




7



It is very important to understand the different arguments that functions take, the values that can be added to those functions, and the default arguments. Arguments can be anything, not only TRUE or FALSE, but also other objects. Exactly what each argument means differs per function, and must be looked up in the documentation.

It’s good practice to put the non-optional arguments first in your function call, and to specify the names of all optional arguments. If you don’t, someone reading your code might have to look up the definition of a function with unfamiliar arguments to understand what you’re doing.

## Vectors

A vector is the most common and basic data type in R, and is pretty much the workhorse of R. A vector is a sequence of elements of the same type. Vectors can only contain “homogenous” data–in other words, all data must be of the same type. The type of a vector determines what kind of analysis you can do on it. For example, you can perform mathematical operations on numeric objects, but not on character objects.

We can assign a series of values to a vector using the c() function. c() stands for combine. If you read the help files for c() by calling help(c), you can see that it takes an unlimited . . . number of arguments.

For example we can create a vector of checkouts for a collection of books and assign it to a new object checkouts:


```R
checkouts <- c(25, 15, 18)
checkouts
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>25</li><li>15</li><li>18</li></ol>




A vector can also contain characters. For example, we can have a vector of the book titles (title) and authors (author):


```R
title <- c("Macbeth","Dracula","1984")
```

The quotes around “Macbeth”, etc. are essential here. Without the quotes R will assume there are objects called Macbeth and Dracula in the environment. As these objects don’t yet exist in R’s memory, there will be an error message.

There are many functions that allow you to inspect the content of a vector. length() tells you how many elements are in a particular vector:


```R
length(checkouts)  # print the number of values in the checkouts vector
```




3



An important feature of a vector, is that all of the elements are the same type of data. The function class() indicates the class (the type of element) of an object:


```R
class(checkouts)
class(title)
```




'numeric'






'character'



Type ?str into the console to read the description of the str function. You can call str() on an R object to compactly display information about it, including the data type, the number of elements, and a printout of the first few elements.


```R
str(checkouts)
str(title)
```

     num [1:3] 25 15 18
     chr [1:3] "Macbeth" "Dracula" "1984"


You can use the c() function to add other elements to your vector:


```R
author <- "Stoker"
author <- c(author, "Orwell") # add to the end of the vector
author <- c("Shakespeare", author)
author
```




<style>
.list-inline {list-style: none; margin:0; padding: 0}
.list-inline>li {display: inline-block}
.list-inline>li:not(:last-child)::after {content: "\00b7"; padding: 0 .5ex}
</style>
<ol class=list-inline><li>'Shakespeare'</li><li>'Stoker'</li><li>'Orwell'</li></ol>




In the first line, we create a character vector author with a single value "Stoker". In the second line, we add the value "Orwell" to it, and save the result back into author. Then we add the value "Shakespeare" to the beginning, again saving the result back into author.

We can do this over and over again to grow a vector, or assemble a dataset. As we program, this may be useful to add results that we are collecting or calculating.

Use this to open a tab in the Script Pane (upper left) to view your data. This is helpful if you have a very long vector you need to browse.

An **atomic vector** is the simplest R data type and is a linear vector of a single type. Above, we saw 2 of the 6 main atomic vector types that R uses: "character" and "numeric" (or "double"). These are the basic building blocks that all R objects are built from. The other 4 atomic vector types are:

- "logical" for TRUE and FALSE (the boolean data type)
- "integer" for integer numbers (e.g., 2L, the L indicates to R that it’s an integer)
- "complex" to represent complex numbers with real and imaginary parts (e.g., 1 + 4i) and that’s all we’re going to say about them
- "raw" for bitstreams that we won’t discuss further


You can check the type of your vector using the typeof() function and inputting your vector as the argument.

Vectors are one of the many data structures that R uses. Other important ones are lists (list), matrices (matrix), data frames (data.frame), factors (factor) and arrays (array).

#### Exercise 2.3



```R
#Exercise 2.3
```

You’ve probably noticed that objects of different types get converted into a single, shared type within a vector. In R, we call converting objects from one class into another class coercion. These conversions happen according to a hierarchy, whereby some types get preferentially coerced into other types. This hierarchy is: logical < integer < numeric < complex < character < list.

You can also coerce a vector to be a specific data type with as.character(), as.logical(), as.numeric, etc. For example, to coerce a number to a character:


```R
x <- as.character(200)
x
```




'200'



We can also call class()


```R
class(x)
```




'character'



> NOTE : if we try to add a number to x, we will get an error message non-numeric argument to binary operator–in other words, x is non-numeric and cannot be added to a number.

## Removing Objects from the environment

To remove an object from your R environment, use the rm() function. Remove multiple objects with rm(list = c("add", "objects", "here)), adding the objects in c() using quotation marks. To remove all objects, use rm(list = ls()) or click the broom icon in the Environment Pane, next to “Import Dataset.”


```R
x <- 5
y <- 10
z <- 15
rm(x)  # remove x
rm(list =c("y", "z"))  # remove y and z
rm(list = ls())  # remove all objects
```







