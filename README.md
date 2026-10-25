# NetShelter: Capacity Load-Balancing Matrix

A single-page civic dashboard for the NY-11 Congressional App Challenge. It load-balances a simulated shelter intake across four facilities on Staten Island and the Brooklyn edge of the district.

Everything runs in the browser. Occupancy, the score trace, and the telemetry log persist in `localStorage`. No account, server, or database is required.

The entire interface, stylesheet, and allocation algorithm live in [`index.html`](index.html). Leaflet and Chart.js load from public CDNs.

The map is Esri World Light Gray Canvas, which keeps the warm civic-service interface legible and draws Staten Island without requiring an API key.

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

Map markers follow occupancy: emerald under 70%, amber from 70% through 95%, and a pulsing crimson ring above 95%. A cyan dashed vector is drawn from dispatch to every node that receives people.

## Judge shortcuts

The intake panel includes four presets:

- Medical spike · 8
- Youth wave · 12
- General surge · 25
- Crisis overflow · 80

**Restore baseline** returns the four nodes to their opening occupancy and clears the vectors. **Clear** only empties the console.
