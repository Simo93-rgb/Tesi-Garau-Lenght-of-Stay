# Piano di Revisione Slide
L'idea è di aumentare la leggibilità delle slides aumentando vistosamente la quantità di spazio "bianco" ma senza rinunciare alla dimensione del font. Molte slide sono "dense" e questo rompe drasticamente la fruibilità durante la presentazione. 

## Approccio del lavoro
Lavorerai una singola slide alla volta, seguendo delle precise istruzioni che ti verranno fornite nei successivi paragrafi. L'elenco dei paragrafi non sarà numericamente identico al numero di slide in quanto alcune sono già ben impaginate. 

Il lavoro sulla singola slide si dichiarerà concluso SE E SOLO SE io avrò dato specificatamente il comando "Fatto" o sinonimi. 

Il contenuto delle slides è già ben scritto quindi non dovrai mai riformulare frasi o concetti. Questo è un lavoro di "taglia e cuci" dove andremo a ridistribuire i contenuti. 

## Riferimenti alle slide

Ora partiamo con i commenti delle singole slides. Ricorda che il titolo dei paragrafi che seguono sono corrispondenti al titolo della slide di cui discuteremo.

### Dataset e la Sfida dello Sbilanciamento
 Va semplicemente eliminata, ma per comodità l'ho già commentata io. Questo paragrafo è solo di contesto per permetterti di sapere che non è un errore mio che non ci sia la slide.

### Il Modello: BERT e $\alpha$-Balanced Focal Loss 
Questa slide è densa in modo esasperato, dobbiamo sicuramente dividerla in due. Qui creo due sotto-paragrafi così hai chiaro come deve essere divisa. 

#### BERT
La prima slide deve contenere la parte di BERT e per questioni di impaginazione ruba l'idea di layout che ha la slide "Il Problema: Length of Stay (LOS)". Otterrai quindi una slides che presenta `bert-base-uncased` e i 3 punti dell'elenco puntato. 

#### $\alpha$-Balanced Focal Loss 
La seconda parte della slide invece dovrà contenere unicamente la parte di Focal Loss. Siccome è un contenuto molto matematico e denso, vorrei togliere la ripetizione della formula matematica che tanto è già presente nella immagine. Vorrei togliere anche la sezione che mostra i valori di $\gamma$ e $\alpha$ e lasciare solo la parte del fattore che abbatte il gradiente. La didascalia della immagine è troppo grossa e lunga, andrebbe tolta. Se hai un suggerimento per lasciare come commento il comportamento al variare di $\gamma$ scrivilo qui sotto come sottoparagrafo a parte ma non implementarlo. 

### L'Approccio Vincente
Elimina l'intero riquadro in basso e centra meglio i due box che mostrano la conversione da XES a Storytelling. 

### Risultati: Balanced Accuracy 88%
Il titolo deve essere solo "Risultati". Elimina completamente il box di "tradeoff clinico". La tabella dei risultati e la matrice di confusione possono rimanere impilate ma adesso va centrato tutto in centro slide. 

### Il Problema della Black-Box in Sanità
Il titolo diventa "Tecniche di XAI per modelli profondi". Elimina il box "il paradosso della black-box" poiché lo dirò a voce. Commenta la didascalia con la soluzione. Aggiungi all'elenco puntato la parte della didascalia dell'immagine che parla di metodi classici. Lascia come didascialia della immagine la frase "confronto visivo diversi algoritmi di XAI". 

### Integrated Gradients e la Baseline [PAD]
Elimina completamente i due box: "Verifica Convergenza" e "Ottimizzazione Hardware". Riposiziona il testo rimanente su tutta la larghezza andando ad avere una impaginazione simile alla slide "Il Problema: Length of Stay (LOS)". 

### La Sfida: Aggregazione dei Sub-Token
Qui manca un nodo allo schema, bisogna aggiungere un nodo finale in cui i nodi "Cardiology" e "visit" confluscono in "Cardiology visit". Come per gli altri nodi va riportato anche lo score che è di `+39.2`. Assicurati che, in altezza, il centro dello schema corrisponda con il centro del blocco sulla sinistra. 

### Costi di Deployment
Metti la parte di elenco hardware come didascalia della tabella. Togli il box "Metriche Wall-Clock" e assicurati che la tabella sia ben centrata nella pagina. 

### Conclusioni e Sviluppi Futuri
Qui bisogna dividere in 3 slide e come precedentemente fatto, farò un sottoparagrafo per ogni slide. 

#### Conclusioni
Ritaglia la parte di "Risultati e Lezioni Apprese" e mettila in una slide isolata, insieme anche al box "Limite Epistemologico" ma cambia quel titolo perché quella parola non mi piace per niente. 

#### Sviluppi Futuri
Prendila così come è e facci una slide

#### Saluto finale
Semplicemente una slide con "Grazie a tutti per l'attenzione"

 