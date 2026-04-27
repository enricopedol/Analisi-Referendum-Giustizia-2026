# Changelog

Tutti i cambiamenti rilevanti di questo progetto saranno documentati in questo file.

Il formato segue [Keep a Changelog](https://keepachangelog.com/it/) e il versionamento
segue [Semantic Versioning](https://semver.org/lang/it/).

---

## [1.0.0] - 2026-04-30

### Rilascio Finale

#### Added
- **Report principale** (`Finale4.Rmd`): Analisi completa con tutte le sezioni
  - Sezione 1: Introduzione e domanda di ricerca
  - Sezione 2: Analisi esplorativa, gestione dati mancanti, outlier detection
  - Sezione 3: Modelli parametrici (Logistica, LASSO, Relaxed LASSO, Post-LASSO)
  - Sezione 4: Modelli non parametrici (Decision Tree, Random Forest, XAI)
  - Sezione 5: Conclusioni e insights
- **Report tradotto** (`Finale4_EN.Rmd`): Versione integrale in inglese
- **Documentazione GitHub**:
  - `README.md`: Overview completo, struttura del dataset, metodologie, risultati
  - `CONTRIBUTI.md`: Ruoli, contributi del team, decisioni metodologiche condivise
  - `COMPOSIZIONE_DATASET.md`: Dettaglio completo di tutte le 21 variabili
  - `.gitignore`: File da escludere dal versioning
  - `LICENSE`: Licenza MIT
  - `CHANGELOG.md`: Questo file

#### Features Implementate
- ✅ Integrazione dati ISTAT + Eligendo + API ministeriali
- ✅ Analisi dati mancanti (MAR pattern identification)
- ✅ Imputazione MICE (PMM, m=5, estrazione m=1)
- ✅ Data scaling (Min-Max [0,1])
- ✅ Regressione Logistica base
- ✅ LASSO con cv.glmnet (lambda.1se)
- ✅ Relaxed LASSO (gamma optimization)
- ✅ Post-LASSO (selezione + logistica)
- ✅ Decision Tree con pruning
- ✅ Random Forest (500 alberi, OOB validation)
- ✅ Modello Surrogato Globale (XAI via rpart)
- ✅ Visualizzazioni geografiche
- ✅ Matrice di correlazione completa
- ✅ Diagnostica outlier (studentized residuals, Cook's distance)
- ✅ Variable importance plots
- ✅ ROC/AUC analysis

#### Performance Results
- **Post-LASSO** (best parametric): Accuracy 0.764, AUC 0.820, 3 predittori
- **Random Forest** (best overall): Accuracy 0.785, OOB error ~17%
- **Surrogate Tree** (XAI): Highly interpretable, 4 terminal nodes

#### Dataset
- **Comuni nel dataset finale**: 6,710 (95% dei 7,094 totali)
- **Training set**: 5,032 comuni (75%)
- **Test set**: 1,678 comuni (25%)
- **Variabili**: 21 (dopo preprocessing)
- **Bilanciamento target**: 57% SÌ, 43% NO (ben bilanciato)

---

## [0.9.0] - 2026-04-28

### Pre-release per review interno

#### Added
- Draft completo di Finale4.Rmd
- Risultati parametrici e non parametrici
- XAI analysis iniziale

#### Fixed
- Correzzionе bug MICE convergence diagnostics
- Ajustment scaling parameters per evitare data leakage

#### Known Issues
- Alcuni chunk knit lentamente su dataset completo (no optimization done)
- Figure size inconsistency in alcuni output

---

## [0.5.0] - 2026-04-20

### Data Preparation & EDA Phase

#### Added
- Caricamento e integrazione dati da tre fonti
- Missing data analysis (UpSet plots, spatial maps)
- Outlier identification via studentized residuals
- Correlation matrix visualization
- "Quadrante politico" scatter plot

#### Features
- ✅ MICE imputation framework setup
- ✅ Data cleaning pipeline
- ✅ Train/test split infrastructure

---

## [0.1.0] - 2026-04-01

### Progetto Avviato

#### Initial Setup
- Definizione domanda di ricerca
- Pianificazione metodologica
- Allocazione risorse team
- Setup ambiente R + librerie

---

## Note su Versioning

### Come Interpretare i Numeri di Versione

**MAJOR.MINOR.PATCH** (es. 1.0.0)

- **MAJOR**: Cambamenti breaking o milestones significativi
- **MINOR**: Nuove features backward-compatible
- **PATCH**: Bug fixes

### Piano di Versioning Futuro

- **v1.0.0**: Rilascio stabile current (2026-04-30)
- **v1.1.0**: Aggiunta XGBoost + SHAP values
- **v1.2.0**: Serie temporali (2013-2026) se dati disponibili
- **v2.0.0**: Modelli bayesiani + analisi causale (future work)

---

## Roadmap Futuro

### Breve Termine (prossimi mesi)
- [ ] Ottimizzazione runtime Rmd knit
- [ ] Aggiunta test suite in R
- [ ] Documentazione API dati
- [ ] Creazione dashboard interattivo Shiny

### Medio Termine (prossimi 6-12 mesi)
- [ ] Implementazione XGBoost
- [ ] SHAP values per local interpretability
- [ ] Causal inference (double machine learning)
- [ ] Sensitivity analysis su feature selection

### Lungo Termine (future work)
- [ ] Temporal analysis se dati storici disponibili
- [ ] Spatial econometrics (autoregressione spaziale)
- [ ] Integration con API Eligendo aggiornate
- [ ] Web app per predizioni real-time

---

## Contribuenti

Per problemi, domande o suggerimenti, contattare gli autori tramite GitHub Issues.

- **Luca Oggioni** - Data Integration & EDA
- **Filippo Valente** - Parametric Modeling
- **Enrico Pedol** - Non-Parametric & XAI

---

## Come Contribuire (se open source)

1. Fork il repository
2. Crea un branch per la feature (`git checkout -b feature/AmazingFeature`)
3. Commit i cambiamenti (`git commit -m 'Add some AmazingFeature'`)
4. Push al branch (`git push origin feature/AmazingFeature`)
5. Apri una Pull Request

---

**Ultimo update**: 2026-04-30
