---
title: Algorithm - Design of experiments
permalink: /docs/design_of_experiments/
---

The design of experiments API follows the [GdR MASCOT-NUM template](https://www.gdr-mascotnum.fr/template.html) standard, for [R](http://www.r-project.org) language.


Most of algorithms consists in a file 'MyAlgorithm.R' located in 'Funz/plugins/doe' directory, mainly requiring a header and four methods to be implemented:

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
#' @param d the number of variables all set in [0,1]
#' @return matrix of first design step
getInitialDesign <- function(algorithm,input,output) {
    ...
    return(matrix(...,ncol=length(input)))
}

#' iterated design building.
#' @param algorithm object handling options, status, ...
#' @param X matrix of current doe variables (in [0,1])
#' @param Y matrix of current results
#' @return matrix of next doe step
getNextDesign <- function(algorithm,X,Y) {
    ...
    return(matrix(...,ncol=ncol(X)))
}

#' final analysis. All variables are set in [0,1].
#' @param algorithm object handling options, status, ...
#' @param X matrix of current doe variables (in [0,1])
#' @param Y matrix of current results
#' @return HTML string of analysis
displayResults <- function(algorithm,X,Y) {
    ...
    return(html)
}
```
