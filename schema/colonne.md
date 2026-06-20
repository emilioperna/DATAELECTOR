# Schema delle colonne — dataset canonico

Contratti dei file pubblicati. I file `*.parquet` sono la fonte di verità tipizzata; i `*.csv.gz` ne sono la copia testuale per portabilità.

## Tornate (grano AS-VOTED)

`<LIVELLO>/<anno>/<id_elezione>.parquet` — voti per comune com'era al voto.

| colonna | significato |
|---|---|
| id_elezione | identificativo della tornata |
| codice_storico | comune ESISTENTE al voto (ISTAT 6 cifre) |
| codice_canonico | comune di oggi in cui è confluito (rollup derivato) |
| id_lista | etichetta della lista al voto |
| id_canonico_partito | partito canonico, se il mapping è certo |
| voti | voti di lista |
| confidenza_partito | confidenza del mapping lista→partito [0,1] |
| fonte | provenienza (storico/ondata/eligendo_live) |
| pct_scrutinio | completezza dello spoglio (NULL = definitivo) |

Suffisso `__affluenza` = elettori/votanti/bianche/nulle per comune; `__struttura` (comunali) = liste/seggi/candidato/sindaco_eletto/sg_spett.

## Base territoriale

`territori/territori_canonici.parquet` — i comuni di oggi (codice_canonico, nome, popolazione, provincia, regione).
`territori/territori_storico.parquet` — IL CROSSWALK: ogni codice (storico+corrente) → codice_canonico, con `incerto` (scorpori) e `valid_to`. Ricostruisce ENTRAMBE le lenti.
`territori/successioni.parquet` — eventi datati (fusione/ricodifica): codice_da→codice_a.
`territori/comune_lignaggio.parquet` — genealogia navigabile di ogni luogo.

Vedi `METODOLOGIA.md` per armonizzazione, asimmetria fusione/scorporo, strati di fiducia.
