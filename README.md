# NetShelter: Capacity Load-Balancing Matrix

A single-page civic operations dashboard built for the NY-11 Congressional App Challenge. NetShelter simulates shelter intake routing across Staten Island facilities using vacancy, care specialization, and distance weighting.

## Run locally

```bash
npm install
npm run dev -- --host 0.0.0.0 --port 43127
```

Open `http://localhost:43127`.

The complete application UI, styling, and simulation logic live in `index.html`. Leaflet map tiles and Chart.js load from public CDNs. Simulation state is stored in the browser's local storage and can be reset from the control panel.

## Production build

```bash
npm run build
```
