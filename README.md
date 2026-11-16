# Balboa Spa E-Ink Repeater

A read-only indoor display that mirrors the useful parts of a Balboa TP500 LCD
using MQTT data from the Waveshare spa gateway. The display will show the spa's
current temperature, target temperature, heat/mode and configured equipment
icons. The TP500 Wi-Fi glyph will indicate whether fresh spa telemetry is
arriving. Spa notifications and gateway alerts can temporarily replace the
large temperature readout.

The LilyGO T5 board also subscribes to its own maintenance-command topic for
`STATUS`, `DIAGNOSTICS`, and `RESTART`. These commands affect the display device
only; it never publishes spa control commands. Health and diagnostic sensors
will be exposed to Home Assistant through MQTT discovery.

## Hardware target

- LilyGO TTGO T5 V2.3.1, 2.13-inch e-paper, ESP32, TF-card slot.
- Official board documentation lists a 4 MB ESP32 board with a 122 × 250 SPI
  panel and panel variants supported by ESPHome. The exact panel controller must
  be confirmed before selecting the driver.
- Read-only connection: Wi-Fi and MQTT only; no electrical connection to the
  spa bus or TP500 panel.

See [`docs/hardware.md`](docs/hardware.md) for the pin map and driver variants.

## Project status

Project scaffold and message contract are in progress. Board revision/panel
controller and first USB boot are not yet confirmed. No firmware has been
flashed. The ESPHome configuration validates and compiles against ESPHome
2026.10.2; the selected DKE driver is only a compile candidate pending the
panel label. See [`docs/mqtt-contract.md`](docs/mqtt-contract.md) and
[`docs/display-design.md`](docs/display-design.md).

## Local build

Copy `esphome/secrets.yaml.example` to the ignored `esphome/secrets.yaml` and
fill in Wi-Fi, MQTT, and OTA credentials. Confirm the panel controller and set
`epaper_model` in `esphome/spa-eink.yaml`, then compile with ESPHome:

```sh
ESPHOME_BUILD_PATH=/tmp/balboa-spa-eink-build esphome compile esphome/spa-eink.yaml
```

Compilation does not upload firmware. Remove the microSD card before USB
upload. See [`docs/hardware.md`](docs/hardware.md) before selecting a driver.

## License

Apache License 2.0. Preserve upstream notices for any third-party code or assets.
