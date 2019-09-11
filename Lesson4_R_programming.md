---
title: "Software Carpentry Workshop - Lesson4_R_programming"
author: "Software Carpentry"
date: "September 14, 2019"
output: html_document
---
Software Carpentry Workshop: Lesson4: R programming
===
SCW workshop, September 2019  
Instructor: Balan Ramesh  
Adapted from [Software Carpentry](https://swcarpentry.github.io/r-novice-inflammation/02-func-R/index.html)

Time: 1.5 hours  

**Lesson Overview**  

We have covered basic R usage:
- Reading data files
- Creating and manipulating variables
- Data types
- Calling built-in functions

Now we will cover answers to 3 specific questions. They are as follows.

questions:
- "How can I repeat several operations with a single command in R?"
- "How can I make data-dependent choices in R?"
- "How can I repeat operations in R?"

objectives:
- "Write user-defined functions with `function()`"
- "Write conditional statements with `if()` and `else()`."
- "Write and understand `for()` loops."

keypoints:
- "Use `function()` to automate specific tasks."
- "Use `if` and `else` to make choices."
- "Use `for` to repeat operations."

# Creating Functions

Functions are a list of operations/commands that automate something complicated or convenient or both. A function usually gets one or more inputs called arguments. Functions often (but not always) return a value. An example of a function would be the `sqrt()` function. The input (the argument) must be a number, and the output (the return value) is the square root of the input number. Executing a function (‘running it’) is called `calling the function`. An example of a function call is:

`b <- sqrt(a)`

Functions provide:
- a name we can remember and invoke it by
- relief from the need to remember the individual operations
- a defined set of inputs and expected outputs
- rich connections to the larger programming environment

Let’s create a new script, call it `functions_lesson.R`, and write some examples of our own. Let’s define a function `fahrenheit_to_celsius` that converts temperatures from Fahrenheit to Kelvin:

```r
fahrenheit_to_celsius <- function(temp) {
  celsius <- ((temp - 32) * (5/9))
  return(celsius)
}
```

We define `fahrenheit_to_celsius` by assigning it to the output of `function`. The list of argument names are contained within parentheses. Next, the body of the function–the statements that are executed when it runs–is contained within curly braces (`{}`). The statements in the body are indented by two spaces, which makes the code easier to read but does not affect how the code operates.

When we call the function, the values we pass to it are assigned to those variables so that we can use them inside the function. The last line within the function is what R will evaluate as a returning value. For example, let’s try running our function. Calling our own function is no different from calling any other function:


```r
fahrenheit_to_celsius(32)

## [1] 0

fahrenheit_to_celsius(212)
## [1] 100
```

We’ve successfully called the function that we defined, and we have access to the value that we returned.

**Composing Functions**  
Now that we’ve seen how to turn Fahrenheit into Celsius, it’s easy to turn Celsius to Kelvin:
```r
celsius_to_kelvin <- function(temp_C) {
  temp_K <- temp_C + 273.15
  return(temp_K)
}
```

```r
# Freezing Point of water
celsius_to_kelvin(0)
```

```
## [1] 273.15"
```

What about converting Fahrenheit to Kelvin? We could write out the formula, but we don’t need to. Instead, we can compose the two functions we have already created:
```r
fahrenheit_to_kelvin <- function(temp_F) {
  temp_C <- fahrenheit_to_celsius(temp_F)
  temp_K <- celsius_to_kelvin(temp_C)
  return(temp_K)
}
```

```r
# Freezing point of water in Kelvin
fahrenheit_to_kelvin(32)
```

```
## [1] 273.15
```
This is our first taste of how larger programs are built: we define basic operations, then combine them in ever-large chunks to get the effect we want. Real-life functions will usually be larger than the ones shown here—typically half a dozen to a few dozen lines—but they shouldn’t ever be much longer than that, or the next person who reads it won’t be able to understand what’s going on.

## Challenge 1
In the last lesson, we learned to combine elements into a vector using the `c` function, e.g. `x <- c("A", "B", "C")` creates a vector `x` with three elements. Furthermore, we can extend that vector again using `c`, e.g. `y <- c(x, "D")` creates a vector y with four elements. Write a function called `highlight` that takes two vectors as arguments, called `content` and `wrapper`, and returns a new vector that has the wrapper vector at the beginning and end of the content:


Example function call and output:
```shell=
best_practice <- c("Write", "programs", "for", "people", "not", "computers")
asterisk <- "***"  # R interprets a variable with a single value as a vector
                   # with one element.
highlight(best_practice, asterisk)
```

```
[1] "***"       "Write"     "programs"  "for"       "people"    "not"      
[7] "computers" "***"  
```
**Solution**

```shell=
highlight <- function(content, wrapper) {
  answer <- c(wrapper, content, wrapper)
  return(answer)
}
```
```
[1] "***"       "Write"     "programs"  "for"       "people"    "not"      
[7] "computers" "***"  
```

Let's say we have the following problem:

> In the gapminder dataset, which continents have the mean life expectancy smaller or larger than 50 years?

How would we approach such a problem? What are the steps that are needed? The concepts in the next sections will help us breakdown this problem into smaller bits and eventually solve it. I want to you think about the concept we learn and how that may be applicable to this problem as we move along the lesson.

# if...else statements

Often when we're coding we want to control the flow of our actions. This can be done
by setting actions to occur only if a condition or a set of conditions are met.
Alternatively, we can also set an action to occur a particular number of times.

There are several ways you can control flow in R.
For conditional statements, the most commonly used approaches are the constructs:

```r
# if
if (condition is true) {
  perform action
}

# if ... else
if (condition is true) {
  perform action
} else {  # that is, if the condition is false,
  perform alternative action
}
```
Say, for example, that we want R to print a message if a variable `x` has a particular value:

```r
x <- 8

if (x >= 10) {
  print("x is greater than or equal to 10")
}

x
```

```
8
```
The print statement does not appear in the console because x is not greater than 10. To print a different message for numbers less than 10, we can add an `else` statement.

```r
x <- 8

if (x >= 10) {
  print("x is greater than or equal to 10")
} else {
  print("x is less than 10")
}
```

You can also test multiple conditions by using `else if`.

```r
x <- 8

if (x >= 10) {
  print("x is greater than or equal to 10")
} else if (x > 5) {
  print("x is greater than 5, but less than 10")
} else {
  print("x is less than 5")
}
```

Important: when R evaluates the condition inside `if()` statements, it is looking for a logical element, i.e., `TRUE` or `FALSE`. This can cause some headaches for beginners. For example:

```r
x  <-  4 == 3
if (x) {
  "4 equals 3"
} else {
  "4 does not equal 3"
}
```

```r
[1] "4 does not equal 3"
```

As we can see, the not equal message was printed because the vector `x` is `FALSE`.

```
x <- 4 == 3
x
```

```
[1] FALSE
```

## Challenge 2
Use an `if()` statement to print a suitable message reporting whether there are any records from 2002 in the `gapminder` dataset. Now do the same for 2012.

*Hint: Use any() if you see an error*
**Solution**
```r
if(any(gapminder$year == 2002)){
   print("Record(s) for the year 2002 found.")
}
```

> Do you think we can apply `if` and `else` to our problem?

# Repeating operations

If you want to iterate over a set of values, when the order of iteration is important, and perform the same operation on each, a `for()` loop will do the job. We saw `for()` loops in the shell lessons earlier. This is the most flexible of looping operations, but therefore also the hardest to use correctly. Avoid using `for()` loops unless the order of iteration is important: i.e. the calculation at each iteration depends on the results of previous iterations.

The basic structure of a `for()` loop is:

```r
for(iterator in set of values){
  do a thing
}
```

```r
for(i in 1:10){
  print(i)
}
```

```r
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
[1] 6
[1] 7
[1] 8
[1] 9
[1] 10
```
The 1:10 bit creates a vector on the fly; you can iterate over any other vector as well.

For example, in our previous R lesson we had a `myorder_df`. We could iterate over each `menuItem` and display its cost.

```r
menuItems<-c("chicken", "soup", "salad", "tea")  
menuType<-factor(c("solid", "liquid", "solid", "liquid"))  
menuCost<-c(4.99, 2.99, 3.29, 1.89)  
myorder_df<-data.frame(menuItems, menuType, menuCost)

for (items in myorder_df$menuItems){
  myorder_df_subset <-  myorder_df[myorder_df$menuItems == items,]
  print(items)
  print(myorder_df_subset$menuCost)
}
```
> Now, you are equipped with all things necessary to solve our problem.

## Challenge 3

Write a script that loops through the `gapminder` data by continent and prints out whether the mean life expectancy is smaller or larger than 50 years.

**Solution**

```r
gapminder <- read.table("Data/gapminder.txt")

thresholdValue <- 50

for( Continent in unique(gapminder$continent) ){
   tmp <- mean(gapminder[gapminder$continent == Continent, "lifeExp"])

   if(tmp < thresholdValue){
       cat("Average Life Expectancy in", Continent, "is less than", thresholdValue, "\n")
   }
   else{
       cat("Average Life Expectancy in", Continent, "is greater than", thresholdValue, "\n")
        } # end if else condition
   rm(tmp)
   } # end for loop
```


## Key Points
- Define a function using `function()`.
- The body of a function should be indented.
- Call a function using `name_of_the_function()`.
- Return the result using the `return` statement.
- Use `if` and `else` to make choices.
- Use `for` to repeat operations.
