---
title: Setup
permalink: /docs/setup/
---

## Network

To enable network connection between backend services and all users frontend, the following firewall rules have to be checked:
  * discovery/heartbeat feature:
    * on backend computers/clusters/...: open **19001-19004 outgoing UDP** ports,
    * on frontend computers: open **19001-19004 incoming UDP** ports,
  * data transfer: 
    * on backend computers/clusters/...: **all incoming** (easier, if possible), or one specified in 'calculator.xml' **TCP** port (see below)
    * on frontend computers: open **all outgoing** (easier, if possible), or one specified in 'calculator.xml' **TCP** port (see below)


## Frontend configuration file

The configuration file 'Funz.conf' of the frontend is located in the Funz installation path, and may be overloaded with the custom file Funz.conf, located in the user Funz home directory: 'C:\Documents and Settings\mylogin\.Funz\Funz.conf' on Windows, or '/home/mylogin/.Funz/Funz.conf' in Linux.

The following keys may be changed in these files:

* Network proxy configuration
  * `proxy.host=192.168.1.123`
    to define the HTTP proxy host or IP
  * `proxy.username=myusername`
    to define the username (if needed) for the proxy access
  * `proxy.port=8080`
    to define the proxy port
  * `proxy.domain=`
    to define the proxy domain name (if needed)
  * `proxy.password=ABCDEFGHIKL`
    the proxy password associated to the username (this field is stored in a crypted key).
* Math. engine setup
  * `R.server=R://123.456.789:6311`
    to setup the R server math engine. If this field is provided, it implies that a running Rserve instance is available at the given address & port. Otherwise, a Rserve instance will be automatically started when Funz GUI is launched.
  * `R.home=/usr/lib/R`
    to customize the R installation path when using a local R+RServe.
  * `R.source=MySource.R`
    to initialize the R sessions started sourcing the given .R file.
  * `R.load=MyData.Rdata`
    to initialize the R sessions started loading the given .Rdata file.
  * `R.verbose=true`
    to ask for an extended verbosity of the R environment

## Backend configuration file

The Funz daemon configuration XML file ('calculator.xml') holds:
  * the __list of hosts__ and network port allowed for users to run calculations,
  * the __list of codes__ available on the server,
  * [optional] __spool directory__ path,
  * [optional] __TCP port__ used for frontend file transfer,
  * [optional] __timeout__ to auto-shutdown when inactive.


Each **HOST** element contains:
  * the __network name__ of the user computer where graphical interface is installed (IP address or hostname), possibly a multicast IP (in this case, needs special configuration on frontend side),
  * __a port__((note that the same network name can be used in different HOST elements, if the port varies)) of this computer, waiting for Funz service (like 19001 19002, ...), matching the frontend configuration,


Each **CODE** elements contain:
  * the __name of the code__ to launch,
  * the __shell command__ used to launch code:
    * Using shell alias commands, even well configured in environment is highly discouraged, prefer a complete path call: `/path/to/my/startup/script.sh` instead
    * It is often preferable to modify the startup script code to adapt it to Funz. For example, applying a `dos2unix` on the input files is often necessary to limit the problems of portability of data files
  * [optionally] a backend 'cplugin' for better support of code startup, error, kill or running status report.

Then, the 'calculator.xml' file lists all hosts and codes in this format:
```xml
<CALCULATOR spool="/tmp" port="10000" timeout="3600">
   <HOST name="192.168.1.123" port="19001" />
   <HOST name="192.168.1.123" port="19002" />
 
   <HOST name="192.168.1.124" port="19001" />
   <HOST name="192.168.1.124" port="19002" />
 
   <HOST name="192.168.1.125" port="19001" />
   <HOST name="192.168.1.125" port="19002" />
   <HOST name="192.168.1.125" port="19003" />
 
   <CODE command="cristal.v1.2 -p" name="Cristal_1.2" />

   <CODE command="/opt/Funz/scripts/mcnp5" name="MCNP"
         cplugin="file:./plugins/calc/MCNP.cplugin.jar"/>
</CALCULATOR>
```


## Startup of backend service

The startup of the backend service is basically done by hand, using the '**FunzDaemon.sh**' / '**FunzDaemon.bat**' script. A more complete script, adapted for multi-CPU usage is also available for bash environment : '**FunzDaemon_start.sh**' (and 'FunzDaemon_stop.sh'), which accepts an arbitrary number of CPU as first argument, and properly manage shutdown of its subprocesses.

It is also possible to use these scripts for automatic service startup:
  * on Unix / Linux:  create a daemon startup script (in **/etc/init.d** directory for example) calling **FunzDaemon.sh** or **FunzDaemon_start.sh** : 
    ```bash
    #!/bin/sh
     
    echo " * Launching FUNZ daemon ..."
     
    FUNZ_HOME=/opt/FUNZ
     
    . /home/user/.bash_profile
    start-stop-daemon --start --chuid user --exec "$FUNZ_HOME/FunzDaemon_start.sh 4" --chdir $FUNZ_HOME > /var/log/FunzDaemon.log 2>&1 &
    echo " * FUNZ daemon running ..."
     
    exit 0
    fi
    ```
  * on Windows: you can use the [SC.exe](http://support.microsoft.com/kb/251192), to record 'FunzDaemon.bat' as a service: `sc.exe create "FunzDaemon" binPath= "C:\Program Files\Funz\FunzDaemon.bat"` (pay attention to keep space between `binpath=` and `C:\...`)

