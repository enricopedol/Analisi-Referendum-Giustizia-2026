# Contributi al Progetto

## 👥 Team del Progetto

### Team di Analisi

#### **Luca Oggioni**
- **Sezione**: Data Integration & EDA
- **Contributi**:
  - Integrazione dataset ISTAT + Eligendo + API ministeriali
  - Pulizia dati e gestione valori mancanti
  - Analisi esplorativa (EDA) completa
  - Identificazione anomalie e outlier
  - Creazione della matrice di correlazione
  - Analisi del "quadrante politico" (relazione 2022 vs. 2026)
  - Diagnostica MICE e validazione imputazione
  - Visualizzazioni geografiche dei risultati

#### **Filippo Valente**
- **Sezione**: Parametric Modeling 
- **Contributi**:
  - Implementazione Regressione Logistica base
  - Implementazione LASSO e Relaxed LASSO via glmnet
  - Tuning dei parametri λ e γ
  - Cross-validation per selezione modelli
  - Implementazione Post-LASSO (LASSO + Logistica)
  - Analisi comparativa delle performance
  - Interpretazione dei coefficienti
  - Creazione ROC curves e metriche AUC
  - Documentazione metodologica dei modelli lineari

#### **Enrico Pedol**
- **Sezione**: Non-Parametric Modeling & XAI 
- **Contributi**:
  - Implementazione Decision Tree con RPART e Tree package
  - Tree pruning e cross-validation
  - Implementazione Random Forest (500 alberi)
  - Tuning mtry e ntree
  - Analisi OOB (Out-of-Bag) error
  - Variable importance plots
  - Implementazione Modello Surrogato Globale (XAI)
  - Estrazione regole interpretabili dal Random Forest
  - Analisi dell'architettura decionale e nodi terminali

---


## 💻 Competenze Utilizzate


- ✅ Data Wrangling (tidyverse, dplyr)
- ✅ Analisi esplorativa (EDA)
- ✅ Data cleaning & preprocessing
- ✅ MICE imputation
- ✅ Visualizzazione (ggplot2, mapview)
- ✅ Analisi geografica (sf, spatial data)
- ✅ Diagnostica statistica
- ✅ Regressione logistica
- ✅ LASSO e varianti (Ridge, Elastic Net)
- ✅ glmnet e cv.glmnet
- ✅ Cross-validation strategies
- ✅ ROC/AUC analysis
- ✅ Model comparison
- ✅ Inferenza statistica
- ✅ Feature selection
- ✅ Decision Tree algorithms
- ✅ Tree pruning methods
- ✅ Random Forest
- ✅ Ensemble methods
- ✅ Variable importance
- ✅ Interpretable ML
- ✅ XAI (Explainable AI)
- ✅ Rule extraction

---

## 🏆 Risultati 

**Highlight**: Scoperta del pattern MAR nei dati mancanti
- Poteva essere scartata come MCAR
- Identificazione corretta di Trentino/Valle d'Aosta come fattore
- Giustificò l'uso di MICE piuttosto che list-wise deletion

**Highlight**: Analisi outlier sofisticata
- Distinzione outlier vs. leverage points
- Caso Roma particolarmente illuminante
- Micro-comuni come effetto specifico, non errori

**Highlight**: Model parsimony con Post-LASSO
- Raggiunto accuracy 0.764 con soli 3 predittori
- Accuracy più alta tra modelli parametrici
- Interpretazione diretta: ALTA_ISTR, POSIZIONE_GEO, LOG_POPOLAZIONE

**Highlight**: Confronto cross-validation
- Dimostrato che segnale è concentrato
- Trade-off bias-varianza ben gestito
- AUC stabili (0.815-0.822 range)

**Highlight**: Random Forest dominante
- Accuracy 0.785 (miglior modello complessivo)
- OOB convergence a 150 alberi (effizienza)
- Ranking importanza variabili molto pulito

**Highlight**: Surrogate tree interpretabilità
- Regole estratte altamente leggibili
- Nord = 98% SÌ (nodo più omogeneo)
- Sud = 93% NO (secondo nodo omogeneo)
- Densità abitativa come fattore secondario ma rilevante

---

## 📝 Note Finali

### Qualità del Lavoro
- **Code Quality**: Elevato (R style guide, DRY principle)
- **Documentation**: Completo (inline + README)
- **Testing**: Validazioni diagnostiche rigorose
- **Reproducibility**: Tutti gli script gestiscono seed e version control

--- 

**Progetto completato**: Aprile 2026
**Data del documento**: Aprile 2026
