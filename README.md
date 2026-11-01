# Point Break

WarGames-inspired MeshCore planning tool. Runs as a dependency-free Cloudflare Worker with embedded browser assets and fixed-origin public-data endpoints.

[Open the live application](https://point-break-mesh-lab.restedbaton1.chatgpt.site/)

This repository contains the application source. The live app is hosted by Sites on Cloudflare Workers. GitHub Actions builds and tests changes to `main`; it does not automatically publish to Sites. GitHub Pages alone cannot run the `/api/nodes` and `/api/elevation` backend routes.

Run `npm run build`, `npm test`, then `npm run dev` for a local preview on port 8765. No install is needed. Edit `web/` and `worker/api.mjs`; `dist/server/index.js` is generated.

## Data and model

- Public MeshCore type-2 repeater listings: `https://map.meshcore.io/api/v1/nodes`. Regional results cache for 15 minutes; upstream records are streamed to bound memory. Only public keys, names, advertised positions, radio parameters, source and advert time are retained. Regions are capped at 500 nearest repeaters, with a visible truncation notice.
- Exact advertised radio-profile equality, recency and a distance limit define an explicitly hypothetical proximity graph. Minimum-distance bridges between groups produce investigation targets. They do not prove partitions in the RF network.
- Timestamped directed observations may be imported locally as `{ "version": 1, "links": [{ "from": "64 hex public key", "to": "64 hex public key", "observedAt": "ISO timestamp with timezone", "snr": 7.5 }] }`. No packet-path short IDs are automatically interpreted as direct links. Imports are session-only; reports include the imported evidence.
- Terrain comes from Open-Elevation on demand. Three geometric search points, each with two 15-point paths, use one 90-coordinate request. Profiles cache 24 hours. The screen uses a 4/3 effective Earth radius and 60% first Fresnel zone with actual advertised frequency and user-set AGL heights. Rank first by both legs being within the distance limit, then by weakest sampled clearance.
- Candidate search points do not establish physical site access, land cover, structural suitability or successful bidirectional links. Trees, buildings, RF noise, equipment losses and a calibrated link-budget/propagation model remain outside this screen. Source elevation resolution is not guaranteed; sample spacing is shown explicitly.

No automatic observer service is connected. Real public listings and elevations power the planning workflow; observed links require an evidence import. Provider failures show an explicit error rather than sample data. Basemap attribution is displayed using OpenStreetMap tiles and Leaflet 1.9.4 (BSD-2-Clause; see `web/vendor/LEAFLET-LICENSE`).
