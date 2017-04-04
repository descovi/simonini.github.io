---
layout: post
title: Riordinare le colonne in Rails
date: 2017-04-04 09:02:49 +0200
categories: rails migration migrazioni rename
---

Si possono riordinare le colonne in Rails tramite le migrazioni?

Si, usando il metodo __change_column__ e la chiave __after__.
Tutto ciò non è documentato ne nella guideline ne nelle api ma è visibile il parametro nelle sorgenti:
https://github.com/rails/rails/blob/5-0-stable/activerecord/lib/active_record/connection_adapters/abstract/schema_definitions.rb

{% highlight ruby %}
change_column :tabella, :nome_colonna_da_spostare, :tipologia, after: :nome_colonna_dopo_la_quale_posizionarsi
{% endhighlight %}

A quanto pare questo parametro non è utilizzabile con i database postgresql ma non l'ho sperimentato direttamente poichè normalmente uso mysql o sqlite.

Tra le varie fonti:
[Stackoverflow - Come riordinare le colonne](http://stackoverflow.com/questions/18899011/rails-4-migration-how-to-reorder-columns)