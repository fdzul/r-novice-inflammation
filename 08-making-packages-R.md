---
layout: page
title: Programming with R
subtitle: Making packages in R
minutes: 30
---



> ## Learning Objectives {.objectives}
>
> Quick summary on how (and why) making R packages

Why should you make your own R packages?

**Reproducible research!**

An R package is the **basic unit of reusable code**.
If you want to reuse code later or want others to be able to use your code, you should put it in a package.

An R package requires two components:
  - a DESCRIPTION file with metadata about the package
  - an R directory with the code

  *There are other optional components. Go [here](http://adv-r.had.co.nz/Package-basics.html) for much more information.*

DESCRIPTION file
----------------

    Package: Package name
    Title: Brief package description
    Description: Longer package description
    Version: Version number(major.minor.patch)
    Author: Name and email of package creator
    Maintainer: Name and email of package maintainer (who to contact with issues)
    License: Abbreviation for an open source license
    
The package name can only contain letters and numbers and has to start with a letter.

.R files
--------
Functions don't all have to be in one file or each in separate files.
How you organize them is up to you.
Suggestion: organize in a logical manner so that you know which file holds which functions.

Making your first R package
---------------------------

Let's turn our temperature conversion functions into an R package.


~~~{.r}
fahr_to_kelvin <- function(temp) {
    #Converts Fahrenheit to Kelvin
    kelvin <- ((temp - 32) * (5/9)) + 273.15
    kelvin
}
~~~


~~~{.r}
kelvin_to_celsius <- function(temp) {
  #Converts Kelvin to Celsius
  Celsius <- temp - 273.15
  Celsius
}
~~~


~~~{.r}
fahr_to_celsius <- function(temp) {
  #Converts Fahrenheit to Celsius using fahr_to_kelvin() and kelvin_to_celsius()
  temp_k <- fahr_to_kelvin(temp)
	result <- kelvin_to_celsius(temp_k)
  result
}
~~~

We will use the `devtools` and `roxygen2` packages, which make creating packages in R relatively simple.
First, install the `devtools` package, which will allow you to install the `roxygen2` package from GitHub ([code][]).

[code]: https://github.com/klutometis/roxygen


~~~{.r}
install.packages("devtools")
library("devtools")
install_github("klutometis/roxygen")
library("roxygen2")
~~~

Set your working directory, and then use the `create` function to start making your package.
Keep the name simple and unique.
  - package_to_convert_temperatures_between_kelvin_fahrenheit_and_celsius (BAD)
  - tempConvert (GOOD)


~~~{.r}
setwd(parentDirectory)
create("tempConvert")
~~~

Add our functions to the R directory.
Place each function into a separate R script and add documentation like this:


~~~{.r}
#' Convert Fahrenheit to Kelvin
#'
#' This function converts input temperatures in Fahrenheit to Kelvin.
#' @param temp The input temperature.
#' @export
#' @examples
#' fahr_to_kelvin(32)

fahr_to_kelvin <- function(temp) {
  #Converts Fahrenheit to Kelvin
  kelvin <- ((temp - 32) * (5/9)) + 273.15
  kelvin
}
~~~

The `roxygen2` package reads lines that begin with `#'` as comments to create the documentation for your package.
Descriptive tags are preceded with the `@` symbol. For example, `@param` has information about the input parameters for the function.
Now, we will use `roxygen2` to convert our documentation to the standard R format.


~~~{.r}
setwd("./tempConvert")
document()
~~~

Take a look at the package directory now.
The /man directory has a .Rd file for each .R file with properly formatted documentation.

Now, let's load the package and take a look at the documentation.


~~~{.r}
setwd("..")
install("tempConvert")

?fahr_to_kelvin
~~~

Notice there is now a tempConvert environment that is the parent environment to the global environment.


~~~{.r}
search()
~~~

Now that our package is loaded, let's try out some of the functions.


~~~{.r}
fahr_to_celsius(32)
~~~



~~~{.output}
[1] 0

~~~



~~~{.r}
fahr_to_kelvin(212)
~~~



~~~{.output}
[1] 373.15

~~~



~~~{.r}
kelvin_to_celsius(273.15)
~~~



~~~{.output}
[1] 0

~~~

> ## Challenge - Creating a package for distribution {.challenge}
> 
> - Create some new functions for your tempConvert package to convert from Kelvin to Fahrenheit or from Celsius to Kelvin or Fahrenheit.
> - Create a package for our `analyze` function so that it will be easy to load when more data arrives.
