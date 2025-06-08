# Intex Poollampe via ESPHome

Dieses Projekt integriert eine **Intex LED Poollampe** vollständig in **Home Assistant** mittels **ESPHome**.  
Es werden zwei getrennte LED-Kanäle (Innen / Außen) unterstützt, die per Taster oder über Home Assistant gesteuert werden können.

> **Idee & Verdrahtung:**  
> [Andy200877/intex_poollampe](https://github.com/Andy200877/intex_poollampe)

---

## Funktionen

- Steuerung von **zwei Kanälen** (Innen & Außen)
- Durchschalten der Farbmodi per:
  - physischem Taster
  - virtuellem Taster (ESPHome Button)
  - `select`-Auswahl mit automatischer Durchschaltlogik
- **Manuelle Synchronisation** des Status (falls z. B. physisch geschaltet wurde)
- **Diagnosefunktionen**: WLAN, IP-Adresse, Laufzeit, Neustart

---

## Hardware

Die Intex-Poollampe schaltet ihre Farbmodi durch kurzes Drücken der Taster. Dieses Verhalten wird hier mit kurzen GPIO-Pulsen (ca. 70 ms LOW) nachgebildet.

Folgende Hardware muss in die Lampe (Außenteil) eingebaut werden, Details: [Andy200877/intex_poollampe](https://github.com/Andy200877/intex_poollampe).
- [ESP8266 D1 Mini](https://www.az-delivery.de/products/d1-mini) (abweichend von der originalen Schaltung)
    - Zusätzlich: [D1 Mini 7–24V Eingangsshield](https://www.berrybase.de/7-24v-dc-eingang-shield-fuer-d1-mini)
    - Wandelt die 12V Lampenspannung auf 5V für den D1 Mini
- **Kein Relais notwendig** → Der ESP gibt direkt GPIO-Impulse auf die Lampenelektronik

---

## Einrichtung

1. ESPHome YAML-Datei (`poollampe.yaml`) auf den D1 Mini flashen
2. In Home Assistant über **ESPHome-Integration** einbinden
3. Optional:
   - Schalter-Helper `input_boolean.sync_anzeigen` anlegen (siehe unten)
   - Lovelace-UI übernehmen (siehe Beispiel)

---

## Home Assistant Einstellungen

### Steuerung
| Funktion | Entität |
|----------|---------|
| Innenfarbe wählen | `select.esphome_poollamp_farbe_innen_w_hlen` |
| Taster Innen simulieren | `button.esphome_poollamp_simuliere_taster_innen` |
| Außenfarbe wählen | `select.esphome_poollamp_farbe_au_en_w_hlen` |
| Taster Außen simulieren | `button.esphome_poollamp_simuliere_taster_au_en` |

### Konfiguration (manuelle Sync-Korrektur)  
Einklappbar über Schalter-Helfer `input_boolean.sync_anzeigen`

| Funktion | Entität |
|----------|---------|
| Innenfarbe manuell setzen | `select.esphome_poollamp_farbe_innen_korrigieren` |
| Außenfarbe manuell setzen | `select.esphome_poollamp_farbe_au_en_korrigieren` |

### Diagnose

| Funktion | Entität |
|----------|---------|
| WLAN-Signal | `sensor.esphome_poollamp_wlan_signal` |
| IP-Adresse | `text_sensor.esphome_poollamp_ip_adresse` |
| ESP Neustart | `button.esphome_poollamp_neustarten` |

---

## Helper: Konfigurationsbereich ein-/ausblenden

Um den Konfigurationsbereich (Sync) in Lovelace ein- und ausblenden zu können, wird ein **input_boolean** (Schalter-Helfer) benötigt:

### Variante A: In `configuration.yaml`

```yaml
input_boolean:
  sync_anzeigen:
    name: Sync anzeigen
    icon: mdi:tools