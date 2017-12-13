---
layout: post
title: Login con Facebook e redirect Devise
date: 2017-07-22 18:30:49 +0200
categories: rails ruby facebook devise
---

# Il login con facebook


## Il setup all'interno dell'area sviluppatori di facebook

Nella maggior parte dei casi non è quasi mai stata l'installazione della gemma per il login con facebook a darmi noia.

Solitamente quello che più mi rallentava il flusso di sviluppo era configurare all'interno del pannello di facebook i vari valori per poter sviluppare sia in locale che in produzione senza troppe difficoltà.

Il problema è sempre stato riuscire a far funzionare correttamente il login sia nel proprio ambiente di sviluppo (localhost:3000) che in produzione (www.ilsitochevuoicostruire.it).

Spesso si arrivava alla soluzione assai scomoda di avere il login con facebook funzionante solamente in produzione.

## La soluzione

Per far funzionare il tutto si deve partire dal solito [sito degli sviluppatori di facebook](http://developer.facebook.om/).

Era da un pò che non ero obbligato a infilarmi per queste nefaste strade ma ho scoperto che per il login con facebook è oggi sufficente nel menu a sinistra individuare il prodotto __Facebook Login__ e aggiungere __localhost:3000__ all'interno della voce _URI di reindirizzamento OAuth validi_.

Magari c'è sempre stata tale voce e me ne accorgo solo ora.  
O magari qualcuno ha deciso di darci una mano.  
Fatto sta che ora è molto più semplice.  
Per evitare potenziali problemi di sicurezza è importante in ogni caso ricordarsi di rimuovere la url _localhost:3000_ una volta terminato il lavoro col famigerato __login di facebook__.

![Impostazioni facebook login](/assets/facebook_login.png)


# Redirect con Devise

## Ritornare indietro alla pagina di origine dopo il login con facebook

Per quanto riguarda la configurazione invece di Devise per Rails una cosa che mi ha lasciato sempre perplesso è la mancanza del redirect alla pagina che si stava visitando prima di procedere col login.

## La soluzione

In realtà la soluzione in questo caso è abbastanza semplice.
L'importante è non confondersi nel wiki molto grosso di [Devise](https://github.com/plataformatec/devise/wiki/How-To:-redirect-to-a-specific-page-on-successful-sign-in) che è pieno di metodi fin troppo complessi rispetto allo scopo.

<script src="https://gist.github.com/simonini/ab0552ad3b9dca6e46f0b6025337183d.js"></script>

Fonte: [How To: redirect to a specific page on successful sign-in](https://github.com/plataformatec/devise/wiki/How-To:-redirect-to-a-specific-page-on-successful-sign-in)
