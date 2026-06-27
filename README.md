# DATAELECTOR — dataset elettorale italiano canonico

Dataset **canonico, validato e immutabile** delle elezioni italiane (ultimi 15 anni + ultimo anno
pubblicato), 4 livelli: NAZIONALI, EUROPEE, REGIONALI, COMUNALI. È **solo dati**: l'indice e i
checksum sono in `manifest.json`; il metodo è in `METODOLOGIA.md`; i contratti colonne in `schema/`.

Versione: **v2026.13** · generato: 2026-06-27

## Garanzie
- **Grano AS-VOTED**: il voto è memorizzato sul comune *com'era al voto* (`codice_storico`); il
  rollup sul comune di oggi (`codice_canonico`) è derivabile col crosswalk `territori/`.
- **Integrità**: ogni file ha un `sha256` nel `manifest.json`; un lettore lo verifica al download.
- **Validato**: una tornata è pubblicata solo se supera il quality gate (campo `validazione`).

## Sondaggi (Poll of Polls)
`sondaggi/` contiene l'archivio dei **sondaggi nazionali validati** (Registro dei Sondaggi
Politico-Elettorali della Presidenza del Consiglio): un file con i sondaggi e uno con le intenzioni
di voto per partito. Snapshot append-only — la "supermedia" si ricalcola da qui.

## Fonti
ISTAT (confini, popolazione, variazioni amministrative) e Ministero dell'Interno —
Eligendo / Open Data (risultati). Sondaggi: Registro PCM (sondaggipoliticoelettorali.it).
Dati aperti, con attribuzione.
