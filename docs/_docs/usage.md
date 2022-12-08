---
title: Usage
permalink: /docs/usage/
---

Funz is basically composed of the main user interface (called from Python, R, Java, bash or cmd) and its backend which supports simulations launching.

## Computing backend

Funz is designed to make the most of whatever computing resources you could rely:

  * [Standalone]({{ "/docs/sim_local/" | prepend: site.baseurl }}) : you just use your own comuter to launch simulations
  * [Local network]({{ "/docs/sim_net/" | prepend: site.baseurl }}) : you launch simulations on many computers within the same network or cloud subnet (Funz supports the distribution task)
  * [HPC]({{ "/docs/sim_cluster/" | prepend: site.baseurl }}) : your simulations are launch through a standard cluster management scheduler (SLURM, SGE, PBS, OAR, ...)


## User interface

You can use Funz through several interfaces for both Windows, OSX and Linux:

  * [Command-line usage]({{ "/docs/usage_cli/" | prepend: site.baseurl }}) 
  * [Python usage]({{ "/docs/usage_python/" | prepend: site.baseurl }}) 
  * [R usage]({{ "/docs/usage_r/" | prepend: site.baseurl }}) 
  * [Java usage]({{ "/docs/usage_java/" | prepend: site.baseurl }})

Each of these interfaces have its own requirements, in addition to Java required by Funz core itself.

As a short reminder of available commands, you can use the following cheatsheet:

[![Usage cheatsheet]({{ site.baseurl }}/docs/Clients.png)]({{ site.baseurl }}/docs/Clients.png)


