# FireBeetle 2 ESP32-C6 Pellet Hopper Monitor

A compact, wireless ultrasonic sensor project for monitoring the pellet level in a pellet stove hopper. Built with ESPHome and integrated into Home Assistant.

## Features

- Real-time hopper level measurement using A02YYUW waterproof ultrasonic sensor
- Calculates **percent full** based on your physical measurements (full: 127mm, empty: 381mm)
- Battery voltage monitoring (FireBeetle 2)
- "Last Updated" timestamp
- Low-level binary sensor for automations/notifications
- Throttled updates (every 60 seconds) to save battery and reduce noise
- Clean Home Assistant dashboard integration with styled gauge card

## Hardware Required

- **DFRobot FireBeetle 2 ESP32-C6**
- **A02YYUW** Waterproof Ultrasonic Sensor
- USB-C power adapter or 3.7V LiPo battery

### Wiring

| A02YYUW Pin | FireBeetle 2 Pin |
|-------------|------------------|
| VCC         | 5V / VIN         |
| GND         | GND              |
| TX (Sensor) | GPIO17 (RX)      |
| RX (Sensor) | GPIO16 (TX)      |

**Note**: TX/RX pins are crossed relative to the sensor.

## ESPHome Configuration

The complete `FireBeetleESP32.yml` file is included in this repository.

### Key Settings

- Update interval: **60 seconds**
- Distance in **mm** (native output of A02YYUW)
- Calibrated for your hopper: Full = 127 mm, Empty = 381 mm

## Home Assistant Dashboard

```yaml
type: gauge
entity: sensor.pellet_stove_level_hopper_percent_full
name: Pellet Hopper
unit_of_measurement: "%"
min: 0
max: 100
needle: true
segments:
  - from: 0
    to: 25
    color: "#f44336"
  - from: 25
    to: 50
    color: "#ff9800"
  - from: 50
    to: 100
    color: "#4caf50"
severity:
  green: 50
  yellow: 25
  red: 0
tap_action:
  action: more-info
card_mod:
  style: |
    ha-card {
      --ha-card-background: rgba(0,0,0,0.35);
      border-radius: 16px;
      box-shadow: none;
      height: 100%;
    }
    ha-gauge {
      --gauge-label-font-size: 18px;
      --gauge-value-font-size: 48px;
      --gauge-value-font-weight: 600;
    }
    ha-gauge$ {
      .dial {
        stroke: rgba(255,255,255,0.12) !important;
      }
      .value-text {
        font-size: 2.7em !important;
        font-weight: 700 !important;
      }
      .needle {
        stroke: #ffffff;
        stroke-width: 5px;
      }
    }
