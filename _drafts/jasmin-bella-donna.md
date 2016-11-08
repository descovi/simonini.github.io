---
layout: post
title: Jasmin bella donna.
---

# Seducente Jasmine

Jasmine è una bella donna. È viola. Ti dice cosa è giusto e cosa è sbagliato.
Jasmine è una libreria per javascript.
Jasmine è fatta per Rails.
Io ancora non la uso.
Sono timido ma lei è li.
Con gli occhi seducenti [verdi scuro della sua documentazione](https://jasmine.github.io/2.0/introduction.html).

## Come installo jasmine?

{% highlight ruby %}
group :test, :development do
  gem 'jasmine-rails'
  gem 'jasmine-jquery-rails'
end
{% endhighlight %}

## Come faccio a vedere il risultato dei test?

1. Avvia il server.
2. Vai su [/specs](http://localhost:3000/specs)

La gemma crea una rotta /specs dove si possono vedere i risultati dei test.

## Come posso testare la manipolazione del dom?

Con l'utilizzo di [jasmine-fixtures](https://github.com/searls/jasmine-fixture).

{% highlight javascript %}
affix("div.test")
{% endhighlight %}

## Come si installa Jasmine Fixtures?''

Per l'utilizzo all'interno di Rails:

1. Scaricare jasmine-fixture.js dalle [releases](https://github.com/searls/jasmine-fixture/releases).
2. Posizionare il file dentro /spec/javascript/helpers

Se la cartella _helpers_ non è presente va creata.

## Come inserisco dei valori validi per ogni test?

Utilizzando il metodo beforeEach

{% highlight javascript %}
describe('TuoOgetto', function(){
  beforeEach(function(){
    // Qui dentro ci metti quello che ti piace che venga eseguito prima di ogni test
    // ad esempio:
    affix("div.test")
  })
})
{% endhighlight %}


## Video e materiale
Come testare sul DOM
http://stackoverflow.com/questions/16163852/how-to-unit-test-dom-manipulation-with-jasmine

Video che introduce all'uso di Jasmine in contesti reali di legacy code.
La prima parte riguarda un esempio dove javascript viene usato nel classico modo (jquery e manipolazione del dom) mentre il secondo è un esempio con backbone.
https://www.youtube.com/watch?v=PWHyE1Ru4X0