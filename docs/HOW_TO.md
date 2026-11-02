# How to use Point Break

Point Break helps you investigate MeshCore repeater topology and compare possible relay locations. Its map and rankings are planning aids: they do not prove a mesh break, a working RF path, or a suitable installation site.

## Quick start: follow the seven steps

The **Field Guide** at the top of the map workspace tracks the workflow and takes you to the relevant controls.

1. **Area:** choose Central Indiana, CVG, or custom coordinates and set a radius. Wait for the public listings or a saved snapshot to load.
2. **Map:** choose streets to navigate, topographic maps to read terrain, aerial imagery to inspect rooftops, or Field Grid when offline.
3. **Layers (optional):** add mapped towers, wireless sites, amateur-radio sites, or possible host structures. These public map features are incomplete and do not imply access.
4. **Analyze:** model nearby links using the selected radio profile and distance filters. The results are planning hypotheses, not confirmed breaks.
5. **Bridge:** review a possible bridge between modeled groups. If none appear, adjust the filters or region and analyze again.
6. **Placement:** choose a candidate search point and screen its two terrain paths. Review the assumptions; positive sampled clearance does not establish a working radio link.
7. **Offline:** save the regional data pack for field use. Online map tiles are not included; Field Grid works without a connection. Confirm site access and field-test both directions before installation.

## 1. Download and install

