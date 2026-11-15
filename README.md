# ErdemNOVA-SpaceTwin

## Project Overview

SpaceTwin is an independent educational web application developed by ErdemNOVA for the NASA Space Apps Challenge 2026 local event in Niğde, Türkiye. It helps students and other users explore how specific features of Earth sites, such as extreme aridity, impact craters, or volcanic landforms, can inform research questions about the Moon and Mars.

## Challenge

[Identify Earth Locations that Analog the Permanent Moon Base Locations and Mars](https://www.spaceappschallenge.org/2026/challenges/permanent-moon-base-locations-and-mars/)

SpaceTwin addresses the challenge by presenting established analog examples alongside preliminary research candidates in Türkiye and letting users examine historical Earth meteorology for each site.

## Features

- **Interactive Earth map.** Select a marker or site card on the schematic world map to view the site's coordinates, research rationale, and evidence status. Filters show all sites, known analogs, or candidates in Türkiye.
- **Known analog locations.** The prototype features the Atacama Desert in Chile, Haughton Crater in Canada, and Death Valley in the United States.
- **Exploratory locations in Türkiye.** The prototype presents Cappadocia, Karapınar, Nemrut Caldera, and the Lake Salda area as preliminary research candidates.
- **Historical weather data.** Selecting a site requests NASA POWER data for its coordinates. The application displays monthly charts, a table of monthly values, and annual summaries. The request details include a link to the raw JSON.
- **Moon and Mars research context.** The target selector changes the explanatory text. The data displayed always describe the selected Earth site.

## NASA Data Sources

### Implemented data integration

The browser sends a live HTTPS request to the [NASA POWER](https://power.larc.nasa.gov/) Monthly Point API at `/api/temporal/monthly/point`. It sends the site's latitude and longitude, requests both parameters in JSON, and sets both `start` and `end` to the browser's current UTC year minus two. For example, the application requests 2024 data during 2026. The API returns historical MERRA-2 reanalysis data. Each request retrieves archived values and provides no real-time satellite observations. See the [NASA POWER Monthly API documentation](https://power.larc.nasa.gov/docs/services/api/temporal/monthly/).

| Parameter | Monthly value | Unit |
| --- | --- | --- |
| `T2M` | Mean air temperature at 2 meters above the surface | °C |
| `PRECTOTCORR` | Mean daily corrected precipitation | mm/day |

### Sources for future research

- The Mars Orbiter Laser Altimeter archive provides topographic data for possible future terrain comparisons. [NASA MOLA archive](https://pds-geosciences.wustl.edu/missions/mgs/mola.html)
- The Lunar Orbiter Laser Altimeter aboard the Lunar Reconnaissance Orbiter provides lunar topographic data for possible future terrain comparisons. [NASA LOLA / LRO](https://lunar.gsfc.nasa.gov/lola/)

MOLA and LOLA are reference sources in the current prototype. Raster processing and quantitative terrain comparison are future research work.

## Scientific Method

1. Review the selected site's description and evidence status. Use the stated features, such as aridity or volcanic landforms, to frame a specific analog research question.
2. Retrieve historical temperature and precipitation for the site's coordinates. The application requires valid values for both parameters across all 12 months before displaying a summary.
3. Calculate each annual mean by multiplying each monthly value by the number of days in that month, summing the weighted values, and dividing by the number of days in the year. The precipitation summary reports mean daily precipitation in mm/day.

## Limitations

- The map is schematic and provides approximate geographic context.
- NASA POWER values come from a gridded Earth reanalysis. A single year offers a historical snapshot. Long-term climate characterization or site-scale field measurements require additional evidence.
- The comparisons are qualitative and concern specific features. Earth temperature and precipitation alone cannot establish that a site is equivalent to a lunar or Martian environment or suitable as a base analog. The sites in Türkiye are preliminary candidates, and the prototype provides no scientific validation or quantitative similarity scores.

## Team

**ErdemNOVA**

| Member | Class |
| --- | --- |
| Göktürkhan YOZBATIRAN | 10/C |
| Zeynep Su DOĞAÇ | 10/C |
| Mehmet Berat AVLANMAZ | 10/A (Anatolian program) |
| Yusuf AKDEMİR | 10/C |

**Scrum Master** — Zeynep Su DOĞAÇ

**Faculty mentor** — Vahit Koray ŞENPEKMEZCİ

## Live Demo

[Open ErdemNOVA-SpaceTwin on Netlify](https://erdemnova-spacetwin.netlify.app)

Select a map marker or site card to review the site's profile and load its historical data. Use **Load NASA POWER data** to repeat the request. Data retrieval requires an internet connection.

## Project Documentation

- [Technical documentation](docs/TECHNICAL_DOCUMENTATION.md)
- [Jury presentation (PDF)](docs/presentation/ErdemNOVA-SpaceTwin-Jury-Presentation.pdf)
- [Sprint 1 — Scope and task planning](sprint-reports/sprint-1.md)
- [Sprint 2 — Data flow and technical testing](sprint-reports/sprint-2.md)
- [Sprint 3 — Scientific content and user experience](sprint-reports/sprint-3.md)
- [Sprint 4 — Demo and submission preparation](sprint-reports/sprint-4.md)
