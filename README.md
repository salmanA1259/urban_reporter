# 🗺️ urban_reporter

**A citizen-powered urban issue reporting app built with Flutter, OpenStreetMap & Supabase.**

urban_reporter lets anyone, anywhere in the world, pin and report urban infrastructure problems — potholes, broken streetlights, damaged roads, open manholes, and more — directly on an interactive map. All reports are publicly visible so the community can see what's already been flagged in their area.

No account needed. Open the app, tap the map, describe the issue, submit.

---

## 📱 Download

| Platform | Link |
|----------|------|
| Android APK | [Download latest release](../../releases/latest) |

> iOS build not available at this time.

---

## ✨ Features

- 📍 **Tap-to-pin reporting** — drop a marker anywhere on the map to report an issue at that exact location
- 🗺️ **Live OpenStreetMap layer** — full interactive map with zoom, pan, and satellite detail
- 📋 **Issue categorisation** — tag reports by type (pothole, broken light, damaged road, flooding, etc.)
- 📸 **Photo attachment** — attach an image from your camera or gallery to document the issue
- 🌍 **Global scope** — works in any city, any country
- 👁️ **Community feed** — all submitted reports are visible on the map so you can see what's already flagged near you
- ⚡ **No login required** — fully open, zero friction submission

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Mobile Framework | Flutter (Dart) |
| Maps | flutter_map + OpenStreetMap (free, no API key needed) |
| Backend & Database | Supabase (PostgreSQL + PostGIS) |
| Spatial Storage | PostGIS geometry columns for lat/lng and geospatial queries |
| Storage (images) | Supabase Storage buckets |
| Auth | None — anonymous public submissions |

---

## 🗄️ Database Schema (Supabase)

```sql
create table reports (
  id          uuid primary key default gen_random_uuid(),
  created_at  timestamptz default now(),
  category    text not null,
  description text,
  latitude    float8 not null,
  longitude   float8 not null,
  image_url   text,
  location    geography(Point, 4326) generated always as (
                st_point(longitude, latitude)::geography
              ) stored
);
```

Spatial indexing is enabled on the `location` column for fast bounding-box queries as the map viewport changes.

---

## 📁 Project Structure

```
urban_reporter/
│
├── lib/
│   ├── main.dart               # Entry point
│   ├── screens/
│   │   ├── map_screen.dart     # Main map view with all report pins
│   │   └── report_screen.dart  # New report submission form
│   ├── models/
│   │   └── report_model.dart   # Report data model
│   ├── services/
│   │   └── supabase_service.dart  # DB read/write logic
│   └── widgets/
│       └── report_marker.dart  # Custom map marker widget
│
├── android/                    # Android build files
├── pubspec.yaml                # Flutter dependencies
└── README.md
```

---

## 📦 Key Dependencies

```yaml
dependencies:
  flutter_map: ^6.0.0          # OpenStreetMap rendering
  latlong2: ^0.9.0             # Lat/lng data types
  supabase_flutter: ^2.0.0     # Supabase client
  image_picker: ^1.0.0         # Camera / gallery access
  geolocator: ^11.0.0          # Device GPS
  uuid: ^4.0.0                 # Unique ID generation
```

---

## 🚀 How It Works

1. User opens the app — the map loads centred on their GPS location
2. User taps a location on the map where an issue exists
3. A form slides up — user selects category, adds a description, optionally attaches a photo
4. On submit, the report (coordinates, category, description, image URL) is written to Supabase
5. The new pin appears on the map immediately, visible to all other users in real time

---

## 🌐 Academic Context

This app was developed as part of the **MS in Geospatial Data Science (MSDS-GDS)** programme at **NED University of Engineering & Technology, Karachi**, as a WebGIS assignment simulating a production civic-tech deployment for an urban authority.

The project demonstrates:
- Real-world WebGIS application development with Flutter + PostGIS
- Spatial data modelling and storage best practices
- Integration of open-source mapping (OpenStreetMap) with a cloud backend
- Citizen-facing UX design for geographic data collection

---

## 👤 Author

**Salman Arif**
MS Geospatial Data Science — NED University
[linkedin.com/in/salmanarif1259](https://linkedin.com/in/salmanarif1259) • [salmana1259.github.io](https://salmana1259.github.io)

---

## 📄 License

This project is released for portfolio and academic demonstration purposes.
APK is free to download and test. Source code available on request.
