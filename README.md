# NetShelter: Capacity Load-Balancing Matrix

A single-page civic dashboard for the NY-11 Congressional App Challenge. It load-balances a shelter intake across four facilities on Staten Island and the Brooklyn edge of the district.

The citywide census comes from the [NYC Open Data DHS Daily Report](https://data.cityofnewyork.us/Social-Services/DHS-Daily-Report/k46n-sa2m) (`k46n-sa2m`). On load, the page requests the latest 42 census days. The header, the census totals, the day-to-day chart, and the intake presets all use that response. A saved snapshot is bundled in the page so the dashboard still opens if the request fails. The report is a citywide count of people in shelter. It does not publish facility capacity or a borough split, so the four NY-11 nodes stay a planning model with opening bed counts.

Everything runs in the browser. Occupancy, the score trace, and the telemetry log persist in `localStorage`. No account, server, or database is required.

The entire interface, stylesheet, and allocation algorithm live in [`index.html`](index.html). Leaflet and Chart.js load from public CDNs.

The map is Esri World Light Gray Canvas. CARTO’s dark basemap currently returns a watermark tile unless you attach an API key, so a judge would see no streets. Esri’s light canvas still draws Staten Island without a key, and it sits in the same daylight palette as the rest of the dashboard.

## Run it

```bash
npm install
npm run dev
```

Open [http://127.0.0.1:3847](http://127.0.0.1:3847).

You can also open `index.html` directly in a browser while online. The dev server is the supported way to present it.

## How a wave is placed

Entry dispatch is fixed at 40.5895, −74.1154. Each facility gets

```text
score = (vacancies × 0.5) + (specialization match ? 30 : 0) − (distance km × 0.2)
```

Distance is the haversine length from dispatch to the facility. A node already at maximum capacity receives a hard penalty and is skipped. The matrix then fills nodes in score order. Anyone still unplaced is routed to the NY-11 Emergency Foster & NGO Distributed Network and the release valve switches to its critical override state.

Map markers follow occupancy: green under 70%, amber from 70% through 95%, and a pulsing coral ring above 95%. A pine dashed vector is drawn from dispatch to every node that receives people.

## Judge shortcuts

The intake panel builds four presets from the loaded census:

- Single adults, the latest one-day rise
- Children, the latest one-day rise, placed with the Juvenile constraint
- People in families with children, the latest one-day rise
- The largest one-day rise in the loaded window

The intake field starts at the latest one-day rise in total people in shelter. Clicking a chart bar for a rise loads that count as the intake.

**Restore baseline** returns the four nodes to their opening occupancy and clears the vectors. **Clear** only empties the console.
