---
layout: post
title: Log ordinati con Rails
categories: rails
---

Usando Rails è fondamentale avere dei log ordinati.
Uno degli strumenti meno conosciuti è lo strumento di tagging:

{% highlight ruby %}
logger.tagged("Sistema di API esterno") do
  logger.debug "Inizio procedura"
  login_api "username", "password"
  logger.debug "Fine procedura"
end
{% endhighlight %}

L'output generato dal logger diventa il seguente:

{% highlight log %}
[Sistema di API esterno] Inizio procedura
[Sistema di API esterno] POST login_password
[Sistema di API esterno] Fine procedura
{% endhighlight %}

Tramite poi il comando tail di unix insieme a grep diventa molto facile isolare le porzioni a cui si potrebbe essere interessati per motivi di debug:

{% highlight bash %}
tail -f log/production.log | grep api
{% endhighlight %}

## Fonti

- [Api Ufficiali ~ Ruby On Rails](http://api.rubyonrails.org/classes/ActiveSupport/TaggedLogging.html)
- [Keeping Your Logs From Becoming an Unreadable Mess](http://www.justinweiss.com/articles/keeping-your-logs-from-becoming-an-unreadable-mess/)