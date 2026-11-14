# Sprint 3 · Scientific Content and User Experience

**Event** — NASA Space Apps Challenge Niğde

**Scrum Master** — Zeynep Su DOĞAÇ

**Official sprint date** — Sunday, 15 November 2026

**Status as of Saturday, 14 November 2026** — Chart development and initial verification were completed early as Sprint 3 preparation. The official sprint and its remaining checks are scheduled for Sunday.

## Sprint Goal

Help users interpret monthly Earth meteorology for selected regions and understand the evidence and limitations behind each analog or research candidate.

## Completed Early on Saturday, 14 November 2026

- Added a line chart for 12 months of NASA POWER mean air temperature at 2 m (T2M, °C) and a bar chart for 12 months of corrected mean daily precipitation (PRECTOTCORR, mm/day).
- Connected both charts to location selection through the map, region cards, and region selector. Both charts show January–December labels, units, the data year, and NASA POWER attribution. The annual summaries retain calendar-day weighting.
- Added checks and clear messages for incomplete data, HTTP or network failures, timeouts, and invalid responses. Charts appear after both series have 12 valid monthly values.
- Codex reported that the JavaScript syntax check and simulated tests passed. The tests covered month order, leap-year weighting, error handling, and rapid location changes.
- The project owner confirmed that both charts appeared on the live website and displayed correctly on mobile.
- Recorded the chart implementation in Git commit `f45284e`.

## Planned Verification for Sunday, 15 November 2026

- [ ] For all seven regions, compare the chart values with the NASA POWER JSON and independently verify the day-weighted annual summaries.
- [ ] Test the deployed map, filters, region cards, and selector on desktop and mobile. Check chart readability and verify that each location change updates both charts.
- [ ] Exercise the browser error and retry flows with a blocked request, a timeout, and an incomplete or invalid response. Record test results and any issues that remain.
- [ ] Check the scientific rationale and coordinates in the region cards against their sources. Review how the prototype presents known analogs and preliminary research candidates in Türkiye.
- [ ] Review the demo explanation of Earth meteorology and lunar and Martian topography. MOLA and LOLA are research sources. Raster integration remains pending for both.

## Planned Team Contributions

Students will review sources and region descriptions, test the deployed prototype, and record usability findings. Document actual contributions and verification results after the Sunday session.
