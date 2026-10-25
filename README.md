# NetShelter: Capacity Load-Balancing Matrix

A single-file civic dashboard built for the NY-11 Congressional App Challenge. It simulates routing intake batches across four Staten Island shelter facilities using a multi-variable scoring algorithm, with an emergency "release valve" when primary capacity is exhausted.

Everything lives in `index.html` (HTML, CSS, and JavaScript). State persists in the browser's `localStorage`. There is no backend and no build step.

## Run it

Open `index.html` directly in a desktop browser, or use the dev server:

```bash
npm install
npm run dev
# open http://localhost:4317
```

Any static server works too, e.g. `python3 -m http.server 47213`.

An internet connection is needed for the CDN assets (Leaflet, Chart.js, CARTO dark tiles, Google Fonts).

## How the algorithm works

For each facility:

```
Score = (Vacancies * 0.5) + (Specialization match ? 30 : 0) - (Distance_km * 0.2)
```

- Distance is the haversine distance from the dispatch center (40.5895, -74.1154).
- A facility at absolute max capacity is hard-penalized (score -9999) and disqualified from the batch.
- Facilities are sorted by score, and units are placed greedily until the batch is satisfied.
- Units left over trigger the release valve and are routed to the simulated NY-11 Emergency Foster & NGO Distributed Network.

## Controls

- **Execute Balance Matrix**: run an intake batch (size 1-5000, plus a specialization constraint).
- **Discharge** (download icon): simulate natural discharge to free up capacity.
- **Reset** (circular arrow icon): restore baseline facility data and clear the telemetry log.
