---
layout: post
title: devise-multimodel-registration-path
---

Devise è molto comodo per impostare velocemente un sistema di login full optional.
Diventa più complesso quando si tratta di personalizzarne il comportanemento anche se per apparenti aspetti banali.

Ad esempio costruire una _pagina profilo_ per l'utente loggato diventa improvvisamente una cosa complessa.

I problemi di Devise che finora ho individuato sono tre:
  
- operare in questo modo a mio parere ondeggia pericolosamente al di fuori delle tipiche convenzioni di Rails.

- l'uscita dai binari obbliga lo sviluppatore a sfogliare in continuazione il wiki di devise che in questo caso risulta anche obsoleto (ed è il motivo principale per cui mi sto appuntando queste cose; infatti la soluzione 1, quella in apparenza più semplice, nasconde delle insidie la cui soluzione è praticamente nasconsta nella soluzione 2).

- A livello di esperienza utente, il default di devise con l'obbligo di inserire due volte la password per ogni modifica è davvero noioso e poco proponibile come struttura standard per un cliente (e anche dal mio punto di vista).

- l'introduzione dei strong parameters ha ulteriormente complicato il percorso di customizzazione.

__I vantaggi__ dall'altra parte __sono innumerevoli__:

- Registrazione secondo gli standard di sicurezza

- Recupero password

- Attivazione via email

- Helper utilizzabili facilmente ovunque nell'applicazione

- Relativa facilità nell'introdurre il login facebook/google

Mi sono quindi adattato alla situazione per trasformarse il default di devise in qualcosa di più standard seperando in due schermate la modifica dei dati personali e in un altra ciò che riguarda la password.

Ho due risorse seperate per il sistema di login. Quindi non userò lo standard _user_ ma in questo caso il modello di riferimento si chiama _agent_.

## View

Per la pagina dell'utente, il codice della view va cambiato definendo una diversa _path_ e un diverso _method_. Da

    # devise/shared/links.html.erb
    simple_form_for(resource, as: resource_name, url: registration_path(resource_name), html: { method: :put })

a
    
    # devise/shared/links.html.erb
    simple_form_for(resource, as: resource_name, url: agent_registration_path(), html: { method: :patch })

In questo modo si può creare un semplice link nella view:
    
    # layouts/agent-logged.haml
    %li= link_to 'user', edit_agent_registration_path, 'Profilo', 'devise'

Dalla view di devise togliere password e password_confirmation per avere una pagina meno tediosa. Si costruirà poi una pagina specifica per cambiare la password.

    # devise/registrations/edit.html.erb
    <%= simple_form_for(resource, as: resource_name, url: agent_registration_path(), html: { method: :patch } do |f|) %>
      <%= f.input :name %>
      <%= f.input :surname %>
      <%= f.input :email, required: true, autofocus: true %>
      <%= f.button :submit %>

## Controller

Per poter effettuare modifiche tramite devise al profilo è necessario personalizzare il controller di devise.
    
    # controllers/Agent/registrations_controller.rb
    class Agent::RegistrationsController < Devise::RegistrationsController
      protected
      def update_resource(resource, params)
        resource.update_without_password(params)
      end
    end

## Routes

Per poter utilizzare il controller che abbiamo appena definito personalizzare il routing standard:
    
    devise_for :agents, controllers: {registrations: 'agent/registrations'}
