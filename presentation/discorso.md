### Slide 1: Frontespizio
**Discorso:**
"Buongiorno alla commissione. Sono Simone Garau e oggi vi presento la mia tesi dal titolo 'Explainable AI in Sanità: Storytelling e Modelli Transformer per l’Ottimizzazione del Length of Stay'. Ringrazio il mio relatore, il Professor Giorgio Leonardi."

### Slide 2: Sezione "Contesto e Motivazione"
**Discorso:**
"L'obiettivo di questo lavoro è applicare l'Intelligenza Artificiale avanzata per affrontare uno dei problemi gestionali più complessi per gli ospedali: prevedere e comprendere i ricoveri eccessivamente prolungati."

### Slide 3: Il Problema: Length of Stay (LOS)
**Discorso:**
"Il punto di partenza è il Length of Stay (LOS), ovvero la durata della degenza ospedaliera. La sua ottimizzazione è una sfida fondamentale per conciliare efficienza economica e qualità delle cure. Nel nostro studio, abbiamo identificato una soglia critica: le degenze pari o superiori ai 20 giorni sono classificate come 'long-LOS'. Questi casi generano sovraccarico di letti e fanno lievitare i costi. 
Questa tesi, sviluppata all'interno del più ampio progetto TEXLOS, propone un sistema innovativo di intelligenza artificiale per l'individuazione dei colli di bottiglia logistici responsabili di queste degenze prolungate partendo dai log degli eventi.
L'idea di base che vogliamo trasmettere è quella di addestrare una rete capace di discriminare i pazienti a lunga degenza da quelli normali, per poi aprire questa 'scatola nera' e interrogare a posteriori le sue scelte. Vogliamo capire ed estrarre esattamente quali azioni cliniche hanno portato a questa classificazione."

### Slide 4: I Limiti dell'Approccio Classico
**Discorso:**
"L'approccio classico a questo problema, il Process Mining, mostra però dei limiti sui dati clinici reali. Mentre estrae efficacemente grafi deterministici per processi stabili, fatica di fronte all'alta variabilità, al rumore e alle complesse dipendenze temporali tipiche dei percorsi di cura ospedalieri. 
La nostra conclusione è stata chiara: per estendere e superare i limiti delle metodologie tradizionali, serviva un approccio capace di catturare il contesto semantico. Per questo motivo abbiamo introdotto il paradigma dello Storytelling, traducendo i log in narrazioni, per alimentare un classificatore Transformer addestrato con Focal Loss per mitigare lo sbilanciamento. Solo così possiamo catturare realmente il significato bidirezionale di una sequenza clinica."

### Slide 5: Sezione "Metodologia"
**Discorso:**
"Per rispondere a questa esigenza, abbiamo sviluppato una pipeline metodologica innovativa che integra NLP e Deep Learning."

### Slide 6: Dataset e la Sfida dello Sbilanciamento
**Discorso:**
"Abbiamo lavorato su un dataset reale composto da 7.393 casi e oltre 88.000 eventi. L'analisi esplorativa ha subito evidenziato una prima grande sfida: il forte sbilanciamento delle classi. L'89% dei casi appartiene alla classe normale, mentre solo l'11% ricade nel long-LOS. Come mostrano i boxplot, i casi critici presentano una variabilità di eventi molto maggiore. In questo scenario, una funzione di costo standard tenderebbe a far collassare il modello sulla classe maggioritaria."

### Slide 7: Il Modello: BERT e Focal Loss
**Discorso:**
"Per gestire lo sbilanciamento abbiamo integrato la Focal Loss al cuore predittivo del nostro sistema, ovvero un modello *bert-base-uncased* (preferito ai modelli clinici pre-addestrati a causa del 'vocabulary mismatch' con i nostri log locali). La Focal Loss abbatte il gradiente degli esempi facili, forzando il modello a concentrarsi e imparare i pattern dei casi rari del long-LOS."

### Slide 8: La Ricerca dell'Embedding Ideale
**Discorso:**
"Una delle sfide ingegneristiche principali è stata: come diamo in pasto a BERT, un modello linguistico, dei log formati da azioni e delta temporali? Abbiamo testato tre filosofie: l'uso di array concatenati, il binning temporale, e infine lo Storytelling."

### Slide 9: L'Approccio Vincente: Il Paradigma "Storytelling"
**Discorso:**
"L'approccio vincente si è rivelato proprio lo Storytelling, ispirato fortemente da una ricerca dell'università di Pisa: LEGOLAS. Abbiamo tradotto semanticamente le tracce XES in veri e propri paragrafi narrativi in inglese. Questo permette a BERT di sfruttare al massimo la sua capacità nativa di estrarre contesto. A livello algoritmico, il tempo viene gestito iniettando i delta esatti in secondi e gli eventi simultanei vengono aggregati dinamicamente per non sovraccaricare l'Attention."

