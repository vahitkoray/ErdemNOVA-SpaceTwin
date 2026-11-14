# Sprint 2 – Solution Development and Prototype Review

- **Project:** ErdemNOVA – SpaceTwin
- **Event:** NASA Space Apps Challenge Niğde 2026
- **Date:** November 14, 2026
- **Scrum Master:** Zeynep Su DOĞAÇ

## Objective

Review the existing SpaceTwin prototype and verify the live NASA POWER data flow.

## Team Participation

Göktürkhan YOZBATIRAN, Zeynep Su DOĞAÇ, Mehmet Berat AVLANMAZ, and Yusuf AKDEMİR all participated in online team collaboration on November 14, 2026, using Zoom and WhatsApp.

## Activities and Evidence

- **Pre-event preparation:** The prototype already included a web interface with a schematic interactive map and information on terrestrial analog candidates, including exploratory locations in Türkiye. The GitHub source repository and technical documentation were already available.
- **Pre-event testing:** The NASA POWER integration for 2-meter air temperature (T2M) and precipitation (PRECTOTCORR) was tested before the event. Git commit `068a197` records the connection fix.
- **Confirmed on the event day:** All four students collaborated online. Specific event-day code changes and tests have yet to be documented.

## Deliverables and Status

- **Available before the event:** Web prototype, NASA POWER integration, GitHub source repository, and technical documentation.
- **Planned sprint outputs:** A regional test log and a list of any technical issues found. Event-day results have yet to be documented.

## Limitations

NASA POWER provides Earth meteorological data. Mars MOLA and lunar LOLA are research references, and their datasets remain outside the prototype’s data integration. Potential testing issues include network or CORS errors, missing monthly data, and unexpected API responses.

## Next Steps

Run temperature and precipitation queries for different Earth regions. Compare the displayed year, units, and averages with the raw JSON, and check the message shown when data cannot be retrieved. Log results by region and capture the request and error message for any failure. Use the raw JSON link to investigate discrepancies.
