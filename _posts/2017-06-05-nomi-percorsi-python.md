---
layout: post
title: Gestire nomi e percorsi in Python.
date: 2017-06-05 20:30:49 +0200
categories: python
---

## I percorsi dei file con Python

Per lavorare con i percorsi in Python è necessario usare il modulo os.path.

## Cosa è importante sapere?

### La variable \_\_file\_\_ 

La variabile **\_\_file\_\_** contiene il percorso del file che sta eseguendo in quel momento il codice Python.
Il percorso ottenuto _non è assoluto_ ma relativo rispetto a dove viene lanciato lo script python.

Per esempio 

1) creo un file esempio.py e lo salvo in questa posizione:

    /Users/ale/script/esempio.py

2) all'interno del file scrivo questo codice:

    print(\_\_file\_\_)

3) Salvo il file e vado nel terminale ed eseguo il seguente semplice comando:

    python /script/esempio.py

4) L'output generato sarà il seguente:

    /script/esempio.py

La variabile \_\_file\_\_ è una delle variabili speciali di python.  
I 2 underscore precedenti alla parola file e successivi servono ad evitare che lo sviluppatore sovrascriva questi valori in maniera fortuita/casuale.

## Come ottengo il percorso _reale_?

### Con il metodo: os.path.realpath

Una volta ottenuto il nome del file è solitamente necessario ricostruire l'intero percorso.  
Per poterlo ricostruire c'è il metodo __os.path.realpath__.

Esempio di utilizzo:

    print(os.path.realpath(__file__))

Output:

    /Users/ale/script/esempio.py

Come si vede l'output contiene l'intero percorso per arrivare al file.

## Come ottengo il percorso della directory?

### Con il metodo: os.path.dirname(percorso\_del\_file)

Ottenuto il nome del file, spesso c'è bisogno di conoscere la directory che contiene il file.
Per poterlo fare si può usare il metodo __os.path.dirname()__.

Esempio di utilizzo:
    
    percorso = "/directory/app.js"
    print(os.path.dirname(percorso))

Output:
    
    /directory/

Dall'esempio è chiaro dedurre il meccanismo di funzionamento del metodo. Il metodo utilizzato su un percorso __restituisce la directory__ che contiene il file.