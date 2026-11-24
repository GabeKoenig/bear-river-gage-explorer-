# Bear River Gage Explorer

## App overview
The Bear River Gage Explorer is a responsive, map-first web app for exploring USGS streamflow sites across Utah, Idaho, and Wyoming. It will map the HUC 1601 basin, make recently reporting gages easy to see, let users reveal historical sites, and show a selected gage's metadata, latest flow, and daily hydrograph with its 365-day mean.

## Purpose
Help people explore streamflow throughout the Bear River basin in Utah, Idaho, and Wyoming. The app will show stream gage locations on an interactive map and let users view current streamflow and historical flow data for individual gages, supporting interests in rivers, water resources, recreation, and basin runoff.

## Inputs
- Historical gages: checkbox, off by default. When on, include stream sites whose latest available flow observation is more than 30 days old or unavailable. “Recent” means a flow observation within the last 30 days; this does not assert the USGS operational status of a site.
- Selected gage: choose by clicking a map marker.
- Map view: pan and zoom with standard map controls and gestures.
- Hydrograph period: fixed to the trailing 365 days ending on the latest complete daily record available; users do not enter dates.

## Calculations
- Retrieve stream sites and flow observations from the current USGS Water Data API, and obtain the HUC 1601 basin boundary from the USGS Watershed Boundary Dataset. Do not use the retired `waterservices.usgs.gov` service.
- Include a stream gage in the recent layer when its latest available discharge observation is no more than 30 days old relative to the current date/time. The historical layer contains the other stream sites in HUC 1601.
- For a selected gage, show the newest available discharge observation as the latest flow, in cubic feet per second (cfs), with its observation timestamp shown in the site's local time zone and labeled.
- Plot daily mean discharge observations over the trailing 365 days ending on the latest complete daily record available. Let `Q_i` be a valid daily mean discharge in cfs and `n` be the number of valid daily records in the period. Compute mean flow as `Q_mean = (sum of Q_i) / n`, in cfs. Exclude dates with missing/invalid values rather than treating them as zero, and report `n`.
- Use the USGS-reported drainage area in square miles (mi²); if its source unit differs, convert it to mi² before display.
- Limit the mapped stream sites to those within the Bear River basin boundary, HUC 1601.

## Outputs
- An interactive map showing the Bear River basin boundary and markers for active/recently reporting stream gages. A control turns historical/inactive gage markers on and off, with a legend distinguishing them.
- For the selected gage: name, USGS site identification number, drainage area in square miles (mi²), most recent streamflow in cfs, and the date and time of that measurement in the site's labeled local time zone.
- A hydrograph of daily streamflow for the 365 days ending on the latest complete daily record available, with date and discharge (cfs) axes. Missing daily records appear as gaps, not interpolated values.
- Mean streamflow for the trailing 365-day period, in cfs, with the number of valid daily records used.
- A link to the selected gage's USGS webpage.

## Look and feel
A clean, readable, map-first layout. On desktop, show the map on the left and selected-gage details and chart on the right; on phones, show a full-width map followed by the details. On first open, fit the basin in view, show recent gages with the historical layer off, and prompt the user to select a gage. Show a loading state while live data is retrieved and a clear error with a retry option if a source is unavailable.

## Validation
Hand-worked calculation check using four hypothetical daily mean flows: Day 1 = 100 cfs, Day 2 = 120 cfs, Day 3 = 140 cfs, and Day 4 = 160 cfs. The mean is `(100 + 120 + 140 + 160) / 4 = 130 cfs`. Use this as a calculation-function test; it does not change the app's fixed trailing-365-day live display.

Required live-data validation still to complete: compare latest flow and drainage area against three actual USGS site pages, then independently calculate the mean of one gage's year of daily flows and compare it with the app.

## Out of scope
- Forecasting streamflow, issuing alerts, or editing/submitting stream data.
- Non-USGS sites or gages outside HUC 1601.
- User-selectable hydrograph dates; the live display remains a trailing 365-day period.
- Using the retired `waterservices.usgs.gov` service.

## Build stages
1. **Page mockup:** Build the single-file responsive layout with an illustrative basin map, gage markers, historical toggle, selected-gage details, placeholder hydrograph, mean, and clear sample-data labeling. No live data or engineering calculations yet. Check desktop/phone layout and that all specified controls and outputs are represented.
2. **Live basin and gage map:** Load the HUC 1601 boundary and USGS stream sites from current data sources, show sites with flow in the last 30 days by default, and make the historical layer toggle work. Check boundary rendering, marker grouping, loading, and source-error states.
3. **Selected-gage details:** Load and display the selected site's name, USGS number, area, latest flow and timestamp, and USGS page link. Check that selecting different markers updates the site details and units.
4. **Daily hydrograph and mean:** Load daily mean flow for the trailing 365-day period, plot gaps without interpolation, calculate the mean from valid values, and show the valid-day count. Check the four-day hand calculation (130 cfs) and labels.
5. **Required live-data validation and final checks:** Compare flow and drainage area against three USGS site pages; independently calculate one gage's year mean and compare it with the app. Resolve discrepancies and check responsive layout and recoverable data-source errors before treating the must-haves as complete.
