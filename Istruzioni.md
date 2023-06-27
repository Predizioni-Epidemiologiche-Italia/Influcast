# Istruzioni per l'invio dei dati.


Ogni team potrà partecipare a Influcast con uno o più modelli.


Ad ogni team partecipante saranno associati uno o più di uno utenti _GitHub_. Per l'invio delle previsioni ogni team dovrà effettuare un _fork_ del repository e inviare delle _pull requests_.


Ogni venerdì verranno aggiornati nel repository i file con i valori di incidenza settimanale riportati dal Sistema di Sorveglianza Integrata dell’Influenza di InfluNet, a livello nazionale a per ciascuna regione.


Le previsioni dell'andamento influenzale andranno inviate sul repository entro il martedì della settimana successiva.


Le previsioni dell'incidenza andranno indicate per settimana, analogamente a quanto riportato dai dati di sorveglianza, specificando l'anno e la settimana dell'anno relativi.
Le settimane sono definite in accordo con il formato ISO [ISO week date - Wikipedia](https://en.wikipedia.org/wiki/ISO_week_date), iniziano il lunedì e terminano la domenica.
Nel repository è presente un file CSV di riferimento riportante tutte le settimane della stagione in corso.


## Cos'e' una previsione?
I team sono invitati a inviare previsioni sull'incidenza influenzale settimanale nelle prossime 4 settimane a livello nazionale e regionale. Queste previsioni devono essere considerate come proiezioni "incondizionate" sul futuro, ovvero devono riflettere l'incertezza relativa a tutti i possibili scenari futuri ritenuti ragionevoli dai gruppi partecipanti. Nella pratica, i modelli di previsione formulano ipotesi su come le informazioni attuali e le tendenze dei dati possano influenzare l'andamento dell'incidenza influenzale nel breve e medio termine. Le previsioni inviate a questo repository saranno valutate in base ai dati osservati.
Il valore da indicare in ciascuna previsione è rappresentato dall'incidenza per numero di assistiti. 
I valori andranno indicati per le quattro (4) settimane successive a quella corrispondente all'ultimo rapporto di sorveglianza dell'ISS. Qualora il modello consentisse di effettuare previsioni solo per un numero di settimane inferiore a quattro, è possibile inviare previsioni per un numero minore di settimane.
Le previsioni andranno riportate sotto forma di quantili. I quantili di ordine 0.01, 0.025, 0.05, 0.1, 0.15, 0.2, 0.25, 0.3, 0.35, 0.4, 0.45, 0.5, 0.55, 0.6, 0.65, 0.7, 0.75, 0.8, 0.85, 0.9, 0.95, 0.975 e 0.99, sono obbligatori per ciascun modello partecipante; è possibile riportare quantili di qualsiasi altro ordine.


## Struttura del repository 
Tutte le previsioni verranno archiviate all'interno della cartella `previsioni/` che si trova al primo livello del repository. All'interno della cartella `previsioni` si troverà una cartella per la stagione in corso; dentro di essa una cartella per ogni team partecipante a influcast; all'interno di quest'ultima una cartella per ciascun modello associato a quel team. Il nome team ed il nome modello coincideranno con le sigle fornite in fase di adesione. Per ciascun modello il percorso in cui andranno caricati i file delle previsioni avrà la forma: 

`previsioni/stagione/nome_TEAM/nome_MODELLO/`

Ciascun file di previsioni dovrà essere nominato nel seguente modo:

`YYYY_WW.csv`

Dove `YYYY` rappresenta l'anno e `WW` la settimana indicata con due cifre (ovvero anteponendo lo 0 per le prime 9 settimane).


## Dati di sorveglianza (InfluNet).
I dati della sorveglianza verranno aggiornati nel repository ogni venerdì, in seguito alla pubblicazione del Rapporto InfluNet da parte dell'Istituto Superiore di Sanità. Il dati forniti in una specifica settimana sono riferiti ai valori riportati nella settimana precedente.
All'interno della cartella `sorveglianza/` che si trova al primo livello del repository sarà presente una cartella per la stagione in corso (esempio: `sorveglianza/2023-2024/`), all'interno della quale verranno verranno caricati ogni settimana i seguenti file:

- `italia-YYYY_WW.csv`
- `abruzzo-YYYY_WW.csv`
- `basilicata-YYYY_WW.csv`
- `calabria-YYYY_WW.csv`
- `campania-YYYY_WW.csv`
- `emilia_romagna-YYYY_WW.csv`
- `friuli_venezia_giulia-YYYY_WW.csv`
- `lazio-YYYY_WW.csv`
- `liguria-YYYY_WW.csv`
- `lombardia-YYYY_WW.csv`
- `marche-YYYY_WW.csv`
- `molise-YYYY_WW.csv`
- `pa_bolzano-YYYY_WW.csv`
- `pa_trento-YYYY_WW.csv`
- `piemonte-YYYY_WW.csv`
- `puglia-YYYY_WW.csv`
- `sardegna-YYYY_WW.csv`
- `sicilia-YYYY_WW.csv`
- `toscana-YYYY_WW.csv`
- `umbria-YYYY_WW.csv`
- `valle_d_aosta-YYYY_WW.csv`
- `veneto-YYYY_WW.csv`

Dove `YYYY` rappresenta l'anno e `WW` la settimana indicata con due cifre (ovvero anteponendo lo 0 per le prime 9 settimane). 


Ciascun file conterrà i valori di incidenza riportati dal Sistema di Sorveglianza Integrata dell’Influenza dall'inizio della stagione influenzale fino alla settimana corrispondente (settimana precedente a quella corrente).
I file saranno in formato CSV (comma-separated values) e conterranno le seguenti colonne:

- anno
- settimana
- incidenza
- numero_casi
- numero_assistiti

Le prime due colonne indicano l'anno e la settimana a cui si riferisce il dato di sorveglianza; la terza è un numero decimale che indica l'incidenza rilevata per numero di assistiti; la quarta e la quinta rappresentano rispettivamente il numero di casi rilevati e il numero di assistiti cui è riferito. 


## Formato richiesto per l'invio dei dati delle previsioni. 
Per ogni previsione settimanale e per ogni modello partecipante andrà inviato un file al repository (tramite un'operazione di _pull request_). Come anticipato in precedenza, i file delle previsioni avranno un percorso all'interno del repository della forma:

`previsioni/2023-2024/TEAM-X/ModelY/2024_05.csv`

I file delle previsioni dovranno essere dei file CSV (comma-separated values) con le seguenti colonne:

- anno
- settimana
- luogo
- quantile
- valore_1w
- valore_2w
- valore_3w
- valore_4w


__anno__, __settimana__: l'anno e la settimana a cui si riferisce la previsione, indicati come numeri interi. Questi devono corrispondere all'anno e alla settimana del report di sorveglianza. Devono inoltre essere gli stessi utilizzati nel nome del file (a meno dello 0 iniziale per le prime 9 settimane dell'anno).
Per comodità di riferimento il file `previsioni/stagione/settimane.csv` riporta tutte le settimane possibili per la stagione, indicando per ciascuna settimana: anno, numero della settimana, data di inizio e data di fine. 


__luogo__: un codice di due caratteri (una stringa) che indica il luogo a cui si riferisce la previsione. I valori ammessi sono i seguenti: IT, 01, 02, 03, 04, 05, 06 07, 08, 09, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21. Il codice IT indica la previsione a livello nazionale, mentre i codici da 01 a 21 si riferiscono alle regioni secondo la seguente mappatura:

- 01 : Abruzzo
- 02 : Basilicata
- 03 : Calabria
- 04 : Campania
- 05 : Emilia Romagna
- 06 : Friuli Venezia Giulia
- 07 : Lazio
- 08 : Liguria
- 09 : Lombardia
- 10 : Marche
- 11 : Molise
- 12 : Provincia autonoma di Bolzano
- 13 : Provincia autonoma di Trento
- 14 : Piemonte
- 15 : Puglia
- 16 : Sardegna
- 17 : Sicilia
- 18 : Toscana
- 19 : Umbria
- 20 : Valle d'Aosta
- 21 : Veneto


__quantile__: il quantile per il quale si indicano i valori delle previsioni nelle colonne seguenti (valore_1w, …, valore_4w), nella forma di numero decimale compreso tra 0 e 1. Per ciascuna previsione è obbligatorio indicare i valori per i seguenti quantili: 0.01, 0.025, 0.05, 0.1, 0.15, 0.2, 0.25, 0.3, 0.35, 0.4, 0.45, 0.5, 0.55, 0.6, 0.65, 0.7, 0.75, 0.8, 0.85, 0.9, 0.95, 0.975, 0.99.


__valore_1w__: un numero decimale che indica la previsione ad una settimana, ovvero il valore dell'incidenza stimato al termine della settimana (la domenica), per il quantile ed il luogo indicati. 


__valore_2w__: un numero decimale che indica la previsione a due settimane, ovvero il valore dell'incidenza stimato al termine della settimana (la domenica), per il quantile ed il luogo indicati. 


__valore_3w__: un numero decimale che indica la previsione a tre settimane, ovvero il valore dell'incidenza stimato al termine della settimana (la domenica), per il quantile ed il luogo indicati. 


__valore_4w__: un numero decimale che indica la previsione a quattro settimane, ovvero il valore dell'incidenza stimato al termine della settimana (la domenica), per il quantile ed il luogo indicati. 


Ad esempio, i quantili di ordine 0.025 e 0.975 per una determinata settimana ed un determinato luogo devono rappresentare il 95% dei valori stimati, corrispondenti ad un intervallo di confidenza del 95%.


