# ErdemNOVA – SpaceTwin Technical Documentation

**Updated November 15, 2026** · NASA Space Apps Challenge 2026, Niğde, Türkiye

## 1. Project objective and challenge

SpaceTwin is an independent educational web prototype for exploring Earth sites with features relevant to lunar or Martian research. The official challenge asks participants to use open Earth, Moon, and Mars datasets to identify and characterize terrestrial analogs for future bases or landing sites and present their findings in a map, visualization, or other tool. SpaceTwin contributes an exploratory map, site profiles, and historical Earth meteorology for seven locations. [Identify Earth Locations that Analog the Permanent Moon Base Locations and Mars](https://www.spaceappschallenge.org/2026/challenges/permanent-moon-base-locations-and-mars/)

## 2. Website and interactive map

- **Known analog examples** include the Atacama Desert (Chile), Haughton Crater (Canada), and Death Valley (United States). These sites provide research context for specific traits such as aridity or impact geology.
- **Preliminary research candidates in Türkiye** include Cappadocia (volcanic tuff and erosional landforms), Karapınar (volcanic terrain and aridity), Nemrut Caldera (caldera landforms), and the Lake Salda area (mineralogy and geological processes).
- The schematic SVG map positions markers from each site's latitude and longitude. Filters display all sites, known analogs, or candidates in Türkiye. A marker or site card opens the profile with coordinates, research rationale, evidence status, and tags. Site cards provide a keyboard-operable selection path.
- Selecting a marker, card, or entry in the Earth site dropdown requests data for that site. On initial load, the interface displays a default profile and the API query details. Use **Load NASA POWER data** to request the default site's data or retry the selected site. The Moon/Mars selector changes the research context text only.
- A successful request displays a January–December temperature line chart, a precipitation bar chart, an expandable table of monthly values, and two annual summaries. The data transparency panel shows the endpoint, selected coordinates, dataset year, raw JSON request link, and last successful retrieval time in Türkiye time.

## 3. Software architecture and deployment

The repository's [index.html](../index.html) contains the HTML interface, responsive CSS, schematic SVG map, site records, and vanilla JavaScript. The application requires no build step, application server, database, account, or API key. The site records in `window.SITES` define the coordinates and descriptive metadata used by the map, profiles, and API requests.

Netlify is the documented static hosting destination. The browser loads the page from Netlify and sends requests directly to NASA POWER over HTTPS. It validates the JSON and renders the charts and summaries locally. Static hosting and NASA data retrieval have separate availability dependencies. For a local preview, open `index.html` in a browser. Data retrieval requires access to NASA POWER from the browser, including permission for the cross-origin request. The Netlify publish branch, publish directory, and automatic deployment settings were not verified. [Netlify demo](https://erdemnova-spacetwin.netlify.app/)

## 4. NASA POWER integration

SpaceTwin uses live HTTPS GET requests to the NASA Prediction Of Worldwide Energy Resources (POWER) Monthly Point API. Responses contain historical Earth meteorology derived from MERRA-2 reanalysis. The retrieval timestamp records when the application fetched the historical response. The application supplies no real-time satellite observations. [NASA POWER Monthly API documentation](https://power.larc.nasa.gov/docs/services/api/temporal/monthly/)

```text
GET https://power.larc.nasa.gov/api/temporal/monthly/point
    ?parameters=T2M,PRECTOTCORR
    &community=RE
    &longitude={site longitude}
    &latitude={site latitude}
    &start={year}&end={year}
    &format=JSON
```

The client uses the browser's current UTC year minus two for both `start` and `end`. In 2026 it requests January through December 2024.

| Parameter | Monthly statistic | Unit |
| --- | --- | --- |
| `T2M` | Mean air temperature at 2 meters above the surface | °C |
| `PRECTOTCORR` | Mean daily corrected precipitation within the month | mm/day |

The client reads each parameter under `properties.parameter` using the twelve `YYYYMM` keys in calendar order. It requires finite numeric values for both parameters in every month, rejects the response's declared fill value and values at or below −900, and rejects negative precipitation. It computes and displays summaries only after both series pass these checks.

The browser aborts the previous request when a new one starts and prevents stale responses from updating the display. Each request has a 45-second timeout. HTTP errors, network failures, timeouts, invalid JSON, and incomplete monthly data produce an error message. The transparency panel retains the request URL for inspection or retry.

### Annual calculations

For month `m`, let `d[m]` be its actual number of calendar days, `T[m]` its monthly mean temperature, and `P[m]` its monthly mean daily precipitation. February contributes 29 days in leap years. The application calculates:

```text
Annual mean temperature (°C)          = sum(T[m] × d[m]) / sum(d[m])
Annual mean daily precipitation (mm/day) = sum(P[m] × d[m]) / sum(d[m])
```

Both summaries display two decimal places. The precipitation result is a rate in mm/day. An estimated annual accumulation in millimeters would use the numerator alone, which the interface does not display.

## 5. Verification and evidence

| Check | Evidence and result |
| --- | --- |
| November 15 source and local checks | Node.js parsed the inline script and confirmed seven site records, including three known analog examples. Local checks exercised the current parsing and calculation functions with simulated values. They passed for January–December ordering, the 29-day February in 2024, both day-weighted annual means, and rejection of missing, fill, string, and negative precipitation values. |
| Pre-event POWER integration | Sprint 2 records testing of both parameters. Git commit `068a197` records the monthly endpoint change. This provides implementation history. The underlying dated browser log and raw response are unavailable in the reviewed records. |
| November 14 chart verification | Sprint 3 records a passing JavaScript syntax check and simulated tests for month ordering, leap-year weighting, error handling, and rapid selection changes. It also records the project owner's observation that both charts appeared on the deployed site and displayed correctly on mobile. The report identifies commit `f45284e` for the chart implementation. |
| Historical Atacama output | The earlier technical document reports a 2024 display result. A dated capture, raw NASA response, and independent recalculation were unavailable for this review, so the reported output is unverified here. |
| Current deployment and end-to-end results | This review's network checks could not resolve the Netlify or NASA API hostnames from the execution environment. The review did not establish public service availability, the deployed revision, or live browser behavior on November 15. |

The local checks used simulated inputs and did not exercise browser requests. The sprint records leave the seven-site raw JSON comparison, deployed desktop and mobile checks, accessibility and retry checks, candidate source review, reviewer access, and submission confirmation without documented November 15 results. [Sprint 2](../sprint-reports/sprint-2.md) · [Sprint 3](../sprint-reports/sprint-3.md) · [Sprint 4](../sprint-reports/sprint-4.md) · [Earlier technical documentation](TECHNICAL_DOCUMENTATION.md)

## 6. Scientific limitations and future research

The map provides approximate geographic context. NASA POWER provides gridded Earth reanalysis at the selected coordinates. A single year describes historical conditions for that year. Long-term climate characterization and local field observations require additional evidence.

Air temperature and precipitation alone cannot establish planetary equivalence, surface mineralogy, terrain similarity, or suitability for base operations. The analog examples illustrate selected research traits. The four locations in Türkiye are preliminary candidates whose suitability remains unverified. The prototype performs qualitative exploration and supplies no quantitative similarity score or scientific validation of planetary equivalence.

- **MOLA** is the Mars Orbiter Laser Altimeter. Its archive provides data for potential future Martian topography analysis. [MOLA archive](https://pds-geosciences.wustl.edu/missions/mgs/mola.html)
- **LOLA** is the Lunar Orbiter Laser Altimeter aboard NASA's Lunar Reconnaissance Orbiter. It provides data for potential future lunar topography analysis. [NASA LOLA / LRO](https://lunar.gsfc.nasa.gov/lola/)

The prototype links to these sources. It has no MOLA or LOLA data ingestion, raster processing, reprojection, or quantitative terrain comparison.

## 7. Development process

The sprint records describe the GitHub repository, initial web prototype, challenge scope, and POWER integration as pre-event preparation. They record online collaboration by all four students on November 14. Individual implementation assignments and contributions are not documented in the reviewed sprint records.

| Sprint | Date | Recorded progress |
| --- | --- | --- |
| [1. Problem analysis and team coordination](../sprint-reports/sprint-1.md) | November 14 | Online coordination confirmed. Backlog priorities and acceptance criteria awaited confirmation in the report. |
| [2. Solution development and prototype review](../sprint-reports/sprint-2.md) | November 14 | Pre-event NASA POWER testing recorded. The regional test log and event-day technical results were pending. |
| [3. Scientific content and user experience](../sprint-reports/sprint-3.md) | November 15 | Chart implementation and initial verification took place early on November 14. The report lists broader verification as planned for November 15. |
| [4. Demo and submission preparation](../sprint-reports/sprint-4.md) | November 15 | Final checks, demo rehearsal, and submission were planned. The report contains no completion or submission confirmation. |

## 8. Team and project links

| Team member | Class |
| --- | --- |
| Göktürkhan YOZBATIRAN | 10/C |
| Zeynep Su DOĞAÇ | 10/C |
| Mehmet Berat AVLANMAZ | 10/A (Anatolian program) |
| Yusuf AKDEMİR | 10/C |

**Scrum Master** — Zeynep Su DOĞAÇ

**Faculty mentor** — Vahit Koray ŞENPEKMEZCİ

- [GitHub repository](https://github.com/vahitkoray/ErdemNOVA-SpaceTwin)
- [Netlify demo](https://erdemnova-spacetwin.netlify.app/)
- [Project README](../README.md)
- [Earlier technical documentation in Turkish](TECHNICAL_DOCUMENTATION.md)
- [Jury presentation PDF](presentation/ErdemNOVA-SpaceTwin-Jury-Presentation.pdf)
