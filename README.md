# fall_search

Monte-Carlo landing dispersion simulator for falling or tumbling objects under wind and gust conditions.  
Generates **CSV landing data** and **GeoJSON/KML search areas** with distance-based radii and spiral search paths.  
Implemented with the Python standard library only (no external dependencies).  

---

## Features

- Physics-based simulation of falling/tumbling objects with:
  - Power-law wind profiles  
  - Stochastic gusts (Ornstein-Uhlenbeck process)  
  - Tumble dynamics with variable drag/area  
  - Exponential air density model  

- Monte-Carlo sampling with configurable distributions:
  - Mass, drag coefficient (Cd), reference area (A)  
  - Wind speed, direction, gusts  
  - Pre-release offset (glide ratio or timed displacement)  

- Outputs:
  - `landings_v6.csv` – individual landing points with metadata  
  - `search_area_v6.json` – structured summary  
  - `search_area_v6.geojson` – GIS-ready polygons  
  - `search_area_v6.kml` – Google Earth visualization  
  - `spiral_search_v6.geojson` – Archimedean spiral search path  

---

## Repository Layout

```
fall_search.py                   # main simulation driver
data/
  preset_configs/                # reusable object configs (JSON)
  runs/                          # simulation outputs (ignored by Git)
```

---

## Usage

1. **Run with default config**  
   ```bash
   python fall_search.py
   ```

   By default, runs a Shahed-136 preset with Monte-Carlo sampling and saves results under `data/runs/...`.

2. **Run with external config**  
   Place a JSON config in `data/preset_configs/` and edit the `json_path` in `__main__`.  
   Example schema includes:
   ```json
   {
     "incident": {"name": "ExampleRun"},
     "N_runs": 1000,
     "location": {
       "release_lat": 49.27,
       "release_lon": 11.80,
       "release_alt_m_agl": 61.0
     },
     "object": {
       "mass": 1.0,
       "A_mean": 0.06,
       "Cd_mean": 0.35
     },
     "winds": {
       "wind_speed_kph_mean": 10.0,
       "wind_dir_met_from_mean": 90.0
     }
   }
   ```

3. **Outputs**  
   After the run, check `data/runs/<timestamp>_<incident>_v6/` for all generated files.

---

## Example Output

Console summary (truncated):
```
Incident: Shahed-136  |  Seed: 1337  |  N=1500
Wind@10m: 10.0±2.0 kph FROM 0±0 (MET)
Mode(hist):   49.274580, 11.806080
Centroid:     49.274612, 11.806055
Radii (from Release, m): 50=30.2, 90=95.4, 95=112.7
Spiral out-from-release to 95% radius: 112.7 m (spacing 50 m)
Files:
  data/runs/.../landings_v6.csv
  data/runs/.../search_area_v6.json
  data/runs/.../search_area_v6.geojson
  data/runs/.../search_area_v6.kml
  data/runs/.../spiral_search_v6.geojson
```

---

## Requirements

- Python 3.9+  
- No external dependencies (stdlib-only).  

---

## Notes

- Run results (`data/runs/`) are ignored via `.gitignore`.  
- Preset configs in `data/preset_configs/` may be versioned for reproducibility.  
- This is **v6.2.5.1** of the simulation code.  
