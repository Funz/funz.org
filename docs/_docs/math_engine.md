---
title: Math. engine
permalink: /docs/math_engine/
---

The math. engine is a **mandatory** component for the frontend. It is used for all numerical evaluations, as well as formulas or functions interpretation.
It relies on the **[R]** language, which is available through [Rsession](https://github.com/yannrichet/rsession) library, which holds many implementations:

* "true" [R](http://www.r-project.org) (3.5, 3.6 and 4.x), through [Rserve](https://github.com/s-u/Rserve) (locally spawned automatically if necessary, fully compatible with legacy R),
* [Renjin](https://www.renjin.org/) 3.5 (lower compatibility, but still very good),
* and **default** R2js, which is on-the-fly R translation to math.js, with lower compatibility but full BSD licence.

It is also possible to host this R+Rserve components on a **remote machine**, and to configure the frontend consistently using the **R.server=** key in [Frontend configuration file]({{ "/docs/setup/" | prepend: site.baseurl }}).

For that purpose:
  * [R] is available for following systems, in both 32 and 64 architectures : 
    * [Windows](https://cran.r-project.org/bin/windows/)
    * [Apple OS X](https://cran.r-project.org/bin/macosx/)
    * [Linux](https://cran.r-project.org/bin/linux/) ((We recommend to use the previous repository of R-project, instead of your distro default one. Usually, the common Linux distributions are not up-to-date and lead to some incompatibility.))
  * proceed with Rserve installation, one of these ways:
    * automatically at first startup of Funz (regular way)
    * manually using `install.packages("Rserve")` command from your R environment previously installed (**internet connection needed**)
    * manually with the [installation archive](http://cran.r-project.org/web/packages/Rserve/index.html) and then using some `install.packages("C:\tmp\Rserve_1.8-xxx.zip", repos=NULL)` command from R environment (**internet connection NOT needed**)

Alternatively, using Linux you can try the apt or yum packages manager, but take care that you must use a recent enough version of Rserve.

Once R+Rserve are installed on a erver, you can setup basic scripts:
  * '/opt/Rserve/Rserved':
    ```bash
    #!/bin/bash
    echo " * Starting Rserve ..."
    start-stop-daemon --start --chuid myuser --exec /opt/Rserve/Rserve.sh > /tmp/Rserve.log 2>&1 &
    echo " * Rserve is running."
    exit
    ```
  * '/opt/Rserve/Rserve.sh':
    ```bash
    #!/bin/bash
    /usr/bin/R CMD /usr/lib/R/bin/Rserve --vanilla --RS-conf /opt/Rserve/Rserve.conf
    ```
  * '/opt/Rserve/Rserve.conf':
    ```
    remote enable
    fileio enable
    ```
  * and then enable at startup: ''ln -s /opt/Rserve/Rserved /etc/rc2.d/S99Rserve''
