# Software Carpentry Workshop

## Lesson4: R programming
Adapted from Software Carpentry's [Functions](https://swcarpentry.github.io/r-novice-inflammation/02-func-R/index.html) and [Control Flow](http://swcarpentry.github.io/r-novice-gapminder/07-control-flow/index.html)

#### Please make sure your directory structure is setup as described [here](https://github.com/uta-carpentries/SoftwareCarpentryWorkshops_general/blob/master/Data_DirectoryStructure_Setup.md)

## Introduction
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

### Objectives:
- "Write user-defined functions with `function()`"
- "Write conditional statements with `if()` and `else()`."
- "Write and understand `for()` loops."

### Keypoints:
- "Use `function()` to automate specific tasks."
- "Use `if` and `else` to make choices."
- "Use `for` to repeat operations."

## Setup

1. Open RStudio and set Lesson4_ProgrammingR as your working directory.
2. Copy the gapminder dataset from `Data` folder.
3. Clear your history in R enviroment removing all the variables that your created previously.
4. Open a new R script and read the file as `gapminder <- read.table("gapminder.txt", header = TRUE)`
___

## Creating Functions

Functions are a list of operations/commands that automate something complicated or convenient or both. A function usually gets one or more inputs called arguments. Functions often (but not always) return a value. An example of a function would be the `sqrt()` function. The input (the argument) must be a number, and the output (the return value) is the square root of the input number. Executing a function (‘running it’) is called `calling the function`. An example of a function call is:

`b <- sqrt(a)`

Functions provide:
- a name we can remember and invoke it by
- relief from the need to remember the individual operations
- a defined set of inputs and expected outputs
- rich connections to the larger programming environment

Let’s create a new script, call it `functions_lesson.R`, and write some examples of our own. Let’s define a function `fahrenheit_to_celsius` that converts temperatures from Fahrenheit to Kelvin:

**Input**
```r
fahrenheit_to_celsius <- function(temp) {
  celsius <- ((temp - 32) * (5/9))
  return(celsius)
}
```

We define `fahrenheit_to_celsius` by assigning it to the output of `function`. The list of argument names are contained within parentheses. Next, the body of the function–the statements that are executed when it runs–is contained within curly braces (`{}`). The statements in the body are indented by two spaces, which makes the code easier to read but does not affect how the code operates.

When we call the function, the values we pass to it are assigned to those variables so that we can use them inside the function. The last line within the function is what R will evaluate as a returning value. For example, let’s try running our function. Calling our own function is no different from calling any other function:

**Input**
```r
fahrenheit_to_celsius(32)
```

**Output**  

```## [1] 0```  

**Input**  

```fahrenheit_to_celsius(212)```

**Output**  

```## [1] 100 ```

We’ve successfully called the function that we defined, and we have access to the value that we returned.

**Composing Functions**  
Now that we’ve seen how to turn Fahrenheit into Celsius, it’s easy to turn Celsius to Kelvin:

**Input**  

```r
celsius_to_kelvin <- function(temp_C) {
  temp_K <- temp_C + 273.15
  return(temp_K)
}
```
**Input**
```r
# Freezing Point of water
celsius_to_kelvin(0)
```
**Output**
```
## [1] 273.15"
```

What about converting Fahrenheit to Kelvin? We could write out the formula, but we don’t need to. Instead, we can compose the two functions we have already created:

**Input**
```r
fahrenheit_to_kelvin <- function(temp_F) {
  temp_C <- fahrenheit_to_celsius(temp_F)
  temp_K <- celsius_to_kelvin(temp_C)
  return(temp_K)
}
```
**Input**
```r
# Freezing point of water in Kelvin
fahrenheit_to_kelvin(32)
```
**Output**
```
## [1] 273.15
```
This is our first taste of how larger programs are built: we define basic operations, then combine them in ever-large chunks to get the effect we want. Real-life functions will usually be larger than the ones shown here—typically half a dozen to a few dozen lines—but they shouldn’t ever be much longer than that, or the next person who reads it won’t be able to understand what’s going on.

### Challenge 1
Create a function `MeanlifeExp()`, that takes a continent as its argument and returns the mean life expectancy of that continent. For example `MeanlifeExp("Europe")` returns `71.90369`



Example function call and output:
```r
MeanLifeExp("Europe")
```

```
[1] 71.90369
```
**Solution**

