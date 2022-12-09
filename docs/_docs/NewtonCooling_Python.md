---
title: Newton cooling from Python
permalink: /demos/NewtonCooling_Python
---

We will simulate a basic cooling system follwoing Newton's law. 
For that purpose, [OpenModelica is a standard solution](https://mbe.modelica.university/behavior/equations/physical/).

_You can refer to [https://mbe.modelica.university/](https://mbe.modelica.university/) website to learn and play with Modelica PDE solver._

TLDR: you can checkout Google colab notebook for this demo:
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Funz/funz.github.io/blob/master/docs/_docs/Funz_py_NewtonCooling.ipynb)


## Requirements

  * install OpenModelica on your platform:
    * from [OpenModelica website](https://openmodelica.org), 
    * or using some package manager (eg. for debian/ubuntu): 
```bash
for deb in deb deb-src; do echo "$deb http://build.openmodelica.org/apt `lsb_release -cs` release"; done | sudo tee /etc/apt/sources.list.d/openmodelica.list
wget -q http://build.openmodelica.org/apt/openmodelica.asc -O- | sudo apt-key add -
apt update && apt install openmodelica
```
  * check that `omc` command will be recognized in your `PATH`


## Problem setup

We will then work on the 'NewtonCooling' example, which solves a basic PDE on temperature, provided in the `Funz-Modelica/samples` directory:
```
// @ref http://book.xogeny.com/behavior/equations/physical/
model NewtonCooling "An example of Newton's law of cooling"
  parameter Real T_inf=25 "Ambient temperature";
  parameter Real T0=90 "Initial temperature";
  parameter Real h=0.7 "Convective cooling coefficient";
  parameter Real A=1.0 "Surface area";
  parameter Real m=0.1 "Mass of thermal capacitance";
  parameter Real c_p=1.2 "Specific heat";
  Real T "Temperature";
initial equation
  T = T0 "Specify initial value for T";
equation
  m*c_p*der(T) = h*A*(T_inf-T) "Newton's law of cooling";
end NewtonCooling;
```
Our engineering goal is to adjust cooling convection in order to control the minimum temperature reached.
So, in order to 'Funzify' this model, we will just replace the numerical value of the convection coefficient by a parametrized expression:
<pre class="highlight"><code>// @ref http://book.xogeny.com/behavior/equations/physical/
model NewtonCooling "An example of Newton's law of cooling"
  parameter Real T_inf=25 "Ambient temperature";
  parameter Real T0=90 "Initial temperature";
  parameter Real h=<font style="background-color:rgb(255,200,0)">$convection</font> "Convective cooling coefficient";
  parameter Real A=1.0 "Surface area";
  parameter Real m=0.1 "Mass of thermal capacitance";
  parameter Real c_p=1.2 "Specific heat";
  Real T "Temperature";
initial equation
  T = T0 "Specify initial value for T";
equation
  m*c_p*der(T) = h*A*(T_inf-T) "Newton's law of cooling";
end NewtonCooling;
</code></pre>
... and now play with this 'functional' wraping ...


## Funz

### Install

Install Funz through pypi:
```bash
pip install Funz
```

Then install plugin to support Modelica I/O:
```python
import Funz
Funz.installModel('Modelica')
```

Wake up the 3 Funz 'daemons' which will provide calculation services:
```python
Funz.startCalculators(3)
```


### Basic parametric run

Launch 6 calculations for different `convection` values (0.5, 0.6, 0.7, 0.8, 0.9, 1.0):
```python
Funz.Run(model="Modelica",input_files="NewtonCooling.mo.par", input_variables={'convection':[0.5,0.6,0.7,0.8,0.9,1.0]}, output_expressions="min(T)")
```

### Algorithm-driven root finding

Find the `convection` value leading to `min(T) = 25.2` (with relative precision of 0.01 on `convection` value), using Brent root finding algorithm:
```python
Funz.installDesign('Brent')
Funz.RunDesign(model="Modelica",input_files="NewtonCooling.mo.par", input_variables={'convection':"[0.5,1.0]"}, output_expressions="min(T)", 
               design="Brent", design_options={'ytarget':25.2, 'ytol':0.01})
```
