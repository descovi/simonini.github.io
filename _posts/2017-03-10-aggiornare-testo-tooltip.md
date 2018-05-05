---
layout: post
title: Come creare un tooltip con bootstrap e come aggiornarlo al click.
date: 2017-03-16 08:04:49 +0200
categories: bootstrap javascript html
redirect_to: https://simonini.netlify.com/aggiornare-testo-tooltip.html
---

Per aggiungere un tooltip con boostrap è sufficente aggiungere un data attribute e un title attribute.

Di seguito un esempio in html classico.

```html
<div data-toggle="tooltip" title="Messaggio del tooltip">
Testo sul quale far apparire il tooltip
</div>
```

Un esempio con ruby haml:

```haml
 link_to 'esempio di testo sul quale far apparire il tooltip',
         'percorso di destinazione (esempio articles_path)',  
          data:  { toggle: "tooltip", placement: "left" }
```

Per poter aggiornare il tooltip al click si può usare il seguente javascript:

```javascript
$('elemento-selezionato').attr('data-original-title', 'Caricamento in corso...')
                         .tooltip('show');
```
