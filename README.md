# Cosa guida il voto? Un'analisi dei risultati referendari tra demografia e storia elettorale

## 📊 Descrizione del Progetto

Questo progetto si propone di investigare i fattori che determinano il comportamento elettorale e referendario in Italia, con particolare focus sul **Referendum sulla separazione delle carriere del 22 e 23 aprile 2026**. 

L'obiettivo principale è comprendere **in che misura caratteristiche socio-demografiche e geografiche riescono a spiegare le scelte di voto a livello comunale**, indipendentemente dall'inerzia elettorale storica.

### Domanda di Ricerca
**"Da cosa viene guidato il voto per comune?"**

La ricerca affronta questa domanda attraverso un approccio **multi-metodologico**, combinando tecniche di machine learning parametriche e non parametriche per una comprensione profonda dei fattori predittivi.

---

## 🎯 Obiettivi

1. **Integrazione dati**: Costruire un dataset comprehensivo combinando dati ISTAT, risultati Eligendo e dati ministeriali
2. **Data Quality**: Affrontare i dati mancanti attraverso imputazione MICE e analisi esplorativa approfondita
3. **Modellistica Parametrica**: Confrontare Regressione Logistica, LASSO, Relaxed LASSO e Post-LASSO
4. **Modellistica Non Parametrica**: Implementare Decision Tree, Random Forest e tecniche di XAI
5. **Interpretabilità**: Fornire una spiegazione leggibile del comportamento dei modelli black-box

---

## 📁 Struttura del Dataset

### Origine dei Dati

#### 1. **Dati Demografici (ISTAT)**
- Riferiti al **1° gennaio 2026**
- Fonte: Istituto Nazionale di Statistica
- Variabili coperte: età, genere, nazionalità, istruzione, reddito, densità abitativa

