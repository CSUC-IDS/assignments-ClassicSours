---
title: "R programming (r4ds Section III)"
author: "Aaron Shaffer"
date: "January 23, 2018"
output: 
  html_document: 
    keep_md: yes
---

# Ch 18: Pipes

_Demonstrate the use of the regular pipe (`%>%`) and the "explosion" operator (`%$%`) in some example_


```r
library(magrittr)

x <- "Hello World"
x %>% print()
```

```
## [1] "Hello World"
```

```r
"x" %>% print()
```

```
## [1] "x"
```

```r
"x" %>% assign("Fizz Buzz")
x
```

```
## [1] "Hello World"
```

```r
env <- environment()
"x" %>% assign("Fizz Buzz", envir = env)
x
```

```
## [1] "Fizz Buzz"
```


```r
mtcars %$% cor(wt,mpg)
```

```
## [1] -0.8676594
```

```r
mtcars[mtcars %$% which(mpg > 20),]
```

```
##                 mpg cyl  disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4      21.0   6 160.0 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag  21.0   6 160.0 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710     22.8   4 108.0  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive 21.4   6 258.0 110 3.08 3.215 19.44  1  0    3    1
## Merc 240D      24.4   4 146.7  62 3.69 3.190 20.00  1  0    4    2
## Merc 230       22.8   4 140.8  95 3.92 3.150 22.90  1  0    4    2
## Fiat 128       32.4   4  78.7  66 4.08 2.200 19.47  1  1    4    1
## Honda Civic    30.4   4  75.7  52 4.93 1.615 18.52  1  1    4    2
## Toyota Corolla 33.9   4  71.1  65 4.22 1.835 19.90  1  1    4    1
## Toyota Corona  21.5   4 120.1  97 3.70 2.465 20.01  1  0    3    1
## Fiat X1-9      27.3   4  79.0  66 4.08 1.935 18.90  1  1    4    1
## Porsche 914-2  26.0   4 120.3  91 4.43 2.140 16.70  0  1    5    2
## Lotus Europa   30.4   4  95.1 113 3.77 1.513 16.90  1  1    5    2
## Volvo 142E     21.4   4 121.0 109 4.11 2.780 18.60  1  1    4    2
```

----

# Ch 19: Functions

### 19.2: When should you write a function?
Answer any 3

_Describe one time where you wish you had written a function, but didn't._


### 19.3: Functions are for humans and computers
Answer any 2

_What does "Functions are for humans and computers" mean to you?_

### 19.4 Conditional execution
Answer any 3

### 19.5 Function arguments
Answer any 2

### 19.6: Return values
_Is it mandatory that you `return()` a value from a function? If not, give on reasons for why you would want to do so. If so, explain what happens if you don't include the return._

### 19.7: Environment
_What's the problem with the example function at the beginning of this chapter?_

----

# 20: Vectors

###  20.3: Important types of atomic vector
Answer any 2

### 20.4: Using Atomic Vectors
Answer #4, and then 2 others. 

### 20.5: Recursive vectors (lists)
Answer either one

### 20.7: Augmented vectors

_Describe at least two differences between a `data.frame` and a `tibble`_

----

# Chapter 21: Iteration

### 21.2: For Loops
Any one question

### 21.3: For Loop Variations
Problem #1

### 21.4: For loops vs functionals
Problem #2

### 21.5: the map function
Any 2 problems

----

End here. The rest of 21 is for your info only

