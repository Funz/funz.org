---
title: Algorithm
permalink: /docs/algorithm/
---

## Overview

Funz 'algorithm' is the component that features funz to interact with an algorithm implementation, to drive an external simulation software.
It mainly provides some methods in [R](http://www.r-project.org) language to define numerical simulations to perform, and returns information about engineering target.

[![algorithm cheatsheet]({{ site.baseurl }}/docs/Algorithm.png){:width="400}]({{ site.baseurl }}/docs/Algorithm.png)


For conveniency, a [basic template](https://github.com/Funz/algorithm-template) is provided and should be used as a scratch project.

## Implementation

### Requirements

* Java 8 Runtime Environment (dev Kit not needed)
* [ant](http://ftp.heanet.ie/mirrors/www.apache.org/dist//ant/binaries/apache-ant-1.10.6-bin.zip)
* [common funz ressource directory](https://github.com/Funz/funz-profile/archive/master.zip)
* git (for github integration or checkout if needed)

### Step-by-step

These steps will guide you to build a basic algorithm, which is sufficient for most of your needs.

1. __Checkout__ the algorithm template: 
  * [fork from github](https://github.com/Funz/algorithmn-template/generate) and clone: `git clone https://github.com/MyUserName/algorithm-MyAlgorithmName`
  * [download algorithm-template directory](https://github.com/Funz/algorithm-template/archive/master.zip)
2. Edit the __'build.xml'__ file to replace `MyAlgorithm` by the name of your algorithm (usually, the name of the simulation software)
3. Edit the __'README.md'__ description file
4. Rename and implement the __'src/main/doe/MyAlgorithm.R'__ file:
  * fill the header:
    ```r
    #title:...
    #help:...
    #author:...
    #type:...
    #output:...
    #options:...
    #options.help:...
    ```
  * fill the constructor:
    ```r
    #' constructor and initializer of algorithm
    #' @param options algorithm options
    #' @return algorithm object : environment options, status
    MyAlgorithm <- function(options) {
        ...
        algorithm = new.env()
        ...
        return(algorithm)
    }
    ```
  * fill the initalizer of experiments (typically with random points):
    ```r
    #' first design building.
    #' @param algorithm object handling options, status, ...
    #' @param d the number of variables all set in [0,1]
    #' @return matrix of first design step
    getInitialDesign <- function(algorithm,input,output) {
        ...
        return(matrix(...,ncol=length(input)))
    }
    ```
  * fill the iterator (considering previous calculations resutls):
    ```r
    #' iterated design building.
    #' @param algorithm object handling options, status, ...
    #' @param X matrix of current doe variables (in [0,1])
    #' @param Y matrix of current results
    #' @return matrix of next doe step
    getNextDesign <- function(algorithm,X,Y) {
        ...
        return(matrix(...,ncol=ncol(X)))
    }
    ```
  * fill the results renderer (with plots & string return):
    ```r
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
5. Provide (at least) one test case __'src/test/cases/MyTest.R'__ containing R::testthat tests:
    ```r
    ## This file should provide following objects, when loaded:
    # f : function
    # input.f : list of input dimensions, contains list of properties like lower & upper bounds of each dimensions
    # output.f : list of output dimensions
    # *.f : list of math properties. To be compared with algorithm results
    # [print.f] : method to print/plot the function for information
    
    f = function(X) {
        matrix(Vectorize(function(x) {cos(pi*x)})(X),ncol=1)
    }
    input.f = list(
        x=list(min=0,max=1)
    )
    output.f = "cos_pi"
    root.f = 0.5
    
    test = function(algorithm_file) {
        results = run.algorithm(algorithm_file, options=NULL,fun=list(input=input.f,output=output.f,fun=f))
        library(testthat)
        test_that("cos_pi info",{expect_equal(as.numeric(results$info),root.f,tolerance = .0001)})
    }
    ```
6. Check all test cases using `ant test`, which returns a json report, and should __not__ finished by `FAILED`
