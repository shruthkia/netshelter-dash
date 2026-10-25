# NetShelter: Capacity Load-Balancing Matrix

A civic command-center dashboard for the NY-11 Congressional App Challenge. It simulates how incoming intake batches get balanced across Staten Island and South Brooklyn shelter facilities.

The whole app is **one self-contained file: `index.html`**. It has all the HTML, CSS, and JavaScript, and loads Leaflet, Chart.js, and Google Fonts from CDNs. All state (facility occupancy, telemetry logs, overflow totals) is stored in the browser's `localStorage`.

## Run it

**Option A: open the file.** Double-click `index.html`, or drag it into Chrome, Edge, Firefox, or Safari. You need an internet connection for the map tiles and CDN libraries.

**Option B: dev server.**

```bash
npm install
npm run dev
# open http://localhost:4317
```

## What it does

- **GIS map** (Leaflet + CartoDB Dark Matter) centered on Staten Island, with 4 facility nodes. Each node changes color with its occupancy: green below 70%, amber from 70–95%, and pulsing red above 95%.
- **Intake simulation.** Pick an intake size and a specialization (General, Medical Rehab, Juvenile), then run the matrix. Each facility gets a score:

  `Score = Vacancies × 0.5 + (Specialization match ? 30 : 0) − DistanceKm × 0.2`

  Facilities that are completely full are disqualified. Units go to facilities in score order until the whole batch is placed.
- **Release valve.** Any units still unplaced once every facility is full go to the simulated *NY-11 Emergency Foster & NGO Distributed Network*. They appear as an amber critical-override panel and amber map routes.
- **Routes.** Neon cyan dashed lines run from the dispatch center (40.5895, −74.1154) to each facility that received units.
- **Analytics.** A Chart.js horizontal stacked bar chart shows occupied and vacant beds for each facility, plus network load tiles.
- **Live telemetry console.** Every run adds timestamped log lines.
- **Discharge cycle / Reset network.** Free up beds, or restore the starting occupancy.
