---
title: Model plugin
permalink: /docs/plugin/
---

## Overview

Funz model 'plugin' is the component that features funz to interact with all external simulation software.
It mainly provides input/output parsing and launch scripts, but can also (optionally) provides a way to follow simulations progress, failover support, ...

For now, following are available out-of-the-box:

* [![Funz-Bash](https://github.com/Funz/plugin-Bash/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Bash/)
* [![Funz-Cast3m](https://github.com/Funz/plugin-Cast3m/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Cast3m/)
* [![Funz-Cmd.exe](https://github.com/Funz/plugin-Cmd.exe/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Cmd.exe/)
* [![Funz-Cristal](https://github.com/Funz/plugin-Cristal/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Cristal/)
* [![Funz-Excel](https://github.com/Funz/plugin-Excel/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Excel/)
* [![Funz-MCNP](https://github.com/Funz/plugin-MCNP/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-MCNP/)
* [![Funz-Modelica](https://github.com/Funz/plugin-Modelica/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Modelica/)
* [![Funz-Moret](https://github.com/Funz/plugin-Moret/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Moret/)
* [![Funz-Python](https://github.com/Funz/plugin-Python/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Python/)
* [![Funz-R](https://github.com/Funz/plugin-R/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-R/)
* [![Funz-Scale](https://github.com/Funz/plugin-Scale/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Scale/)
* [![Funz-Telemac](https://github.com/Funz/plugin-Telemac/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-Telemac/)
* [![Funz-VBS](https://github.com/Funz/plugin-VBS/actions/workflows/ant.yml/badge.svg)](https://github.com/Funz/plugin-VBS/)

For convenience, a [basic template](https://github.com/Funz/plugin-template) is provided and should be used as a scratch project.


## Implementation

### Requirements

* Java 8 Runtime Environment (dev Kit not needed)
* [ant](http://ftp.heanet.ie/mirrors/www.apache.org/dist//ant/binaries/apache-ant-1.10.6-bin.zip)
* [common funz ressource directory](https://github.com/Funz/funz-profile/archive/master.zip)
* git (for github integration or checkout if needed)

### Step-by-step

These steps will guide you to build a basic plugin, which is sufficient for most of your needs. Nevertheless, it may be necessary to access lower level features (ie. Java plugin API) to solve some issues (eg. special binary format for output files, ...). Then, you should use the dedicated reference guide of this API: [plugin API](../io_parser/).

1. __Checkout__ the plugin template: 
  * [fork from github](https://github.com/Funz/plugin-template/generate) and clone: `git clone https://github.com/MyUserName/plugin-MyPluginName`
  * [download plugin-template directory](https://github.com/Funz/plugin-template/archive/master.zip)
2. Edit the __'build.xml'__ file to replace `MyPlugin` by the name of your plugin (usually, the name of the simulation software)
3. Edit the __'README.md'__ description file:
  * replace 'MyPlugin' by the name of your plugin
  * choose the variable syntax and replace `$(` and `)` (`$` should be replaced by a reserved character unused in the code syntax)
  * choose the formula syntax and replace `@{` and `}` (`@` should be replaced by a reserved character unused in the code syntax)
  * change the comment character `#` by the one used in your code syntax
  * provide a (small & simple) typical input file, containing some parameters
```
...
... $(x1) ...
... $(x2) ...
...
```
  * manually replace 'x1' and 'x2', and run this file with your simulation code to get the output files
  * select the main output files containing interest values (eg. by their extension), and give some sample:
```
...
z= ...
...
```
  * describe what output values are read
4. According to previous choices, rename and implement the __'src/main/io/MyPlugin.ioplugin'__ file:
    ```
    variableStartSymbol=$
    variableLimit=(...)
    formulaStartSymbol=@
    formulaLimit={...}
    commentLineChar=#
    ```
5. Rename and adapt the shell script to launch the code __'src/main/scripts/MyPlugin.sh'__ and/or __'src/main/scripts/MyPlugin.bat'__,
6. Provide (at least) one __non-parametric__ test case in __'src/test/cases/MyTestCase.in/'__, containing all input files of this test case (without any parameter), including the main file which is passed as first argument to the '.sh' script:
  * then, launch the simulation on all test cases (one in each 'src/test/cases/' subdirectory)
        - possibly by calling `ant run-reference-cases` (which will use the previous script),
  * so that once finished, all directories 'src/test/cases/MyTestCase.in/' will contain these files & dirs:
        - 'input/' which contains all input files (which includes MyTestCase.in file also)
        - 'output/' which contains all output files
        - 'info.txt' which contains optional informations about the run (without output values extracted for now)
7. Now you can fill the __'src/main/io/MyPlugin.ioplugin'__ file with parsing of output values:
    ```
    variableStartSymbol=$
    variableLimit=(...)
    formulaStartSymbol=@
    formulaLimit={...}
    commentLineChar=#
        
    outputlist=x y z
    output.x.get=lines("(.*)out") >> filter("^x=(.*)") >> after("=")
    output.y.get=lines("(.*)out") >> filter("^y=(.*)") >> after("=")
    output.z.get=lines("(.*)out") >> filter("^z=(.*)") >> after("=")
    ```
    Note that `output.XXX.get=` syntax is a pipe suite expression wich API is given in [plugin API](../io_parser/). In order to check these expressions, you can test instantly your plugin behavior using the `ant read-output-tests` and `ant parse-input-tests` from plugin directory.
  * Once stable, you can append the extracted output (from `ant read-output-tests` result) as new keys in __'info.txt'__:
```
...
output.x=1.23
output.y=4.56
output.z=7.89
```
    so that this test case is now complete.
8. Check all test cases using `ant test`, which returns a json report, and should __not__ finished by `FAILED`
9. Provide some sample case files in __'src/main/samples/OneSample.in'__.
