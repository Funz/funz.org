---
title: Install
permalink: /docs/install/
---

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
  * for Windows: [cmd.exe](https://github.com/Funz/plugin-Cmd.exe/releases/latest)
  * for Linux & OSX: [bash](https://github.com/Funz/plugin-Bash/releases/latest)
* then add all required plugins:
  * get 'plugin-*.zip' file in previous releases
  * put and unzip in'Funz/plugins/io' directory
* and add all required algorithms:
  * get 'algorithms-*.zip' file in previous releases
  * put and unzip in'Funz/plugins/doe' directory
