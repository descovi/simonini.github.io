---
layout: post
title: Appunti su Docker
date: 2017-11-17 18:30:49 +0200
categories: devops
---

## A cosa serve docker-compose?

Lo strumento **docker-compose** serve a mettere assieme i container costruiti attraverso lo strumento **docker**.
Docker compose aggrega i tuoi container utilizzando immagini diverse ed eventualmente i tuoi _dockerfile_.
Metti insieme tra loro diversi servizi per poterle gestire in modo seperato e allo stesso tempo organico.

## Come si può entrare in un container già avviato?
In un container già avviato tramite il comando _docker run_ si può entrare col comando:

    docker attach


Nel mio caso sto cercando di entrare in una macchina grafana/grafana.

In questo caso sembra non succedere nulla.

La soluzione la ho trovata in questa [domanda di stackoverflow](https://stackoverflow.com/questions/30172605/how-to-get-into-a-docker-container).

Nel mio caso per entrare correttamente all'interno della macchina grafana ho dovuto usare il comando:

    docker exec -it <id del container> bash


## Come si cancella un container?

Per cancellare un container il comando da utilizzare è:

    docker container rm <id container>


## Appunti in Italiano

Su gist ho trovato questo elenco di appunti in italiano dell'utente savez    

<script src="https://gist.github.com/savez/d91d8269e1fde5bd70e6023932a50683.js"></script>

## Fonti

- [Sito ufficiale di Docker](https://docs.docker.com/get-started/part2/#recap-and-cheat-sheet-optional)
