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

The LilyGO T5 V2.3.1 has been flashed with the live MQTT renderer over USB. Its
panel is unmarked. The DKE profile timed out during a full refresh; the B74
profile rendered reliably with reversed drawing colors, though that does not
prove the panel's part number. The renderer uses bold centered temperature,
set point, six state icons, and a prioritized alert/notification area. MQTT
connectivity and the display's own retained availability have been observed.
The broker has shown the LilyGO online with Wi-Fi at −43 dBm and confirmed 20
retained Home Assistant discovery configs plus live health-entity state
messages. A fresh spa snapshot and matching HA readings (37.5°C current, 38°C
target) were observed after the gateway briefly returned; its availability
continued to alternate during observation. Home Assistant entity presentation
and physical rendering of notifications and gateway alerts still need
validation. MQTT `STATUS`, diagnostics overlay toggling, and the token-gated
display restart were exercised; restart produced a new boot ID. The diagnostics
command requested `STATUS 8` and `DEBUG 8`. Physical rendering of the diagnostics
overlay, notifications, and gateway alerts still needs validation. Notification
labels stay below the temperature with a bell; unknown code `0x28` is shown as
`SPA NOTE 28`, but that bell/text rendering has not yet been observed with a
live notification.
See [`docs/hardware.md`](docs/hardware.md),
[`docs/mqtt-contract.md`](docs/mqtt-contract.md), and
[`docs/display-design.md`](docs/display-design.md).

## Local build

Copy `esphome/secrets.yaml.example` to the ignored `esphome/secrets.yaml` and
fill in Wi-Fi, MQTT, and OTA credentials. Confirm the panel controller and set
`epaper_model` in `esphome/spa-eink.yaml`, then compile with ESPHome:

```sh
ESPHOME_BUILD_PATH=/tmp/balboa-spa-eink-build esphome compile esphome/spa-eink.yaml
```

Compilation does not upload firmware. Remove the microSD card before USB
upload. See [`docs/hardware.md`](docs/hardware.md) for the empirical panel
profile and current validation status.

## License

Apache License 2.0. Preserve upstream notices for any third-party code or assets.
