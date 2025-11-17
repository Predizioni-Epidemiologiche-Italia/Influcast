# Influcast 
![alt text](https://github.com/Predizioni-Epidemiologiche-Italia/Influcast/blob/main/Influcast-logo.png)


[![it](https://img.shields.io/badge/lang-it-yellow.svg)](https://github.com/Predizioni-Epidemiologiche-Italia/Influcast/blob/main/README.md)

__Influcast__ is the first Italian hub for epidemiological forecasts that aggregates estimates produced by different research teams about the future trends of acute respiratory infections at both national and regional levels. The project is coordinated and maintained by the [ISI Foundation](https://www.isi.it/en/home) in Turin.

To view the latest forecasts, visit the project's [website](https://influcast.org/it/).

### How can I join Influcast?
To participate in the project, it is necessary to follow the instructions provided [here](https://github.com/Predizioni-Epidemiologiche-Italia/Influcast/wiki/How-to-join-Influcast). 

Details regarding the format and the methods of data upload are outlined in the [Wiki](https://github.com/Predizioni-Epidemiologiche-Italia/Influcast/wiki/Home.en) of this repository. 

For any questions regarding joining Influcast, feel free to contact us via email (influcast@isi.it).

### How does Influcast work?
During the influenza season, participating teams weekly submit probabilistic forecasts from their models regarding acute respiratory infections incidence for the upcoming four weeks. Specifically, Influcast considers the number of reported cases from the network of sentinel doctors across Italy as the target variable for the forecasts. This data is communicated every Friday by the Italian National Institute of Health (ISS) through the RespiVirNet bulletin. The incidence data published by the ISS is for the previous week, while the update of the Influcast platform occurs on the following Tuesday, allowing the teams some time to process the new data and calibrate their models. Consequently, the forecasts published on Influcast on Tuesdays pertain to the previous week (for which there isn't yet a consolidated public data), the current week, and the two subsequent weeks.

The forecasts from individual models are displayed alongside historical data, enabling the perspective of each model on the short-term evolution of acute respiratory infections incidence in Italy and its regions to be captured. Finally, the forecasts from each model are combined to produce an ensemble prediction.

**Note**: Starting from the 2025/26 season, Influcast also supports new forecasting targets that combine ARI incidence with data from virological surveillance of respiratory viruses. This aims to provide a more influenza-specific incidence signal. More details on the forecasting targets and associated methodology can be found [here](https://github.com/Predizioni-Epidemiologiche-Italia/Influcast/wiki/Forecast-Targets-and-Surveillance-Data).

### Data Sources
Influcast utilizes data processed and made available by the Istituto Superiore di Sanità through the Integrated Surveillance System [RespiVirNet](https://www.epicentro.iss.it/influenza/respivirnet).


All the forecast data sent by the participating teams are subject to the Creative Commons license __CC BY-NC 4.0__ (Attribution - NonCommercial): https://creativecommons.org/licenses/by-nc/4.0/deed.en