**Input**
```r
MeanLifeExp <- function(Continent) {
  Subset_Continent_LifeExp <- gapminder[gapminder$continent == Continent, "lifeExp"]
  lifeExp <- mean(Subset_Continent_LifeExp)
  return(lifeExp)
}
MeanLifeExp("Europe")
```
**Output**
```
[1] 71.90369
```

Let's say we have the following problem:

> In the gapminder dataset, which continents have the mean life expectancy smaller or larger than 50 years?

How would we approach such a problem? What are the steps that are needed? The concepts in the next sections will help us breakdown this problem into smaller bits and eventually solve it. I want to you think about the concept we learn and how that may be applicable to this problem as we move along the lesson.

## if...else statements

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

**Input**
```r
x <- 8

if (x >= 10) {
  print("x is greater than or equal to 10")
}

x
```
**Output**
```
8
```
The print statement does not appear in the console because x is not greater than 10. To print a different message for numbers less than 10, we can add an `else` statement.

**Input**
```r
x <- 8

if (x >= 10) {
  print("x is greater than or equal to 10")
} else {
  print("x is less than 10")
}
```

**Output**
```
[1] x is less than 10
```

You can also test multiple conditions by using `else if`.

**Input**
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
**Output**

```r
[1] x is greater than 5, but less than 10
```

Important: when R evaluates the condition inside `if()` statements, it is looking for a logical element, i.e., `TRUE` or `FALSE`. This can cause some headaches for beginners. For example:

**Input**
```r
x  <-  4 == 3
if (x) {
  "4 equals 3"
} else {
  "4 does not equal 3"
}
```
**Output**
```r
[1] "4 does not equal 3"
```

As we can see, the not equal message was printed because the vector `x` is `FALSE`.

**Input**
```
x <- 4 == 3
x
```
**Output**
```
[1] FALSE
```

### Challenge 2
Calculate the mean life Expectancy (using the previous user-defined function) of Asia. If the life expectancy is greater than or equal to 50, `print("Life Expectancy of Asia is greater than or equal to 50")`, if not `print("Life Expectancy of Asia is lower than 50")`

**Solution**


**Input**
```r
Asia_lifeExp <-  MeanLifeExp("Asia")
if(Asia_lifeExp >= 50){
  print("Life Expectancy of Asia is greater than or equal to 50")
} else {
  print("Life Expectancy of Asia is lower than 50")
}
```

**Output**
```r
[1] "Life Expectancy of Asia is greater than or equal to 50"
```
> Do you think we can apply `if` and `else` to our problem?

## Repeating operations

If you want to iterate over a set of values, when the order of iteration is important, and perform the same operation on each, a `for()` loop will do the job. We saw `for()` loops in the shell lessons earlier. This is the most flexible of looping operations, but therefore also the hardest to use correctly. Avoid using `for()` loops unless the order of iteration is important: i.e. the calculation at each iteration depends on the results of previous iterations.

The basic structure of a `for()` loop is:

```r
for(iterator in set of values){
  do a thing
}
```

**Input**
```r
for(i in 1:10){
  print(i)
}
```
**Output**
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

**Input**
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

**Output**
```r
[1] "chicken"
[1] 4.99
[1] "soup"
[1] 2.99
[1] "salad"
[1] 3.29
[1] "tea"
[1] 1.89
```

> Now, you are equipped with all things necessary to solve our problem.

### Challenge 3

Write a script that loops through the `gapminder` data by continent and prints out whether the mean life expectancy is smaller or larger than 50 years.

*Hint: Try unique() function to get unique values of a column*

**Solution**

```r
gapminder <- read.table("../Data/gapminder.txt", header = TRUE)

thresholdValue <- 50

continent_list <- unique(gapminder$continent)

for(continent in continent_list){

  continent_subset <- gapminder[gapminder$continent == continent, "lifeExp"]
  tmp <- mean(continent_subset)
  if(tmp <= thresholdValue){
    print(paste0("Average Life Expectancy in ", continent, " is less than ", thresholdValue))
  }
  else{
    print(paste0("Average Life Expectancy in ", continent, " is greater than ", thresholdValue))
  } # end if else condition
  rm(tmp)
}
```


## Key Points
- Define a function using `function()`.
- The body of a function should be indented.
- Call a function using `name_of_the_function()`.
- Return the result using the `return` statement.
- Use `if` and `else` to make choices.
- Use `for` to repeat operations.