1. Open the [Windows desktop workflow](https://github.com/njsnowman/point-break/actions/workflows/desktop.yml).
2. Select the latest successful **Windows desktop** run on `main`.
3. Under **Artifacts**, download `Point-Break-Windows-x64` and extract the ZIP.
4. Run `Point-Break-Setup-0.4.0-x64.exe` to install, or run `Point-Break-Portable-0.4.0-x64.exe` without installing.

The installer supports Start menu and desktop shortcuts. Builds are unsigned; Windows may show a security prompt. Use an internet connection to collect public data and map tiles, then save or export an area pack for field use offline. Action artifacts expire after 30 days; the workflow builds a fresh one whenever it runs.

## 2. Load the region you want to study

On launch, Point Break loads the Central Indiana area. It does not read your device location.

1. Select the region button near the top of Network Explorer.
2. Enter latitude, longitude, and a radius from 10 to 150 km.
3. Choose **Load public repeaters** and wait for the feed status to turn green.

The region is centered on those coordinates. The quick-region menu includes Central Indiana and CVG (Cincinnati / Northern Kentucky), centered near the airport with a 60 km starting radius; change the radius or coordinates for your field area. If the feed is unavailable, the app shows an error and does not substitute demo data.

## 3. Set the model filters

- **Recency** sets the maximum age of a listed advertisement used in analysis.
- **Radio profile** selects an advertised frequency, bandwidth, spreading factor, and coding rate. Only exact profile matches are grouped together.
- **Proximity limit** sets the maximum straight-line distance for a hypothetical link.
- **Refresh** requests the public listing feed again.

These filters affect the analysis, not the public network. They help compare a consistent group of recently advertised repeaters.

## 4. Read the map and bridge suggestions

The map shows eligible advertised repeaters. Dotted segments are **proximity hypotheses** drawn from the distance limit. Imported observations use a separate solid line style.

Choose **Analyze topology** to recalculate the modeled groups and the shortest proposed bridges between groups. Select a bridge card to focus the investigation and see candidate search points.

Treat the results this way:

- **Listed repeaters** are public advertisements, not online health checks.
- **Modeled groups** follow the selected radio, recency, and distance settings; they do not prove network partitions.
- **Potential bridges** connect modeled groups geometrically; they are not confirmed broken links.
- **Candidate search points** are coordinates to investigate, not approved or accessible mounting sites.

The **Map** menu offers Field Grid (offline), OpenStreetMap, Esri Streets, Esri Topographic, USGS Topo, Esri Aerial Imagery, and USGS Aerial + Topo. OpenStreetMap is the standard OSM street style; USGS basemaps are available across the United States. The map status chip shows whether you are using the local offline grid or online tiles. The app opens in **Field Atlas**, a light map workspace with a dark diagnostics panel. Choose another color theme or use the **Accent** color control; the accent choice is saved on this device. The CRT switch toggles the display effect.

Open **Overlays** to enable mapped antennas/towers, cellular and wireless sites, explicitly tagged amateur-radio sites, and potential host structures. MeshCore repeaters, proximity hypotheses, and imported observations remain separate map layers. These OpenStreetMap infrastructure overlays are incomplete and do not show whether a site is available or permitted.

## 5. Prepare an offline field snapshot

While online, load each region you plan to use. For example, load Central Indiana, save its pack, then select CVG and save a second pack. Each pack is stored separately by its region center and radius. Select **Offline Pack** and review the five preparation steps: MeshCore snapshot, local map, optional mapped overlays, optional terrain profiles, and app-shell readiness. Choose **Save field pack** to refresh the local snapshot. It records the MeshCore capture time, OpenStreetMap capture time when available, terrain profile date, map choice, settings, and imported observations. Missing overlays or terrain are clearly marked as optional. To bring a pack to another device, choose **Export pack file** and move the `.json` file to that device, then choose **Import pack file** there. Export each region pack separately.

Choose **Run offline check** to activate the saved snapshot, switch to Field Grid, and verify that the required app files are cached. The result explicitly says whether the app shell can launch offline. This preview does not turn off the device's internet connection. The Field Grid is generated locally; all other basemaps load tiles online, and those tiles are never included in packs. Refresh the pack when connected because public advertisements and mapped features may change. The app shell is cached by supported HTTPS browsers; the desktop app bundles the shell locally. A first-ever browser visit still needs internet to load the app before a pack can be imported.

Offline packs are local files containing public regional listings, mapped infrastructure, and any imported measured-link observations. Treat exported files as shareable data.

## 6. Screen candidate terrain

1. Select a potential bridge.
2. Review the three candidate search points and choose a candidate.
3. Set the candidate antenna height and the assumed remote repeater height in meters above ground level (AGL).
4. Select **Screen terrain** and wait for the elevation samples to load.

Each candidate is checked along both repeater-to-candidate legs using 15 ground samples per leg. Profiles include Earth curvature (4/3 effective Earth radius) and the 60% first Fresnel zone. Candidate ranking prioritizes both legs being inside the selected distance limit, then the weakest sampled clearance.

Positive sampled clearance does not establish a working radio path. Sparse elevation samples may miss local obstacles. The screen does not model trees, buildings, interference, equipment losses, land access, or a calibrated link budget. Verify the actual site and perform field tests before making deployment decisions.

## 7. Import measured link observations

To add direct, timestamped evidence, select **Download format** in the Measured Link Evidence section. Fill the JSON `links` array using the template. Each record requires:

- `from`: full 64-character hexadecimal public key of the sending repeater
- `to`: full 64-character hexadecimal public key of the receiving repeater
- `observedAt`: ISO-8601 timestamp with a timezone (for example, `2026-11-01T16:00:00Z`)
- `snr`: optional numeric signal-to-noise ratio in dB

The observation is directed: an A-to-B observation does not establish B-to-A. Do not convert a repeater’s appearance in a packet path into a direct link unless the measurement supports that inference. Imported observations are stored locally with the active area pack; **Clear** removes them from both the current analysis and saved pack. Current recency and radio-profile filters determine which imported links appear in analysis.

## 8. Export an analysis

Use the export arrow in the app header or **File → Export analysis…** (Ctrl+E). The JSON report includes the feed snapshot and timestamp, selected assumptions, imported evidence, modeled groups, bridge candidates, ranked search points, terrain source, and limitations.

Review the assumptions and evidence before sharing a report. A report records the analysis; it does not certify coverage or a deployment plan.

## Troubleshooting

| What you see | What to check |
| --- | --- |
| Public feed unavailable | Check internet access, then select **Refresh**. The app does not fill missing data with examples. |
| Few or no eligible repeaters | Widen the region, choose a longer recency window, or inspect the available radio profiles. |
| No bridge targets | The selected filters may produce one modeled group, or too few eligible repeaters. This does not prove full RF connectivity. |
| Terrain screen fails | Check internet access and retry. Elevation requests occur only when you screen terrain. |
| Candidate clearance looks strong | Confirm ground data and antenna heights, inspect the actual site, account for vegetation/buildings/RF noise, then field-test both directions. |
| Windows warns about the app | GitHub builds are unsigned. Download the artifact from the project’s Actions workflow and inspect the source/build history if needed. |

## Build from source

Requires Node.js 22 or newer.

```sh
npm ci
npm run build
npm test
npm run desktop
```

To build Windows x64 installer and portable outputs, run `npm run desktop:pack` on Windows. Outputs go to `release/`. For more technical detail, see the repository [README](../README.md) and [data/model notes](../README.md#data-privacy-and-limitations).