### Slide 10: Risultati: Balanced Accuracy 88%
**Discorso:**
"I risultati ci hanno dato ragione. L'approccio Storytelling ha raggiunto una Balanced Accuracy dell'88%, superando le altre metodologie. Ma in sanità il trade-off clinico impone di minimizzare i falsi negativi: non identificare un paziente che diventerà critico è un danno grave. Grazie all'uso combinato di Storytelling e Focal Loss, il modello ha commesso solo 17 falsi negativi su 121 casi critici reali."

### Slide 11: Sezione "Explainable AI"
**Discorso:**
"Tuttavia, avere un modello accurato non basta per il deploy in corsia."

### Slide 12: Il Problema della Black-Box in Sanità
**Discorso:**
"Entriamo qui nel cosiddetto 'Paradosso della Black-Box': una rete neurale restituisce una probabilità, ma è un oracolo opaco che non spiega il *perché* della sua decisione. In ambito sanitario, questa opacità è inaccettabile per il clinico. Abbiamo scartato le tecniche XAI classiche perché Grad-CAM è incompatibile con la Self-Attention, e metodi perturbativi come SmoothGrad si basano su spazi continui: sfocare un pixel ha senso, ma perturbare un token di testo discreto genera solo vettori privi di alcun significato clinico."

### Slide 13: Integrated Gradients e la Baseline [PAD]
**Discorso:**
"La soluzione adottata è l'algoritmo Integrated Gradients, che calcola l'importanza integrando i gradienti lungo un percorso logico da una *baseline* fissa (lo zero semantico garantito dal token PAD) fino all'input reale. Per calcolare l'approssimazione discreta di questo integrale senza saturare la CPU, ho implementato un'ottimizzazione vettorializzata su GPU (Riemann Trapezoid) che ha abbattuto i tempi di calcolo da 15 a 2.5 secondi per traccia, rispettando pienamente l'assioma di Completeness."

### Slide 14: La Sfida: Aggregazione dei Sub-Token
**Discorso:**
"C'era un ultimo ostacolo. Il tokenizzatore WordPiece frammenta le parole cliniche in sub-token privi di significato autonomo, come 'card' e '##iology'. Per rendere l'output intellegibile, ho aggregato i gradienti collassandoli in un valore scalare e ricomposto le parole. In questo modo ogni azione clinica ottiene un unico punteggio esatto."

### Slide 15: Il Cruscotto Clinico: Reparto 701 Cardiochirurgia
**Discorso:**
"Il risultato finale è visibile in queste mappe di attribuzione su un campione del reparto di Cardiochirurgia. I gradienti aggregati isolano le procedure maggiormente associate allo spreco del LOS. Questo fornisce alla direzione ospedaliera un vero e proprio 'cruscotto' clinico per identificare in modo data-driven i colli di bottiglia del processo."

### Slide 16: Sezione "Conclusioni"
**Discorso:**
"Per concludere l'analisi del progetto..."

### Slide 17: Fattibilità Aziendale e Costi di Deployment
**Discorso:**
"Abbiamo dimostrato non solo la validità teorica, ma anche la reale fattibilità aziendale del sistema. Come mostrano le metriche Wall-Clock, l'overhead totale per processare l'intera pipeline è di soli 30 minuti. Questo garantisce che il sistema sia sostenibile, in locale su hardware di fascia consumer invece che su macchine cloud ad alte prestazioni e alti costi operativi. Questo strizza anche un occhio alla privacy: i dati non devono per forza uscire dall'ambiente protetto dell'ospedale."

### Slide 18: Conclusioni e Sviluppi Futuri
**Discorso:**
"In sintesi, abbiamo realizzato una pipeline end-to-end efficace. Ribadiamo sempre che questi modelli misurano forti correlazioni e non causalità: la valutazione spetta sempre allo specialista umano. Per gli sviluppi futuri, puntiamo a evolvere il sistema passando a una classificazione dinamica in runtime ai vari checkpoint di degenza, trasformando così il sistema da predittivo a diagnostico. Grazie a tutti per l'attenzione. Sono a disposizione per le vostre domande."

***

### Slide 19: (Backup) Hardware e Hyperparameter Tuning
**Discorso (Solo se richiesto dalla commissione):**
"I 340 milioni di parametri di BERT-Large avrebbero richiesto un batch size troppo piccolo (circa 4) per stare nella VRAM, rendendo la Cross-Validation impraticabile. Inoltre, con sole 7.000 tracce eravamo in *data plateauing*: il modello grande avrebbe solo overfittato senza apprendere nuove feature."

### Slide 20: (Backup) L'Esperimento Fallito: Data Augmentation con LLM
**Discorso (Solo se richiesto dalla commissione):**
"Abbiamo tentato di generare tracce sintetiche per bilanciare i dati usando LLM in locale, ma l'esperimento è fallito a causa delle *allucinazioni semantiche*. I modelli inventavano dettagli clinici confusi e non previsti dal nomenclatore ospedaliero originario. Questo rumore lessicale confondeva BERT: la pipeline esige dati categorici fedeli e non creatività testuale."