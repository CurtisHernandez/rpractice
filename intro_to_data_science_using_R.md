# rpractice

---
title: "Intro to Data Science Using R"
author: "Ellie"
date: "8/13/2018"
output:
  html_document: default
  pdf_document: default
---

8/13/18 - Morning Session


BASIC STUFF
Okay, so first up, we are going to open up the package "tidyverse."
```{r}
#install.packages("tidyverse")
```
"tidyverse" is part of a movement to standardize and quality-control a lot of R packages.  It's a bunch of packages that are all important for data science stuff.

Now open up "tidyverse" to be used in this session:
```{r}
library("tidyverse")
```

So R is kind of a super fancy calculator.
```{r}
3 + 7
10 * exp(3)
log(pi^2) #this is always natural log by default
```

But it can do other things too.
```{r}
hist(cars$dist)
```
This produces a histogram of...cars$dist!

You can store stuff (like output) in an 'object.' Generally you do this with a little arrow guy, like so.
```{r}
avg <- (5 + 7 + 6)/3
avg
```
Note that it lives in the "Environment" pane now.

You can do it with strings also!
```{r}
words <- 'I am pretty hungry and wish I had eaten more breakfast!'
words
```

There are five different kinds of object.  The first is an atomic vector - a set of elements that you have an order for.  You can make one with the c() function.
```{r}
x <- c(1, 3, 10, -20, sqrt(2))
y <- c("cat", "dog", "bird", "floor")
```
(Note that if you make an atomic vector with strings AND numbers, it just makes it all strings.  Like z <- c(1, "cat") outputs "1" "cat")

Many functions output atomic vectors.  LISTS of VALUES with an ORDER.
```{r}
1:20/20
seq(from = 1, to = 10, by = 2)
runif(4, min = 0, max = 1) #this is not "run if" but actually "random uniform"
```

How do I learn what a function does?  I ask R for help using help()!
```{r}
help(c) #hey look at the help screen in the bottom right
```

So atomic vectors aren't usually helpful in and of themselves, because they're just 1-dimensional, but they can build up others.  Matrices, for instance - a collection of vectors of the same type and length.
```{r}
x <- rep(0.2, times = 6) # repeat "0.2" six times
y <- c(1, 3, 4, -1, 5, 6)
# we first must check to make sure they can be combined into a matrix
is.numeric(x)
is.numeric(y)
# are they the same length?
length(x)
length(y)
# since x and y are both numeric vectors, and they're both 6 items long, we can combine them into a matrix
matrix(c(x, y), ncol = 2)  # or: make a matrix that binds together x and y in two columns! note - use ncol; the nrow argument makes it ALL weird unless you use byrow = TRUE
matrix(c(x, y), nrow = 2) # uh oh
matrix(c(x, y), nrow = 2, byrow = TRUE) # yay!
```

Matrices can also include character-based things!
```{r}
x <- c("Keanu", "Reeves")
y <- c("Carrie Anne", "Moss")
is.character(x)
is.character(y)
length(x)
length(y)
matrix(c(x,y), nrow = 2, byrow = TRUE)
```

So the problem is that oftentimes you have a dataset that needs to involve BOTH numeric AND character-based data.  You need to make a data frame.
```{r}
x <- c("a","b","c","d","e","f")
y <- c(1, 3, 4, -1, 5, 6)
z <- 10:15
blep <- data.frame(x, y, z)
blep
```
A data frame just decides the row-column orientation you get.  The number of columns is the number of vectors you're giving it, and the number of rows is the number of observations you have in all your vectors. (Which, again, has to be all the same number.) The names of the vectors become the column names.

Finally, we have lists, which are atomic vectors except you can do different elements of all kinds of types and lengths.  It's a vector of vectors!
```{r}
list("Hi", 1:3, rnorm(2), c("!","?")) # check out all the different lengths of things
```
It's more flexible than a data frame but it's also bananas.

THE WRAP-UP ACTIVITY FOR THE FIRST PART OF THE MORNING SESSION:
```{r}
# (1) Create and save a vector that contains the elements 4, 7, 10, 13. Call the object 'vec'.
vec <- c(4, 7, 10, 13)
# Note: When performing a standard mathematical operation on a vector in R each element of the vector has the mathematical operation applied to it.
# Add 8 to each element of vec.
vec + 8
# Square each element of vec.
vec^2
# Sum the elements of vec.
sum(vec)
```

```{r}
# (2) Create and save a vector of length 4 that consists of adjectives. Call the object 'adj'.
adj <- c("foamy","caffeinated","cold brew","delicious")
# Create and save a second vector of length 4 that consists of nouns. Call the object 'nouns'.
nouns <- c("espresso","macchiato","cappuccino","mocha latte")
# Run the following code: 
paste(adj, nouns)
```

