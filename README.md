# Home Assistant Nachtverbrauch

Deutsch | [English](README.en.md)

Dieses Verzeichnis enthaelt ein kleines Python-Tool, das stuendliche Home-Assistant-Statistikwerte per REST abruft, den Nachtanteil berechnet und eine Excel-Datei exportiert.

Standardmaessig wird die Nacht fuer Flensburg so definiert:

```text
Sonnenuntergang - 1 Stunde bis Sonnenaufgang am Folgetag + 1 Stunde
```

Randstunden werden anteilig gewichtet. Wenn also nur 30 Minuten einer Stundenmessung im Nachtfenster liegen, zaehlt nur 50 Prozent des kWh-Werts.

## Dateien im Repo

| Datei | Zweck |
|---|---|
| `ha_nachtverbrauch_excel_rest.py` | Hauptskript fuer REST-Abruf, Nachtberechnung und Excel-Export. |
| `STARTPARAMETER.md` | Referenz aller wichtigen Startparameter. |
| `README.en.md` | Englische Fassung dieser README. |
| `STARTPARAMETER.en.md` | Englische Fassung der Startparameter-Referenz. |
| `requirements.txt` | Python-Abhaengigkeiten. |
| `.gitignore` | Schliesst venv, Cache und erzeugte Excel-Dateien aus. |

Lokale Arbeitsverzeichnisse koennen zusaetzlich eine venv (`bin/`, `lib/`, `pyvenv.cfg`) und erzeugte Excel-Dateien enthalten. Diese Dateien gehoeren nicht ins Git-Repo.

## Schnellstart in WSL

Im Verzeichnis:

```bash
cd ~/ha_energy
```

Mit Umgebungsvariablen:

```bash
export HA_URL="http://homeassistant.local:8123"
export HA_TOKEN="DEIN_LONG_LIVED_ACCESS_TOKEN"

./bin/python ha_nachtverbrauch_excel_rest.py \
  --output nachtverbrauch_homeassistant.xlsx
```

Oder mit direkten Parametern:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --ha-url "http://homeassistant.local:8123" \
  --token "DEIN_LONG_LIVED_ACCESS_TOKEN" \
  --output nachtverbrauch_homeassistant.xlsx
```

## Typische Beispiele

Sonnenbasierte Nacht fuer Flensburg, letzte 3 Monate:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py
```

Sonnenbasierte Nacht fuer 12 Monate:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --months 12 \
  --output nachtverbrauch_12_monate.xlsx
```

Altes fixes Zeitfenster von 18:00 bis 07:00:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --night-mode fixed \
  --night-start 18 \
  --night-end 7
```

Bei selbstsigniertem HTTPS-Zertifikat:

```bash
./bin/python ha_nachtverbrauch_excel_rest.py \
  --no-verify-ssl
```

## Voraussetzungen

Mit vorhandener lokaler venv:

```bash
./bin/python -m pip install -r requirements.txt
```

Ohne venv:

```bash
python3 -m pip install -r requirements.txt
```

Kurzer Check:

```bash
./bin/python -m py_compile ha_nachtverbrauch_excel_rest.py
./bin/python ha_nachtverbrauch_excel_rest.py --help
```

## Ergebnis

Die Excel-Datei enthaelt u. a.:

| Blatt | Inhalt |
|---|---|
| `Ueberblick` | Parameter, Zeitraum, Ort, Summen und Durchschnitt. |
| `Monate` | Monatsaggregation des Nachtverbrauchs. |
| `Naechte` | Verbrauch je Nacht, Vollstaendigkeit und Diagramm. |
| `Stundenwerte` | Details je Stundenwert inkl. Ueberlappung und anteiliger kWh. |
