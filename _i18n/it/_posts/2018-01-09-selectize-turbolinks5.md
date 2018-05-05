---
layout: post
title: Selectize e Turbolinks
date: 2018-01-08 14:30:49 +0200
categories: rails
---

Integrare selectize e turbolinks5 assieme può essere più difficile rispetto a quel che appare inizialmente.

Infatti il problema non è immediatamente evidente.

Non ci si accorge subito ma andando avanti e indietro (utilizzando le frecce del browser ad esempio)
si può arrivare ad ottenere la "duplicazione" dei campi di selectize.

Per gestire in modo opportuno la faccenda è sufficente impostare i listener anche "before-cache"
e cancellare i campi selectize prima di rilanciare il metodo selectize.

<script src="https://gist.github.com/simonini/c237df3770b223acf3c3de6c3cd38226.js"></script>

A quanto ho letto il problema si presenta anche con datatables.

Fonte: [Turbolinks 5 e Datatables](https://m.phillydevshop.com/turbolinks-5-and-datatables-a882c29d6eff)
