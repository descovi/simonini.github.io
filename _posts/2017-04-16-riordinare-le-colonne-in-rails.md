---
layout: post
title: Riordinare le colonne in Rails
date: 2017-04-16 19:02:49 +0200
categories: rails migration migrazioni rename
---

Si possono ordinare oppure spostare le colonne di un database in Rails tramite l'utilizzo delle migrazioni?

Si, usando il metodo __change_column__ e la chiave __after__.
La soluzione del problema non è semplice poichè non c'è traccia dell'argomento [nella guida ufficiale di rails](http://guides.rubyonrails.org/) e neppure nel codice sorgente.

Andando a scavare su stackoverflow ho trovato la soluzione che risiede nel file: _schema\_definitions.rb_.
All'interno è possibile vedere l'utilizzo del metodo __chage_column__ al fine di spostare l'ordine delle colonne:
https://github.com/rails/rails/blob/5-0-stable/activerecord/lib/active_record/connection_adapters/abstract/schema_definitions.rb

Ecco un esempio concreto di utilizzo del codice per cambiare l'ordine delle colonne:
{% highlight ruby %}
change_column :tabella, :nome_colonna_da_spostare, :tipologia, after: :nome_colonna_dopo_la_quale_posizionarsi
{% endhighlight %}

A quanto pare questo parametro non è utilizzabile con i database postgresql ma non l'ho sperimentato direttamente poichè normalmente uso mysql o sqlite.

Tra le varie fonti:
[Stackoverflow - Come riordinare le colonne](http://stackoverflow.com/questions/18899011/rails-4-migration-how-to-reorder-columns)