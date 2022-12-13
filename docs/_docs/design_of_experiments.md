---
title: 'Algorithm: [iterative] design of experiments'
permalink: /docs/design_of_experiments/
---

The design of experiments API follows the [GdR MASCOT-NUM template](https://www.gdr-mascotnum.fr/template.html) standard, for [R](http://www.r-project.org) language.


Most of algorithms are implemented in a file 'MyAlgorithm.R' located in 'Funz/plugins/doe' directory, mainly requiring a header and four methods:

```r
#title:...
#help:...
#author:...
#type:...
#output:...
#options:...
#options.help:...

#' constructor and initializer of algorithm
#' @param options algorithm options
#' @return algorithm object : environment options, status
MyAlgorithm <- function(options) {
    ...
    algorithm = new.env()
    ...
    return(algorithm)
}

#' first design building.
#' @param algorithm object handling options, status, ...
#' @param input the input variables and their properties (like min, max)
#' @param output the output targets names
#' @return matrix of first design step
getInitialDesign <- function(algorithm,input,output) {
    ...
    return(matrix(...,ncol=length(input)))
}

#' iterated design building.
#' @param algorithm object handling options, status, ...
#' @param X matrix of current doe variables
#' @param Y matrix of current results
#' @return matrix of next doe step
getNextDesign <- function(algorithm,X,Y) {
    ...
    return(matrix(...,ncol=ncol(X)))
}

#' final analysis.
#' @param algorithm object handling options, status, ...
#' @param X matrix of current doe variables
#' @param Y matrix of current results
#' @return HTML string of analysis
displayResults <- function(algorithm,X,Y) {
    ...
    return(html)
}
```

Note that you can also test and use these templated algorithms from R (without Funz), using the [templr](https://cran.r-project.org/package=templr) package:

```r
install.packages('templr')
library(templr)

test_objective_function = function(X) {
    x1 = X[1]
    x2 = X[2]
    ... #
}

run.algorithm(algorithm_file = 'MyAlgorithm.R',
              objective_function = test_objective_function,
              input = list( x1=list(min=0,max=1), x2=list(min=0,max=1)),
              options = list(maxit=10),
              work_dir=tempdir())
```
