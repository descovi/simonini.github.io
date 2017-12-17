---
layout: post
title: Configurare Amazon S3 con Paperclip
date: 2017-12-16 14:30:49 +0200
categories: rails
---

# Come si configura l'integrazione tra Amazon S3 e Paperclip?

## Configurazione Amazon S3:

- creare un bucket su amazon s3 attraverso il pannello apposito.
- creare un identità IAM che ti darà:
  - un identificativo (access_key_id)
  - un segreto (secret_access_id)
- aggiungere una configurazione specifica per i percorsi delle immagini relative al posizionamento in Europa

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
