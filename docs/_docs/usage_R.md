---
title: "Usage: R"
permalink: /docs/usage_R/
---

### Requirements

  * R 4.x (should work with 3.5+, but not officially supported)

### Loading Funz

#### Using a (pip) module

Once Funz is installed form Python ('pip install Funz'), then just load module using:
```r
library(Funz)
```

#### or Using pre-installed Funz

You need to load Funz interactively: 
```r
source("/opt/Funz/Funz.R")
```
Then you can initialize Funz engine calling `Funz.init()` with following arguments:
```r
FUNZ_HOME = Sys.getenv("FUNZ_HOME")
java.control = if (Sys.info()[['sysname']]=="Windows") list(Xmx="512m",Xss="256k") else list(Xmx="512m"))
verbosity = 0
...
```
like:
```r
Funz.init(verbosity=10)
```
which returns:
```
Initializing JVM ...
    -Dapp.home=/opt/Funz
    -Duser.language=en
    -Duser.country=US
    -Dverbosity=10
    -Douterr=.Funz
    -Xmx512m
    -Djava.awt.headless=TRUE
  Loading java/lang/System ...
Java Java(TM) SE Runtime Environment
 version 1.8.0_201
 from path /usr/lib/jvm/java-8-oracle/jre
  Loading org/funz/Constants ...
Funz 1.9 <build 27/03/2019 15:05>
  Loading org/funz/api/Funz_v1 ...
  Initializing Funz...
  Funz models: bash cmd.exe Modelica
  Funz designs: GradientDescent Brent
```


### Using Funz

Main features & functions:
  * to run external parametric calculations of simulator `model` with input files `input.files`: `Run()` with following arguments: 
```r
Run(model,input.files)
```
```r
model = NULL
input.files
input.variables = NULL
is.factorial = FALSE
output.expressions = NULL
run.control = list(force.retry=2, cache.dir=NULL)
archive.dir = NULL
verbosity = 0
log.file = TRUE
monitor.control = list(sleep=5, display.fun=NULL)
```
  * to drive a R function `fun` by algorithm `design`: `Design()` with following (default) arguments:
```r
fun # R function to drive by algorithm
design # algorithm to use
options = NULL
input.variables = NULL
fun.control = list(cache=FALSE,vectorize="for",vectorize.by=1)
monitor.control = list(results_tmp=TRUE}
archive.dir = NULL
verbosity = 0
log.file = TRUE
...
```
  * to drive external parametric calculations of simulator `model` with input files `input.files` by algorithm `design`: `RunDesign()` with following (default) arguments:
```r
model = NULL
input.files = NULL
output.expressions = NULL
design = NULL
input.variables = NULL
design_options = NULL
run.control = list(force_retry=2,cache_dir=NULL)
monitor.control = list(results_tmp=TRUE,sleep=5,display.fun=NULL)
archive.dir = NULL
verbosity = 0
log.file = TRUE
```

Access to some intern features is also available:
  * parse parametric `input.files` to identify parameters and expected output from `model`:
```r
ParseInput(model,input.files)
```
  * parse & compile parametric `input.files`  from `model` to replace parameters by `input.values`:
```r
CompileInput(model,input.files,input.values,output.dir=".")
```
  * read output files in `output.dir` to get expected values of interest from `model` with `input.files`:
```r
ReadOutput(model, input.files, output.dir)
```
  * scan newtwork & local computers suitable to launch calculations: `Grid()`, which returns:
<pre class="highlight"><div style="width: 1400px; overflow-x:scroll;"><code>| Computer | host name | OS              | address:port      | local status | since    | activity                               | codes          |
|----------|-----------|-----------------|-------------------|--------------|----------|----------------------------------------|----------------|
| neutro-1 | localhost | Linux 4.15.0-43 | 192.168.0.1:34485 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-2 | localhost | Linux 4.15.0-43 | 192.168.0.2:37265 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-3 | localhost | Linux 4.15.0-43 | 192.168.0.3:18925 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-4 | localhost | Linux 4.15.0-43 | 192.168.0.4:24495 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-5 | localhost | Linux 4.15.0-43 | 192.168.0.5:36544 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |</code></div></pre>


