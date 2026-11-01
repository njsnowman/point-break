# How to use Point Break

Point Break helps you investigate MeshCore repeater topology and compare possible relay locations. Its map and rankings are planning aids: they do not prove a mesh break, a working RF path, or a suitable installation site.

## 1. Download and install

1. Open the [Windows desktop workflow](https://github.com/njsnowman/point-break/actions/workflows/desktop.yml).
2. Select the latest successful **Windows desktop** run on `main`.
3. Under **Artifacts**, download `Point-Break-Windows-x64` and extract the ZIP.
4. Run `Point-Break-Setup-0.4.0-x64.exe` to install, or run `Point-Break-Portable-0.4.0-x64.exe` without installing.

The installer supports Start menu and desktop shortcuts. Builds are unsigned; Windows may show a security prompt. The app needs internet for the public node feed, OpenStreetMap map tiles, and terrain elevation requests. Action artifacts expire after 30 days; the workflow builds a fresh one whenever it runs.

## 2. Load the region you want to study

On launch, Point Break loads the Central Indiana area. It does not read your device location.

1. Select the region button near the top of Network Explorer.
2. Enter latitude, longitude, and a radius from 10 to 150 km.
3. Choose **Load public repeaters** and wait for the feed status to turn green.

The region is centered on those coordinates. If the feed is unavailable, the app shows an error and does not substitute demo data.

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

The map basemap can be toggled between street and hypothesis views. Use the **CRT** switch to change the display effect.

## 5. Screen candidate terrain

1. Select a potential bridge.
2. Review the three candidate search points and choose a candidate.
3. Set the candidate antenna height and the assumed remote repeater height in meters above ground level (AGL).
4. Select **Screen terrain** and wait for the elevation samples to load.

Each candidate is checked along both repeater-to-candidate legs using 15 ground samples per leg. Profiles include Earth curvature (4/3 effective Earth radius) and the 60% first Fresnel zone. Candidate ranking prioritizes both legs being inside the selected distance limit, then the weakest sampled clearance.

Positive sampled clearance does not establish a working radio path. Sparse elevation samples may miss local obstacles. The screen does not model trees, buildings, interference, equipment losses, land access, or a calibrated link budget. Verify the actual site and perform field tests before making deployment decisions.

## 6. Import measured link observations

To add direct, timestamped evidence, select **Download format** in the Measured Link Evidence section. Fill the JSON `links` array using the template. Each record requires:

- `from`: full 64-character hexadecimal public key of the sending repeater
- `to`: full 64-character hexadecimal public key of the receiving repeater
- `observedAt`: ISO-8601 timestamp with a timezone (for example, `2026-11-01T16:00:00Z`)
- `snr`: optional numeric signal-to-noise ratio in dB

The observation is directed: an A-to-B observation does not establish B-to-A. Do not convert a repeater’s appearance in a packet path into a direct link unless the measurement supports that inference. Imports are local to the current app session and are cleared when the session ends or when **Clear** is selected. Current recency and radio-profile filters determine which imported links appear in analysis.

## 7. Export an analysis

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

