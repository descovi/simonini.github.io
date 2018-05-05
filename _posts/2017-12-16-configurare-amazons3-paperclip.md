---
layout: post
title: Configurare Amazon S3 con Paperclip
date: 2017-12-16 14:30:49 +0200
categories: rails
redirect_to: https://simonini.netlify.com/configurare-amazons3-paperclip/

---

# Come si configura l'integrazione tra Amazon S3 e Paperclip?

Amazon S3 è il servizio di hosting dei file che fa pagare i propri servigi a "consumo" invece che ad un prezzo fisso.

Nella maggiorparte dei casi il sistema è abbastanza conveniente sopratutto per i progetti piccoli non mi è mai capitato di spendere cifre importanti anzi mi ha spesso stupito la quasi gratuità del servizio.

La configurazione con Rails - Paperclip purtroppo non è immediata e semplice.

Trovo in particolare il pannello di configurazione di Amazon molto confusionario e pieno di opzioni per lo più inutili per il mio utilizzo.

Inoltre la configurazione tramite Rails non è per nulla agevole e ho dovuto fare diverse prove e leggere numerosi articoli.

Questi appunti rappresentano il mio attuale flusso di lavoro per configurare Amazon S3 con la gemma Paperlcip.

Paperclip per chi non lo conoscesse è la più famosa gemma per l'integrazione e il caricamento di immagini.

La configurazione di integrazione tra la gemma Paperclip e Amazon S3 l'ho divisa in due macro spezzoni.

Il primo riguarda la configurazione di Amazon S3 e il secondo riguarda la configurazione specifica per Rails.

## Configurazione Amazon S3:

1. Creare un bucket su amazon s3 attraverso il pannello apposito.
2. Creare un identità IAM che ti darà:
  - un identificativo (access_key_id)
  - un segreto (secret_access_id)
3. Aggiungere una configurazione specifica per i percorsi delle immagini relative al posizionamento in Europa

   <script src="https://gist.github.com/simonini/9d5341413d70bef99077bfe24c44bf3b.js"></script>


## Configurazione Rails:

- Aggiungere nel Gemfile:

    gem 'aws-sdk', '~> 2.3.0'

- Inserire in development e/o production la seguente configurazione:

   <script src="https://gist.github.com/simonini/948c94aeb337e44e54e5480c92907104.js"></script>

- Ovviamente è necessario impostare i segreti tramite il tool di Rails5 tramite

    EDITOR=vi rails secrets:edit

- Per poter far si che in development si possano leggere i valori che utilizza rails secrets ricorda di impostare questa regola:

    config.read_encrypted_secrets = true  

- In produzione (ad esempio su heroku) è necessario ricordarsi anche di impostare la variabile per la lettura dei secrets. La variabile:


    ENV["RAILS_MASTER_KEY"]


## Fonti:

- https://devcenter.heroku.com/articles/paperclip-s3
- http://www.starkandwayne.com/blog/rails-5-1-applications-can-be-a-lot-more-secretive-on-cloud-foundry-and-heroku/
