# Data

## Files

| File | Size | In git? | Purpose |
|------|------|---------|---------|
| `ACCRA_ALL_SITES_PM25_HOURLY.csv` | 63 MB | **no** (git-ignored) | Main dataset — all sites, hourly PM2.5 |
| `accra_basemap.png` | 3.3 MB | yes | Pre-rendered basemap for Paper F maps |
| `accra_basemap_bounds.json` | 130 B | yes | Exact geographic extent of the basemap image |

## Dataset facts

- **Rows:** 358,540 valid observations (after dropping missing timestamps/values)
- **Sites:** 71 monitoring locations across Accra, Ghana
- **Period:** 2025-01-01 00:00 UTC → 2026-09-29 13:00 UTC (hourly)
- **Source:** [OpenAQ](https://openaq.org) — the open air-quality platform (providers
  include Clarity and others); assembled from OpenAQ's public daily CSV archive.

### Columns

`location_id`, `location_name`, `locality`, `sensor_id`, `datetime_utc`,
`datetime_local`, `datetime_to_utc`, `datetime_to_local`, `pm25_value`,
`latitude`, `longitude`, `provider`, `timezone`

Notes:
- `timezone` is `Africa/Accra` (UTC+0 year-round), so UTC and local clock times
  coincide — but always convert explicitly anyway; it makes the intent clear.
- `location_name` values carry **trailing whitespace** — strip before filtering.

## Why the CSV is not committed

63 MB of tabular data does not belong in a Git history. Options for a reviewer:

1. Keep it local (exams are self-contained on your machine).
2. Attach it to a **GitHub Release** and download it from there.
3. Use **Git LFS** if you must version it.
4. Rebuild from OpenAQ's public daily archive.

Drop the file here at exactly this path and the notebooks will run as-is.

## Verifying your copy

```python
import pandas as pd
df = pd.read_csv("data/ACCRA_ALL_SITES_PM25_HOURLY.csv", usecols=["datetime_utc", "location_name", "pm25_value"])
print(len(df), df["location_name"].nunique(), df["datetime_utc"].min(), df["datetime_utc"].max())
# 358540 71 2025-01-01 00:00:00+00:00 2026-09-29 13:00:00+00:00
```
