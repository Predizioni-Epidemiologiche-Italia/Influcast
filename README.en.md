# Influcast 

__Influcast__ is the first Italian hub for epidemiological forecasts that aggregates estimates produced by different research teams about the future trends of seasonal influenza at both national and regional levels. The project is coordinated and maintained by the [ISI Foundation](https://www.isi.it/en/home) in Turin.

To view the latest forecasts, visit the project's [website](https://influcast.org/it/).

### How can I join Influcast?
To participate in the project, it is necessary to follow the instructions provided in the file: `Adesione.md`. Details regarding the format and the methods of data upload are outlined in the file: `Istruzioni.md`.

### How does Influcast work?
During the influenza season, participating teams weekly submit probabilistic forecasts from their models regarding the influenza incidence for the upcoming four weeks. Specifically, Influcast considers the number of reported cases from the network of sentinel doctors across Italy as the target variable for the forecasts. This data is communicated every Friday by the Italian National Institute of Health (ISS) through the Flunews bulletin. The incidence data published by the ISS is for the previous week, while the update of the Influcast platform occurs on the following Tuesday, allowing the teams some time to process the new data and calibrate their models. Consequently, the forecasts published on Influcast on Tuesdays pertain to the previous week (for which there isn't yet a consolidated public data), the current week, and the two subsequent weeks.

The forecasts from individual models are displayed alongside historical data, enabling the perspective of each model on the short-term evolution of influenza incidence in Italy and its regions to be captured. Finally, the forecasts from each model are combined to produce an ensemble prediction .

### Data Sources
Influcast utilizes processed data made available in machine-readable format within this repository: https://github.com/fbranda/influnet


All the forecast data sent by the participating teams are subject to the Creative Commons license __CC BY-NC 4.0__ (Attribution - NonCommercial): https://creativecommons.org/licenses/by-nc/4.0/deed.en
