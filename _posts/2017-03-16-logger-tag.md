---
layout: post
title: Log ordinati con Rails
date: 2017-03-16 18:28:49 +0200
categories: rails
---

Usando Rails è fondamentale avere _log_ chiari e ordinati.  
Uno tra gli strumenti più comodi (e poco conosciuti) è il metodo _tagged_ di logger.  
In breve si usa nel seguente modo:  

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

Nel grande flusso di informazioni di un applicazione Rails è necessario sapere come filtrare i dati in modo, veloce, semplice e chiaro.  
In tal senso aiuta il comando _grep_ associato a _tail_.  
Con questi comandi diventa molto più facile isolare le porzioni del log a cui si potrebbe essere interessati per motivi di debug:

{% highlight bash %}
tail -f log/production.log | grep api
{% endhighlight %}

## Fonti

- [Api Ufficiali ~ Ruby On Rails](http://api.rubyonrails.org/classes/ActiveSupport/TaggedLogging.html)
- [Keeping Your Logs From Becoming an Unreadable Mess](http://www.justinweiss.com/articles/keeping-your-logs-from-becoming-an-unreadable-mess/)