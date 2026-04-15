# Tesi di Laurea Magistrale

**Titolo:** Explainable AI in Sanità: Storytelling e Modelli Transformer per l'Ottimizzazione del Length of Stay

**Autore:** Simone Garau  
**Relatore:** Prof. Giorgio Leonardi  
**Anno Accademico:** 2024/2025

---

## Compilazione

Il documento è compilato con **pdfLaTeX** (TeX Live 2026/Arch Linux).

> ⚠️ Il pacchetto `minted` (usato per il syntax highlighting del codice) richiede il flag `-shell-escape` ad ogni invocazione di `pdflatex`.

### Prerequisiti

- **TeX Live** installato (su CachyOS/Arch Linux: `sudo pacman -S texlive-meta texlive-langitalian`)
- **Python** con **Pygments** installato (richiesto da `minted`):
  ```bash
  pip install pygments
  ```
- **Biber** o **BibTeX** per la bibliografia (incluso in `texlive-meta`)

### Comandi di compilazione

Nella root del repository, esegui la sequenza completa:

```bash
pdflatex -shell-escape -interaction=nonstopmode main.tex
bibtex main
pdflatex -shell-escape -interaction=nonstopmode main.tex
pdflatex -shell-escape -interaction=nonstopmode main.tex
```

**Oppure**, con `latexmk` (consigliato, gestisce automaticamente i passaggi):

```bash
latexmk -pdf -shell-escape -interaction=nonstopmode main.tex
```

Per pulire i file ausiliari generati:

```bash
latexmk -c
```

### LaTeX Workshop (VS Code / Cursor / Antigravity)

Se usi LaTeX Workshop, assicurati che la ricetta attiva usi `pdflatex` con `-shell-escape`. Puoi aggiungere nel tuo `settings.json` utente:

```json
"latex-workshop.latex.tools": [
  {
    "name": "pdflatex",
    "command": "pdflatex",
    "args": [
      "-shell-escape",
      "-interaction=nonstopmode",
      "-synctex=1",
      "%DOC%"
    ]
  }
]
```

---

## Struttura del progetto

```
.
├── main.tex                  # File principale della tesi
├── abstract.tex              # Abstract
├── appendici.tex             # Appendici (config hardware, software, hyperparameter tuning)
├── license.tex               # Dichiarazione licenza CC BY 4.0
├── ringraziamenti.tex        # Ringraziamenti
├── bibliografia.bib          # Database BibTeX con le citazioni
├── LICENSE                   # Testo completo della licenza Creative Commons Attribution 4.0
├── capitoli/
│   ├── 01_introduzione.tex
│   ├── 02_stato_arte.tex
│   ├── 03_metodologia.tex
│   ├── 04_risultati.tex
│   └── 05_conclusioni.tex
├── assets/                   # Script Python della pipeline
│   ├── action_aggregator.py  # Ricomposizione semantica dei sub-token (Step A + B)
│   ├── ig_completeness.py    # Algoritmo adattivo Integrated Gradients
│   └── hyperparameters.py    # Configurazione iperparametri Optuna
├── presentation/
│   └── slide_deck.tex        # Slide della presentazione (Beamer)
├── img/                      # Figure e grafici della tesi
│   └── logo_upo.jpg
└── README.md                 # Questo file
```

---

## Come aggiungere citazioni

1. Apri `bibliografia.bib` e aggiungi le fonti seguendo gli esempi presenti
2. Nel testo usa `\cite{chiave}` (es: `\cite{vaswani2017attention}`)
3. Dopo ogni modifica alla bibliografia, riesegui la sequenza completa di compilazione (pdflatex → bibtex → pdflatex × 2)

Tipi di entry BibTeX supportati:
- `@article` — Articoli di rivista
- `@book` — Libri
- `@inproceedings` — Atti di conferenze
- `@phdthesis` — Tesi di dottorato/laurea
- `@misc` — Siti web, report tecnici, preprint

---

## Licenza

Quest'opera è distribuita con licenza **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

**Sei libero di:**
- Condividere — copiare e ridistribuire il materiale in qualsiasi formato o mezzo
- Adattare — remixare, trasformare e costruire sul materiale per qualsiasi scopo, anche commerciale

**Alle seguenti condizioni:**
- **Attribuzione** — Devi riconoscere la paternità dell'opera, fornire un link alla licenza e indicare se sono state effettuate modifiche.

**Link alla licenza:**
- Testo completo: https://creativecommons.org/licenses/by/4.0/legalcode
- Riassunto: https://creativecommons.org/licenses/by/4.0/

**Esempio di attribuzione consigliata:**
> Simone Garau, "Explainable AI in Sanità: Storytelling e Modelli Transformer per l'Ottimizzazione del Length of Stay", Anno Accademico 2024/2025, licensed under CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/).

---

## Contatti

Per domande o suggerimenti, contatta l'autore tramite GitHub o email.
