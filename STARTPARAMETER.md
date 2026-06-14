# Startparameter

Das Tool kann mit Parametern oder Umgebungsvariablen gestartet werden.

```bash
cd ~/ha_energy
./bin/python ha_nachtverbrauch_excel_rest.py [OPTIONEN]
```

## Pflichtangaben

| Parameter | Alternative | Beschreibung |
|---|---|---|
| `--ha-url` | `HA_URL` | URL der Home-Assistant-Instanz, z. B. `http://homeassistant.local:8123`. |
| `--token` | `HA_TOKEN` | Long-Lived Access Token fuer Home Assistant. |

## Allgemeine Parameter

| Parameter | Default | Beschreibung |
|---|---:|---|
| `--statistic-id` | `sensor.energiebezug_taglich` | Home-Assistant-Statistik-ID, die stuendliche `change`-Werte liefert. |
| `--months` | `3` | Anzahl Monate rueckwirkend ab heute. |
| `--timezone` | `Europe/Berlin` | Zeitzone fuer lokale Datums- und Uhrzeitberechnung. |
| `--output` | `nachtverbrauch_homeassistant.xlsx` | Zielpfad der Excel-Datei. |
| `--verify-ssl` | aktiv | SSL-Zertifikat pruefen. |
| `--no-verify-ssl` | - | SSL-Pruefung deaktivieren. |

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
| `--location-name` | `Flensburg` | Ortsname fuer Dokumentation und Fehlermeldungen. |
| `--latitude` | `54.7937` | Breitengrad des Orts. |
| `--longitude` | `9.4469` | Laengengrad des Orts. |
| `--sunset-offset-minutes` | `-60` | Verschiebung relativ zum Sonnenuntergang. Negative Werte starten frueher. |
| `--sunrise-offset-minutes` | `60` | Verschiebung relativ zum Sonnenaufgang. Positive Werte enden spaeter. |

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

## Vollstaendigkeit und Randstunden

- Das Nacht-Datum ist das Datum des Sonnenuntergangs bzw. des Fensterstarts.
- Stundenwerte, die nur teilweise im Nachtfenster liegen, werden anteilig gerechnet.
- Die Vollstaendigkeit einer Nacht basiert auf abgedeckten Minuten im Nachtfenster.
- Die letzte, noch laufende Nacht darf unvollstaendig sein.

## Minimaler Produktivaufruf

```bash
cd ~/ha_energy
export HA_URL="http://homeassistant.local:8123"
export HA_TOKEN="DEIN_LONG_LIVED_ACCESS_TOKEN"

./bin/python ha_nachtverbrauch_excel_rest.py \
  --months 3 \
  --output nachtverbrauch_homeassistant.xlsx
```
