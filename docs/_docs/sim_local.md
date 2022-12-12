---
title: "Simulation on standalone computer"
permalink: /docs/sim_local/
---

The simplest way to launch a simulation is to use your own computer, where you also launch Funz commands. 

_It is almost what is done in these demo notebooks:_
  * Python: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Funz/funz.github.io/blob/master/docs/_docs/Funz_py_NewtonCooling.ipynb)
  * R: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Funz/funz.github.io/blob/master/docs/_docs/Funz_R_NewtonCooling.ipynb)

Assuming that the simulation software (say Modelica, our standard example) is already installed on your computer, you just have to:

  * intall Funz: 
    * Python: `pip install Funz`, then `import Funz`
    * R: `remotes::install_github('Funz/Funz.R')`, then `library(Funz)`
    * bash/cmd.exe: download and unzip [Funz-Bash.zip](https://github.com/Funz/plugin-Bash/releases/latest) or [Funz-Cmd.exe.zip](https://github.com/Funz/plugin-Cmd.exe/releases/latest)
  * install simulation plugin:
    * Python: `Funz.installModel('Modelica')`
    * R: `Funz::install.Model('Modelica')`
    * bash/cmd.exe: download and unzip [plugin-Modelica.zip](https://github.com/Funz/plugin-Modelica/releases/latest)
  * if needed, setup simulation script 'Funz/scripts/Modelica.sh' or 'Funz/scripts/Modelica.bat'
  * start background Funz computing daemon:
    * Python: `Funz.startCalculators(1)`
    * R: `Funz::startCalculators(1)`
    * bash/cmd.exe: launch backend `Funz/FunzDaemon.sh` or `Funz/FunzDaemon.bat`

  ---

  You can now check that your computer is well setup by running basic example:

  *  check that you well receive network Funz heartbeats: `nc -lu 19001` or `socat -u udp-recv:19001`
  *  launch basic calculation:
    * Python: `Funz.Run(model="Modelica",input_files="samples/NewtonCooling.mo")`
    * R: `Funz::Run(model="Modelica",input.files="samples/NewtonCooling.mo")`
    * bash/cmd.exe: `./Funz.sh Run -m Modelica -if samples/NewtonCooling.mo` or `./Funz.bat Run -m Modelica -if samples/NewtonCooling.mo` 