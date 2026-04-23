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

### Slide 5.1: La Pipeline Architetturale
**Discorso:**
"Come si può vedere in questo schema riassuntivo, l'intero flusso dei dati si sviluppa attraverso cinque fasi concettuali: partiamo dai log degli eventi ospedalieri in formato XES, li convertiamo con una forma di embedding appropriata e li passiamo al cuore del sistema, un modello Transformer BERT associato a un layer di classificazione binaria. Infine, applichiamo un algoritmo formale di Explainable AI, ovvero gli Integrated Gradients, per ripercorrere a ritroso la decisione e fornire l'interpretazione causale dei colli di bottiglia."

### Slide 6: Dataset e la Sfida dello Sbilanciamento
**Discorso:**
"Abbiamo lavorato su un dataset reale composto da 7.393 casi e oltre 88.000 eventi. L'analisi esplorativa ha subito evidenziato una prima grande sfida: il forte sbilanciamento delle classi. L'89% dei casi appartiene alla classe normale, mentre solo l'11% ricade nel long-LOS. Come mostrano i boxplot, i casi critici presentano una variabilità di eventi molto maggiore. In questo scenario, una funzione di costo standard tenderebbe a far collassare il modello sulla classe maggioritaria."

### Slide 7: Il Modello: L'Architettura BERT
**Discorso:**
"Passando all'architettura, il cuore predittivo è un modello BERT-base-uncased. Abbiamo scelto un approccio encoder-only per sfruttare la sua capacità di catturare il contesto bidirezionale nelle storie cliniche. Nonostante l'esistenza di modelli specialistici come ClinicalBERT, abbiamo preferito la versione base per evitare il vocabulary mismatch causato dalle nomenclature locali dei nostri dataset ospedalieri.

Come vediamo dal diagramma, il flusso è lineare. All'inizio della sequenza testuale inseriamo il token speciale [CLS]. Man mano che l'input viene elaborato dai layer del Transformer, grazie al meccanismo di attention, questo token 'guarda' e assorbe il contesto di tutte le altre parole. Alla fine del processo, estraiamo unicamente l'embedding finale associato al [CLS], che a questo punto agisce come un 'distillato' semantico dell'intera sequenza. La vera personalizzazione risiede nella Classification Head che abbiamo aggiunto: un layer lineare che mappa la conoscenza di BERT sulle nostre classi di target. Questa struttura è fondamentale per la nostra analisi XAI: utilizzando i Gradienti Integrati, possiamo infatti risalire dalla decisione della Head fino ai singoli token di input che hanno pesato maggiormente sulla classificazione."

### Slide 7.1: Il Modello: La Focal Loss
**Discorso:**
"Tuttavia, addestrare questo modello non è stato banale a causa del forte sbilanciamento delle classi visto in precedenza. Per questo abbiamo integrato la Focal Loss al momento dell'addestramento. Questa particolare funzione abbatte il gradiente degli esempi facili (ossia la classe dominante), forzando BERT a concentrarsi e imparare i pattern dei casi rari e complessi del long-LOS."

### Slide 8: La Ricerca dell'Embedding Ideale
**Discorso:**
"Una delle sfide ingegneristiche principali è stata: come diamo in pasto a BERT, un modello linguistico, dei log formati da azioni e delta temporali? Abbiamo testato tre filosofie: l'uso di array concatenati, il binning temporale, e infine lo Storytelling."

### Slide 8.1: Da XES a Storytelling
**Discorso:**
"Ma in che modo otteniamo una narrazione clinica dai dati tabulari? Il flusso logico è automatizzato da uno script deterministico. Partiamo dal log XES, dove ci scontriamo col problema delle terminologie locali. Grazie all'uso preliminare delle API di Google Translate, mappiamo il nomenclatore ospedaliero italiano su una terminologia medica inglese standardizzata. Successivamente, l'algoritmo raggruppa gli eventi simultanei. Infine, un sistema di pattern matching basato su template sintattici inietta i delta temporali esatti in secondi, creando una narrativa strutturata."

### Slide 9: L'Approccio Vincente: Il Paradigma Storytelling
**Discorso:**
"L'idea di fondo, fortemente ispirata da una ricerca del 2025 del team di Pasquadibisceglie et al., è di trasformare la natura formale dei log clinici in linguaggio naturale. Questo permette a BERT di sfruttare a pieno la sua architettura nativa, pensata proprio per trovare e processare pattern semantici bidirezionali nel testo. Come vedete nell'esempio, i delta temporali vengono espressi in secondi e le azioni contemporanee vengono descritte come simultanee per non disorientare il meccanismo di Self-Attention."

