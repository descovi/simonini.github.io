---
layout: post
title:  "Responder significa controller più semplici"
date:   2017-02-10 10:34:49 +0200
categories: rails ruby responder
---

# Integrazione dei responder

L'utilizzo dei responder permette una semplificazione della scrittura dei controller con notevole risparmio di codice, chiarezza, leggibilità e quindi minor possibilità di compiere degli errori in fase di scrittura:

{% highlight ruby %}

def create
  @utente = Utente.new(params[:utente])
  respond_with(@utente)
end

{% endhighlight %}

È la stessa cosa di

{% highlight ruby %}

respond_to do |format|
  if @utente.save
    format.html { redirect_to(@utente) }
    format.xml { render xml: @utente, status: :created, location: @utente }
  else
    format.html { render action: "new" }
    format.xml { render xml: @utente.errors, status: :unprocessable_entity }
  end
end

{% endhighlight %}

È inoltre possibile utilizzare risorse annidate, personalizzare la location e lo status code in caso di successo.

Per ulteriori approfondimenti è disponibile la [documentazione ufficiale](http://edgeapi.rubyonrails.org/classes/ActionController/Responder.html).