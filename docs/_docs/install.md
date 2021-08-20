---
title: Installation
permalink: /docs/install/
---

### Package manager

Installation from following package managers is supported:

* Python: {% highlight bash %}pip install Funz{% endhighlight %}
* R/devtools: {% highlight r %}devtools::install_github("Funz/Funz.R"){% endhighlight %}

Then you can install more models & algorithms using:

* Python: {% highlight python %}Funz.installModel("Modelica"){% endhighlight %}/{% highlight python %}Funz.installDesign("gradientDescent"){% endhighlight %}
* R/devtools: {% highlight r %}Funz::install.Model("Modelica"){% endhighlight %}/{% highlight r %}Funz::install.Design("GradientDescent"){% endhighlight %}


### Manual install

For conveniency, many integrated packages are available, bundled with plugins and algorithms in GH releases:

* [Excel](https://github.com/Funz/plugin-Excel/releases/latest)
* [Python](https://github.com/Funz/plugin-Python/releases/latest)
* [R](https://github.com/Funz/plugin-R/releases/latest)
* [Modelica](https://github.com/Funz/plugin-Modelica/releases/latest)
* [Cast3M](https://github.com/Funz/plugin-Cast3m/releases/latest)
* [VBS](https://github.com/Funz/plugin-VBS/releases/latest)
* [MCNP](https://github.com/Funz/plugin-MCNP/releases/latest)
* [Scale](https://github.com/Funz/plugin-Scale/releases/latest)
* [Cristal](https://github.com/Funz/plugin-Cristal/releases/latest)
* [Telemac](https://github.com/Funz/plugin-Telemac/releases/latest)

You can get 'Funz-*.zip' archive of whole working Funz directory, unzip and just follow [usage guidelines]({{ "/docs/usage_cli/" | prepend: site.baseurl }}).

If you prefer a custom installation, you can 

* start with a quite-scratch distributions:
  * e.g. for Windows: [cmd.exe](https://github.com/Funz/plugin-Cmd.exe/releases/latest)
  * e.g. for Linux & OSX: [bash](https://github.com/Funz/plugin-Bash/releases/latest)
* test that it works properly with example files (available in 'samples' directory)
* then add some other plugins you need:
  * get 'plugin-*.zip' file in any of the previous GH releases
  * unzip in 'Funz/plugins/io' directory
* add some algorithms you need:
  * get 'algorithms-*.zip' file in any of the previous GH releases
  * unzip in 'Funz/plugins/doe' directory
