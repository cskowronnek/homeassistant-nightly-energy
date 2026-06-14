# Home Assistant Nightly Energy

[Deutsch](README.md) | English

This directory contains a small Python tool that fetches hourly Home Assistant statistics through the REST API, calculates the nighttime share, and exports the result as an Excel workbook.

By default, nighttime for Flensburg is defined as:

```text
sunset - 1 hour through sunrise on the next day + 1 hour
```

Boundary hours are weighted proportionally. If only 30 minutes of an hourly measurement fall inside the night window, only 50 percent of that kWh value is counted.

## Repository Files

| File | Purpose |
|---|---|
| `ha_nachtverbrauch_excel_rest.py` | Main script for REST fetching, night-window calculation, and Excel export. |
| `STARTPARAMETER.md` | German parameter reference. |
| `README.en.md` | English version of this README. |
| `STARTPARAMETER.en.md` | English parameter reference. |
| `requirements.txt` | Python dependencies. |
| `.gitignore` | Excludes virtual environments, caches, and generated Excel exports. |

Local working directories may also contain a virtual environment (`bin/`, `lib/`, `pyvenv.cfg`) and generated Excel files. Those files do not belong in the Git repository.

## Quick Start in WSL

Change into the project directory:

```bash
cd ~/ha_energy
```

With environment variables:

```bash
export HA_URL="http://homeassistant.local:8123"
export HA_TOKEN="YOUR_LONG_LIVED_ACCESS_TOKEN"

./bin/python ha_nachtverbrauch_excel_rest.py \
  --output nachtverbrauch_homeassistant.xlsx
```

Or with explicit parameters:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --ha-url "http://homeassistant.local:8123" \
  --token "YOUR_LONG_LIVED_ACCESS_TOKEN" \
  --output nachtverbrauch_homeassistant.xlsx
```

## Common Examples

Sun-based night window for Flensburg, last 3 months:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py
```

Sun-based night window for 12 months:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --months 12 \
  --output nachtverbrauch_12_monate.xlsx
```

Legacy fixed time window from 18:00 to 07:00:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --night-mode fixed \
  --night-start 18 \
  --night-end 7
```

For self-signed or otherwise invalid HTTPS certificates:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --no-verify-ssl
```

## Requirements

With an existing local virtual environment:

```bash
./bin/python -m pip install -r requirements.txt
```

Without a virtual environment:

```bash
python3 -m pip install -r requirements.txt
```

Quick checks:

```bash
./bin/python -m py_compile ha_nachtverbrauch_excel_rest.py
./bin/python ha_nachtverbrauch_excel_rest.py --help
```

## Output

The Excel workbook contains, among other things:

| Sheet | Contents |
|---|---|
| `Ueberblick` | Parameters, date range, location, totals, and average values. |
| `Monate` | Monthly aggregation of nighttime energy consumption. |
| `Naechte` | Consumption per night, completeness flag, and chart. |
| `Stundenwerte` | Hourly details including overlap and weighted kWh values. |
