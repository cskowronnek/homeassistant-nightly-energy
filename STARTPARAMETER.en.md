# Command-Line Parameters

[Deutsch](STARTPARAMETER.md) | English

The tool can be started with command-line parameters or environment variables.

```bash
cd ~/ha_energy
./bin/python ha_nachtverbrauch_excel_rest.py [OPTIONS]
```

## Required Values

| Parameter | Alternative | Description |
|---|---|---|
| `--ha-url` | `HA_URL` | URL of the Home Assistant instance, for example `http://homeassistant.local:8123`. |
| `--token` | `HA_TOKEN` | Long-lived access token for Home Assistant. |

## General Parameters

| Parameter | Default | Description |
|---|---:|---|
| `--statistic-id` | `sensor.energiebezug_taglich` | Home Assistant statistic ID that provides hourly `change` values. |
| `--months` | `3` | Number of months to look back from today. |
| `--timezone` | `Europe/Berlin` | Time zone used for local date and time calculations. |
| `--output` | `nachtverbrauch_homeassistant.xlsx` | Target path for the Excel workbook. |
| `--verify-ssl` | enabled | Verify the SSL certificate. |
| `--no-verify-ssl` | - | Disable SSL verification, for example with self-signed or otherwise invalid certificates. |

## Night Mode

| Parameter | Default | Description |
|---|---:|---|
| `--night-mode` | `sun` | Night-window definition. Allowed values are `sun` and `fixed`. |

## Sun Mode `sun`

Default definition:

```text
sunset - 60 minutes through sunrise on the next day + 60 minutes
```

| Parameter | Default | Description |
|---|---:|---|
| `--location-name` | `Flensburg` | Location name used for documentation and error messages. |
| `--latitude` | `54.7937` | Latitude of the location. |
| `--longitude` | `9.4469` | Longitude of the location. |
| `--sunset-offset-minutes` | `-60` | Offset relative to sunset. Negative values start earlier. |
| `--sunrise-offset-minutes` | `60` | Offset relative to sunrise. Positive values end later. |

Example:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --night-mode sun \
  --months 6 \
  --output nachtverbrauch_flensburg.xlsx
```

## Fixed Mode `fixed`

This mode keeps the old fixed hourly window behavior.

| Parameter | Default | Description |
|---|---:|---|
| `--night-start` | `18` | Local start hour. |
| `--night-end` | `7` | Local end hour. |

Example:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --night-mode fixed \
  --night-start 18 \
  --night-end 7
```

## Completeness and Boundary Hours

- The night date is the date of sunset or the fixed-window start.
- Hourly values that only partially overlap the night window are weighted proportionally.
- Night completeness is based on covered minutes inside the night window.
- The latest, still-running night may be incomplete.

## Minimal Production Run

```bash
cd ~/ha_energy
export HA_URL="http://homeassistant.local:8123"
export HA_TOKEN="YOUR_LONG_LIVED_ACCESS_TOKEN"

./bin/python ha_nachtverbrauch_excel_rest.py \
  --months 3 \
  --output nachtverbrauch_homeassistant.xlsx
```
