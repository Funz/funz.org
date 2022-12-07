---
title: "Usage: Simulation on HPC backend"
permalink: /docs/sim_cluster/
---

If you can access a HPC cluster, it is efficient way to launch many simulation is to use the compute distribution engine, while using the front node to launch Funz commands. 

Assuming that the simulation software (say Modelica, our standard example) is already installed on the cluster, you can use one of these ways:

  * start daemons and run Funz from front node
  * start daemons on computing nodes, run Funz from front node
  * start daemons on front node, and run Funz from your own computer

  * intall Funz: 
    * Python: `pip install Funz`, `import Funz`
    * R: `remotes::install_github('Funz/Funz.R'); library(Funz)`
    * bash: download and unzip https://github.com/Funz/plugin-Bash/releases/latest
  * install simulation plugin:
    * Python: `Funz.installModel('Modelica')`
    * R: `Funz::install.Model('Modelica')`
    * bash: download and unzip plugin at https://github.com/Funz/plugin-Modelica/releases/latest
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
    * Python: `Funz.startCalculators(1)`
    * R: `Funz::startCalculators(1)`
    * bash/cmd.exe: launch `Funz/FunzDaemon.sh` or `Funz/FunzDaemon.bat`

  ---

  You can now check that these computers are well setup by running basic example on your own, that will use one of the local network computers:

  * Python: `Funz.Run(model="Modelica",input_files="NewtonCooling.mo")`
  * R: `Funz::Run(model="Modelica",input.files="NewtonCooling.mo")`
  * bash/cmd.exe: `./Funz.sh Run -m Modelica -if samples/NewtonCooling.mo` or `./Funz.bat Run -m Modelica -if samples/NewtonCooling.mo` 
