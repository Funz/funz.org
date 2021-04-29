---
title: "Usage: Python"
permalink: /docs/usage_python/
---

### Requirements

  * Python 3.x (should work with 2.x, but not officially supported)
  * [py4j](https://www.py4j.org/) library, to be install with `pip install py4j` (or see [py4j install](https://www.py4j.org/install.html))


### Loading Funz

You need to load Funz with dedicated scipt: `python3 -i /opt/Funz/Funz.py` or load it interactively: 
```python
exec(open("/opt/Funz/Funz.py").read())
```
Then you can initialize Funz engine calling `Funz_init()` with following arguments:
```python
FUNZ_HOME = os.getenv('FUNZ_HOME',None)
java_control = {'Xmx':"512m",'Xss':"256k"} if sys.platform=="Windows" else {'Xmx':"512m"}
verbosity = 0
**jvmargs
```
like:
```python
Funz_init(verbosity=10)
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
    -Djava.awt.headless=True
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


### Using

Main features & functions:
  * to run external parametric calculations of simulator `model` with input files `input_files`: `Funz_Run()` with following arguments: 
```python
runFile("Funz.py")
Funz_init()
Funz_Run(model,input_files)
```
```python
model = None
input_files = None
input_variables = None
is_factorial = False
output_expressions = None
run_control = {'force_retry':2, 'cache_dir':None}
archive_dir = None
verbosity = 0
log_file = True
monitor_control = {'sleep':5, 'display_fun':None}
```
  * to drive a python function `fun` by algorithm `design`: `Funz_Design()` with following (default) arguments:
```python
fun # python function to drive by algorithm
design # algorithm to use
options = None
input_variables = None
fun_control = {'cache':False,'vectorize':"for",'vectorize_by':1}
monitor_control = {'results_tmp':True}
archive_dir = None
verbosity = 0
log_file = True
*vargs
```
  * to drive external parametric calculations of simulator `model` with input files `input_files` by algorithm `design`: `Funz_RunDesign()` with following (default) arguments:
```python
model = None
input_files = None
output_expressions = None
design = None
input_variables = None
design_options = None
run_control = {'force_retry':2,'cache_dir':None}
monitor_control = {'results_tmp':True,'sleep':5,'display_fun':None}
archive_dir = None
verbosity = 0
log_file = True
```

Access to some intern features is also available:
  * parse parametric `input_files` to identify parameters and expected output from `model`:
```python
Funz_ParseInput(model,input_files)
```
  * parse & compile parametric `input_files`  from `model` to replace parameters by `input_values`:
```python
Funz_CompileInput(model,input_files,input_values,output_dir=".")
```
  * read output files in `output_dir` to get expected values of interest from `model` with `input_files`:
```python
Funz_ReadOutput(model, input_files, output_dir)
```
  * scan newtwork & local computers suitable to launch calculations: `Funz_GridStatus()`, which returns:
<pre class="highlight"><div style="width: 1400px; overflow-x:scroll;"><code>| Computer | host name | OS              | address:port      | local status | since    | activity                               | codes          |
|----------|-----------|-----------------|-------------------|--------------|----------|----------------------------------------|----------------|
| neutro-1 | localhost | Linux 4.15.0-43 | 192.168.0.1:34485 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-2 | localhost | Linux 4.15.0-43 | 192.168.0.2:37265 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-3 | localhost | Linux 4.15.0-43 | 192.168.0.3:18925 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-4 | localhost | Linux 4.15.0-43 | 192.168.0.4:24495 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-5 | localhost | Linux 4.15.0-43 | 192.168.0.5:36544 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |</code></div></pre>