```{r}
# (3) Create a matrix that has first column the adj values and the second column the nouns values.
matrix(c(adj,nouns),ncol=2)
```

```{r}
# (4) Create and save a data frame that has three columns given by vec, adj, and nouns. Call the object 'dataDF'.
dataDF <- data.frame(vec,adj,nouns)
dataDF
```



ATTRIBUTES AND BASIC DATA MANIPULATION
We want to use R to handle complex data sets.  There's a common data set called iris that we will use to learn about this stuff.

R can first off explain to you what kind of a dataset iris is.
```{r}
str(iris) #tells us the structure of the object.
```
Note: all the variables are numeric except for Species, which is a "factor", which is just another kind of thing we haven't talked about yet.  These are vectors that are really good for storing levels in an independent variable.  It also makes it easier to move stuff around.  Like if you just want to look at a control group, you can make a vector in a data frame or whatever into a factor and then sort everything by however you want.

We also have an attributes function that tells us metadata about the object.
```{r}
attributes(iris)
```

We might just want to get an individual part of our object.  Like just a column, just an attribute, etc.  Use square brackets when you build vectors that are made of subsets of your thing.
```{r}
letters #...is a built-in atomic vector that comes in
letters[5] # indexing starts at 1
letters[c(5,11,8)]
# you can also give it a vector that basically works as the indices of another vector
x <- c(5,11,8)
letters[x]
```

Same idea with matrices, but rows and columns.  Still gotta use square brackets.  Let's make just a toy matrix first.
```{r}
mat <- matrix(c(1:4,20:17),ncol=2)
mat
# same indexing convention as matlab - [row,column]
# the comma means "all of em"
# the colon, just as usual, means point A through to point B
mat[2,2] # note that it doesn't make it look like a column (UNLIKE matlab), it just prints you out a row
mat[,1]
mat[2,]
mat[2:4,1]
mat[c(2,4),] # this keeps the orientation tho
```

You can also name the dimensions if you want!  Just make sure you make a vector that has as many elements as there are elements in the dimension you're naming.  This is convenient because then you can select by the column name instead of the index.
```{r}
mat <-matrix(c(1:4,20:17), ncol=2, dimnames = list(NULL,c("First","Second")))
mat
mat[, "First"]
```

