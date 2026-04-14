# Piano di Revisione Slide

Questo documento traccia l'analisi slide per slide del mazzo di presentazione, annotando problemi di spazio, overflow del testo e possibili soluzioni di impaginazione.

## Metodologia
- Analisi visiva di ogni slide.
- Identificazione di margini, testo clippato, sovrapposizioni.
- Proposta di refactoring (suddivisione in 2 slide, rimozione testo verboso, ridimensionamento font o figure).

---

### Slide 1/12 — Il Problema: Length of Stay (LOS)
**Problema:** Overflow verticale netto. La colonna di sinistra è troppo capiente e l'ultimo punto elenco ("Ospedale di Alessandria — 20 DRG") viene tagliato pesantemente.
**Analisi:** La slide presenta la definizione di LOS, il box "Soglia Critica" e poi una sezione "Il dataset:". Quest'ultima sezione elenca il numero di casi (7.393) e di eventi, informazioni che vengono **perfettamente duplicate** più avanti nella slide "Dataset e la Sfida dello Sbilanciamento" (Slide 4).
**Soluzione suggerita:** Rimuovere l'intero blocco "Il dataset:" da questa slide. In questo modo si elimina la duplicazione e la colonna di sinistra rientra abbondantemente nei margini senza necessità di rimpicciolire i font.

### Slide 2/12 — I Limiti dell'Approccio Classico
**Problema:** Nessuno.
**Analisi:** L'impaginazione è ottima. Il testo nella colonna di sinistra è ben bilanciato, i riquadri e i bullet point non creano overflow. Il grafico (coda lunga) nella colonna di destra ha la giusta dimensione e la didascalia è posizionata correttamente.
**Soluzione suggerita:** Nessuna modifica necessaria.

### Slide 3/12 — L'Intuizione: il Paradigma "Storytelling"
**Problema:** Lieve wrap nel codice, ma nell'insieme è OK. Nessun overflow critico.
**Analisi:** I blocchi affiancati "Da Log XES" e "a Narrazione" sono ben proporzionati. Nel blocco di sinistra la riga `-> Complete abdomen ultrasound` va a capo in modo un po' grezzo nel riquadro, ma è perfettamente leggibile e non esce dai bordi. Il footer e l'alert block non si accavallano.
**Soluzione suggerita:** Nessuna modifica strutturale necessaria. Se si vuole pignoleggiare si può ridurre di pochissimo il font del blocco "Da Log XES", ma attualmente va bene così.

### Slide 4/12 — Dataset e la Sfida dello Sbilanciamento
**Problema:** Nessuno.
**Analisi:** Entrambe le colonne sono ben formattate e i box si inseriscono perfettamene senza andare a sbattere sui bordi. Il grafico è centrato e leggibile.
**Soluzione suggerita:** Nessuna modifica.

### Slide 5/12 — Il Modello: BERT e la Focal Loss
**Problema:** Alta densità testuale, ma nessun overflow.
**Analisi:** La slide contiene molto testo (lista puntata + spiegazione formula) ma ci sta tutta comodamente. Il margine inferiore è adeguato e non interferisce col footer.
**Soluzione suggerita:** Nessuna modifica strutturale necessaria.

### Slide 6/12 — Risultati: Balanced Accuracy 88%
**Problema:** Nessuno.
**Analisi:** Resa visiva ottima. La tabella d'impatto e la matrice di confusione si bilanciano perfettamente con il box esplicativo del trade-off clinico sulla destra. Le proporzioni evitano del tutto l'effetto "muro di testo" e prevengono il clipping.

### Slide 7/12 — Il Problema della Black-Box in Sanità
**Problema:** Overflow verticale della colonna di sinistra. La frase conclusiva ("sequenze testuali.") cade fuori dal box del testo e viene tagliata.
**Analisi:** C'è tantissimo contenuto: intro, alertblock, elenco puntato "Già disponibili", e infine la soluzione. La densità verticale è accentuata da diversi `\vspace` inseriti manualmente nel codice LaTeX.
**Soluzione suggerita:** Ridurre o eliminare alcuni `\vspace` espliciti nella colonna di sinistra tra un blocco e l'altro, oppure stringere l'elenco puntato "Già disponibili" rimuovendone un punto, per far salire la riga finale ed evitare il clipping.

### Slide 8/12 — Integrated Gradients e la Baseline [PAD]
**Problema:** Grave overflow verticale sulla sinistra. La frase sulla convergenza ("picchi fino a m=5500 per tracce molto lunghe") è pesantemente tagliata fuori dal bordo.
**Analisi:** Stessa dinamica della slide precedente. Troppi blocchi sovrapposti verticalmente (Intuizione, Formula matematica, Baseline, Nota sulla convergenza). L'equazione di Riemann prende molto spazio.
**Soluzione suggerita:** Spostare il paragrafo "Convergenza adattiva" in basso a destra (sotto o dentro a "Ottimizzazione Hardware"), oppure asciugare notevolmente il testo introduttivo riducendo le spaziature interne.

### Slide 9/12 — La Sfida: Aggregazione dei Sub-Token
**Problema:** Nessuno.
**Analisi:** La slide che prima presentava problemi col diagramma TikZ ora è perfetta grazie allo scalebox inserito in precedenza. Anche la colonna sinistra è ben bilanciata e non genera alcun overflow in basso.
**Soluzione suggerita:** Nessuna modifica.

### Slide 10/12 — Il Cruscotto Clinico: Reparto 701 Cardiochirurgia
**Problema:** Nessuno.
**Analisi:** Sebbene ci siano moltissime informazioni (due grandi figure affiancate), l'impaginazione orizzontale salva lo spazio. Didascalie e immagini rientrano perfettamente nei bordi della slide.
**Soluzione suggerita:** Nessuna modifica strutturale.

### Slide 11/12 — Fattibilità Aziendale e Costi di Deployment
**Problema:** Nessuno.
**Analisi:** Tabella a sinistra e box testuale a destra sono molto eleganti, bilanciati e in perfetto allineamento.
**Soluzione suggerita:** Nessuna modifica.

### Slide 12/12 — Conclusioni e Sviluppi Futuri
**Problema:** Nessuno.
**Analisi:** Layout ottimale, il testo "Grazie per l'attenzione." spicca chiaramente a destra e la colonna di sinistra è ben distanziata dal bordo inferiore.
**Soluzione suggerita:** Nessuna modifica.

### Slide 13 — (Backup) Hardware e Hyperparameter Tuning
**Problema:** Overflow verticale del grafico in basso a destra.
**Analisi:** L'immagine sulle metriche di loss (Train vs Val) ha dimensioni verticali eccessive. Supera il margine in basso, al punto che l'asse X e le etichette vengono tagliate dal bordo inferiore dello schermo.
**Soluzione suggerita:** Rimpicciolire l'immagine settando un parametro `[height=...]` oppure `[width=0.8\linewidth]` più stretto per farla salire in sicurezza.

### Slide 14 — (Backup) L'Esperimento Fallito: Data Augmentation
**Problema:** Nessun clipping intrinseco o conflitto.
**Analisi:** La colonna di sinistra è molto densa verso il basso, ma non impatta la leggibilità né collide con nulla, godendo dell'assenza di footer numerato per gli allegati.
**Soluzione suggerita:** Va bene così (nessun intervento).
