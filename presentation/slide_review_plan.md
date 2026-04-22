# Piano revisione
Sostanzialmente devo aumentare il contenuto delle mie slide siccome mi hanno dato una finestra temporale di esposizione di 20 minuti, quindi maggiore di quella con cui le ho costruite queste slides. Creerò una serie di paragrafi che conterranno le istruzioni per la revisione di ogni singola slide. Le slide sono un progetto latex e sono salvate nella cartella `presentation/slide_dec.tex`.

Mi serve anche aiuto con il discorso, che al momento è in una vecchia edizione e scritto nel file `presentation/discorso.md`. Dovrai aggiornarlo con i contenuti delle nuove slide e con le nuove indicazioni che ti darò.

Le slide nuove che ti chiederò di creare potranno fare riferimento a file python che ti fornirò o a descrizioni del mio lavoro che scriverò nei paragrafi di istruzioni.

## Metodologia

Faremo una slide alla volta e procederemo alla successiva solo dopo che avrò esplicitamente confermato la mia soddisfazione nel lavoro svolto per la slide corrente. 

## Lavoro da svolgere
Qui iniziamo con il lavoro, ogni paragrafo fa riferimento alla slide con cui dobbiamo lavorare. 

### Il Problema: Length of Stay (LOS)
Qui sono stato troppo sintetico, ci troviamo nel contesto e nella motivazione del lavoro. Il cuore della questione da questo abstrct:

```
L’ottimizzazione del Length of Stay (LOS) ospedaliero è una sfida fondamentale per con-
ciliare efficienza economica e qualità delle cure. Questa tesi, sviluppata all’interno del
progetto TEXLOS [1], propone un sistema innovativo d’intelligenza artificiale per la clas-
sificazione retrospettiva e l’individuazione dei colli di bottiglia logistici responsabili di
degenze critiche (≥ 20 giorni) partendo dai Log degli Eventi clinici.
L’approccio metodologico estende le metodologie del Process Mining tradizionale in-
troducendo il paradigma dello Storytelling: una traduzione semantica dei pattern cro-
nologici in narrazioni in linguaggio naturale. Tali testi alimentano un classificatore Tran-
sformer (bert-base-uncased), addestrato mediante Focal Loss per mitigare il severo
sbilanciamento dei dati ospedalieri.
Avendo accertato l’apprendimento strutturale del modello, viene risolto il classico pro-
blema clinico della scatola nera ("black box"). Si applica una variante adattiva degli Inte-
grated Gradients – supportata da una parallela logica di ricomposizione semantica dei
token frammentati da BERT – per interrogare a posteriori le scelte della rete. Assegnan-
do matematicamente un punteggio esplicito d’impatto ad ogni singola azione nosocomiale
Per operare al meglio e avere una divisione del discorso ottimizzata, leggi anche la slide successiva "I Limiti dell'Approccio Classico". 
```
In questa fase si vuole passare il messaggio al pubblico che l'idea è quella di estrarre da una rete che ha imparato a discriminare quali pazienti sono di degenza lunga quali sono le azioni cliniche subite da questi pazienti che hanno portato a questa classificazione LOS >= 20 giorni. 

### Pipeline
Questa slide non esiste e andrà messa subito dopo la sezione Metodologia", ossia prima della slide "Il Modello: L'Architettura BERT". 

In questa slide si vuole dare una panoramica generale del processo che porta all'estrazione delle informazioni che vogliamo ottenere. Dovrai costruire uno schema con `tikzpicture` un po' come è stato fatto con la slide "La Sfida: Aggregazione dei Sub-Token". La pipeline è così: 
$$
\text{XES} \rightarrow \text{Embedding} \rightarrow \text{BERT} \rightarrow \text{Classification} \rightarrow \text{Integrated Gradients}
$$
Se pensi ci sia un nodo da modificare o aggiungere lascia qui sotto un commento per suggerire la modifica, ma non implementarla automaticamente. 

