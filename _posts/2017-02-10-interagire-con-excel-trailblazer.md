---
layout: post
title:  Prove con trail blazer e cell
date:   2017-02-10 08:34:49 +0200
categories: rails ruby trailblazer cell
---

# Prima interazione
La mia prima interazione con __Trailblazer__ / __Cell__ ha __funzionato__.  
Il refactoring di una vista condivisa da due view mi ha dato ottime sensazioni.  
Capisco che la _sensazione_ possa essere un cattivo criterio di giudizio.  
Cerco di spiegarmi meglio.  
Tramite l’uso di _cell_ l’esperienza è stata quella di avere finalmente sotto controllo alcune logiche prime delegate completamente alla view.  
La sensazione di controllo non deriva da una mia rinnovata virtù in quanto programmatore ma dall’essere costretto a delle logiche di buona programmazione da parte del framework.  
L’effetto è lo stesso che da Rails all’inizio: __Trailblazer da un posto alle cose__.  
Lo spiega molto bene anche nel libro l’autore stesso.  
Sapere che puoi riutilizzare una partial per un pulsante ad esempio è diverso dall’essere "benevolmente costretti" dal framework perchè è la struttura anche dei file ad aiutarti oltre all'ampio uso di classi.

# Seconda interazione
Durante la seconda interazione sto facendo molta più fatica. In particolare mi sono accorto di aver fatto un uso eccessivo del _classismo_ di _cell_ e di essermi forse spinto troppo presto ad implementare per intero trailblazer. In particolare adesso potrebbe risultare anche più difficile (ma anche più stimolante) per via delle release 2.0.  
Il discorso nel suo insieme rimane comunque molto stimolante. L'idea di lanciare un __comando chiuso__ al posto di richiamare un "insieme di cose" (struttura mvc senza livelli ulteriori come prevista da Rails) mi è da stimolo per continuare l'esplorazione di questa settimana.

# Terza interazione e ritiro
Il rapporto è stato di sicuro importante e stimolante e mi ha fatto intravedere le ragioni sia dell'autore di Trailblazer che di DHH (autore di Rails).  
Durante il percorso ho sicuramente esagerato e in grossisima parte __sono pentito di aver gestito fuori dai canoni railsiani il tutto__.
In particolare se è vero che Trailblazer da un posto alle cose è anche vero che conduce ad un __annidamente spesso esagerato e rindondante__.  
Con __meno righe__ e con __maggiore chiarezza__ si fa tutto ugualmente con la classica via Railsiana.  
All'interno di questa considerazione c'è probabilmente anche l'abitudine pluriennale nell'uso di Rails. Quindi è una considerazione da prendere con le molle ma che nel mio specifico ha le sue ragioni d'essere.  
Infine l'ultimo elemento per cui mi sono pentito definitivamente di aver introdotto cell è stato quello di dover spiegare come funzionava quella parte del software ad un ragazzo che sta imparando ad usare rails.  
Spiegargli già tutta l'architettura Railsiana aggiungendo anche Cell mi è sembrato davvero eccessivo.

# Conclusione
Insomma in sostanza per ora __non__ me la sento di consigliare di usare cell - trailblazer in produzione.

Perchè no:

- impone maggiore codice
- crea layers in più di complessità
- è un ulteriore elemento da spiegare a chi sta imparando rails
- non si implementa facilmente la cache come con le normali view
- al momento rimane una cosa ancora seguita per il 90% da un solo autore

Perchè si:

- utile per fare del refactoring delle view poichè ti permetta di analizzare le varie parti e di evidenziarne i diffetti
- da una sensazione di ordine maggiore se si evita l'eccessivo annidamento