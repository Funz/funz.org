---
title: Installation from Python
permalink: /docs/install_Python/
---

### using Python Package manager

  * {% highlight bash %}pip install Funz{% endhighlight %}, or {% highlight bash %}pip install https://github.com/Funz/Funz.py/tarball/master{% endhighlight %}
  * Then load Funz: {% highlight python %}import Funz{% endhighlight %}
  * Later, you can install more models & algorithms using:
    {% highlight python %}Funz.availableModels(){% endhighlight %}
    {% highlight python %}Funz.installModel("Modelica"){% endhighlight %}
    {% highlight python %}Funz.availableDesigns(){% endhighlight %}
    {% highlight python %}Funz.installDesign("GradientDescent"){% endhighlight %}
