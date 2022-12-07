---
title: Usage
permalink: /docs/usage/
---

## Simulations & computing

Funz is designed to make the most of whatever computing resources you could rely:

  * [Standalone]({{ "/docs/sim_local/" | prepend: site.baseurl }}) : you just use your own comuter to launch calculations
  * [Local network]({{ "/docs/sim_net/" | prepend: site.baseurl }}) : you launch calculations on many computers within the same network or cloud subnet (Funz supports the distribution task)
  * [HPC backend]({{ "/docs/sim_cluster/" | prepend: site.baseurl }}) : your calculations are launch through a standard cluster management scheduler (SLURM, SGE, PBS, OAR, ...)


## End user interface

You can use Funz through several interfaces for both Windows, OSX and Linux:

  * [Command-line usage]({{ "/docs/usage_cli/" | prepend: site.baseurl }}) 
  * [Python usage]({{ "/docs/usage_python/" | prepend: site.baseurl }}) 
  * [R usage]({{ "/docs/usage_r/" | prepend: site.baseurl }}) 
  * [Java usage]({{ "/docs/usage_java/" | prepend: site.baseurl }})

Each of these interfaces have its own requirements, in addition to Java required by Funz core itself.

As a short reminder of available commands, you can use the following cheatsheet:

[![Usage cheatsheet]({{ site.baseurl }}/docs/Clients.png)]({{ site.baseurl }}/docs/Clients.png)


