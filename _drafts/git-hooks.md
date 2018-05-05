# Come impostare un aggancio (hook) con git?

Per impostare un __hook__ con git basta entrare in un repository e aggiungere il proprio script all'interno della cartella __/.git/hooks__.

All'interno di questa cartella vi sono già svariati file di esempio dai quali si possono apprendere i concetti di base dello strumento.

# Come impostare una path per tutti i repository di git?

Come appena detto git va a cercare gli _hook_ all'interno della cartella .git/hooks.

## Come possiamo cambiare questo comportamento?

Attraverso l'uso della variabile di configurazione __core.hooksPath__.

# Come si cambiano le configurazioni di git?

Ci sono diversi modi:
  - la riga di comando
  - modificare il .gitconfig presente nella home del proprio utente
  - e altri..

post-receive
