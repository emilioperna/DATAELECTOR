# Metodologia — dataset elettorale canonico

Questa nota accompagna il dataset pubblicato e ne dichiara metodo, chiavi di ripartizione,
assunzioni, limiti e incertezze. È scritta perché un analista o un revisore possa **contestare**
ogni scelta: nulla è applicato in silenzio.

## 1. Grano del dato: as-voted

La verità è memorizzata al grano del **comune com'era al voto** (`codice_storico`): il dato non
viene mai collassato sul comune di oggi prima di essere salvato. Il «territorio attuale»
(`codice_canonico`) è una **vista derivata** (somma sul comune di oggi). Così non si perde la
traiettoria della singola comunità (es. Castegnero e Nanto restano distinti prima della fusione
2025 nel comune di Castegnero Nanto).

## 2. Armonizzazione temporale ad anno di riferimento

Per confrontare elezioni di anni diversi si proietta la serie sulla geografia di **un anno di
riferimento scelto** (2011, 2019, oggi…), non su «oggi» cablato. Il crosswalk territoriale è un
**grafo temporale**: ogni evento (fusione/scorporo/ricodifica) è datato e si applica solo se già
avvenuto all'anno di riferimento.

### Asimmetria fusione / scorporo (principio cardine)

- **Fusione N→1 = SOMMA ESATTA.** Proiettando in avanti (verso un anno in cui i comuni sono già
  fusi) si sommano i componenti: dato **aggregato**, pulito e citabile come fatto.
- **Scorporo 1→N = STIMA.** Proiettando indietro (verso un anno in cui un comune non esisteva
  ancora, perché nato da una fusione successiva) servirebbe **ripartire** un aggregato sui
  predecessori. Non è una somma: è un **modello**. Le due cose non si mescolano mai.

## 3. Strato stimato (modellato), quarantenato

Le ripartizioni (scorporo all'indietro) e i controfattuali vivono in uno strato **separato**,
**opt-in**, mai nel default e mai citabile come fatto. Tecnica: **ripartizione dasimetrica /
areal interpolation** — il valore aggregato si distribuisce sui componenti in proporzione a una
**chiave dichiarata** (popolazione alla data, quota storica, superficie). Ogni cella stimata
porta metodo e **incertezza**.

## 4. Stato / fiducia per-cella

Ogni figura porta **una** dimensione di fiducia esplicita, dal più diretto al più cauto:

| stato | significato |
|---|---|
| `osservato` | misurato direttamente su un singolo comune com'era al voto |
| `aggregato(N)` | somma esatta di N comuni osservati poi fusi (ricostruzione pulita) |
| `provvisorio(%)` | fonte live, scrutinio non completo |
| `stimato(metodo,±)` | ricavato da un modello: porta metodo e incertezza, **non è un fatto** |

Quando più fattori si applicano alla stessa cella vince il **più cauto**, ma tutti i fattori
restano tracciati.

## 5. Membership territoriale tempo-variante

Non cambia solo l'esistenza di un comune, ma anche la sua **regione/provincia**. L'aggregazione
regionale storica usa la membership **alla data dell'elezione**, non quella odierna. Casi in
scope (2011+): Sappada (Belluno/Veneto → Udine/FVG, 2017); Montecopiolo e Sassofeltrio
(Pesaro-Urbino/Marche → Rimini/Emilia-Romagna, 2021). Fonte: `trasferimenti_regione.csv`.

## 6. Lignaggio dei partiti e delle coalizioni

L'asse partitico è il secondo problema di armonizzazione temporale, sugli stessi 3 strati:

- **osservato** — l'etichetta della lista al voto (il fatto grezzo);
- **armonizzato** — il partito canonico, quando il mapping è certo;
- **stimato** — mapping fuzzy/incerto (porta la confidenza).

Le **coalizioni cambiano a ogni elezione**: la stessa lista può appartenere a un polo diverso nel
tempo. Nessun confronto diacronico «di coalizione» è lecito senza tenerne conto.

## 7. Guardia di comparabilità

Confrontando due elezioni il sistema verifica la comparabilità e **rifiuta** i confronti
cross-livello (es. comunali ↔ politiche), **armonizza** automaticamente quando i perimetri
territoriali differiscono, procede solo quando livello e perimetro coincidono. Nessun confronto
silenzioso «mele vs pere».

## 8. Covariate armonizzate (anti-MAUP)

Popolazione (e, in futuro, socioeconomico) sono armonizzate con lo **stesso** crosswalk dei voti:
così l'unità territoriale dei numeratori e dei denominatori coincide sempre, evitando il
*Modifiable Areal Unit Problem* (un revisore lo smonterebbe subito).

## 9. Limiti e incertezze

- Floor temporale **2011**; tornate precedenti fuori scope.
- Esclusioni *by-design* (non sono errori): Valle d'Aosta e Trentino-Alto Adige al Senato
  (uninominale separato).
- Lo strato **stimato** è un modello, non una misura: va sempre letto con la sua incertezza.
- Le date degli eventi territoriali sono al meglio della fonte; l'armonizzazione ragiona per
  **anno**.
