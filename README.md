# SELENE Real-data starter (Vercel + Groq)

## Run / deploy
1. Import this folder/repository into Vercel (static `index.html` is served at root; `/api/mission-advice` is a serverless function).
2. Vercel Project Settings → Environment Variables: `GROQ_API_KEY` (required). Optional `GROQ_MODEL`.
3. Redeploy. Do not put secrets in client files.

## Data source
The map uses the LROC WMS endpoint (`https://wms.lroc.asu.edu/lroc`) and asks for LROC WAC global / relief layers. This is orbital LRO camera-derived map imagery. Layer names/service behavior may change; if a layer fails, inspect the service capabilities at the endpoint and select a currently advertised layer. Data browser: https://data.lroc.im-ldi.com/lroc/ . NASA LRO archives include LROC, LOLA, Diviner, and SPICE: https://pds-geosciences.wustl.edu/missions/lro/ .

## Physics model (read this)
`app.js` integrates a vertical point-mass rocket equation with lunar gravity 1.625 m/s², thrust, Isp-based propellant flow, changing mass, and touchdown/fuel-depletion conditions. It is educational—not flight certified, not a full 6-DOF guidance/navigation/control model, and currently assumes constant throttle and vertical motion. No terrain elevation is sampled in collision detection.

## Actual LOLA terrain integration next
Download a LOLA DEM from the PDS/PGDA archive (South Pole 5 m products are documented at https://grailteam.mit.edu/ and NASA data book references https://ntrs.nasa.gov/). Convert its GeoTIFF/IMG into a tiled terrain dataset (e.g. COG/terrain tiles), serve it, and wire DEM sampling into the integrator's ground-altitude and slope checks. Do not label placeholder elevations as LOLA measurements. Diviner temperature grids likewise require selecting a specific archive product, time/illumination context, projection, and resampling pipeline.

## Notes
Leaflet WMS may be affected by provider CORS/service availability. If direct WMS browser access fails, use a compliant server-side tile proxy/cache (respect LROC terms and attribution) rather than embedding the LROC browser in an iframe.