### Il Modello: L'Architettura BERT
Questa slide al momento è scarnissima e incompleta
Mi piacerebbe aggiungere un'immagine che mostri l'architettura di BERT, e vorrei usare sempre lo stesso tool: `tikzpicture`. Ecco un inizio per lavorare: 
```tex
\begin{tikzpicture}[
    node distance=1.2cm and 0cm,
    % Stili dei nodi
    input/.style={rectangle, draw=black!70, thick, fill=gray!10, text width=5cm, align=center, minimum height=1cm, rounded corners},
    bert/.style={rectangle, draw=black!80, thick, fill=green!15, text width=5.5cm, align=center, minimum height=1.8cm, rounded corners},
    cls/.style={rectangle, draw=black!70, thick, fill=blue!15, text width=4cm, align=center, minimum height=1cm, rounded corners},
    head/.style={rectangle, draw=black!80, thick, fill=orange!20, text width=4.5cm, align=center, minimum height=1.2cm, rounded corners},
    output/.style={rectangle, draw=black!70, thick, fill=red!15, text width=3.5cm, align=center, minimum height=1cm, rounded corners},
    % Stile delle frecce
    arrow/.style={-{Stealth[scale=1.2]}, thick, draw=black!80}
]

% Definizione dei nodi (Flusso dal basso verso l'alto)
\node[input] (input) {Input Tokens\\ \small \texttt{[CLS] Token1 Token2 ... [SEP]}};
\node[bert, above=of input] (bert) {\textbf{Modello BERT}\\ \small (Transformer Encoder Base)};
\node[cls, above=of bert] (cls) {Rappresentazione \texttt{[CLS]}\\ \small (Vettore contestuale)};
\node[head, above=of cls] (classifier) {\textbf{Classification Head}\\ \small (Linear Layer + Dropout)};
\node[output, above=of classifier] (output) {Output Binario\\ \small (Sigmoide: Classe 0 / 1)};

% Connessioni (Frecce)
\draw[arrow] (input) -- (bert);
\draw[arrow] (bert) -- node[right, font=\footnotesize, xshift=2mm] {Estrazione pooler} (cls);
\draw[arrow] (cls) -- (classifier);
\draw[arrow] (classifier) -- (output);

\end{tikzpicture}
``` 
Possiamo descrivere lo schema così: 
```mardown
# Descrizione Architetturale: Classificatore Binario basato su BERT

Il seguente schema descrive un'architettura di rete neurale top-down (o bottom-up in termini di flusso dei dati) utilizzata per un task di classificazione binaria, partendo da un modello linguistico pre-addestrato (BERT). Il flusso dei dati procede verticalmente dal basso verso l'alto.

## Struttura dei Nodi (Dal basso verso l'alto)

1. **Nodo di Input (Input Tokens)**:
   - **Rappresenta**: La sequenza di testo in ingresso tokenizzata.
   - **Dettagli**: Mostra l'uso dei token speciali tipici di BERT, iniziando con il token di classificazione `[CLS]`, seguito dai token di testo effettivo e terminando con il token di separazione `[SEP]`.

2. **Nodo Core (Modello BERT)**:
   - **Rappresenta**: L'architettura encoder-only del Transformer pre-addestrato.
   - **Dettagli**: Agisce come estrattore di feature complesse e contestuali. Elabora l'intera sequenza di input in parallelo.

3. **Nodo di Pooling (Rappresentazione `[CLS]`)**:
   - **Rappresenta**: Il vettore di embedding risultante (hidden state) associato esclusivamente al token `[CLS]`.
   - **Dettagli**: In BERT, questo specifico vettore aggregato cattura il significato semantico complessivo dell'intera sequenza in ingresso.

4. **Nodo Classificatore (Classification Head)**:
   - **Rappresenta**: Lo strato aggiunto in cima a BERT (fine-tuning).
   - **Dettagli**: È tipicamente composto da uno strato lineare (Dense/Fully Connected layer) accoppiato spesso con un meccanismo di Dropout per prevenire l'overfitting. Riceve in input unicamente l'embedding del token `[CLS]`.

5. **Nodo di Output (Output Binario)**:
   - **Rappresenta**: La previsione finale del modello.
   - **Dettagli**: Utilizza una funzione di attivazione (tipicamente una Sigmoide per la classificazione binaria) per schiacciare i logit in una probabilità compresa tra 0 e 1, determinando così l'appartenenza alla classe positiva o negativa.

## Connessioni (Flusso delle informazioni)
- Una freccia diretta collega i **Tokens di Input** a **BERT**, indicando il passaggio della sequenza tokenizzata al modello.
- Una freccia collega **BERT** alla **Rappresentazione `[CLS]`**; questa transizione è annotata come "Estrazione pooler", indicando che, tra tutti gli output della sequenza, viene filtrato e mantenuto solo il vettore `[CLS]`.
- Una freccia passa la rappresentazione isolata dal nodo **`[CLS]`** alla **Classification Head**.
- L'ultima freccia collega la **Classification Head** all'**Output Binario**, fornendo il risultato della previsione.
```

