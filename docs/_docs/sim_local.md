---
title: "Simulation on standalone computer"
permalink: /docs/sim_local/
---

The simplest way to launch a simulation is to use your own computer, where you also launch Funz commands. 

Assuming that the simulation software (say Modelica, our standard example) is already installed on your computer, you just have to:

  * intall Funz: 
    * Python: `pip install Funz`, `import Funz`
    * R: `remotes::install_github('Funz/Funz.R'); library(Funz)`
    * bash/cmd.exe: download and unzip https://github.com/Funz/plugin-Bash/releases/latest or https://github.com/Funz/plugin-Cmd.exe/releases/latest
  * install simulation plugin:
    * Python: `Funz.installModel('Modelica')`
    * R: `Funz::install.Model('Modelica')`
    * bash/cmd.exe: download and unzip plugin at https://github.com/Funz/plugin-Modelica/releases/latest
  * if needed, setup simulation script 'Funz/scripts/Modelica.sh' or 'Funz/scripts/Modelica.bat'
  * start background Funz computing daemon:
    * Python: `Funz.startCalculators(1)`
    * R: `Funz::startCalculators(1)`
    * bash/cmd.exe: launch `Funz/FunzDaemon.sh` or `Funz/FunzDaemon.bat`

  ---

  You can now check that your computer is well setup by running basic example:

  * Python: `Funz.Run(model="Modelica",input_files="NewtonCooling.mo")`
  * R: `Funz::Run(model="Modelica",input.files="NewtonCooling.mo")`
  * bash/cmd.exe: `./Funz.sh Run -m Modelica -if samples/NewtonCooling.mo` or `./Funz.bat Run -m Modelica -if samples/NewtonCooling.mo` 