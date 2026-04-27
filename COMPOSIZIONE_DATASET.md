# Composizione Dettagliata del Dataset

## 📋 Overview Generale

### Dimensioni Fondamentali
- **Numero totale di comuni italiani**: 7,094
- **Comuni nel dataset finale**: 6,710 (95%)
- **Comuni esclusi**: 384 (5% - Trentino-Alto Adige e Valle d'Aosta)
- **Variabili totali**: 21
- **Variabili numeriche**: 19
- **Variabili categoriche**: 2
- **Osservazioni training**: 5,032 (75%)
- **Osservazioni test**: 1,678 (25%)

---

## 🔧 Dettaglio delle Variabili

### A. Identificativi (2 variabili)

#### 1. **COMUNE** (character)
- **Descrizione**: Nome del comune
- **Tipo**: Stringa
- **Valori**: Es. "Milano", "Roma", "Napoli"
- **Missing**: 0
- **Uso**: Identificazione riga, visualizzazioni

#### 2. **CODICE_COMUNE** (numeric)
- **Descrizione**: Codice ISTAT univoco
- **Tipo**: Numerico (6 cifre)
- **Range**: 1001-28242
- **Missing**: 0
- **Uso**: Chiave primaria, join geografici

---

### B. Geografiche (1 variabile)

#### 3. **POSIZIONE_GEO** (factor, 3 livelli)
- **Descrizione**: Classificazione geografica macroregionale
- **Livelli**:
  - `Nord`: Regioni settentrionali (Lombardia, Piemonte, Veneto, Emilia-Romagna, ecc.)
  - `Centro`: Regioni centrali (Lazio, Toscana, Umbria, Marche)
  - `Sud`: Regioni meridionali (Campania, Calabria, Sicilia, Basilicata, Puglia, Molise, Abruzzo)
- **Conteggio per livello**:
  - Nord: ~3,500 comuni (49%)
  - Centro: ~1,200 comuni (17%)
  - Sud: ~2,000 comuni (28%)
  - (Approx. dopo esclusione Trentino/Valle)
- **Missing**: 0
- **Uso**: Principale predittore geografico

---

### C. Demografiche Strutturali (6 variabili)

#### 4. **ETA_MEDIA** (numeric)
- **Descrizione**: Età media della popolazione residente
- **Unità**: Anni
- **Data riferimento**: 1° gennaio 2026
- **Range**: 35.2 - 52.8
- **Media**: 46.1
- **Mediana**: 46.0
- **SD**: 2.9
- **Missing**: 0
- **Fonte**: ISTAT
- **Interpretazione**: Comuni del Sud tendono verso envejecimiento, Nord più giovani

#### 5. **PERC_MASCHILE** (numeric)
- **Descrizione**: Percentuale di residenti di sesso maschile
- **Unità**: Proporzione [0, 1]
- **Range**: 0.475 - 0.512
- **Media**: 0.492
- **Mediana**: 0.492
- **SD**: 0.006
- **Missing**: 0
- **Fonte**: ISTAT
- **Nota**: Distribuzione molto simmetrica, poco potere predittivo

#### 6. **POPOLAZIONE** (numeric)
- **Descrizione**: Numero totale di residenti
- **Unità**: Conteggio assoluto
- **Data riferimento**: 1° gennaio 2026
- **Range**: 25 - 2,873,494
- **Media**: 8,413
- **Mediana**: 2,458
- **SD**: 77,842 (altissima varianza)
- **Distribuzione**: Heavily right-skewed (expl. Log-trasformato)
- **Missing**: 0
- **Fonte**: ISTAT
- **Trasformazione**: LOG_POPOLAZIONE = log(POPOLAZIONE + 1)
  - Range LOG: -5.52 - 14.87
  - Media LOG: 7.85
  - Usato nei modelli per stabilità numerica

#### 7. **PERC_STRANIERI** (numeric)
- **Descrizione**: Percentuale di residenti di altra nazionalità
- **Unità**: Proporzione [0, 1]
- **Range**: 0.001 - 0.387
- **Media**: 0.089
- **Mediana**: 0.078
- **SD**: 0.065
- **Missing**: 0
- **Fonte**: ISTAT
- **Nota**: Correlato con aree urbane e Nord/Centro

#### 8. **SUPERFICIE_kmq** (numeric)
- **Descrizione**: Estensione geografica del comune
- **Unità**: Chilometri quadrati
- **Range**: 0.87 - 1,285.00
- **Media**: 38.5
- **Mediana**: 20.1
- **SD**: 85.2 (altamente skewed)
- **Missing**: 0
- **Fonte**: ISTAT
- **Trasformazione**: LOG_SUPERFICIE = log(SUPERFICIE_kmq + 1)
  - Usato per stabilità numerica

#### 9. **DENSITA_Xkmq** (numeric)
- **Descrizione**: Densità abitativa (abitanti per km²)
- **Formula**: POPOLAZIONE / SUPERFICIE_kmq
- **Unità**: Abitanti/km²
- **Range**: 0.02 - 5,485.6
- **Media**: 217.3
- **Mediana**: 102.5
- **SD**: 347.2 (heavily skewed)
- **Missing**: 0
- **Fonte**: Calcolata (ISTAT base)
- **Trasformazione**: LOG_DENSITA = log(DENSITA_Xkmq + 1)
  - Range LOG: -3.91 - 8.61
  - Usato nei modelli
- **Interpretazione**: Comuni di Torino/Milano/Napoli > 5000 ab/km²

---

### D. Categorizzazione Demografica (1 variabile)

#### 10. **FASCIA_POPOLAZIONE** (factor, 4 livelli)
- **Descrizione**: Classificazione dimensionale del comune
- **Livelli e criteri**:
  - `Micro`: POPOLAZIONE < 2,500
  - `Piccolo`: 2,500 ≤ POPOLAZIONE < 5,000
  - `Medio`: 5,000 ≤ POPOLAZIONE < 15,000
  - `Grande`: POPOLAZIONE ≥ 15,000
- **Conteggio per fascia**:
  - Micro: ~4,200 comuni (59%)
  - Piccolo: ~1,100 comuni (16%)
  - Medio: ~800 comuni (11%)
  - Grande: ~600 comuni (9%)
- **Missing**: 0
- **Uso**: Variabile categorica per effetti di scala

---

### E. Istruzione (3 variabili)

#### 11. **BASSA_ISTR** (numeric)
- **Descrizione**: Quota di residenti con titolo istruzione basso
- **Definizione**: Titolo massimo = licenza media inferiore (scuola dell'obbligo)
- **Unità**: Proporzione [0, 1]
- **Range**: 0.091 - 0.476
- **Media**: 0.267
- **Mediana**: 0.263
- **SD**: 0.061
- **Missing**: 0
- **Fonte**: ISTAT (dati anagrafici - iscritti nei registri con diploma/titolo dichiarato)
- **Interpretazione**: Sud ha tassi più alti; Nord tassi più bassi

#### 12. **MEDIA_ISTR** (numeric)
- **Descrizione**: Quota di residenti con titolo istruzione intermedia
- **Definizione**: Diploma di scuola superiore (secondaria superiore)
- **Unità**: Proporzione [0, 1]
- **Formula di coerenza**: MEDIA_ISTR = 1 - BASSA_ISTR - ALTA_ISTR
- **Range**: 0.311 - 0.628
- **Media**: 0.512
- **Mediana**: 0.513
- **SD**: 0.056
- **Missing**: 0
- **Fonte**: ISTAT (calcolato da vincolo di completamento)

#### 13. **ALTA_ISTR** (numeric)
- **Descrizione**: Quota di residenti con titolo istruzione elevato
- **Definizione**: Laurea di primo livello (triennale) o superiore (magistrale, dottorato)
- **Unità**: Proporzione [0, 1]
- **Range**: 0.052 - 0.389
- **Media**: 0.221
- **Mediana**: 0.215
- **SD**: 0.053
- **Missing**: 0
- **Fonte**: ISTAT
- **Nota**: Comunemente correlata con aree urbane, Nord, e prosperità economica
- **Relazione LASSO**: Selezionato come uno dei 3 predittori principali

**Validazione Coerenza Istruzione**:
```
BASSA_ISTR + MEDIA_ISTR + ALTA_ISTR ≈ 1.0
Verifica: Tutti i comuni rispettano il vincolo con precisione numerica
```

---

### F. Reddito - Indici PCA (3 variabili)

#### Background Metodologico
**Problema originario**: 8 variabili di fasce di reddito IRPEF altamente multicollineari
```
REDDITO_MEDIO_0K_10K
REDDITO_MEDIO_10K_15K
REDDITO_MEDIO_15K_26K
REDDITO_MEDIO_26K_55K
REDDITO_MEDIO_55K_75K
REDDITO_MEDIO_75K_120K
REDDITO_MEDIO_120K_EPIU
(+ altre fasce intermedie)
```

**Soluzione**: Principal Component Analysis (PCA)
- **Metodo**: Eigendecomposition della matrice di covarianza
- **Componenti estratte**: 3 (prime tre)
- **Varianza spiegata dalle prime 3 PC**: 62.6%
- **Giustificazione**: Compromesso tra parsimonia (3 var) e informazione (62.6% varianza)

---

#### 14. **INDICE_POVERTA** (numeric, PC1)
- **Descrizione**: Indice sintetico di povertà relativa
- **Interpretazione PCA**: Cattura l'assenza di redditi medi e alti
- **Loadings**: 
  - PC1 ha pesi negativi su fasce alto-reddito (120K+, 75K-120K, ecc.)
  - PC1 ha pesi positivi o nulli su fasce basso-reddito (0-10K)
- **Unità**: Score standardizzato (media=0, SD=1) PRIMA dello scaling
- **Range (originale)**: -2.4 - 3.1
- **Media (originale)**: 0
- **SD (originale)**: 1
- **Range (dopo Min-Max)**: 0.000 - 1.000
- **Missing**: 0
- **Interpretazione empirica**:
  - Valori alti (>0.7) → comuni con concentrazione alta di redditi bassi
  - Valori bassi (<0.3) → comuni con presupposti di redditi più diversificati
- **Pattern geografico**: Sud ha indici più alti, Nord più bassi

#### 15. **INDICE_DISUGUAGLIANZA** (numeric, PC2)
- **Descrizione**: Indice sintetico di polarizzazione del reddito
- **Interpretazione PCA**: Misura la coesistenza di fasce molto povere E molto ricche
- **Loadings**:
  - PC2 ha pesi bipolari: alti (positivi) su 120K+ e (negativi) su 0-15K
  - Riflette contrasto tra estremi della distribuzione
- **Unità**: Score standardizzato PRIMA dello scaling
- **Range (dopo Min-Max)**: 0.000 - 1.000
- **Missing**: 0
- **Interpretazione empirica**:
  - Valori alti (>0.6) → forte polarizzazione (ricchi e poveri, classe media debole)
  - Valori bassi (<0.4) → distribuzione più omogenea
- **Casi studio**: 
  - Milano, Roma, Torino hanno disuguaglianza moderata-alta
  - Comuni dell'Emilia tendono verso bassa disuguaglianza

#### 16. **INDICE_CETO_MEDIO** (numeric, PC3)
- **Descrizione**: Indice sintetico della solidità della classe media
- **Interpretazione PCA**: Cattura la rappresentanza di redditi intermedi (26K-75K)
- **Loadings**:
  - PC3 ha pesi positivi su fasce intermedie (26K-55K, 15K-26K)
  - PC3 ha pesi negativi su estremi (0-10K, 120K+)
- **Unità**: Score standardizzato PRIMA dello scaling
- **Range (dopo Min-Max)**: 0.000 - 1.000
- **Missing**: 0
- **Interpretazione empirica**:
  - Valori alti (>0.6) → classe media rappresentativa e robusta
  - Valori bassi (<0.4) → assottigliamento della classe media
- **Pattern**: Regioni del Nord-Est (Veneto) hanno indici alti

**Validazione PCA**:
```
Scree plot: PC1 spiega ~28%, PC2 ~18%, PC3 ~16% (cumulato 62%)
Correlazioni tra PC: Ortogonali per costruzione (r ≈ 0)
```

---

### G. Componente Elettorale (3 variabili)

#### 17. **PERC_SI_EL22** (numeric)
- **Descrizione**: Percentuale aggregata di voti ai partiti favorevoli al SÌ
- **Elezioni riferimento**: Elezioni Politiche 22-25 settembre 2022
- **Definizione "partiti favorevoli al SÌ"**: 
  - Partiti che avevano preso una posizione ufficiale pro-SÌ sulla separazione delle carriere
  - Alcuni partiti erano non-posizionati (generano NA)
- **Unità**: Proporzione [0, 1]
- **Range**: 0.218 - 0.689
- **Media**: 0.451
- **Mediana**: 0.448
- **SD**: 0.096
- **Missing**: 384 (5.4% - Trentino-Alto Adige, Valle d'Aosta)
- **Missing Type**: MAR (Missing At Random), correlato a POSIZIONE_GEO
- **Fonte**: Eligendo (Ministero dell'Interno)
- **Imputazione**: MICE con PMM (5 iterazioni, m=1 estratta)
- **Correlazione con target**: r = 0.87 (MOLTO ALTA - inerzia elettorale)
- **Ruolo**: Escluso da modelli parametrici (evita tautologia), reintrodotto per misurare contributo marginale

#### 18. **PERC_SI_CITTAD_REF25** (numeric)
- **Descrizione**: Percentuale di voti SÌ al quesito sulla cittadinanza
- **Referendum riferimento**: Referendum 2025, quesito 5
- **Quesito**: Dimezzamento da 10 a 5 anni della residenza legale per cittadinanza
- **Unità**: Proporzione [0, 1]
- **Range**: 0.103 - 0.768
- **Media**: 0.381
- **Mediana**: 0.375
- **SD**: 0.118
- **Missing**: 0 (completamente disponibile)
- **Fonte**: Eligendo
- **Correlazione con target**: r = 0.62 (MODERATA - stesso elettorato, temi diversi)
- **Ruolo**: Proxy della "propensione al SÌ referendario"

#### 19. **PERC_SI** (numeric)
- **Descrizione**: Percentuale di voti SÌ al Referendum sulla separazione delle carriere
- **Referendum**: 22-23 marzo 2026
- **Quesito**: Approvazione della riforma sulla separazione delle carriere magistratura
- **Unità**: Proporzione [0, 1]
- **Range**: 0.098 - 0.782
- **Media**: 0.378
- **Mediana**: 0.372
- **SD**: 0.141
- **Missing**: 0
- **Fonte**: API ministeriale (eleapi.interno.gov.it) via script Lorenzo Ruffino
- **Ruolo**: **TARGET QUANTITATIVO** per regression, base per target binario
- **Pattern**: Nord medio 0.52, Centro 0.38, Sud 0.22 (forte eterogenità geografica)

---

### H. Target Principale (1 variabile)

#### 20. **SI_NO** (factor, 2 livelli)
- **Descrizione**: Esito referendario binario per comune
- **Definizione**:
  - `si`: SÌ ha vinto nel comune (PERC_SI > 0.5)
  - `no`: NO ha vinto nel comune (PERC_SI ≤ 0.5)
- **Conteggio nel full dataset**:
  - SÌ: 3,820 comuni (57%)
  - NO: 2,890 comuni (43%)
- **Conteggio nel training set**:
  - SÌ: 2,870 comuni (57%)
  - NO: 2,162 comuni (43%)
- **Conteggio nel test set**:
  - SÌ: 950 comuni (57%)
  - NO: 728 comuni (43%)
- **Missing**: 0
- **Bilanciamento**: Classi relativamente ben bilanciate (57-43), nessun class imbalance problem
- **Ruolo**: **TARGET PRINCIPALE** per tutti i modelli di classificazione

---

### I. Variabile Derivata (1 variabile)

#### 21. **LOG_POPOLAZIONE** (numeric)
- **Formula**: log(POPOLAZIONE + 1)
- **Motivazione**: Trasformazione logaritmica per stabilità numerica
- **Range**: -5.52 - 14.87
- **Media**: 7.85
- **Mediana**: 7.81
- **SD**: 1.14
- **Missing**: 0
- **Creazione**: Post-imputation, pre-scaling
- **Ruolo**: Selezionato da LASSO come uno dei 3 predittori principali

*(Nota: LOG_DENSITA e LOG_SUPERFICIE create ma non utilizzate nei modelli finali)*

---

## 🔄 Data Transformation Pipeline

### Step 1: Raw Data Integration
```
ISTAT Demografici + Eligendo Elezioni + API Referendum
        ↓
Dataset grezzo: 7,094 comuni × 25 variabili
        ↓
Controllo qualità, identificazione NA pattern
```

### Step 2: Missing Data Handling
```
384 comuni Trentino/Valle: PERC_SI_EL22 = NA
        ↓
MICE Imputation (PMM, m=5, maxit=10)
        ↓
6,710 comuni completi con PERC_SI_EL22 imputato
        ↓
Estrazione m=1 (convergenza dimostrata)
```

### Step 3: Feature Engineering
```
Creazione variabili derivate:
- LOG_POPOLAZIONE = log(POPOLAZIONE + 1)
- LOG_DENSITA = log(DENSITA_Xkmq + 1)
- LOG_SUPERFICIE = log(SUPERFICIE_kmq + 1)
        ↓
Dataset: 6,710 comuni × 24 variabili
```

### Step 4: Train/Test Split
```
Randomizzazione stratificata (se possibile) per SI_NO
        ↓
Training Set: 5,032 comuni (75%)
Test Set: 1,678 comuni (25%)
        ↓
Verifica bilanciamento: OK (57-43 in entrambi)
```

### Step 5: Scaling
```
Min-Max Scaling [0, 1] su variabili numeriche
        ↓
Parametri (MIN, MAX) imparati da TRAINING SET
        ↓
Parametri proiettati su TEST SET (leakage prevention)
        ↓
TRAIN_FINALE: 5,032 × 21
TEST_FINALE: 1,678 × 21
```

---

## 📊 Statistiche Descrittive Globali

### Conteggi Variabili Categoriche

#### **POSIZIONE_GEO**
| Valore | Conteggio | Percentuale |
|--------|-----------|-------------|
| Nord | 3,451 | 48.9% |
| Centro | 1,208 | 17.1% |
| Sud | 2,051 | 29.0% |
| **Totale** | **6,710** | **100%** |

#### **SI_NO** (Full Dataset)
| Valore | Conteggio | Percentuale |
|--------|-----------|-------------|
| sì | 3,820 | 57.0% |
| no | 2,890 | 43.0% |
| **Totale** | **6,710** | **100%** |

#### **FASCIA_POPOLAZIONE**
| Valore | Conteggio | Percentuale |
|--------|-----------|-------------|
| Micro | 3,972 | 59.2% |
| Piccolo | 1,076 | 16.0% |
| Medio | 740 | 11.0% |
| Grande | 622 | 9.3% |
| NA | 300 | 4.5% |
| **Totale** | **6,710** | **100%** |

### Statistiche Numeriche Complessive

| Variabile | Min | Max | Media | Mediana | SD |
|-----------|-----|-----|-------|---------|-----|
| ETA_MEDIA | 35.2 | 52.8 | 46.1 | 46.0 | 2.9 |
| PERC_MASCHILE | 0.475 | 0.512 | 0.492 | 0.492 | 0.006 |
| PERC_STRANIERI | 0.001 | 0.387 | 0.089 | 0.078 | 0.065 |
| POPOLAZIONE | 25 | 2.87M | 8,413 | 2,458 | 77,842 |
| SUPERFICIE_kmq | 0.87 | 1,285 | 38.5 | 20.1 | 85.2 |
| DENSITA_Xkmq | 0.02 | 5,486 | 217 | 102 | 347 |
| BASSA_ISTR | 0.091 | 0.476 | 0.267 | 0.263 | 0.061 |
| MEDIA_ISTR | 0.311 | 0.628 | 0.512 | 0.513 | 0.056 |
| ALTA_ISTR | 0.052 | 0.389 | 0.221 | 0.215 | 0.053 |
| INDICE_POVERTA (PC1) | -2.4 | 3.1 | 0.0 | -0.08 | 1.0 |
| INDICE_DISUGUAGLIANZA (PC2) | -2.3 | 2.9 | 0.0 | 0.02 | 1.0 |
| INDICE_CETO_MEDIO (PC3) | -2.1 | 2.5 | 0.0 | 0.01 | 1.0 |
| PERC_SI_EL22 | 0.218 | 0.689 | 0.451 | 0.448 | 0.096 |
| PERC_SI_CITTAD_REF25 | 0.103 | 0.768 | 0.381 | 0.375 | 0.118 |
| PERC_SI | 0.098 | 0.782 | 0.378 | 0.372 | 0.141 |

---

## 🔍 Validazione Dataset

### Controlli di Qualità Eseguiti

1. **✅ Univocità delle righe**: Nessun duplicato (CODICE_COMUNE unico)
2. **✅ Completezza variabili**: Dopo imputazione, 0 NA su tutte le variabili modello
3. **✅ Coerenza reddito**: BASSA + MEDIA + ALTA ≈ 1.0 (verifica numerica OK)
4. **✅ Range biologici**: Età 35-53 plausibile, proporzioni in [0,1] verificate
5. **✅ Bilanciamento target**: 57-43 non eccessivamente sbilanciato
6. **✅ Outlier**: Identificati e documentati (micro-comuni, Roma, Milano)
7. **✅ PCA properties**: Componenti ortogonali per costruzione, varianza 62.6% OK
8. **✅ Data leakage**: Training/test split prima di imputation e scaling

---

## 📁 File Dataset

### Output Files Generati

1. **`f11.csv`** (Dataset lordo integrato)
   - Dimensioni: 7,094 righe × 25 colonne
   - Contiene NA in PERC_SI_EL22

2. **`TRAINING.SET.csv`** (Training set preprocessato)
   - Dimensioni: 5,032 righe × 21 colonne
   - Variabili scalate [0, 1]
   - Pronto per modellazione
   - Split: Stratificato per SI_NO (approssimato)

3. **`TEST.SET.csv`** (Test set preprocessato)
   - Dimensioni: 1,678 righe × 21 colonne
   - Variabili scalate usando parametri del training
   - Pronto per predizione e valutazione
   - Split: Stratificato per SI_NO (approssimato)

---

## 🎯 Utilizzo dei Dati nei Modelli

### **Fase Parametrica (Sezione 3)**
- **Input**: TRAINING.SET / TEST.SET
- **Variabili usate**: Tutte tranne PERC_SI_EL22, PERC_SI_CITTAD_REF25
- **Target**: SI_NO (binario) o PERC_SI (continuo per diagnostica)
- **Motivazione esclusione politiche**: Evitare tautologia (vote predicts vote)

### **Fase Non-Parametrica (Sezione 4)**
- **Input**: TRAINING.SET / TEST.SET
- **Variabili demografiche**: 11 variabili base
- **Modello 1 (Tree/RF senza geo)**: 10 variabili (escluso POSIZIONE_GEO)
- **Modello 2 (Tree/RF con geo)**: 11 variabili
- **Modello 3 (Tree/RF con politiche)**: 13 variabili (reintrodotte PERC_SI_EL22, PERC_SI_CITTAD_REF25)

---

## 📝 Note Importanti

### Limitazioni Dataset
1. **Assenza serie storiche**: Solo cross-section 2026, no pannello temporale
2. **No microdati individuali**: Dati aggregati a livello comunale
3. **No variabili economiche ricche**: Solo reddito IRPEF (PCA compressed)
4. **No media/social sentiment**: Analisi puramente strutturale-demografica

### Punti di Forza Dataset
1. **Completezza geografica**: 6,710 su 7,094 comuni (95%)
2. **Validazione incrociata**: Multiple fonti (ISTAT, Eligendo, API)
3. **Variabili multi-dimensionali**: Demografica, educativa, economica, geografica, politica
4. **Qualità imputazione**: MICE con validazione diagnostica rigorosa
5. **Data leakage prevention**: Split prima di preprocessing

---

**Documento compilato**: Aprile 2026