#### 2. **Risultati Elettorali (Eligendo)**
- **Elezioni Politiche 2022**: Percentuale aggregata di voti per partiti favorevoli al SÌ
- **Referendum Cittadinanza 2025**: Risultati del 5° quesito
- Fonte: Portale Eligendo (Ministero dell'Interno)

#### 3. **Risultati Referendari 2026**
- **Referendum 22-23 marzo 2026**: Risultati ufficiali sulla separazione delle carriere
- Metodo di estrazione: REST API interne del portale ministeriale (`eleapi.interno.gov.it`)
- Script utilizzato: Open-source di Lorenzo Ruffino [@Ruffino_Lorenzo](https://x.com/Ruffino_Lorenzo)
- Repository: [GitHub - lorenzo-ruffino](https://github.com/lorenzo-ruffino)

### Variabili Principali

#### **Struttura Base**
- `COMUNE`: Nome del comune
- `CODICE_COMUNE`: Codice numerico ISTAT (7,094 comuni)
- `POSIZIONE_GEO`: Classificazione geografica (Nord, Centro, Sud)

#### **Geometria e Demografia**
- `ETA_MEDIA`: Età media della popolazione
- `PERC_MASCHILE`: Quota di residenti di sesso maschile
- `POPOLAZIONE`: Numero totale dei residenti
- `PERC_STRANIERI`: Quota di residenti di altra nazionalità
- `SUPERFICIE_kmq`: Superficie in km²
- `DENSITA_Xkmq`: Densità abitativa (abitanti/km²)
- `FASCIA_POPOLAZIONE`: Categorizzazione in 4 classi (Micro, Piccolo, Medio, Grande)

#### **Istruzione**
- `BASSA_ISTR`: Quota con licenza media o inferiore
- `MEDIA_ISTR`: Quota intermedia
- `ALTA_ISTR`: Quota con titolo terziario secondo livello o superiore

#### **Reddito (Indici PCA)**
Derivati da **Analisi delle Componenti Principali** applicata a 8 fasce di reddito IRPEF originali:
- `INDICE_POVERTA` (PC1): Cattura la diffusa assenza di redditi medi e alti
- `INDICE_DISUGUAGLIANZA` (PC2): Misura la polarizzazione della distribuzione del reddito
- `INDICE_CETO_MEDIO` (PC3): Cattura la solidità della classe media
- **Varianza spiegata**: 62.6% dalle prime tre componenti

#### **Componente Elettorale**
- `PERC_SI_EL22`: Percentuale SÌ aggregata alle Elezioni 2022
- `PERC_SI_CITTAD_REF25`: Percentuale SÌ al Referendum Cittadinanza 2025
- `PERC_SI`: Percentuale SÌ al Referendum 2026 (target quantitativo)
- `SI_NO`: Target binario (esito comunale)

### Qualità dei Dati

#### **Dati Mancanti**
- **Tasso complessivo**: ~5% concentrato in `PERC_SI_EL22`
- **Pattern**: MAR (Missing At Random), correlato a posizione geografica
- **Localizzazione**: Principalmente Trentino-Alto Adige e Valle d'Aosta
- **Metodo risolutivo**: Imputazione MICE con Predictive Mean Matching (PMM)

#### **Gestione dei Dati Mancanti**
- Split training/test (75/25) *prima* dell'imputazione
- Addestramento MICE sul training set
- Applicazione al test set via `mice.mids`
- 5 iterazioni con m=5 imputazioni, ma convergenza verso m=1

#### **Outlier e Anomalie**
- Identificati tramite residui studentizzati (soglia |3|, estremi |5|)
- Effetto "micro-comune": comuni con <500 abitanti mostrano volatilità alta
- Roma e altre grandi città sono leverage points ma con residui contenuti

### Preprocessing e Normalizzazione

#### **Trasformazioni Logaritmiche**
Applicate prima dello scaling a variabili con range strutturale elevato:
- `LOG_POPOLAZIONE`: log(POPOLAZIONE + 1)
- `LOG_DENSITA`: log(DENSITA_Xkmq + 1)
- `LOG_SUPERFICIE`: log(SUPERFICIE_kmq + 1)

#### **Scaling Min-Max**
- Range: [0, 1]
- Motivo: Mantenere geometricamente le proporzioni originali
- Data Leakage Prevention: Parametri (MIN, MAX) appresi dal training set, proiettati sul test set

#### **Composizione Finale**
- **Numero di comuni**: 7,094 (dataset completo)
- **Numero di comuni con target**: 6,710 (comuni con esito referendum definito)
- **Variabili numeriche**: 19 dopo preprocessing
- **Variabili categoriche**: 2 (POSIZIONE_GEO, FASCIA_POPOLAZIONE)
- **Training set**: 75% dei comuni
- **Test set**: 25% dei comuni
- **Bilanciamento classi**: 42-43% NO, 57-58% SÌ (ben bilanciato)

---

## 🔍 Metodologie Implementate

### Fase 1: Analisi Esplorativa (EDA)

#### **Analisi Descrittive**
- Matrice di correlazione con visualizzazione ggcorrplot
- Identificazione del principale predittore: `PERC_SI_EL22` (r = 0.87)
- Analisi del "quadrante politico": relazione 2022 vs. 2026

#### **Analisi dei Dati Mancanti**
- UpSet plot per intersezioni di missingness
- Mappa spaziale della localizzazione geografica
- Marginplot per distribuzione condizionata
- Conclusione: MAR pattern, non MCAR

#### **Identificazione di Anomalie**
- Residui studentizzati da regressione lineare diagnostica
- Cook's distance per influenza sui coefficienti
- Distinzione tra outlier (residuo alto) e leverage points (covariate estreme)

### Fase 2: Imputazione e Data Scaling

#### **MICE (Multivariate Imputation by Chained Equations)**
- Metodo: Predictive Mean Matching (PMM)
- Iterazioni: 10
- Imputazioni generate: 5
- Estrazione finale: m=1 (convergenza dimostrata)
- Diagnostica: stripplot e densityplot per validazione

#### **Scaling**
- Tecnica: Min-Max Scaling [0, 1]
- Giustificazione: Preservare proporzioni originali vs. standardizzazione
- Data Leakage Prevention: Parametri imparati solo da training set

### Fase 3: Modelli Parametrici

#### **3.1 Regressione Logistica Base**
```
Modello: SI_NO ~ tutte le covariate
Test Accuracy: 0.760
AUC: 0.822
Vantaggi: Baseline, performance buone
Limitazioni: Coefficienti instabili per collinearità
```

#### **3.2 LASSO Logistico**
```
Formula: ℓ(β) - λ Σ|βⱼ|
Lambda scelto: lambda.1se (1 SE rule)
Test Accuracy: 0.757
AUC: 0.817
Predittori attivi: ~6 su 19
Vantaggi: Selezione automatica variabili
```

#### **3.3 Relaxed LASSO**
```
Formula: ℓ(β) - γλ Σ|βⱼ| (solo su predittori selezionati)
Gamma ottimale: < 1 (rilassamento parziale vantaggioso)
Test Accuracy: 0.761
AUC: 0.817
Vantaggi: Riduce shrinkage bias sui coefficienti
```

#### **3.4 Post-LASSO (LASSO + Logistica)**
```
Passo 1: LASSO per selezione variabili
Passo 2: Logistica classica (senza penalizzazione) sul subset
Predittori selezionati: 3 (ALTA_ISTR, POSIZIONE_GEO, LOG_POPOLAZIONE)
Test Accuracy: 0.764 (MIGLIOR performance parametrica)
AUC: 0.820
Vantaggi: Parsimonia massima + inferenza valida sui coefficienti
```

### Fase 4: Modelli Non Parametrici

#### **4.1 Decision Tree (RPART)**
```
Criterio splitting: Gini Index
Primo split: POSIZIONE_GEO (geografico)
Nodi terminali ottimi: 4
Test Accuracy: 0.694
Pattern: Nord vs. Centro-Sud nettissimo, poi densità abitativa
```

#### **4.2 Random Forest**
```
Numero alberi: 500
Feature subsampling: √p predittori per nodo
Bootstrap samples: Stratificati
Test Accuracy: 0.785 (MIGLIOR performance complessiva)
OOB Error: Converge a ~150 alberi
Variabile più importante: POSIZIONE_GEO (di gran lunga)
Ranking importanza: POSIZIONE_GEO > ALTA_ISTR/BASSA_ISTR > PERC_STRANIERI > ... > ETA_MEDIA
```

### Fase 5: Explainable AI (XAI)

#### **Modello Surrogato Globale**
```
Metodo: Decision tree (RPART) addestrato su predizioni RF
Target del surrogato: Output Random Forest (non ground truth)
Scopo: Estrarre regole leggibili dal black-box
Prestazioni: Approssimazione fedele delle decisioni RF
```

**Regole Estratte:**
1. **Sud**: 93% NO, 33% osservazioni
2. **Centro + bassa densità**: 78% NO
3. **Centro + alta densità**: 80% SÌ
4. **Nord + alta istruzione**: Incerto (47% SÌ, 53% NO), risolto da bassa istruzione
5. **Nord + bassa istruzione**: 98% SÌ, 51% osservazioni (nodo più omogeneo)

---

## 📈 Risultati Principali

### 1. **Centralità della Geografia**
- `POSIZIONE_GEO` domina tutti i modelli (parametrici e non parametrici)
- Senza variabili politiche, la geograficità spiega ~75% della varianza
- Riflette fratture nord-sud ben documentate nella letteratura italiana

### 2. **Inerzia Elettorale**
- `PERC_SI_EL22` correlazione = 0.87 con target
- Se inclusa, assorbe ~99% del segnale predittivo
- Il referendum 2026 è stato in larga misura echo del voto 2022
- Variabili demografiche marginali una volta nota orientamento storico

### 3. **Performance Modelli**
| Modello | Accuracy | AUC | Predittori |
|---------|----------|-----|-----------|
| Logistica Base | 0.760 | 0.822 | 19 |
| LASSO | 0.757 | 0.817 | ~6 |
| Relaxed LASSO | 0.761 | 0.817 | ~6 |
| **Post-LASSO** | **0.764** | 0.820 | **3** |
| Decision Tree | 0.694 | - | Gerarchia |
| **Random Forest** | **0.785** | - | Ensemble |

### 4. **Paradosso Sociopolitico**
- SÌ trionfa numericamente (più comuni) ma perde nel voto assoluto
- Vittoria SÌ frammentata in migliaia di piccoli comuni demograficamente irrilevanti
- NO concentrato in poche metropoli con elevatissimi pesi demografici

### 5. **Fattori Demografici Secondari**
Ranking importanza (da RF):
1. POSIZIONE_GEO (dominante)
2. ALTA_ISTR
3. BASSA_ISTR
4. PERC_STRANIERI
5. INDICE_DISUGUAGLIANZA
6. INDICE_CETO_MEDIO
... (marginali)
14. ETA_MEDIA
15. PERC_MASCHILE

---

## 🛠️ Strumenti e Tecnologie

### Linguaggi
- **R** (primario): tidyverse, ggplot2, glmnet, randomForest, rpart, mice

### Librerie Principali
```r
# Data Manipulation
library(tidyverse)
library(mice)

# Visualizzazione
library(ggplot2)
library(ggcorrplot)
library(ggrepel)
library(sf)
library(mapview)

# Modellistica Parametrica
library(glmnet)
library(pROC)

# Modellistica Non Parametrica
library(randomForest)
library(rpart)
library(rpart.plot)
library(tree)

# Missing Data Diagnostics
library(VIM)
library(naniar)
library(visdat)
library(UpSetR)
```

### Dati Geografici
- Shapefile comuni italiani (WGS84): `Com01012026_WGS84.shp`
- Elaborazione: sf, ggplot2, leaflet

---

## 📂 Struttura dei File

```
├── README.md                          # Questo file
├── CONTRIBUTI.md                      # Ruoli e contributi team
├── Finale4.Rmd                        # Report principale (italiano)
├── Finale4_EN.Rmd                     # Report principale (inglese)
├── COMPOSIZIONE_DATASET.md            # Documentazione dettagliata dataset
├── data/
│   ├── f11.csv                        # Dataset grezzo integrato
│   ├── TRAINING.SET.csv               # Training set (75%)
│   └── TEST.SET.csv                   # Test set (25%)
├── models/
│   ├── logistic_model.rds             # Modello logistica
│   ├── lasso_model.rds                # Modello LASSO
│   ├── random_forest_model.rds        # Modello Random Forest
│   └── surrogate_tree.rds             # Albero surrogato XAI
├── output/
│   ├── figures/                       # Grafici e visualizzazioni
│   ├── metrics/                       # Tabelle performance
│   └── diagnostics/                   # Output diagnostici
└── scripts/
    ├── 01_data_loading.R              # Caricamento e integrazione dati
    ├── 02_eda.R                       # Analisi esplorativa
    ├── 03_imputation.R                # MICE imputation
    ├── 04_parametric_models.R         # Modelli parametrici
    ├── 05_nonparametric_models.R      # Modelli non parametrici
    └── 06_xai.R                       # Interpretabilità
```

---

## 🚀 Come Usare Questo Repository

### 1. **Clonare il Repository**
```bash
git clone https://github.com/vostroautore/cosa-guida-il-voto.git
cd cosa-guida-il-voto
```

### 2. **Installare Dipendenze**
```r
packages <- c("tidyverse", "mice", "ggplot2", "ggcorrplot", "ggrepel",
              "glmnet", "pROC", "randomForest", "rpart", "rpart.plot",
              "tree", "sf", "mapview", "VIM", "naniar", "visdat", "UpSetR")
              
install.packages(packages)
```

### 3. **Eseguire l'Analisi Completa**
```r
# Aprire e eseguire Finale4.Rmd in RStudio
# File > Open > Finale4.Rmd
# Poi: Ctrl+Shift+K (Knit to HTML/PDF)
```

### 4. **Accedere ai Dati**
```r
# Caricare il dataset principale
f11 <- read.csv("data/f11.csv")

# O il dataset già processato
train <- read.csv("data/TRAINING.SET.csv")
test <- read.csv("data/TEST.SET.csv")
```

---

## 📚 Referenze Bibliografiche

1. James, G., Witten, D., Hastie, T. & Tibshirani, R. (2021). *An Introduction to Statistical Learning: with Applications in R*. Springer Texts in Statistics. Springer, 2nd ed.

2. Meinshausen, N. (2007). Relaxed Lasso. *Computational Statistics & Data Analysis*, 52(1), 374–393.

3. Tibshirani, R. (1996). Regression Shrinkage and Selection via the Lasso. *Journal of the Royal Statistical Society B*, 58(1), 267–288.

4. Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5–32.

5. van Buuren, S. & Groothuis-Oudshoorn, K. (2011). mice: A Package for Missing Data Imputation in R. *Journal of Statistical Software*, 45(3), 1–67.

---

## 📧 Contatti e Domande

Per domande, suggerimenti o segnalazioni di errori, aprire un **Issue** o contattare i contributori.

---

## 📋 Licenza

Questo progetto è rilasciato sotto licenza **MIT**. Vedi il file `LICENSE` per dettagli.

---

## 🙏 Ringraziamenti

- **Lorenzo Ruffino** per lo script open-source di API scraping dei dati referendari
- **ISTAT** per i dati demografici
- **Ministero dell'Interno** per i dati elettorali (Eligendo e API ministeriali)
- **RStudio Community** per supporto tecnico e best practices

---

**Ultimo aggiornamento**: Aprile 2026
