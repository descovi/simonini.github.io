---
layout: post
title: Personalizzare Devise per creare una pagina profilo
categories: rails
---

# Vantaggi e svantaggi nell'uso di Devise e personalizzazione del comportamento

Devise è molto comodo per impostare velocemente un sistema di login full optional.
Diventa più complesso quando si tratta di personalizzarne il comportamento.

Ad esempio costruire una _pagina profilo_ per l'utente loggato diventa improvvisamente una cosa complessa se si decide di seguire il wiki di Devise.

I problemi di Devise e del suo wiki che finora ho individuato sono 4:

- operare secondo le indicazioni del wiki di Devise ondeggia pericolosamente al di fuori delle tipiche convenzioni di Rails.

- l'uscita dai binari obbliga lo sviluppatore a sfogliare in continuazione il wiki di devise che in questo caso risulta anche obsoleto (ed è il motivo principale per cui mi sto appuntando queste cose; infatti la soluzione 1, quella in apparenza più semplice, nasconde delle insidie la cui soluzione è praticamente nascosta nella soluzione 2).

- Il comportamento di default di devise con l'obbligo di inserire due volte la password per ogni modifica è davvero noioso e poco proponibile come struttura standard per un cliente (e anche dal punto di vista dell'esperienza dell'utente).

- l'introduzione dei strong parameters di rails 4 e successivo ha ulteriormente complicato il percorso di personalizzazione.

Il percorso suggerito all'interno del wiki per personalizzare la pagina dell'utente lo trovo decisamente faticosp e inutilmente complesso.

Ad un certo punto si fa prima ad utilizzare rails in modo standard e lasciare a devise "solo" la parte di registrazione e login bypassandolo per quanto riguarda la pagina di editing dell'utente stesso.

Voglio comunque ricordare e ricordarmi gli innumerevoli __vantaggi__ nell'uso di Devise:

- Registrazione secondo gli standard di sicurezza

- Recupero password

- Attivazione via email

- Helper utilizzabili facilmente ovunque nell'applicazione

- Relativa facilità nell'introdurre il login facebook/google

Di seguito racconto come all'inizio, da bravo scolaretto, mi sono lasciato guidare dal wiki e di come abbia trovato in seguito una strada che reputo migliore oltre che più semplice a livello di gestione del codice.

## Introduzione

Ho due risorse seperate per il sistema di login. Quindi non userò lo standard _user_: in questo caso il modello di riferimento si chiama _agent_.

## View

Per la pagina dell'utente, il codice della view l'ho cambiato definendo un _path_ ed un _method_ differente. Da

{% highlight haml%}
# devise/shared/links.haml
= simple_form_for(resource, as: resource_name, url: registration_path(resource_name), html: { method: :put })
{% endhighlight %}

a

{% highlight haml %}
# devise/shared/links.haml
= simple_form_for(resource, as: resource_name, url: agent_registration_path(), html: { method: :patch })
{% endhighlight %}

In questo modo posso creare un semplice link nella view:

{% highlight erb %}
# layouts/agent-logged.haml
%li= link_to 'user', edit_agent_registration_path, 'Profilo', 'devise'
{% endhighlight %}

Dalla view di devise togliere password e password_confirmation per avere una pagina meno tediosa. Si costruirà poi una pagina specifica per cambiare la password.

{% highlight erb %}
# devise/registrations/edit.html.erb
<%= simple_form_for(resource, as: resource_name, url: agent_registration_path(), html: { method: :patch } do |f|) %>
  <%= f.input :name %>
  <%= f.input :surname %>
  <%= f.input :email, required: true, autofocus: true %>
  <%= f.button :submit %>
{% endhighlight %}

## Controller

Per poter effettuare modifiche tramite il controller di devise al profilo è necessario personalizzarlo.

{% highlight ruby %}
# controllers/Agent/registrations_controller.rb
class Agent::RegistrationsController < Devise::RegistrationsController
  protected
  def update_resource(resource, params)
    resource.update_without_password(params)
  end
end
{% endhighlight %}

## Routes

Per poter utilizzare il controller che abbiamo appena definito è necessario personalizzare il routing standard:

{% highlight ruby %}
devise_for :agents, controllers: {registrations: 'agent/registrations'}
{% endhighlight %}

Giunti a questo punto la pagina di modifica del profilo è diventato qualcosa di più accessibile per l'utente.

MA

Se volessi dare la possibilità all'utente di modificare la propria password? Lo stesso wiki di devise suggerisce di creare una azione personalizzata all'interno del nostro controller.

A questo punto ho capito che in generale è molto più semplice evitare del tutto devise ed utilizzare la azione edit per le modifiche "standard" del profilo e una azione specifica invece per la modifica della password.

Ho quindi anullato le modifiche fatte in precedenza e sono tornato agli amati binari.

In questo modo si evita di fare "hacking" sulle configurazioni di devise e allo stesso tempo si evita la fuoriuscita dai binari.

Chiaramente bisogna stare attenti che l'utente possa solo modificare se stesso e non altri profili, ma qui almeno si torna su normali e chiare convenzioni tipiche di Rails.

Alla fine il mio routing diventa qualcosa del genere:

{% highlight ruby %}
resources :agents, only: [:new, :create, :show, :edit] do
  collection do
    get 'edit_update_password'
    patch 'update_password'
  end
end
{% endhighlight %}

In questo modo ho due schermate una per la modifica delle password ed una per la modifica del profilo.