You can run attributes on matrices too. (Attributes themselves are things that aren't like, part of the data, like column names, etc.)
```{r}
str(mat)
attributes(mat)
```
Note that dimnames will be stored as a list - because you don't need to have the same number of columns and rows!  This indicates that double brackets [[]]s are lists.

You can access subsets of a data frame too:
```{r}
iris[c(1:4,6), 2:4]
# or
iris[,c("Sepal.Length","Species")]
# or!
iris$Sepal.Length # note that it returns a vector, not a nice column like it is in the data frame (the way MATLAB would)
```

How do you get to an element of a list?
```{r}
# first make a list and look at it
x <- list("HI", c(10:20), 1)
x
```
Note that it gives you double brackets indexing its elements!

Now you can have it return the vector that you have put in the list - AND even elements of that vector.
```{r}
x[[2]]
x[[2]][4:5]
```

You can also name the list elements, which will make them easier to call:
```{r}
x <- list(First = "HI", Second=c(10:20), Third = 1)
x$Second[1:2]
```

If you want to change the attributes of a thing!  You can apply the structure() function to the results of the attributes() function.  This tells us how R sees the information that it's printed on the screen after running attributes().
```{r}
str(attributes(iris))
```
So it's telling us that the attributes of iris are stored in a list of 3 vectors - names, class, and row.names. (Note that "names" is for column names, even though "row.names"" is its own thing.)

Say you just want to see the column names.
```{r}
attributes(iris)$names
attributes(iris)$names[1]
```

Now WHAT IF YOU WANT TO CHANGE AN ATTRIBUTE
```{r}
attributes(iris)$names[1] <- "Sepal_Length" #instead of Sepal.Length
attributes(iris)
```

A SHORTCUT EASIER WAY TO LOOK AT THE NAMES OF THINGS - the names() function. (This skips all the attributes() blah blah blah stuff.)
```{r}
names(iris)
names(iris)[2] <- "Sepal_Width"
names(iris)
```

THE WRAP-UP ACTIVITY OF THE SECOND PART OF THE MORNING SESSION:
```{r}
# Print out the element in the 1st row and 1st column of the dataDF object.
dataDF # just to take a look
dataDF[1,1]
```

```{r}
# Print out the 3rd row of the dataDF object.
dataDF[3,]
```

```{r}
# Print out the 'adj' column using three different methods.
dataDF$adj
dataDF[,2]
dataDF[2]
```

```{r}
# Print out the 1st and 4th rows of just the 'nouns' column of the dataDF object in one R command.
dataDF$nouns[c(1,4)]
```

```{r}
# Look at the structure of the dataDF object. Note that factors are special types of vectors - we'll cover them later.
str(dataDF)
```



8/13/18 - Afternoon Session


READING AND WRITING DATA

Data comes in many formats (delimited, fixed field [i.e. a character isn't delimiting something, your field is just X number of things long]).  We're going to cover some of them.
```{r}
#install.packages(c("readr", "readxl", "haven", "DBI", "httr"))
```
(The read_excel() function, from the readxl library, reads both xlsx and xls files.  Note that you have to tell it which sheet you want it to look at.  Then it makes a tibble.  Huzzah!  Also if you don't know the names of the sheets you can do use excel_sheets() to find what they are.)

Now that these things are downloaded, we just tell R that we're gonna need that package today. (You can also use a function known as require().)
```{r}
library("readr")
```

There are some ways to read in data that R just comes baked in with, but they're not the best.  The TidyVerse stuff is the best - it's quick, efficient, standardized, and cooperative.  So we'll load it in.
```{r}
library(tidyverse)
```

So how do I have it read in a file?  You can give it the full path name (C:/...), but it's easier if you just change your working directory with setwd().

Let's read in some sample data with read_csv().  (Note that read.csv() is a whole nother redundant guy.)
```{r}
scoreData <- read_csv(file = "https://raw.githubusercontent.com/jbpost2/DataScienceR/master/datasets/scores.csv")
```

How do we know it worked?  We could print it by just writing scoreData:
```{r}
scoreData
```
Note that this doesn't look the same way the iris dataset looked.  This is not a data frame, matrix, or list - those are base R things.  It's a tibble, a data object that tidyverse invented!  Well, it's also a data frame.  It's just got extra tidyverse things.  But this means that you can do data frame things to it.

You can read SPSS files and SAS files too - read_spss() and read_sas() respectively, both from the haven package (?).

R doesn't do a great job of writing to other proprietary structures (like into .sav files in SPSS or whatever) but it can write you a CSV with the write_csv() from the readr package.  NOTE that if you write the data back to the same file, it will overwrite unless you have "append = TRUE" in there.

THE WRAP-UP ACTIVITY FOR THE FIRST PART OF THE AFTERNOON SESSION:
```{r}
#Go to the gitHub datasets folder and look at the breastcancer.dat file. Click on the "raw" button.
#Read in the BreastCancer.dat data file using the URL you got for the gitHub page after clicking "raw". Note this is a tab delimited file. Save the R object as cancerData.
cancerData <- read_tsv("https://raw.githubusercontent.com/jbpost2/DataScienceR/master/datasets/BreastCancer.dat")
```

```{r}
#Repeat the above process to get a link to read in the mosquito.txt file. Save the R object as mosquitoData.
mosquitoData <- read_delim("https://raw.githubusercontent.com/jbpost2/DataScienceR/master/datasets/mosquito.txt", delim="&")
```

```{r}
#Repeat the above process to get a link to read in the mosquito2.txt file. Note this is a tab delimited file. Save the R object as mosquitoData2.
mosquitoData2 <- read_tsv("https://raw.githubusercontent.com/jbpost2/DataScienceR/master/datasets/mosquito2.txt")
```

```{r}
#Use the code below to create a combined version of the mosquito data sets. Save the object as mosquitoFullData.
mosquitoFullData <- rbind(mosquitoData, mosquitoData2)
#Write the mosquitoFullData out to a .csv file with name of your choice.
write_csv(mosquitoFullData, "mosquitos.csv")

```

```{r}
#Repeat the above process to get a link to read in the effort.dta file. Note that this is a stata file. Right click on view raw and copy the link address. Save the R object as effortData.
library("haven")
effortData <- read_stata("https://github.com/jbpost2/DataScienceR/blob/master/datasets/effort.dta?raw=true")
```


LOGICAL STATEMENTS AND CONDITIONALS AND STUFF

Similar to Python and Matlab and stuff.  Typical Boolean stuff:
```{r}
"hi" == " hi" #will evaluate to FALSE because there's a leading space
"hi" == "hi" #will evaluate to TRUE
# ! is "not"
# > is "greater than"
# >= is "greater than or equal to"
is.numeric("Word") # will evaluate to FALSE because it's not a number
is.character("Word") # will evaluate to TRUE because it's a string
is.na(c(1:2, NA, 3)) #will evaluate to FALSE FALSE TRUE FALSE because it's looking for "NA"s at each element. NA is missing data.  If you ever want to do an operation JUST on non-missing data you want to do this.
```

Let's go back and look at iris.  (Note that since we are in the tidyverse, we are recasting the data frame that is "iris" into a tibble.)
```{r}
iris <- tbl_df(iris)
iris
```

What if you just want to look for the setosa ones?  You can use a Boolean statement.
```{r}
#iris[iris$Species == "setosa", ] # in other words, for every entry in the Species column, say it's TRUE if it is "setosa" and FALSE if it's not, and just show me the TRUEs
# OR, you can also use some tidyverse packages like filter() from the dplyer package
filter(iris, Species == "setosa")
```

(Just a quick aside - if you need something to be of a character type or a numeric type or whatever, you can sometimes coerce it to be that way using stuff like as. functions - as.numeric(), as.character(), and so on.  Note that c() will coerce stuff itself, too.)

You also need ands and ors.  For &, both things have to be true.  For |, just one of them has to be true - and it's okay if it's both.  && are || are the same EXCEPT it's just looking at the first element in a vector.
```{r}
set.seed(3)
x <- runif(n = 10, min = 0, max = 1)
# if it's a single pipe I look at EVERY single one of them
(x < 0.25) | (x > 0.75)
# if it's a double pipe I just look at the first one
(x < 0.25) || (x > 0.75)
```

Let's say you want to pull out just large petal setosa flowers.
```{r}
# basically the filter() function creates an index for stuff based on whether observations meet criteria
filter(iris, (Petal.Length > 1.5) & (Petal.Width > 0.3) & Species=="setosa")
```

IF STATEMENTS look like this:

If (condition) {
  then execute code
}

IF ELSE statements look like this:
if (condition) {
  execute this code
} else {
  execute THIS code
}

If you wanna be more specific:

if (condition) {
  execute this code
} else if (condition2) {
  execute this other code
} else if (condition3) {
  execute this code
}
```{r}
a <- 5
if (a < 10){
  print("hi")
} else if (a < 40){
  print("goodbye") #note that since the first condition is met, you stop the else-if chain - for a(5), you're done with "hi"
} else {
  print("aloha")
}
```

See how it's different if it meets the second criterion but not the first?
```{r}
a <- 20
if (a < 10){
  print("hi")
} else if (a < 40){
  print("goodbye") #note that since the first condition is met, you stop the else-if chain - for a(5), you're done with "hi"
} else {
  print("aloha")
}
```

You can also use logicals to CREATE new variables. Note - the IF function doesn't go element-by-element, so you need to use a vectorized IF function.
```{r}
ifelse((iris$Petal.Length > 1.5) & (iris$Petal.Width > 0.3) & (iris$Species == "setosa"), "L-S", "NotL-S")
```

How do we add this into our old tibble?
```{r}
# take the old iris tibble and make a new variable called Size
# transmute is from dplry so make sure that's loaded in
transmute(iris, Size = ifelse((iris$Petal.Length > 1.5) & (iris$Petal.Width > 0.3) & (iris$Species == "setosa"), "LS", "NotLS"))
```

But this doesn't actually add it to the tibble.  Mutate() adds it to the existing data frame. (For the current session - you have to assign it to the variable if you want it to be in the tibble for the rest of the session.)
```{r}
mutate(iris, Size = ifelse((Petal.Length > 1.5) & (Petal.Width > 0.3) & (Species == "setosa"),"LS","NotLS"))
```



8/14/18 - Morning Session


MORE LOGICAL STATEMENTS AND DATA MANIPULATION

PIPIN N CHAININ -- If you have a nested function, sometimes it can make it so the argument is far away from the function.  So instead, you can use piping/chaining with teh %>% operator.
```{r}
# lemme set you up some fake data here
teamID = c(rep("PIT",times=6),rep("NOTPIT",times=7))
G = runif(13,0,1)
Batting <- data.frame(teamID,G)
# okay, so this
arrange(filter(Batting, teamID == "PIT"),desc(G))
# is the same as this
Batting %>% filter(teamID == "PIT") %>% arrange(desc(G))
# you can read the above as "First, do the filter step, THEN move on to the arrange step"
# this is more user-friendly because the arrange() function is not so far away from the argument desc(G)
```
To use the %>% operator, you need to have loaded dplyr (which I think is in tidyverse so okay)

Here's another example (that you'd never actually do but this is just to show off how %>% works):
```{r}
a<-runif(10)
# Let's find, like, say, the quantiles of a.
a %>% quantile()
# great okay. what if you wanted the range of the quantiles?
a %>% quantile() %>% range()
```

What about more pertinent examples?
```{r}
# lemme add on to your fake data here
Batting <- mutate(Batting, X2B = runif(13))
Batting <- mutate(Batting, X3B = runif(13))
Batting <- mutate(Batting, HR = runif(13))
# okay so you can look at all the columns between two columns
Batting %>% select(X2B:HR)
```

You can use pipin n chainin to make a new variable, too.
```{r}
Batting %>% mutate(ExtraBaseHits = X2B + X3B + HR)
```

transmute() just makes you a new variable.  mutate() lets you add that onto the tibble.  It depends if you need just a variable for your analysis or if you need a whole bunch more stuff.

What if you want to look some descriptive statistics?
```{r}
Batting %>% summarise(AvgX2B = mean(X2B, na.rm = TRUE)) # the na.rm removes missing values when you do the calculation, which yes, you do
# and if you just want to look at the descriptive statistics for a subset...!!!  (PIVOT TABLE)
Batting %>% group_by(teamID) %>% summarise(AvgX2B = mean(X2B, na.rm = TRUE)) #this group_by() has not been assigned to a variable so it won't stick around after this line
```

JOININ -- combining two data sets.  You will never need Excel again!

Here's two data sets.
```{r}
a <- data_frame(color = c("green", "yellow", "red"), num=1:3)
b <- data_frame(color = c("green", "yellow", "pink"), size = c("S", "M", "L"))
a
b
```

If you have an inner join, R will create a new data set that JUST matches up observations that are found in both a and b.
```{r}
inner_join(a,b) # THIS TOTALLY REPLACES =VLOOKUP IN EXCEL
```

A complete join takes every record even if there isn't a match and just leaves in <NA>s where there's no data for an observation x variable.
```{r}
full_join(a,b)
```

In a left join, you take every record from a and where available the match in b. Right join is the reverse.
```{r}
left_join(a,b)
right_join(a,b)
```

Note that you have to make sure that columns/variables need to have the same name across dfs in order to join.  So you can either rename the variables or you can add an argument to the join function.
```{r}
a <- data_frame(color = c("green", "yellow", "red"), num=1:3)
b <- data_frame(col = c("green", "yellow", "pink"), size = c("S", "M", "L"))
inner_join(a,b, by = c("color" = "col")) #i.e., "pretend that col is color for the purposes of this join"
```
The "by" argument in a join function can also tell you that you just want to match BY one given variable, but know that if you have multiple variables with the same name across data sets, one will overwrite the others.

THE WRAP-UP ACTIVITY FOR THE FIRST PART OF THE MORNING SESSION
```{r}
library(readr)
cancerData <- read_tsv(file = "https://raw.githubusercontent.com/jbpost2/DataScienceR/master/datasets/BreastCancer.dat")
```

```{r}
#Have R sort the data set by youngest to oldest.
cancerData %>% arrange(age)
```

```{r}
#Have R print out only women who were over 50.
cancerData %>% filter(age > 50)
```

```{r}
#Have R print out only women who were over 50 and premenopausal.
cancerData %>% filter(age > 50 & meno=="premenopausal")
```

```{r}
#Have R print out only women who were (over 50 and premenopausal) or (under 50 and Postmenopausal).
cancerData %>% filter((age > 50 & meno=="premenopausal") | (age < 50 & meno=="Postmenopausal"))
```

```{r}
#Have R find the average 'size' of the tumor for each of the previous three groups. You'll need one statement for each group to complete this.
cancerData %>% filter(age > 50) %>% summarise(mean(size),na.rm=TRUE)
cancerData %>% filter(age > 50 & meno=="premenopausal") %>% summarise(mean(size),na.rm=TRUE)
cancerData %>% filter((age > 50 & meno=="premenopausal") | (age < 50 & meno=="Postmenopausal")) %>% summarise(mean(size),na.rm=TRUE)
```

```{r}
#Have R create a new variable called 'flag' that is "flag" if the women were (over 50 and premenopausal) or (under 50 and Postmenopausal) and "no flag" if they were not. This variable should be added to the data frame.
#mutate(iris, Size = ifelse((Petal.Length > 1.5) & (Petal.Width > 0.3) & (Species == "setosa"),"LS","NotLS"))
cancerData <- mutate(cancerData, flag = ifelse((age > 50 & meno=="premenopausal") | (age < 50 & meno=="Postmenopausal"),"flag","no flag"))
```

```{r}
#Have R find the average 'size' of the tumor for the "flag" and "no flag" groups in one R statment.
cancerData %>% group_by(flag) %>% summarise(mean(size),na.rm=TRUE)
```

```{r}
#Have R find the number of women that were premenopausal and Postmenopausal, respectively. Hint use summarize with n() as your function: ex: summarize(count = n())
cancerData %>% group_by(meno) %>% summarise(count = n())
```

```{r}
#Create a new data frame called abbCancerData that only has the id, size, grade, and flag variables.
abbCancerData <- data_frame(cancerData$id, cancerData$size, cancerData$grade, cancerData$flag)
abbCancerData <- tbl_df(abbCancerData)
```


SUMMARIZIN

You can have categorical or numerical/quantitative data.
To summarize categorical data you can use contingency tables to see the frequency of stuff, also barplots.
```{r}
# let's first read in a data set about the Titanic
titanicData <- read_csv("https://raw.githubusercontent.com/jbpost2/DataScienceR/master/datasets/titanic.csv")
# you can use the table function to see the counts of things
table(titanicData$embarked)
table(titanicData$survived)
table(titanicData$sex)
# there are some issues with this because the data set we have doesn't always explain the variables - like, what is 0 in survived?  (probably they died)
```

You can create two-way tables too...
```{r}
table(titanicData$survived,titanicData$sex)
```

...and even three-way tables! (the order will dictate how you slice the series of two-way tables)
```{r}
table(titanicData$sex, titanicData$embarked, titanicData$survived)
```

And you can, of course, save all these summaries if you want.
```{r}
tab <- table(titanicData$sex, titanicData$survived)
str(tab)
```
Check it out - another kind of data object, tables.

Except it's an ARRAY, OH SHIT, WHICH MEANS YOU CAN USE OTHER DIMENSIONS BEYOND 2. And when you query it you're going to see a 2-way slice of certain variables (depending how you set it up).
```{r}
tab <- table(titanicData$sex, titanicData$survived, titanicData$embarked)
str(tab)
tab[1, ,] #females were the first
tab[2, ,] #males were the second
```

You COULD use barplot() (the base R function)...but you SHOULD use ggplot2() from tidyverse. It is IMPORTANT TO FIGURE OUT THE SYNTAX, THOUGH.
```{r}
# all R knows is the levels of the variable!  it doesn't know what kind of graph you're making or even the data you're looking at.
g <- ggplot(data = titanicData, aes(x = embarked))
# let's add a bar chart to it
g + geom_bar()
```

```{r}
# and we don't want NAs, so we fuss with the original data set with the drop_na() function
titanicDataNoNAs <- titanicData %>% drop_na(embarked)
g <- ggplot(data = titanicDataNoNAs, aes(x = embarked))
g + geom_bar()
```

```{r}
# and we can make laels too!
#titanicDataNoNAs <- titanicData %>% drop_na(embarked)
#g <- ggplot(data = titanicDataNoNAs, aes(x = embarked)) # note that it just looked at frequency data automatically.  stay tuned...
#g + geom_bar()
g + geom_bar() + labs(x = "City Embarked", title = "Bar Plot of Embarked City for Titanic Passengers") + scale_x_discrete(labels = c("Cherbourg","Queenstown","Southampton"))
```

If we want to change the color of the bars based on a secondary variable...?
```{r}
#g + geom_bar(aes(fill = as.factor(survived)))  # this is the basic thing, below is with fancy labels and stuff
g + geom_bar(aes(fill = as.factor(survived))) + labs(x = "City Embarked", title = "Bar Plot of Embarked City for Titanic Passengers") + scale_x_discrete(labels = c("Cherbourg","Queenstown","Southampton")) + scale_fill_discrete(name = "Survived", labels = c("No","Yes"))
```

What if, instead of this, I want to do a figure with side-by-side bars divided by variable?  I have to make two things by hand.
```{r}
twoWayData <- titanicData %>% drop_na(embarked) %>% group_by(embarked, survived) %>% summarise(count = n()) # note that ggplot just figured out the heights itself by defaulting to looking at the frequency
g <- ggplot(data = twoWayData, aes(x = embarked, y = count, fill = as.factor(survived))) #count came from the twoWayData data frame, which has a variable named count
g + geom_bar(stat="identity", position = "dodge") + labs(x = "City Embarked", title = "Bar Plot of Embarked City") + scale_x_discrete(labels=c("Larry","Moe","Curly")) + scale_fill_discrete(name="Kicked the bucket", labels = c("Yes","No"))
# stat="identity" is "do the thing that is in the count cell, you don't have to count it yourself!"
# position = "dodge" means that the two bars are going to not collide
```

What do you do with this pretty picture?  You save it.
```{r}
ggsave(filename = "titanicBarPlot.png")
ggsave(filename = "titanicBarPlot.pdf")
ggsave("titanicBarPlot2.jpg") #you don't have to say filename!  but you can.
# by default, this saves the last plot you did, like the g + geom + labels + scale + blah blah blah
```

THE WRAP-UP ACTIVITY FOR THE SECOND PART OF THE MORNING SESSION
```{r}
#install.packages("Lahman")
library(Lahman)
#Convert the Master data set to a tibble
master <- tbl_df(Master)
```

```{r}
#Create a table that gives the frequency of players by birth months.
table(master$birthMonth)
```

```{r}
#Create a table that gives the proportion of players by birth months. (Proportion = # in category/total #)
tab <- table(master$birthMonth)
tab <- tab/sum(tab)
tab
```

```{r}
#Create a bar chart of the player birth month frequencies. Give the plot appropriate labels.
```

```{r}
#Create a two-way contingency table that gives the frequencies of how the player bats ("bats") and the way the player throws ("throws").
```

```{r}
#Create a stacked bar chart of the player bat and throw preferences. Give the plot appropriate labels.
```


8/14/18 - AFTERNOON SESSION

GRAPHICAL SUMMARIES OF QUANTITATIVE DATA
```{r}
CO2 <- tbl_df(CO2)
CO2
```

There are ways you can summarize quant data.
```{r}
mean(CO2$uptake)
# if you ever wanted the trimmed mean...
mean(CO2$uptake, trim = 0.05)
median(CO2$uptake)
```

```{r}
CO2 %>% group_by(Treatment) %>% summarise(av = mean(uptake))
```
You can save this as an object and ggplot it as an object to make a bar graph.  Or you can make a "dotplot".

```{r}
g <- ggplot(CO2, aes(x = uptake))
g + geom_dotplot()
g + geom_dotplot(aes(col = Treatment)) # this is a data-driven change - it looks at the levels of treatment and decides to give it different colors
g + geom_dotplot(col = "Red")
```


```{r}
g + geom_dotplot(aes(col = Treatment),stackgroups = TRUE, method = "histodot", binpositions = "all")
#stack groups instead of having them slightly overlap each other
#histodot is a dot plot that kind of works like a histogram
```

```{r}
g + geom_histogram()
g + geom_histogram(color = "blue", fill = "red", linetype = "dashed", size = 2, binwidth = 3)
g + geom_histogram(color = "red", fill = "blue", linetype = "dashed", size = 3, binwidth = 5)
# if you want the histogram that's a little bit nicer - smoothed with a kernel - you can do this
g + geom_density()
# and make it nicer.  or...noisier.
g + geom_density(adjust = 0.25, alpha = 0.5, aes(fill = Treatment))
```

```{r}
g + stat_ecdf(geom = "step") # the different levels of uptake are contributing about the same amount
g + stat_ecdf(geom = "step", aes(color = Treatment)) # by groups
```

And now FINALLY...SCATTER PLOTS
```{r}
scoresFull <- read_csv("https://raw.githubusercontent.com/jbpost2/DataScienceR/master/datasets/scoresFull.csv")
g <- ggplot(scoresFull, aes(x = homeRushYds, y = HFinal))
g + geom_point()
g + geom_point() + geom_smooth()
g + geom_point() + geom_smooth() + geom_smooth(method = lm, col = "Red")
# what if you want to annotate it?  like the regression line formula?  or the correlation?
correlation <- cor(scoresFull$homeRushYds,scoresFull$HFinal)
g + geom_point() + geom_smooth() + geom_smooth(method = lm, col = "Red") + geom_text(x = 315, y = 5, size = 6, label = paste0("Correlation = ",round(correlation,4))) # the x and y are coordinates - it's where you're gonna put this text
# paste0() is just the same as =CONCATENATE() in Excel; paste() is that except you specify a delimiter
```

IMPORTANT FOR MULTIVARIATE ANALYSES - you can use pairs() to look at all possible pairwise scatterplots.
```{r}
pairs(select(scoresFull, Hturnovers, homeRushYds, homePassYds, HFinal), cex = 0.3)
```

Also possible to plot correlations!
```{r}
Correlation <- cor(select(scoresFull, Hturnovers, homeRushYds, homePassYds, HFinal), method = "spearman")
#install.packages("corrplot")
library(corrplot)
corrplot(Correlation, type = "upper", tl.pos = "lt")
corrplot(Correlation, type = "lower", method = "number", add = TRUE, diag = FALSE, tl.pos="n")
# size of circle represents magnitude of correlation; color positivity/negativity
```


```{r}
#install.packages("GGally")
library(GGally)
ggpairs(iris, aes(colour = Species, alpha = 0.4))
```

For change-over-time kinds of data, you need a line plot.  First let's make date-making easier.
```{r}
oneDate <- paste(scoresFull$date[1], scoresFull$season[1], sep="-") #this is just a date we made, don't worry about it
oneDate
library(lubridate)
as.Date(oneDate,"%d-%b-%Y")
as.Date(oneDate,"%d-%b-%Y") + 1
```

What if you want to plot like, a line graph?  For maybe time series?
```{r}
scoresFull$date <- paste(scoresFull$date, scoresFull$season, sep = "-") %>% as.Date("%d-%b-%Y")
subScores <- scoresFull %>% filter(homeTeam %in% c("Pittsburgh Steelers", "Cleveland Browns", "Baltimore Ravens", "Cincinnati Bengals")) %>% group_by(season, homeTeam) %>% summarise(homeAvgYds = mean(homePassYds + homeRushYds))
subScores
```

Now to plot it.
```{r}
g <- ggplot(subScores, aes(x = season, y = homeAvgYds, color = homeTeam)) # R assigned colors by team
g + geom_line(lwd = 2) # lwd is length width
```

Finally - FANCY 3D PLOTS.
```{r}
#install.packages("plot3Drgl")
#library(plot3Drgl)
scatter3D(x = scoresFull$homeRushYds, y = scoresFull$awayRushYds, z = scoresFull$HFinal) # this is pretty but not super informative
```

AN ACTIVITY
```{r}
#We will continue the use of the Lahman package. This time we will look at the Batting data set.
#Convert the Batting data set to a tibble.
library(Lahman)
Batting <- tbl_df(Batting)
```

```{r}
#Create a histogram of the games played variable and overlay a smoothed density.
g <- ggplot(Batting, aes(x=G))
g + geom_histogram()
g + geom_density()
g + geom_histogram() + geom_density(size=1.5)
g + geom_histogram(binwidth = 5, linetype = 1, size = 1.25, color = "black", fill = "lightblue", aes(y = ..density..)) + geom_density(size = 1.5)
```

```{r}
#Create side by side box plots of the hits (H) variable with the lgID as the grouping variable.


#twoWayData <- titanicData %>% drop_na(embarked) %>% group_by(embarked, survived) %>% summarise(count = n()) # note that ggplot just figured out the heights itself by defaulting to looking at the frequency
#g <- ggplot(data = twoWayData, aes(x = embarked, y = count, fill = as.factor(survived))) #count came from the twoWayData data frame, which has a variable named count
#g + geom_bar(stat="identity", position = "dodge") + labs(x = "City Embarked", title = "Bar Plot of Embarked City") + scale_x_discrete(labels=c("Larry","Moe","Curly")) + scale_fill_discrete(name="Kicked the bucket", labels = c("Yes","No"))

twoWayData <- Batting %>% drop_na(lgID) %>% drop_na(H) %>% group_by(lgID) %>% summarize(count=n())
g <- ggplot(data = twoWayData, aes(x = lgID, y = count))
g + geom_bar(stat="identity", position="dodge")
```

```{r}
#Find the 'summary' of the hits variable.
summary(Batting$H)
```

```{r}
#Find the total number of homeruns for each year.
Batting %>% group_by(yearID) %>% summarise(meep = sum(HR))
```

```{r}
#Report the correlation between AB and H. Note: The NA values can cause a problem here. Check the options on the cor function.
BattingNoNAS <- drop_na(Batting)
cor(BattingNoNAS$AB,BattingNoNAS$H)
```

```{r}
#Create a scatter plot of the AB and H variables (AB on x axis). Label and title appropriately.
g <- ggplot(Batting, aes(x = AB, y = H))
g + geom_point() + geom_smooth(method = lm, col = "Red") + labs(x = "Whatever AB is", y = "Whatever H is", title = "This is a graph of AB by H")
```

```{r}
#Add a regression line to the plot of your choosing.
```

```{r}
#Color the points by the number of homeruns.
```

```{r}
#Make all of the points smaller.
```


ANALYSES

Linear regression is one.  So first we got this great visualization with a scatter plot, and we have a nice descriptive value with the correlation.  But let's build a model about it.

predictedVariable = intercept + slope * observation + error (here's the general linear model)
You use
fit <- lm(predictedVariable ~ predictor variable, data = whatever)
