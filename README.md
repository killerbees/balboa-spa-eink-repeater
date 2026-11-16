# Balboa Spa E-Ink Repeater

A read-only indoor companion display for a Balboa TP500 spa. It reads MQTT data
from the Waveshare gateway; it has no RS-485 connection and never publishes spa
control commands. The restomod screen shows temperature and set point, six
equipment/mode icons, spa notifications, and gateway/link status.

The LilyGO also supports its own MQTT `STATUS`, `DIAGNOSTICS`, and token-gated
`RESTART` commands. Health entities are exposed through Home Assistant MQTT
discovery.

## Hardware and compatibility

- LilyGO/TTGO T5 V2.3.1, ESP32, 2.13-inch 122 × 250 e-paper panel.
- The panel is unmarked. ESPHome's B74 profile has rendered on this unit; DKE
  timed out. B74 is an empirical choice, not an identified panel part.
- The MQTT integration uses schema v2 from the gateway's
  [documented interface](https://github.com/killerbees/Balboa_Spa_Controller_ESP32_RS485/blob/45745667af31cdcda17cd18a83abd6ec8bab0bc4/docs/mqtt-command-state-schema.md).
  The gateway has no published release tag, so this accessory versions
  independently; it is not a gateway fork.

See [`docs/hardware.md`](docs/hardware.md),
[`docs/display-design.md`](docs/display-design.md), and
[`docs/mqtt-contract.md`](docs/mqtt-contract.md).

## Build and USB flash

Create `esphome/secrets.yaml` from the example and set Wi-Fi, MQTT, and OTA
credentials. The secrets file is ignored by Git. Remove the microSD card before
USB upload.

```sh
ESPHOME_BUILD_PATH=/tmp/balboa-spa-eink-build esphome compile esphome/spa-eink.yaml
ESPHOME_BUILD_PATH=/tmp/balboa-spa-eink-build esphome upload esphome/spa-eink.yaml --device /dev/ttyUSB0
```

Replace `/dev/ttyUSB0` with the LilyGO's serial port. The tested board uses the
`2.13in-ttgo-b74` profile; see [`docs/hardware.md`](docs/hardware.md) before
changing it. Firmware must be built locally because the configured credentials
are compiled into the image; no firmware binary is published.

## Release status

Version `0.1.0` is an early hardware release. MQTT status, diagnostics toggling,
restart, Home Assistant discovery/state topics, fresh spa readings, and
notification code `0x28` have been exercised. The gateway was intermittent
during bring-up; visual confirmation of the notification/alert and diagnostics
screens, and the Home Assistant UI, remain outstanding. The unmarked panel's
part number is also unknown.

## License

Apache License 2.0. Preserve upstream notices for third-party code and assets.