### Slide 10: Risultati: L'Efficacia di BERT e Storytelling
**Discorso:**
"I risultati empirici hanno confermato la nostra intuizione: come vedete nella tabella, l'approccio Storytelling ha sfiorato il 90% di Balanced Accuracy, superando nettamente le metodologie di embedding tradizionali. Ma c'è un aspetto ancora più rilevante per il dominio sanitario. Il trade-off clinico ci impone di minimizzare a tutti i costi i falsi negativi: non identificare un paziente che risulterà critico è un danno inaccettabile. Come si nota dalla matrice di confusione, grazie all'uso combinato di Storytelling e Focal Loss, il modello ha commesso solamente 17 falsi negativi su 121 casi critici reali. Questo dimostra che la rete non solo è accurata, ma è sicura e clinicamente affidabile."

### Slide 11: Sezione "Explainable AI"
**Discorso:**
"Tuttavia, avere un modello accurato non basta per il deploy in corsia. Dobbiamo capire come la rete prende le sue decisioni."

### Slide 11.1: Explainable AI: Oltre l'Attention
**Discorso:**
"L'architettura Transformer, avendo nativamente il meccanismo di Self-Attention, teoricamente ci permette di 'vedere' a cosa ha prestato attenzione il modello per fare la sua predizione. Purtroppo, questo valore da solo è troppo sintetico e non sempre allineato alla reale importanza causale del token per la classificazione finale. In ambito sanitario, l'attention non è sufficiente. Per superare l'opacità del modello e soddisfare i rigorosi limiti normativi europei e nazionali imposti sulla trasparenza dei sistemi AI in medicina, serve un algoritmo di spiegabilità formale e matematicamente riconosciuto."

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

### Slide 18: Conclusioni
**Discorso:**
"In sintesi, abbiamo realizzato una pipeline end-to-end efficace. L'uso di BERT e Integrated Gradients permette di individuare i colli di bottiglia nei processi ospedalieri. Ribadiamo sempre che questi modelli misurano forti correlazioni e non causalità: la valutazione finale spetta sempre allo specialista umano."

### Slide 18.1: Sviluppi Futuri dell'Architettura AI
**Discorso:**
"Dal punto di vista puramente ingegneristico e algoritmico, ci sono ampi margini di miglioramento. In futuro si potrebbe espandere il dataset includendo log di altri ospedali per testare la generalizzabilità del modello. Si potranno sperimentare Transformer più recenti come RoBERTa o perfino LLM compatti. Inoltre, un'architettura 'Mixture of Experts', con una rete dedicata per ogni singolo reparto ospedaliero, potrebbe abbattere ulteriormente gli errori di classificazione."

### Slide 18.2: Il Contesto Più Ampio: Il Progetto TEXLOS
**Discorso:**
"Per concludere, ci tengo a sottolineare che la metodologia discussa in questa tesi non è fine a se stessa, ma è il motore analitico di un progetto molto più ampio chiamato TEXLOS, pubblicato sulla rivista scientifica 'Frontiers in Artificial Intelligence'. L'obiettivo di TEXLOS è fornire un framework di Business Process Management che operi in corso d'opera. Il focus si sposta dall'analisi 'post-mortem' all'anticipazione online, studiando ad esempio l'impatto dei servizi diagnostici esterni e fornendo Next-Activity-Prediction per il controllo proattivo della degenza. E proprio la validazione umana da parte dei medici ha confermato il potenziale di questo strumento.
Grazie a tutti per l'attenzione. Sono a disposizione per le vostre domande."

***

### Slide 19: (Backup) Hardware e Hyperparameter Tuning
**Discorso (Solo se richiesto dalla commissione):**
"I 340 milioni di parametri di BERT-Large avrebbero richiesto un batch size troppo piccolo (circa 4) per stare nella VRAM, rendendo la Cross-Validation impraticabile. Inoltre, con sole 7.000 tracce eravamo in *data plateauing*: il modello grande avrebbe solo overfittato senza apprendere nuove feature."

### Slide 20: (Backup) L'Esperimento Fallito: Data Augmentation con LLM
**Discorso (Solo se richiesto dalla commissione):**
"Abbiamo tentato di generare tracce sintetiche per bilanciare i dati usando LLM in locale, ma l'esperimento è fallito a causa delle *allucinazioni semantiche*. I modelli inventavano dettagli clinici confusi e non previsti dal nomenclatore ospedaliero originario. Questo rumore lessicale confondeva BERT: la pipeline esige dati categorici fedeli e non creatività testuale."