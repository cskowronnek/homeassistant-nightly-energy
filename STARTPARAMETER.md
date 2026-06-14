# Startparameter

Deutsch | [English](STARTPARAMETER.en.md)

Das Tool kann mit Parametern oder Umgebungsvariablen gestartet werden.

```bash
cd ~/ha_energy
./bin/python ha_nachtverbrauch_excel_rest.py [OPTIONEN]
```

## Pflichtangaben

| Parameter | Alternative | Beschreibung |
|---|---|---|
| `--ha-url` | `HA_URL` | URL der Home-Assistant-Instanz, z. B. `http://homeassistant.local:8123`. |
| `--token` | `HA_TOKEN` | Long-Lived Access Token für Home Assistant. |

## Allgemeine Parameter

| Parameter | Default | Beschreibung |
|---|---:|---|
| `--statistic-id` | `sensor.energiebezug_taglich` | Home-Assistant-Statistik-ID, die stündliche `change`-Werte liefert. |
| `--months` | `3` | Anzahl Monate rückwirkend ab heute. |
| `--timezone` | `Europe/Berlin` | Zeitzone für lokale Datums- und Uhrzeitberechnung. |
| `--output` | `nachtverbrauch_homeassistant.xlsx` | Zielpfad der Excel-Datei. |
| `--verify-ssl` | aktiv | SSL-Zertifikat prüfen. |
| `--no-verify-ssl` | - | SSL-Prüfung deaktivieren. |

## Nachtmodus

| Parameter | Default | Beschreibung |
|---|---:|---|
| `--night-mode` | `sun` | Nachtdefinition. Erlaubt sind `sun` und `fixed`. |

## Sonnenmodus `sun`

Default-Definition:

```text
Sonnenuntergang - 60 Minuten bis Sonnenaufgang am Folgetag + 60 Minuten
```

| Parameter | Default | Beschreibung |
|---|---:|---|
| `--location-name` | `Flensburg` | Ortsname für Dokumentation und Fehlermeldungen. |
| `--latitude` | `54.7937` | Breitengrad des Orts. |
| `--longitude` | `9.4469` | Längengrad des Orts. |
| `--sunset-offset-minutes` | `-60` | Verschiebung relativ zum Sonnenuntergang. Negative Werte starten früher. |
| `--sunrise-offset-minutes` | `60` | Verschiebung relativ zum Sonnenaufgang. Positive Werte enden später. |

Beispiel:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --night-mode sun \
  --months 6 \
  --output nachtverbrauch_flensburg.xlsx
```

## Fixer Modus `fixed`

Dieser Modus entspricht dem alten festen Stundenfenster.

| Parameter | Default | Beschreibung |
|---|---:|---|
| `--night-start` | `18` | Startstunde lokal. |
| `--night-end` | `7` | Endstunde lokal. |

Beispiel:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --night-mode fixed \
  --night-start 18 \
  --night-end 7
```

## Vollständigkeit und Randstunden

- Das Nacht-Datum ist das Datum des Sonnenuntergangs bzw. des Fensterstarts.
- Stundenwerte, die nur teilweise im Nachtfenster liegen, werden anteilig gerechnet.
- Die Vollständigkeit einer Nacht basiert auf abgedeckten Minuten im Nachtfenster.
- Die letzte, noch laufende Nacht darf unvollständig sein.

## Minimaler Produktivaufruf

```bash
cd ~/ha_energy
export HA_URL="http://homeassistant.local:8123"
export HA_TOKEN="DEIN_LONG_LIVED_ACCESS_TOKEN"

./bin/python ha_nachtverbrauch_excel_rest.py \
  --months 3 \
  --output nachtverbrauch_homeassistant.xlsx
```
