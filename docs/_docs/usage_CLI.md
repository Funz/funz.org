---
title: "Usage: Command Line"
permalink: /docs/usage_cli/
---

For bash or cmd.exe, the usage pattern is the following `Funz.sh -h` or `Funz.bat -h`:
<pre class="highlight"><div style="width: 1000px; overflow-x:scroll;"><code>Usage: Funz.(sh|bat) COMMAND [ARGS]

  Run ...             Launch (remote) calculations, replacing variables by given values
  Design ...          Apply an algorithm on an executable program returning target output value
  RunDesign ...       Apply an algorithm and launch required calculations
  ParseInput ...      Find variables inside parametrized input file
  CompileInput ...    Replace variables inside parametrized input file
  ReadOutput ...      Read output files content
  GridStatus          Display calculators list and status
</code></div></pre>

Then, depending on the COMMAND, the pattern becomes:

  * to run simple parametric calculations: `Funz.sh Run -h`
<pre class="highlight"><div style="width: 1000px; overflow-x:scroll;"><code>Funz.(sh|bat) Run ...

--help, -h                    Help
                                        Display help
--model, -m                   Model
                                        Code to launch:
                                                  Modelica
                                                  cmd.exe
                                                  bash
--input_files, -if            Input files
                                        List of input files for code
--output_expression, -oe      Output expression
                                        Output expression to parse when calculation finished:
                                                  GaussianDensity
                                                  Numeric
                                                  NumericArray
                                                  Sequence
                                                  Numeric2D
                                                  Numeric2DArray
                                                  Numeric3D
                                                  Numeric3DArray
                                                  Anything
                                                  AnythingND
                                                  Text
--input_variables, -iv        Input variables
                                        Input variables definition 'name=values|model', e.g. x1=0.1,0.2,0.3 x2=0,1 x3=-0.5,-0.6
--all_combinations, --all     All combinations
                                        Use a factorial design to compute all combinations of input variables.    
--run_control, -rc            Run control
                                        Features of the run, e.g. retry=3 cache=/tmp/MyCache archiveFilter="(.*)"
--monitor_control, -mc        Monitor control
                                        Monitoring options, e.g. sleep=5 display=/usr/local/command_to_display_results
--verbosity, -v               Verbosity
                                        Verbosity level in 0-10
--archive_dir, -ad            Archiving directory
                                        Directory where to store output files and data
--print_filter, -pf           Print filter
                                        Output data filter: x1 x2 y code duration ...
--print, -p                   Print file
                                        Filename with data format: xml json or csv (default if not recognized)
</code></div></pre>
  * to drive a parametric external program by an algorithm: `Funz.sh Design -h`
<pre class="highlight"><div style="width: 1000px; overflow-x:scroll;"><code>Funz.(sh|bat) Design ...

--help, -h                    Help
                                        Display help
--function, -f                Function
                                        Executable to launch with input variables as arguments: /path/to/exec 0.1 0.2
--function_args, -fa          Function arguments
                                        Input variables order to pass to function: x1 x2
--function_parallel, -fp      Function parallel evaluations
                                        Number of function parallel evaluations
--design, -d                  Design
                                        Algorithm to drive calculations points:
                                                  GradientDescent
                                                  Brent
--input_variables, -iv        Input variables
                                        Input variables definition 'name=values|model', e.g. x1=0.1,0.2,0.3 x2=[0,1] x3=-0.5,-0.6
--design_options, -do         Design options
                                        Options of the algorithm definition name=value, e.g. n=100 iMax=20
--run_control, -rc            Run control
                                        Features of the run, e.g. retry=3 cache=/tmp/MyCache
--monitor_control, -mc        Monitor control
                                        Monitoring options, e.g. sleep=5 display=/usr/local/command_to_display_results
--verbosity, -v               Verbosity
                                        Verbosity level in 0-10
--archive_dir, -ad            Archiving directory
                                        Directory where to store output files and data
--print_filter, -pf           Print filter
                                        Output data filter: x1 x2 y code duration ...
--print, -p                   Print file
                                        Filename with data format: xml json or csv (default if not recognized)
</code></div></pre>
  * to drive parametric calculations by an algorithm: `Funz.sh RunDesign -h`
<pre class="highlight"><div style="width: 1000px; overflow-x:scroll;"><code>Funz.(sh|bat) RunDesign ...

--help, -h                    Help
                                        Display help
--model, -m                   Model
                                        Code to launch:
                                                  Modelica
                                                  cmd.exe
                                                  bash
--input_files, -if            Input files
                                        List of input files for code
