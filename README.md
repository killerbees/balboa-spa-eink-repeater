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
- Official board documentation lists a 122 × 250 SPI display and GPIO pins;
  the exact panel-controller variant must be confirmed before choosing a driver.
  Some V2.3.1 panels use the GDEW0213M21 driver, while later/other panels differ.
- Read-only connection: Wi-Fi and MQTT only; no electrical connection to the
  spa bus or TP500 panel.

## Project status

Project scaffold and message contract are in progress. Board revision/panel
controller and first USB boot are not yet confirmed. No firmware has been
flashed. See [`docs/mqtt-contract.md`](docs/mqtt-contract.md) and
[`docs/display-design.md`](docs/display-design.md).

## License

Apache License 2.0. Preserve upstream notices for any third-party code or assets.
