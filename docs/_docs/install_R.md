---
title: Installation from R
permalink: /docs/install_R/
---

### using R Package manager

  * {% highlight r %}# if needed: install.packages("devtools")
devtools::install_github("Funz/Funz.R"){% endhighlight %}
  * Then load Funz: {% highlight r %}library('Funz'){% endhighlight %}
  * Later, you can install more models & algorithms using:
    {% highlight r %}Funz::available.Models(){% endhighlight %}
    {% highlight r %}Funz::install.Model("Modelica"){% endhighlight %}
    {% highlight r %}Funz::available.Designs(){% endhighlight %}
    {% highlight r %}Funz::install.Design("GradientDescent"){% endhighlight %}

