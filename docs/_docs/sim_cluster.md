---
title: "Simulation on HPC"
permalink: /docs/sim_cluster/
---

If you can access a HPC cluster, it is efficient way to launch many simulation is to use the compute distribution engine, while using the front node to launch Funz commands. 

Assuming that the simulation software (say Modelica, our standard example) is already installed on the cluster, you can use one of these ways:

  1. start backend on __computing nodes__, run Funz from __front node__
  2. start backend __and__ run Funz from __front node__
  3. start backend on __front node__, and run Funz from __your own computer__


## 1. Backend on __computing nodes__ + Funz on __front node__

On a __shared path__ between computing and front nodes:

  * intall Funz: 
    * Python: `pip install Funz`, then `import Funz`
    * R: `remotes::install_github('Funz/Funz.R')`, then `library(Funz)`
    * bash: download and unzip [Funz-Bash.zip](https://github.com/Funz/plugin-Bash/releases/latest)
  * install simulation plugin:
    * Python: `Funz.installModel('Modelica')`
    * R: `Funz::install.Model('Modelica')`
    * bash: download and unzip [plugin-Modelica.zip](https://github.com/Funz/plugin-Modelica/releases/latest)
  * setup simulation script 'Funz/scripts/Modelica.sh' to 
  * __add front node IP (say 192.168.1.1) in the 'Funz/calculator.xml' file:__
    ```
    <CALCULATOR>
    ...
    <HOST name="192.168.1.1" port="19001">
    <HOST name="192.168.1.1" port="19002">
    <HOST name="192.168.1.1" port="19003">
    <HOST name="192.168.1.1" port="19004">
    ...
    </CALCULATOR>
    ```
  * start background Funz computing daemon:
    * bash: submit `srun --exclusive Funz/FunzDaemon.sh` task (for SLURM)

  ---

  You can now check that these computers are well setup by running basic example on your own, that will use one of the local network computers:

  * Python: `Funz.Run(model="Modelica",input_files="samples/NewtonCooling.mo")`
  * R: `Funz::Run(model="Modelica",input.files="samples/NewtonCooling.mo")`
  * bash/cmd.exe: `./Funz.sh Run -m Modelica -if samples/NewtonCooling.mo` or `./Funz.bat Run -m Modelica -if samples/NewtonCooling.mo` 


## 2. Backend + Funz on __front node__

On the front node:

  * intall Funz: 
    * Python: `pip install Funz`, then `import Funz`
    * R: `remotes::install_github('Funz/Funz.R')`, then `library(Funz)`
    * bash: download and unzip [Funz-Bash.zip](https://github.com/Funz/plugin-Bash/releases/latest)
  * install simulation plugin:
    * Python: `Funz.installModel('Modelica')`
    * R: `Funz::install.Model('Modelica')`
    * bash: download and unzip [plugin-Modelica.zip](https://github.com/Funz/plugin-Modelica/releases/latest)
  * setup simulation script 'Funz/scripts/Modelica.sh' to suit Modelica on computing nodes
  * __wrap command through cluster scheduler in the 'Funz/calculator.xml' file:__
    ```
    <CALCULATOR>
    ...
    <CODE name="Modelica" command="./scripts/slurm.sh /opt/Funz/scripts/Modelica.sh"/>
    ...
    </CALCULATOR>
    ```
    _note that SLURM, SGE and OAR wraping scripts are available inside 'Funz/scripts' directory_
  * start background Funz computing daemon:
    * Python: `Funz.startCalculators(1)`
    * R: `Funz::startCalculators(1)`
    * bash: launch backend `Funz/FunzDaemon.sh`

  ---

  You can now check that the backend is well setup by running basic example:

  * Python: `Funz.Run(model="Modelica",input_files="samples/NewtonCooling.mo")`
  * R: `Funz::Run(model="Modelica",input.files="samples/NewtonCooling.mo")`
  * bash: `./Funz.sh Run -m Modelica -if samples/NewtonCooling.mo`


## 3. Backend on __front node__ + Funz on __computer__

On the front node:

  * intall Funz: 
    * Python: `pip install Funz`, then `import Funz`
    * R: `remotes::install_github('Funz/Funz.R')`, then `library(Funz)`
    * bash: download and unzip [Funz-Bash.zip](https://github.com/Funz/plugin-Bash/releases/latest)
  * install simulation plugin:
    * Python: `Funz.installModel('Modelica')`
    * R: `Funz::install.Model('Modelica')`
    * bash: download and unzip [plugin-Modelica.zip](https://github.com/Funz/plugin-Modelica/releases/latest)
  * setup simulation script 'Funz/scripts/Modelica.sh' to suit Modelica on computing nodes
  * __wrap command through cluster scheduler in the 'Funz/calculator.xml' file:__
    ```
    <CALCULATOR>
    ...
    <CODE name="Modelica" command="./scripts/slurm.sh /opt/Funz/scripts/Modelica.sh"/>
    ...
    </CALCULATOR>
    ```
    _note that SLURM, SGE and OAR wraping scripts are available inside 'Funz/scripts' directory_
  * __add your computer IP (say 192.168.1.1) in the 'Funz/calculator.xml' file:__
    ```
    <CALCULATOR>
    ...
    <HOST name="192.168.1.1" port="19001">
    <HOST name="192.168.1.1" port="19002">
    <HOST name="192.168.1.1" port="19003">
    <HOST name="192.168.1.1" port="19004">
    ...
    </CALCULATOR>
    ```
  * start background Funz computing daemon:
    * Python: `Funz.startCalculators(1)`
    * R: `Funz::startCalculators(1)`
    * bash: launch backend `Funz/FunzDaemon.sh`

  ---

  You can now check that the backend is well setup by running basic example from your computer (ie. not in cluster):

  * Python: `Funz.Run(model="Modelica",input_files="samples/NewtonCooling.mo")`
  * R: `Funz::Run(model="Modelica",input.files="samples/NewtonCooling.mo")`
  * bash: `./Funz.sh Run -m Modelica -if samples/NewtonCooling.mo`

### Autostart backend

The script 'Funz/FunzDaemon_won.sh' ("Wake on Network") is dedicated to wakeup the backend when Funz is called from user side:

  * on the __front node__, just start `Funz/FunzDaemon_won.sh`
  * on your computer, before and after launching Funz, just wake and sleep the backend using:
    ```
    echo "hi"| curl -m 1 telnet://frontnode:19000
    ./Funz.sh Run -m Modelica -if samples/NewtonCooling.mo
    echo "bye"| curl -m 1 telnet://frontnode:19000
    ```