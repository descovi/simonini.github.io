---
layout: post
title: JSON Rails e codifica
date: 2017-07-12 18:30:49 +0200
categories: rails matlab
---

## La generazione di un JSON secondo Rails

Quando, all'interno di [__Rails__](http://rubyonrails.org/), si genera un JSON tramite l'utilizzo di metodi come hash.__to\_json__ o di gemme che ne fanno uso come [JB](https://github.com/amatsuda/jb), l'output avrà dei caratteri in UNICODE pronti per essere letti da Javascript.

Questo vuol dire che ad esempio che il simbolo maggiore ( _>_ ) verrà convertito in _\u003C_.

_003C_ rappresenta nella mappa [__UNICODE__](https://it.wikipedia.org/wiki/Unicode) il carattere maggiore.

Quindi tutto torna.

La porzione _\u_ è un _escape_ di un carattere __UNICODE__ che assicura la lettura corretta da parte di __Javascript__. Allo stempo serve ad evitare possibili attacchi XSS.

In altre parole questa trasformazione è un messaggio che Javascript sa interpretare perfettamente e trasformare nel simbolo speciale relativo.
Questa operazione viene effettuata per motivi di sicurezza.

Tale conversione come default è molto comoda perchè permette di essere poi utilizzata immediatamente all'interno degli script Javascript senza troppi pensieri ulteriori.

Come al solito Rails ed i suoi strumenti sono solidi nel supportare le situazioni più utilizzate ma hanno necessità di misure precise in caso si voglia uscire dal seminato.

## I casi particolari

Cosa succede se, come nel mio caso, state creando un JSON per un altro linguaggio?  
Il mio caso riguarda [Matlab](https://it.mathworks.com/products/matlab.html).  
Tale caso mi ha obbligato ad __uscire dalle convenzioni di Rails__.  
Matlab non ha un grande supporto per il formato json e neppure un chiaro supporto per le diverse codifiche, quindi con lui bisogna parlare in modo molto chiaro e senza gli escaping di qui sopra che lo incasinano alla grande.  

Soprattutto non ho modo di accedere alle impostazioni di Matlab quindi devo farlo felice con le sue attuali impostazioni rigide.

Dopo aver provato a forzare i caratteri speciali con metodi come .encode, .force_encoding, stupide sostituzioni di carattere senza sortire alcun risultato ho capito che il problema era a monte: Rails mi generava in ogni caso un output pronto per la lettura da parte di javascript con i caratteri già pronti con gli escaping.

In particolare questa [risposta di tiredpixel su stackoverflow](https://stackoverflow.com/questions/10342552/rails-utf-8-response) è stata illuminante.
L'utente evidenzia come il metodo __to\_json__ automaticamente faccia l'escaping dei caratteri.
Al suo posto consiglia di usare __JSON.generate__(hash).
In effetti andando a vedere il sorgente di __to_json__ è evidente la volontà di preparare un JSON con escape pronti.

Alcuni [hanno protestato](
https://groups.google.com/forum/#!topic/rubyonrails-core/H94wweC9NmQ) rispetto a questo comportamento da parte del framework.
In effetti sovrascrivere il comportamento di default del metodo __.to_json__ (incluso di default nella libreria ruby) mi sembra una operazione avventurosa e rischiosa.  
Nel mio caso ho dovuto fare un enorme giro per capire quale era il punto della questione.  
Questi sono i casi che probabilmente hanno generato nella comunità una certa avversione per Rails.
Personalmente sono infastidito ma non troppo perchè ho individuato le basi del discorso rispetto alla sicurezza dello strumento.

## La soluzione

Nel mio caso alla fine ho ritenuto di utilizzare l'opzione nella configurazione di Rails perchè capisco le buone intenzioni che in questo caso hanno animato gli autori del framework e perchè in questo modo mi è facile recuperare le motivazioni che ci stanno dietro.

Nel controller un attimo prima del render finale ho inserito questa riga di codice che ha risolto ogni mio problema:

    ActiveSupport.escape_html_entities_in_json = false
