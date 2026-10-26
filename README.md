# NetShelter: Capacity Load-Balancing Matrix

A civic dashboard for the NY-11 Congressional App Challenge. It routes an incoming group of people across a planning network of shelters, from Staten Island and south Brooklyn out to Queens, Manhattan, the Bronx, and the Newark–Elizabeth border.

The citywide census comes from the [NYC Open Data DHS Daily Report](https://data.cityofnewyork.us/Social-Services/DHS-Daily-Report/k46n-sa2m) (`k46n-sa2m`). On load, the page requests the latest 42 census days. The census cards, the day-to-day chart, and the intake presets all use that response. A saved snapshot is bundled in the page so the dashboard still opens if the request fails. The report is a citywide count. It does not publish facility capacity, so the beds at each site are a planning model.

Everything runs in the browser. Occupancy, the score trace, manual priority, offline facilities, and the audit log persist in `localStorage`. No account, server, or database is required.

The interface, stylesheet, and allocation algorithm live in [`index.html`](index.html). Leaflet and Chart.js load from public CDNs.

## Views

The top bar switches three views:

- **Command Center.** Network load, open beds, and critical nodes sit above a map that fills the rest of the view. The desk on the left approves an intake, crashes a facility to zero capacity, or sends a wave of 120 people.
- **Facilities.** One table combines occupancy bars with available beds, whether the care requirement is met, distance, score, and how many people were placed. Two charts cover occupancy and the DHS day-to-day change.
- **Audit Trail.** The full event log, oldest first, with placement and warning filters.

A light/dark control in the header switches the page and the map. Light mode uses Esri World Light Gray Canvas. Dark mode uses Esri World Dark Gray Canvas. Both draw streets without an API key.

## Run it

```bash
npm install
npm run dev
```

Open [http://127.0.0.1:3847](http://127.0.0.1:3847).

You can also open `index.html` directly in a browser while online. The dev server is the supported way to present it.

## How a wave is placed

Entry dispatch is fixed at 40.5895, −74.1154. The score is hidden behind the info control on the Score column:

```text
score = (available beds × 0.5) + (requirement met ? 30 : 0) − (distance km × 0.2)
```

Distance is the straight-line length from dispatch to the facility. A facility with no available beds, or one whose capacity has been crashed to 0, is skipped. **Manually Force Reroute** fills the chosen facility first, then the score order takes the rest. Anyone still unplaced goes to the NY-11 Emergency Foster & NGO Distributed Network, and the failsafe switches on.

Map markers follow occupancy: green under 70%, amber from 70% through 95%, and a pulsing rose ring above 95%. A slate marker means the facility is offline. A dashed route is drawn from dispatch to every facility that receives people.

The timeline under the map replays a 24-hour projection around the live occupancy. Approving a reroute is available on the live hour.

## Judge shortcuts

The intake panel builds four presets from the loaded census:

- Single adults, the latest one-day rise
- Children, the latest one-day rise, placed with Youth Care
- People in families with children, the latest one-day rise
- The largest one-day rise in the loaded window

The intake field starts at the latest one-day rise in total people in shelter. Clicking a chart bar for a rise loads that count.

The care filter above the map shows all facilities, general shelters, medical focus, or youth care.

**Restore baseline** returns every facility to its opening occupancy, clears manual priority, and brings offline sites back. **Clear** on the audit trail only empties the log.
