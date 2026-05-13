# Scripts

Place supporting automation and analysis scripts here.

## Suggested Scripts

| Script | Purpose |
|---|---|
| `scan_surface.sh` | Enumerate open ports / services against target scope |
| `canary_check.py` | Query canary token API for recent alerts |
| `ioc_lookup.py` | Bulk IOC lookups via VirusTotal / OTX API |
| `alert_export.py` | Export SIEM alerts to CSV for review |

## Usage

Each script should include inline documentation and a `--help` flag.
Follow the principle of least privilege - scripts should only require the
minimum access needed to perform their function.