### L'Approccio Vincente: Il Paradigma "Storytelling"
Qui sono stato super sintetico ma ci sarebbe da dire che l'idea arriva da un paper scientifico 
```bib
@article{PASQUADIBISCEGLIE2025128224,
title = {Leveraging a Large Language Model (LLM) to Predict Hospital Admissions of Emergency Department Patients},
journal = {Expert Systems with Applications},
pages = {128224},
year = {2025},
issn = {0957-4174},
doi = {https://doi.org/10.1016/j.eswa.2025.128224},
author = {Valeria Pasquadibisceglie and Annalisa Appice and Giovanna Castellano and Corrado Mencar}
}
```
Che l'idea presa da questo progetto è di trasformare la forma dei dati da linguaggio/struttura formale in una narrazione in linguaggio naturale così da sfruttare a pieno la caratteristica di un transformer di trovare pattern semantici nel linguaggio naturale. Non ho nemmeno detto che lo script per fare questo lavoro è stato un lavoro deterministico che potrai vedere nel file `assets/story_generator.py`. Mi piacerebbe uno schema che mostri il flusso logico di come si ottiene una storia in formato narrativo a partire da un file XES. Il file di traduzione che viene usato nello script è un file ottenuto con le API Google di google translate traducento un elenco di azioni cliniche uniche contenute nel dataset. 
Come potrei immaginare c'è troppa informazione per una sola slide, ora faccio due sottoparagrafi che rappresentano le due nuove slide. 

#### Da XES a Storytelling
Questo conterrà il solo schema dello script fatto con il tool `tikzpicture`. 

#### Storytelling
Questa slide sarà è semplicemente "L'Approccio Vincente: Il Paradigma Storytelling" con titolo differente. Il contenuto va bene così per ora. 

### Risultati
Qui va bene così sostanzialmente, ma vorrei che la slide comunicasse visivamente il messaggio "ecco come va BERT". Probabilmente il titolo scarno e la mancanza di testo nella slide la rende un po' secca, ma non saprei come migliorare la situazione. Non abusare dello spazio però, mi piace che sia una slide che va al punto mostrando i risultati. 

### Approccio alla XAI
Questa slide non esiste e va subito dopo la sezione "Explainable AI" e subito prima della slide "Integrated Gradients e la Baseline". 
Vorrei spiegare che l'architettura transformer avendo il meccanismo di attention permette di "vedere" a cosa ha prestato attenzione il modello per fare la sua predizione. Questo valore però da solo è troppo sintetico e si ritiene non non all'altezza di essere una spiegazione degna della XAI (anche se probabilmente meglio che non leggere il valore di un neurone di una NN densa). Quindi serve un algoritmo di spiegabilità riconosciuto che permetta di soddisfare i limiti europei (e nazionali) imposti sulla trasparenza nell'utilizzo di AI in ambito sanitario. 

### Sviluppi Futuri
Qui esiste già ma contiene un misto di cose. In questa sezione metti semplicemente le classiche cose da progetto di deep learning: si potrebbe pensare di trovare un dataset più grande, si potrebbe pensare usare altri modelli transformer e magari fare un ensamble per simulare un "mixture of experts"... 

### TEXLOS
Questa slide contiene di base il contenuto della vecchia versione di "Sviluppi Futuri" ma parlando del fatto che il mio lavoro è parte di un contesto più grande ossia TEXLOS, progetto pubblicato su Frontiers in Artificial Intelligence. Precisamente ecco l'abstract dell'articolo: 
```
ABSTRACT={Limiting hospital length of stay (LOS) can prevent patient complications and reduce costs. The identification of activities that could impact LOS, and which may be attributable to outpatient services (OSs - i.e., diagnostic exams or specialist consultations provided by external wards), can be very useful for hospital administrators, and help optimize healthcare processes. In this work, we introduce TEXLoS, a tool able to study the association of OS activities with LOS. The problem is afforded in a Business Process Management perspective, allowing the integration and the adoption of state of the art techniques for trace classification and process model discovery. The focus of TEXLoS output is on explainability, which is the key to the adoption of the tool in practice. In the paper, we present the main steps of the TEXLoS architecture, and the results of its application to the data of Azienda Ospedaliera SS. Antonio e Biagio e Cesare Arrigo in Alessandria, Italy, where a balanced accuracy of 93% has been reached, along with a Matthews Correlation Coefficient of 0.68, confirming the high performance in classification. Interpretability of the provided output has also been successfully validated by a group of end user.}}
```


