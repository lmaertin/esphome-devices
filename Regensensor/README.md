# Regensensor mit ESPHome

Dieses Projekt beschreibt die Konfiguration eines Regensensors mit zuschaltbarer Heizung auf Basis eines D1 Mini (ESP8266) mit ESPHome.

## Funktionen

- **Regenerkennung** über GPIO1 (binär)
- **Heizungsschaltung** über GPIO0
- **Automatische Abschaltung** der Heizung nach 15 Minuten
- Anzeige von:
  - WLAN-Signalstärke
  - Uptime
  - IP-Adresse
- Restart-Button über Home Assistant

## Hardware

- Regensensor mit Heizung (vergleichbar H-Tronic RS12)
- ESP01 (ESP8266)
- 12V Netzteil

## Pinbelegung

| Pin    | Funktion           |
|--------|--------------------|
| GPIO0  | Relais für Heizung |
| GPIO1  | Regensensor        |

Die Versorgungsspannung von dem ESP01 kann an dem Board vom Regensensor abgegriffen werden, variiert ja nach Platine.

## Hinweise

- Der Sensorstatus wird als `moisture` klassifiziert und kann direkt in Home Assistant eingebunden werden.
- Die Heizung wird nach dem Einschalten automatisch nach 15 Minuten ausgeschaltet.
- WLAN-Zugangsdaten und API-Schlüssel sind aus Sicherheitsgründen in der Datei `secrets.yaml` ausgelagert.

## YAML-Datei

Die vollständige Konfiguration befindet sich in der Datei [`regensensor.yaml`](regensensor.yaml).