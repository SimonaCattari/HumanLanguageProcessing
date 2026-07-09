# Multi-Perspective Stance Detection

Progetto per il corso di **Human Language Technologies (HLT)** — Università di Pisa
Group 12: Benedetta Muscato, Praveen Bushipaka, Simona Cattari, Matteo Trivelli

> ⚠️ **Nota:** i notebook originali con il codice completo non sono inclusi in questo repository per limiti di dimensione file. Questo repository contiene il **report finale del progetto in PDF**, che descrive in dettaglio metodologia, esperimenti e risultati.

## Overview

Questo progetto rivisita il task di **stance detection** mettendo in discussione l'approccio tradizionale basato su un'unica "ground truth" ottenuta tramite majority voting sulle annotazioni umane.

Ispirandoci al paradigma del **perspectivism**, esploriamo un approccio alternativo che preserva le diverse prospettive degli annotatori invece di aggregarle in un'unica etichetta, con l'obiettivo di:

1. Verificare se un modello più inclusivo e "perspective-aware" ottenga performance di classificazione migliori
2. Analizzare se il disaccordo tra annotatori influenzi la confidenza del modello

## Dataset

- Documenti giornalistici raccolti da motori di ricerca in risposta a 57 query su temi controversi (educazione, salute, religione, politica, intrattenimento)
- Ogni documento annotato da 3 crowd-worker (Amazon MTurk) con etichetta: `pro`, `neutral`, `against`, `not-about`, `link not-working`
- Dopo preprocessing (rimozione documenti eccessivamente lunghi, duplicati ed etichette non valide): **1062 documenti**
- Split: 80% train / 20% validation / 20% test

## Metodologia

Due paradigmi di training a confronto:

| Approccio | Descrizione |
|---|---|
| **Baseline** | Un esempio per documento, con etichetta di maggioranza (majority voting) |
| **Multi-perspective** | Il documento viene duplicato 3 volte nel training set, una per ciascuna annotazione individuale, senza aggregazione |

Entrambi gli approcci vengono valutati sullo stesso test set (con majority label).

Per gestire testi lunghi oltre il limite dei 512 token, viene applicata anche una tecnica di **chunking** (tramite la libreria `chunkipy`) come alternativa al semplice troncamento.

## Modelli

- **BERT-base**
- **RoBERTa-base**
- **Mistral 7B**
- **Llama 2 7B**
- *(Longformer testato ma scartato per performance scarse)*

## Risultati principali

- I modelli **multi-perspective superano sempre i baseline**, indipendentemente dal modello
- Il **chunking migliora le performance** rispetto al troncamento semplice
- Miglior baseline: RoBERTa-base con chunking
- Miglior modello multi-perspective: BERT-base con chunking (F1 = 50.21), seguito da vicino da RoBERTa (F1 = 47.45)
- RoBERTa mostra maggiore confidenza nelle predizioni rispetto a BERT
- Llama 2 ha la confidenza più alta in assoluto (0.66), ma accuracy/F1 basse — caso di *high confidence, low accuracy*

## Sviluppi futuri

- Hyperparameter tuning
- Strategie alternative di data augmentation (es. duplicazione condizionata al numero di annotazioni distinte)
- Analisi query-wise del livello di disaccordo tra annotatori
- Estensione della metodologia al task di ideology detection

## Contenuto del repository

- `HLT_project.pdf` — report completo del progetto

## Riferimenti

Dataset originale: Gezici G, Lipani A, Saygin Y, Yilmaz E. *Evaluation metrics for measuring bias in search engine results.* Information Retrieval Journal. 2021;24:85-113.

Per la lista completa dei riferimenti, si veda il report PDF.
