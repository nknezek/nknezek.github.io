---
layout: trip-map
title: "WAPTA Traverse — Banff to Yoho"
date: 2026-04-20 00:00:00 -0700
categories: outdoors skiing

# ── Map configuration ──────────────────────────────────────────────────────
# Bounding box for the initial view: [[west, south], [east, north]]
map_bounds: [[-116.82, 51.64], [-116.43, 51.82]]

# Fallback center + zoom used before the map fits to bounds
map_center: [-116.63, 51.73]
map_zoom: 11

# 3-D view angles
map_pitch: 58
map_bearing: -25

# Vertical terrain exaggeration (1.0 = true scale, 1.5–2.0 gives dramatic relief)
map_terrain_exaggeration: 1.6

# Data files — swap track.geojson for your .gpx export at any time
map_track:  /assets/wapta/track.geojson
map_photos: /assets/wapta/photos.json

# Day labels shown in the legend and photo lightbox
map_day_names:
  - "Day 1: Bow Lake → Bow Hut"
  - "Day 2: Bow Hut → Peyto Hut"
  - "Day 3: Peyto Hut → Balfour Hut"
  - "Day 4: Balfour Hut → Scott Duncan Hut"
  - "Day 5: Scott Duncan Hut → Yoho Valley"
---

The WAPTA Traverse is one of the great ski mountaineering routes in the Canadian Rockies — a five-day crossing of the Wapta Icefield from Bow Lake in Banff National Park to the Yoho Valley, hut to hut. The route follows a chain of Alpine Club of Canada huts and crosses some of the highest and most remote terrain in the range.

The interactive map above shows our GPS track coloured by day, with camera pins marking selected photos from the trip. Click any pin to view the photo, or drag and tilt the map to explore the 3-D terrain.

## The Route

| Day | Start | Finish | Distance | Elevation gain |
|-----|-------|--------|----------|----------------|
| 1 | Bow Lake (1,920 m) | Bow Hut (2,330 m) | ~7 km | +410 m |
| 2 | Bow Hut | Peyto Hut (2,485 m) | ~12 km | +280 m |
| 3 | Peyto Hut | Balfour Hut (2,470 m) | ~13 km | ±200 m |
| 4 | Balfour Hut | Scott Duncan Hut (2,510 m) | ~10 km | +250 m |
| 5 | Scott Duncan Hut | Yoho Valley (~1,880 m) | ~14 km | −630 m |

---

*Trip report in progress — photos and narrative coming soon.*

---

## Swapping in Your Own Data

**To use your GPX file** instead of the stub GeoJSON, export your tracks from Garmin Connect, Strava, or CalTopo and update the post front matter:

```yaml
map_track: /assets/wapta/your-track.gpx
```

The map will automatically parse the GPX. If your GPX has multiple `<trk>` elements (one per day), each is coloured separately. If it's a single combined track, everything appears as Day 1 blue — split it by day in your GPS app first for full colour coding.

**To add your photos**, place JPEG exports in `assets/wapta/photos/` and update `assets/wapta/photos.json` with the EXIF GPS coordinates. To nudge a marker to a more visible map position without changing the EXIF record, add `display_lat` / `display_lng` fields:

```json
{
  "id": "photo-01",
  "day": 1,
  "exif_lat": 51.6683,
  "exif_lng": -116.4751,
  "display_lat": 51.6700,
  "display_lng": -116.4730,
  "title": "Bow Lake",
  "caption": "Setting out in the early morning.",
  "file": "/assets/wapta/photos/photo-01.jpg"
}
```
