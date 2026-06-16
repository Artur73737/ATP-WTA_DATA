# ATP-WTA Data

Mirror dei dati di tennis (ATP & WTA) + un visualizzatore web per consultarli online.

Questo repository nasce come **copia di sicurezza** dopo che le fonti originali
(repo di Jeff Sackmann) sono state rimosse. Contiene i match storici, i ranking,
le quote e dati aggiuntivi raccolti da più fonti.

## 🌐 Visualizzatore online

I dati sono consultabili nel browser, senza scaricare nulla:

**👉 https://artur73737.github.io/ATP-WTA_DATA/**

È un'unica pagina (`index.html`) con tutti i CSV incorporati: barra laterale per
fonte e sezione, ricerca e ordinamento delle colonne, download dei singoli file.
Viene ripubblicata automaticamente a ogni aggiornamento (GitHub Pages + Actions).

## 📁 Struttura

```
raw/
├── atp/                 # match ATP per anno (atp_matches_YYYY.csv) + players + odds/
├── wta/                 # match WTA per anno + players + odds/
└── data/                # dati raccolti via scraper, per fonte
    ├── tennis_db/        # matches, rankings, elo, h2h, tournaments, players
    ├── tennis_explorer/  # atp, wta, challenger, futures, rankings, h2h, odds
    └── tennis_abstract/  # elo, surface_ratings, forecast, advanced_stats, match_charting
```

- **`raw/atp`, `raw/wta`** — il dataset principale in formato Sackmann
  (`*_matches_<anno>.csv`), copertura **1968–2026**, più `*_players.csv` e le
  quote bookmaker in `odds/`.
- **`raw/data/...`** — dati aggiuntivi da tre fonti indipendenti (vedi sopra),
  con un `manifest.json` che elenca tutti i CSV disponibili.

## 🔄 Come aggiornare

I dati e il viewer vengono generati dagli script del progetto
[TennisBot](https://github.com/Artur73737/tennisBOT) (cartella `data/scraper`):

```bash
# 1. scarica/aggiorna i dati con gli scraper
python -m data.scraper.tennis_explorer --sections atp,wta --years 2000-2026
# 2. rigenera il viewer standalone
python -m data.scraper.build_viewer
# 3. copia il viewer qui e pubblica
copy data\scraper\viewer.html ..\tennis_data\index.html
cd ..\tennis_data && git add . && git commit -m "update data" && git push
```

Al push, l'Action **Deploy to GitHub Pages** ripubblica il sito da sola.

## ℹ️ Note

- I dati sono **pubblici** (chiunque abbia il link può consultarli).
- Fonti: mirror dei repo Sackmann + scraping di tennis-db.com, tennisexplorer.com,
  tennisabstract.com. Per uso personale/di ricerca.
- Limite GitHub Pages: 100 MB per file — se `index.html` cresce troppo, conviene
  passare a un viewer che carica i CSV via fetch invece di incorporarli.
