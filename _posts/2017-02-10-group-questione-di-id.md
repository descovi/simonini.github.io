---
layout: post
title: Perchè .group() non funziona più con mysql
date: 2017-02-10 10:16:49 +0200
categories: rails
redirect_to: http://simonini.netlify.com/group-questione-di-id.html
---

Ok, hai un database mysql e hai bisogno di sapere quali sono i nomi dei tuoi utenti.
E lo vuoi sapere partendo dalla variabile @comments.
E vuoi evitare di avere nella collezione gli utenti ripetuti.
Una volta con ActiveRecord e mysql potevi fare così:

{% highlight ruby %}
@comments.group(:user_id)
{% endhighlight %}

Se usi mysql 5.7.5 o successivi non puoi più.
Verrà generato un errore con scritto qualcosa del genere:

{% highlight  sql %}
# SELECT list is not in GROUP BY clause and contains nonaggregated column 'nome_colonna' which is not functionally dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by
{% endhighlight %}

Perchè?
Il problema è che mysql in precedenza barava rispetto agli standard mysql e decideva da se in modo _magico_ quali record restituire.
Ora non è più così ed è necessario allinearsi agli standard ed evitare questa tecnica.
