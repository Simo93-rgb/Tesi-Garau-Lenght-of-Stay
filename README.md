# Tesi di Laurea Magistrale

**Titolo:** Explainable AI in Sanità: Storytelling e Modelli Transformer per l’Ottimizzazione del Length of Stay


**Autore:** Simone Garau  
**Relatore:** Prof. Giorgio Leonardi  
**Anno Accademico:** 2024/2025

## Compilazione

Questo documento usa `fontspec` e `unicode-math`, quindi **deve essere compilato con XeLaTeX o LuaLaTeX** (non `pdflatex`).

### Prerequisiti

- **TeX Live** (o MikTeX su Windows) aggiornato
- Font **Libertinus Serif** e **Libertinus Math** installate sul sistema
  - Su Debian/Ubuntu: `sudo apt install fonts-libertinus`
  - Su Fedora/RHEL: `sudo dnf install libertinus-fonts`
  - Su macOS (con Homebrew): `brew install --cask font-libertinus`

### Comandi di compilazione

Nella root del repository, esegui:

```bash
xelatex -interaction=nonstopmode main.tex
bibtex main
xelatex -interaction=nonstopmode main.tex
xelatex -interaction=nonstopmode main.tex
```

**Oppure**, se hai `latexmk` installato:

```bash
latexmk -xelatex -interaction=nonstopmode main.tex
```

**Nota:** se le font Libertinus non sono installate, puoi modificare `main.tex` per usare font di sistema disponibili (ad esempio `\setmainfont{Linux Libertine O}` o `\setmainfont{DejaVu Serif}`).

## Struttura del progetto

```
.
├── main.tex           # File principale della tesi
├── license.tex        # Dichiarazione licenza CC BY 4.0 (inclusa in main.tex)
├── bibliografia.bib   # Database BibTeX con citazioni bibliografiche
├── LICENSE            # Testo completo della licenza Creative Commons Attribution 4.0
├── img/               # Cartella per immagini e figure
│   └── logo_upo.jpg   # Logo dell'Università del Piemonte Orientale
├── .vscode/           # Configurazione VS Code e LaTeX Workshop
│   └── settings.json
└── README.md          # Questo file
```

## Come aggiungere citazioni

1. Apri `bibliografia.bib` e aggiungi le tue fonti seguendo gli esempi presenti
2. Nel testo usa `\cite{chiave}` per citare (es: `\cite{esempio_libro}`)
3. Salva il file: LaTeX Workshop compilerà automaticamente con la sequenza completa (xelatex → bibtex → xelatex × 2)

Tipi di entry BibTeX supportati:
- `@article` - Articoli di rivista
- `@book` - Libri
- `@inproceedings` - Atti di conferenze
- `@phdthesis` - Tesi di dottorato/laurea
- `@misc` - Siti web, report tecnici, preprint

## Licenza

Quest'opera è distribuita con licenza **Creative Commons Attribution 4.0 International (CC BY 4.0)**.

**Sei libero di:**
- Condividere — copiare e ridistribuire il materiale in qualsiasi formato o mezzo
- Adattare — remixare, trasformare e costruire sul materiale per qualsiasi scopo, anche commerciale

**Alle seguenti condizioni:**
- **Attribuzione** — Devi riconoscere una menzione di paternità adeguata, fornire un link alla licenza e indicare se sono state effettuate delle modifiche.

**Link alla licenza:**
- Testo completo (legal code): https://creativecommons.org/licenses/by/4.0/legalcode
- Riassunto leggibile (deed): https://creativecommons.org/licenses/by/4.0/

**Esempio di attribuzione consigliata:**
> Simone Garau, "Forecasting di Consumi Energetici nel Mercato Elettrico Italiano: un Confronto tra Modelli di Machine Learning e Deep Learning", Anno Accademico 2024/2025, licensed under CC BY 4.0 (https://creativecommons.org/licenses/by/4.0/).

## Contatti

Per domande o suggerimenti, contatta l'autore tramite GitHub o email.