--output_expression, -oe      Output expression
                                        Output expression to parse when calculation finished:
                                                  GaussianDensity
                                                  Numeric
                                                  NumericArray
                                                  Sequence
                                                  Numeric2D
                                                  Numeric2DArray
                                                  Numeric3D
                                                  Numeric3DArray
                                                  Anything
                                                  AnythingND
                                                  Text
--design, -d                  Design
                                        Algorithm to drive calculations points:
                                                  GradientDescent
                                                  Brent
--input_variables, -iv        Input variables
                                        Input variables definition 'name=values|model', e.g. x1=0.1,0.2,0.3 x2=[0,1] x3=-0.5,-0.6
--design_options, -do         Design options
                                        Options of the algorithm definition name=value, e.g. n=100 iMax=20
--run_control, -rc            Run control
                                        Features of the run, e.g. retry=3 cache=/tmp/MyCache archiveFilter="(.*)"
--monitor_control, -mc        Monitor control
                                        Monitoring options, e.g. sleep=5 display=/usr/local/command_to_display_results
--verbosity, -v               Verbosity
                                        Verbosity level in 0-10
--archive_dir, -ad            Archiving directory
                                        Directory where to store output files and data
--print_filter, -pf           Print filter
                                        Output data filter: x1 x2 y code duration ...
--print, -p                   Print file
                                        Filename with data format: xml json or csv (default if not recognized)
</code></div></pre>

Access to some intern features is also available:

  * parse parametric input files to identify parameters and expected output: `Funz.sh ParseInput -h`
<pre class="highlight"><div style="width: 1000px; overflow-x:scroll;"><code>Funz.(sh|bat) ParseInput ...

--help, -h                    Help
                                        Display help
--model, -m                   Model
                                        Code to launch:
                                                  Modelica
                                                  cmd.exe
                                                  bash
--input_files, -if            Input files
                                        List of input files for code
--verbosity, -v               Verbosity
                                        Verbosity level in 0-10
--print, -p                   Print file
                                        Filename with data format: xml json or csv (default if not recognized)
</code></div></pre>
  * parse & compile parametric input files to replace parameters: `Funz.sh CompileInput -h`
<pre class="highlight"><div style="width: 1000px; overflow-x:scroll;"><code>Funz.(sh|bat) CompileInput ...

--help, -h                    Help
                                        Display help
--model, -m                   Model
                                        Code ID to launch:
                                                  Modelica
                                                  cmd.exe
                                                  bash
--input_files, -if            Input files
                                        List of input files for code
--input_variables, -iv        Input variables
                                        Input variables definition 'name=values|model', e.g. x1=0.1,0.2,0.3 x2=[0,1] x3=-0.5,-0.6
--verbosity, -v               Verbosity
                                        Verbosity level in 0-10
--archive_dir, -ad            Archiving directory
                                        Directory where to store output files and data
</code></div></pre>
  * read output files to get expected values of interest: `Funz.sh ReadOutput -h`
<pre class="highlight"><div style="width: 1000px; overflow-x:scroll;"><code>Funz.(sh|bat) ReadOutput ...

--help, -h                    Help
                                        Display help
--model, -m                   Model
                                        Code to launch:
                                                  Modelica
                                                  cmd.exe
                                                  bash
--input_files, -if            Input files
                                        List of input files for code
--verbosity, -v               Verbosity
                                        Verbosity level in 0-10
--output_dir, -od             Output directory
                                        Directory where to output files are stored
--print, -p                   Print file
                                        Filename with data format: xml json or csv (default if not recognized)
</code></div></pre>
  * scan newtwork & local computers suitable to launch calculations: `Funz.sh GridStatus`
<pre class="highlight"><div style="width: 1400px; overflow-x:scroll;"><code>| Computer | host name | OS              | address:port      | local status | since    | activity                               | codes          |
|----------|-----------|-----------------|-------------------|--------------|----------|----------------------------------------|----------------|
| neutro-1 | localhost | Linux 4.15.0-43 | 192.168.0.1:34485 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-2 | localhost | Linux 4.15.0-43 | 192.168.0.2:37265 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-3 | localhost | Linux 4.15.0-43 | 192.168.0.3:18925 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-4 | localhost | Linux 4.15.0-43 | 192.168.0.4:24495 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |
| neutro-5 | localhost | Linux 4.15.0-43 | 192.168.0.5:36544 | free         | 22:19:02 | idle (cpu=11.88;mem=26.18;disk=62.17;) | Modelica, bash |</code></div></pre>

For conveniency, an auto-completion profile is also provided in Funz distribution, which will complete your partial command-line (to load in bash with `source Funz/Funz.profile`).

