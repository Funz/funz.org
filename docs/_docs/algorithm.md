---
title: Design algorithm
permalink: /docs/algorithm/
---

## Overview

Funz design 'algorithm' is the component that features funz to drive an external simulation software for an engineering objective.
It mainly provides some methods in [R](http://www.r-project.org) language to define numerical simulations to perform, and returns information about engineering target.

[![algorithm cheatsheet]({{ site.baseurl }}/docs/Algorithm.png){:width="400}]({{ site.baseurl }}/docs/Algorithm.png)

For now, following algorithms are available out-of-the-box:

* Brent [![Funz-Brent](https://github.com/Funz/algorithm-Brent/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/algorithm-Brent/)
* EGO [![Funz-EGO](https://github.com/Funz/algorithm-EGO/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/algorithm-EGO/)
* GradientDescent [![Funz-GradientDescent](https://github.com/Funz/algorithm-GradientDescent/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/algorithm-GradientDescent/)
* PSO [![Funz-PSO](https://github.com/Funz/algorithm-PSO/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/algorithm-PSO/)
* RandomSampling [![Funz-RandomSampling](https://github.com/Funz/algorithm-RandomSampling/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/algorithm-RandomSampling/)
* Sensitivity [![Funz-Sensitivity](https://github.com/Funz/algorithm-Sensitivity/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/algorithm-Sensitivity/)
* NSGA2 [![Funz-NSGA2](https://github.com/Funz/algorithm-NSGA2/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/algorithm-NSGA2/)

For conveniency, a [basic template](https://github.com/Funz/algorithm-template) is provided and should be used as a scratch project.


## Installation

Once installed, Funz can integrate some more algorithms. 
Python and R wrappers provide `install.Design('ALGORITHM')` methods to add one of the previously bundled.

From scratch (when no bundle is available), you can add a new algorithm by hand just by copying file 'MyAlgorithm.R' inside 'Funz/plugins/doe/' directory.


## Bundle implementation

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
    #' @param input the input variables and their properties (like min, max)
    #' @param output the output targets names
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
    #' @param X matrix of current doe variables
    #' @param Y matrix of current results
    #' @return matrix of next doe step
    getNextDesign <- function(algorithm,X,Y) {
        ...
        return(matrix(...,ncol=ncol(X)))
    }
    ```
  * fill the results renderer (with plots & string return):
    ```r
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
5. Provide (at least) one test case __'src/test/cases/MyTest.R'__ containing R::testthat tests:
    ```r
    ## This file should provide following objects, when loaded:
    # f : function
    # input.f : list of input dimensions, contains list of properties like lower & upper bounds of each dimensions
    # output.f : list of output dimensions
    # *.f : list of math properties. To be compared with algorithm results
    # [print.f] : method to print/plot the function for information
    
    f = function(X) { return(cos(pi*x)) }
    
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
