# Instructions for data submission.

Each team can participate in Influcast with one or more models.

Each participating team will be associated with one or more _GitHub_ users. For the submission of the predictions, each team will have to _fork_ the repository and send _pull requests_.

Every Friday, the files with the weekly incidence values reported by the Sistema di Sorveglianza Integrata dell’Influenza by InfluNet will be updated in the repository, nationally and for each region.

The flu trend predictions will be sent to the repository by the following Tuesday.

The incidence predictions will be indicated by week, similarly to what is reported by the surveillance data, specifying the year and the week of the year in question. The weeks are defined in accordance with the ISO format [ISO week date - Wikipedia](https://en.wikipedia.org/wiki/ISO_week_date), starting on Monday and ending on Sunday. A reference CSV file listing all the weeks of the current season is present in the repository.

## What is a prediction?
Teams are invited to send predictions on the weekly influenza incidence in the next 4 weeks at the national and regional level. These predictions must be considered as "unconditional" projections into the future, i.e., they must reflect the uncertainty related to all possible future scenarios deemed reasonable by the participating groups. In practice, forecasting models make hypotheses about how current information and data trends can influence the trend of influenza incidence in the short and medium term. Predictions sent to this repository will be evaluated based on observed data.
The value to be indicated in each prediction is represented by the incidence per number of assisted individuals. 
Values will be indicated for the four (4) weeks following the one corresponding to the last ISS surveillance report. If the model only allows predictions for a number of weeks less than four, it is possible to send predictions for a smaller number of weeks.
Predictions will be reported in the form of quantiles. The order quantiles 0.01, 0.025, 0.05, 0.1, 0.15, 0.2, 0.25, 0.3, 0.35, 0.4, 0.45, 0.5, 0.55, 0.6, 0.65, 0.7, 0.75, 0.8, 0.85, 0.9, 0.95, 0.975 and 0.99, are mandatory for each participating model; it is possible to report quantiles of any other order.

## Repository structure
All predictions will be archived within the `previsioni/` folder found at the first level of the repository. Inside the `previsioni` folder will be a folder for each model used to provide predictions, identified by the name of the team participating in Influcast and the name of the model itself. The team name and the model name will coincide with the acronyms provided at the time of adherence. For each model, the path where the prediction files will be loaded will be:

`previsioni/TEAM_name-MODEL_name/`

Each prediction file will be named as follows:

`YYYY_WW.csv`

Where `YYYY` represents the year and `WW` the week indicated with two digits (i.e., prefixing 0 for the first 9 weeks).

## Surveillance data (InfluNet).
Surveillance data will be updated in the repository every Friday, following the publication of the InfluNet Report by the Higher Institute of Health. The data provided in a specific week are related to the values reported in the previous week.
Inside the `sorveglianza/` folder that is located at the first level of the repository, there will be a folder for the current season (example: `sorveglianza/2023-2024/`), inside which the following files will be uploaded every week:

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

Where `YYYY` represents the year and `WW` the week indicated with two digits (i.e., prefixing 0 for the first 9 weeks).

Each file will contain the incidence values reported by the Integrated Influenza Surveillance System from the start of the flu season up to the corresponding week (the week before the current one).
The files will be in CSV (comma-separated values) format and will contain the following columns:

- anno
- settimana
- incidenza
- numero_casi
- numero_assistiti

The first two columns indicate the year and the week to which the surveillance data refers; the third is a decimal number that indicates the incidence detected per 1000 patients; the fourth and fifth represent the number of detected cases and the number of patients referred to, respectively.

## Required format for submitting forecast data.
For each weekly forecast and for each participating model, a file must be sent to the repository (via a _pull request_ operation). As previously anticipated, the forecast files will have a path within the repository in the form:

`previsioni/Team_X-Model_Y/2024_05.csv`

The forecast files must be CSV (comma-separated values) files with the following columns:

- anno
- settimana
- luogo
- tipo_valore
- id_valore
- orizzonte
- valore


__anno__, __settimana__: the year and the week to which the forecast refers, indicated as integers. These must correspond to the year and the week of the surveillance report. They must also be the same ones used in the file name (minus the initial 0 for the first 9 weeks of the year).
For ease of reference, the `previsioni/settimane_<season>.csv` file lists all possible weeks for the season, indicating for each week: year, week number, start date, and end date.


__luogo__: a two-character code (a string) indicating the location to which the forecast refers. The allowed values are: IT, 01, 02, 03, 04, 05, 06, 07, 08, 09, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21. The IT code indicates the national level forecast, while the codes from 01 to 21 refer to the regions according to the following mapping:

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
- 12 : Autonomous Province of Bolzano
- 13 : Autonomous Province of Trento
- 14 : Piemonte
- 15 : Puglia
- 16 : Sardegna
- 17 : Sicilia
- 18 : Toscana
- 19 : Umbria
- 20 : Valle d'Aosta
- 21 : Veneto


__tipo_valore__: this field must always contain the string 'quantile'. (The field is redundant since all forecasts will currently be provided in the form of quantiles, but it is present to ensure compatibility with similar infrastructures at the European level).


__id_valore__: the quantile for which the forecast values are indicated in the _valore_ column, in the form of a decimal number between 0 and 1. For each forecast, it is mandatory to indicate the values for the following quantiles: 0.01, 0.025, 0.05, 0.1, 0.15, 0.2, 0.25, 0.3, 0.35, 0.4, 0.45, 0.5, 0.55, 0.6, 0.65, 0.7, 0.75, 0.8, 0.85, 0.9, 0.95, 0.975, 0.99.


__orizzonte__: an integer indicating the target week of the forecast, starting from the reference week corresponding to the surveillance report. Therefore, the allowed values will be 1, 2, 3, and 4, indicating the forecast at one week, two weeks, three weeks, or four weeks, respectively.


__valore__: a decimal number indicating the forecast, that is, the estimated weekly incidence value (weekly cases per 1000 patients) at the end of the week (on Sunday) indicated in the _orizzonte_ column, for the quantile and the location indicated in the _id_valore_ and _luogo_ columns.


For example, the 0.025 and 0.975 order quantiles for a given week and location must represent 95% of the estimated values, corresponding to a 95% confidence interval.





