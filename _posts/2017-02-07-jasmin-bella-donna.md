---
layout: post
title: Jasmine bella donna.
date:   2017-02-10 7:34:49 +0200
categories: javascript rails
---

## Seducente Jasmine

Jasmine è una bella donna. È viola. Ti dice cosa è giusto e cosa è sbagliato.  
Jasmine è una libreria di test per javascript.  
Jasmine è fatta per Rails.  
Io ancora non la uso.  
Sono timido ma lei è li.  
Con gli occhi seducenti [verdi scuro della sua documentazione](https://jasmine.github.io/2.0/introduction.html).

## Come installo jasmine?

Jasmine si installa utilizzando il solito Gemfile.
All'interno del gruppo _test_ e _development_ aggiungere le seguenti righe:

{% highlight ruby %}
group :test, :development do
  gem 'jasmine-rails'
  gem 'jasmine-jquery-rails'
end
{% endhighlight %}

Aggiungete i gruppi se già non ci sono.  
Da terminale eseguire le magiche parole:

{% highlight bash %}
bundle install
{% endhighlight%}

## Come faccio a vedere il risultato dei test?

1. Avvia il server rails.
2. Vai su [/specs](http://localhost:3000/specs)

La gemma crea una rotta /specs dove si possono vedere i risultati dei test.
Comodo e veloce.
Potete anche usare la linea di comando ma a me al momento non interessa.

## Come posso testare la manipolazione del dom?

Con l'utilizzo di [jasmine-fixtures](https://github.com/searls/jasmine-fixture).
Jasmine-fixture una [volta installato](#installa-fixtures) si usa nel seguente modo:

{% highlight javascript %}
affix("div.test");
{% endhighlight %}

Potete annidare gli elementi che create. Come?

{% highlight javascript %}
affix("div.test").affix("div.div-annidato");
{% endhighlight %}

Facile facile. Un esempio più strutturato?
Eccolo:

{% highlight javascript %}
    child_0 = affix(".fatherquestion[data-children='1 2 3'][data-id='0']");
    child_0.affix(".inputs").affix("input[type=radio][value='yes']")
                            .affix("input[type=radio][value='no']");
{% endhighlight %}

Solitamente la preparazione del dom avviene nel file di test nella cartella spec/javascripts all'interno del file argomento\_del\_tuo\_test\_spec.js.
Se vuoi utilizzare la stessa struttura più volte lo puoi fare dentro il metodo _before\_each_.
Esempio:

{% highlight javascript %}
describe('TuoOgetto', function(){
  beforeEach(function() {
    affix(".div-ripetuto-per-ogni-test");
  })
})
{% endhighlight %}


## <a name="installa-fixtures"></a>Come si installa Jasmine Fixtures?

Per l'utilizzo all'interno di Rails:

1. Scaricare jasmine-fixture.js dalle [releases](https://github.com/searls/jasmine-fixture/releases).
2. Posizionare il file dentro /spec/javascript/helpers

Se la cartella _helpers_ non è presente va creata.

## Jquery, come aggiungo una funzione ad un elemento di JQuery?

Utilizzando il metodo extend si possono aggiungere tutti i metodi che si ritengono opportuni.

{% highlight javascript %}
jQuery.fn.extend({
  subquestions: function(e){

    var ids_of_children  = $(this).data("children").split(" ")
    var $subquestions = $('')

    ids_of_children.forEach(function(id){
      $subquestions = $subquestions.add($("#acquisition_field_row_id_"+id))
    })
    return $subquestions
  }
})
{% endhighlight %}


## Video e altro materiale

Come testare sul DOM

- [http://stackoverflow.com/questions/16163852/how-to-unit-test-dom-manipulation-with-jasmine](http://stackoverflow.com/questions/16163852/how-to-unit-test-dom-manipulation-with-jasmine)

Video che introduce all'uso di Jasmine in contesti reali di legacy code.  
La prima parte riguarda un esempio dove javascript viene usato nel classico modo (jquery e manipolazione del dom) mentre il secondo è un esempio con backbone.

- [https://www.youtube.com/watch?v=PWHyE1Ru4X0](https://www.youtube.com/watch?v=PWHyE1Ru4X0)
