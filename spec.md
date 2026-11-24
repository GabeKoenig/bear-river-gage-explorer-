# Bear River Gage Explorer

## Purpose
An interactive map for engineers, water managers, and students to explore USGS stream gages in the Bear River basin. It will show recent flow and a one-year flow history for a selected gage.

## Inputs
- Historical gages: checkbox, off by default. When on, include sites without recent flow.
- Selected gage: chosen by clicking a map marker.

## Calculations
- Recent flow: display the latest available discharge observation, in cubic feet per second (cfs), with its timestamp.
- Annual mean flow: arithmetic mean of available daily mean discharges over the displayed year, in cfs: `mean = sum(daily mean flows) / number of valid daily values`.
- The location set is limited to stream gages within the Bear River basin (HUC 1601).

## Outputs
- A map of the HUC 1601 basin boundary and USGS gage markers. Show recently reporting sites first; historical sites are optional.
- For the selected site: name, USGS site number, drainage area, latest flow and timestamp, link to the USGS site page, and a hydrograph of daily flow for the previous year.
- Annual mean daily flow for the displayed year.

## Look and feel
A clean, readable, map-first layout that works on desktop and phone. Show a loading state while live data is retrieved and an actionable error if a source is unavailable. The map opens centered on the Bear River basin with recent gages visible.

## Validation
Draft arithmetic check, worked by hand: if three valid daily mean flows are 10, 20, and 30 cfs, then the annual-mean calculation for those three sample days is `(10 + 20 + 30) / 3 = 20 cfs`. This checks the averaging logic only; it is not the final USGS validation.

Final validation still required: compare latest flow and drainage area against three USGS site pages, then independently calculate the mean of one gage's year of daily flows and compare it with the app.

## Out of scope
- Editing or forecasting streamflow.
- Non-USGS gages or sites outside HUC 1601.
- Using the retired `waterservices.usgs.gov` service.
