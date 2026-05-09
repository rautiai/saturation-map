# Saturation Map

An interactive, locally-run map tool for visualizing geocache saturation: where existing caches are and where there is still room to hide new ones.

## What it does

Each geocache is displayed as a filled circle with a 161 m radius (the minimum allowed distance between geocaches). Overlapping circles immediately show which areas are saturated. The sidebar lists all your caches, and a search box lets you search caches by name or GC code.

## Getting started

The app is a single self-contained HTML file with no build step and no server required.

1. Open `index.html` in any modern browser. That's it.
2. Add your caches manually or import them from a CSV/TSV file.
3. When you're done, click **Download map.html** to save a new copy of the file with your data baked in. Open that file next time to pick up where you left off.

## Adding caches

**One at a time:** click **New cache** and fill in the name, GC code, type, and coordinates. Multi-cache and Mystery Cache entries support additional waypoints.

**Bulk import:** click the import button (top-left) and either drag-and-drop a file or paste text. The expected format is CSV or TSV with these columns:

| Column | Required | Notes |
|---|---|---|
| `name` | Yes | Cache name |
| `latitude` | Yes | Decimal degrees |
| `longitude` | Yes | Decimal degrees |
| `gc_code` | No | e.g. `GC6511N` |
| `cache_type` | No | `traditional`, `multi`, or `mystery` |
| `circle_radius` | No | Metres, defaults to 161 |
| `custom_color` | No | Hex color, overrides type color |
| `primary_gc` | No | For waypoints belonging to a parent cache |

## Cache types

| Type | Color |
|---|---|
| Traditional Cache | Green |
| Multi-Cache | Orange |
| Mystery Cache | Blue |

Each type can also have a custom color set per cache via the edit dialog.

## Map controls

Controls are on the left side of the map, below the sidebar:

| Control | Action |
|---|---|
| Crosshair button | Toggle coordinate pick mode. Click or right-click anywhere on the map to get coordinates and optionally drop a placeholder pin. |
| Zoom +/− | Zoom in and out |
| Compass | Reset map bearing to north |
| Cube button | Toggle 3D pitch view |
| Map button | Toggle satellite imagery |

Right-clicking the map (or using pick mode) drops a **placeholder pin** at that location. Placeholders show the exclusion circle so you can evaluate a potential hiding spot before committing to it. A placeholder can later be converted into a real cache via the edit dialog.

## Saving and sharing

- **Download map.html** rewrites the `INITIAL_CACHES` block inside the HTML and downloads a new standalone file with all your data embedded. Share this file with anyone and they can open it with no setup.
- **Export caches.csv** exports the current cache list as a CSV file, with optional filtering by cache type. The CSV uses the same column format accepted by the importer.

Data is also persisted in the browser's IndexedDB so changes survive page reloads without needing to download a new copy each session.

## Technical notes

- Single-file app: all HTML, CSS, and JavaScript live in `index.html`. No frameworks, no bundler, no dependencies beyond the MapTiler SDK (loaded from CDN).
- Map tiles are served by [MapTiler](https://www.maptiler.com/). The API key in the file is a project-specific key.
- Satellite view uses the MapTiler `hybrid-v4` style.
- Cache circles are rendered as GeoJSON polygon layers via MapLibre GL / MapTiler SDK. The 161 m radius is computed using a geodesic circle approximation.
