# Vorhersage der Tagesrendite des Goldpreises (ML-Projekt)

Dieses Projekt sagt die **prozentuale Tagesrendite** des Goldpreises voraus. Grundlage sind historische Goldpreis-Daten plus makroökonomische & marktbezogene Einflussgrößen (z. B. GPR-Index, Zinsen, BIP). Ziel ist auch, **wichtige Treiber** der Rendite zu identifizieren. :contentReference[oaicite:1]{index=1}

## Inhalte / Dateien
- **Bericht / Dokumentation**
  - `Projekt Vorhersage.pdf` – Ziele, Daten, Pipeline, Modelle, Ergebnisse. :contentReference[oaicite:2]{index=2}
  - `Einschränkungen der Daten.pdf` – Datenqualität, Lücken, Annahmen. :contentReference[oaicite:3]{index=3}
  - `Definitionen.pdf` – Variablenquellen & Definitionen (Inflation, GPR, Fed Funds, …). :contentReference[oaicite:4]{index=4}
- **Notebooks (HTML-Exports)**
  - `Goldpreis_Vorhersage.html`, `Gold_MLRegressor (1).html` – Explorative Analysen, Korrelationen, Features. :contentReference[oaicite:5]{index=5}
- **Daten (Beispiele)**
  - `df_gold_macro.csv`, `gpr_filtered.csv`, `Inflation_Daily.csv`, `InterestDaily.csv`, `GDP_Daily.csv` (Dateinamen exemplarisch aus Projekt).  

## Zielgröße
**gold_tagesrendite (t+1)**: prozentuale Änderung des (Adjusted) Close zum Folgetag; die Rendite wird per `pct_change()` berechnet und **um 1 Tag vorverschoben** (`shift(-1)`), sodass heutige Merkmale die **Rendite von morgen** erklären. :contentReference[oaicite:6]{index=6}

## Datenquellen (Auszug)
- **Goldpreis (Kaggle)** – 1718 Zeilen / 80 Spalten; Preise, Volumina, Indizes, FX, Rohstoffe.  
- **BIP (API)** – auf tägliche Frequenz interpoliert.  
- **GPR (CSV)** – geopolitischer Risikoindex.  
- **Inflation (CSV)**, **Zinsen (CSV)** – makroökonomische Reihen.  
Beschaffung: **öffentlich zugänglich / per API** (siehe Doku). :contentReference[oaicite:7]{index=7}

## Vorverarbeitung
- **Volumina log-transformiert** (Ausreißerreduktion, weniger Schiefe).  
- **Preise in %-Returns** statt Niveaus (Vergleichbarkeit, Modellannahmen).  
- **Feature-Reduktion**: stark korrelierte Preisvarianten (O/H/L/C/AdjClose) auf wenige Spalten reduziert.  
- **Tägliche Frequenz** & sortierte Zeitachse. :contentReference[oaicite:8]{index=8} :contentReference[oaicite:9]{index=9}

## Modellierung & Ergebnisse (Kurzfassung)
Getestet wurden **Lineare Regression (LR)**, **Random Forest (RF)** und **Neurales Netz (NN)**.  
- **Trainings-/CV-Scores** zeigen **RF** stark im Training, aber Tendenz zu **Overfitting**;  
- **Auf Test** performt **LR** robust, RF moderat, NN schwach.  
Details / Tabellen: R² & MAE für Train/CV/GridSearch/Test. :contentReference[oaicite:10]{index=10} :contentReference[oaicite:11]{index=11}

## Bekannte Einschränkungen (wichtig!)
- **Goldpreis in Kaggle** entspricht **GLD-ETF**, nicht Spot – das kann leichte Abweichungen erzeugen.  
- **Fehlende Handelstage** (NYSEListe dokumentiert) und **unklare Trend-Berechnungen** in Fremddaten → sorgfältig behandeln. :contentReference[oaicite:12]{index=12} :contentReference[oaicite:13]{index=13}

## Projektteam / Rollen
- **Lisa**: Preprocessing, RF-Modell.  
- **Tanja**: Dokumentation, Zielwertdefinition, Hilfsfunktionen (z. B. `extract_price_adjclose_columns`, `add_pct_change_columns`).  
- **Nenad**: Lineare Modelle, Methodik für Zielspalten-Shift. :contentReference[oaicite:14]{index=14}

## Setup (Beispiel)
```bash
# Python >= 3.10 empfohlen
python -m venv .venv
# Windows:
.venv\Scripts\activate
# Linux/Mac:
source .venv/bin/activate

pip install -r requirements.txt  # falls vorhanden
# oder interaktiv: pandas, numpy, scikit-learn, matplotlib, jupyter, seaborn, xgboost (optional)
````
