# MQTT contract

The display consumes the Waveshare schema v2 entity topics documented in the
[main gateway repository](https://github.com/killerbees/Balboa_Spa_Controller_ESP32_RS485/blob/pr/bp200g1-reliability-mqtt/docs/mqtt-command-state-schema.md).
It does not subscribe to or publish spa-control entity command topics.

## Gateway inputs

- `balboa/waveshare/availability`: ESPHome online/offline state.
- `balboa/waveshare/sensor/spa_current_temperature/state`
- `balboa/waveshare/sensor/spa_target_temperature/state`
- `balboa/waveshare/select/spa_heating_mode/state`
- `balboa/waveshare/select/spa_temperature_range/state`
- `balboa/waveshare/select/spa_temperature_scale/state`
- `balboa/waveshare/binary_sensor/spa_heating/state`
- `balboa/waveshare/fan/spa_jets/state` and `.../speed_level/state`
- `balboa/waveshare/switch/spa_light/state`
- `balboa/waveshare/status`: typed JSON CONFIG/STATUS/DEBUG responses.

The display should request `STATUS 5` for a current spa snapshot and `DEBUG 8`
for bounded RS-485 provenance when needed. Requests are non-retained and
rate-limited. It must validate the JSON `schema_version` and `message` fields;
unknown fields/messages are ignored. A topic's arrival alone is not proof of
fresh spa telemetry.

The TP500-style spa-link glyph is on only while the gateway is available and
fresh same-boot spa status is arriving. MQTT online by itself does not light it.

## Display-device command and status topics

- Command input: `balboa/spa-eink/command` (non-retained).
- Device status: `balboa/spa-eink/status` (non-retained JSON).
- Availability: `balboa/spa-eink/availability` (retained birth/LWT).
- HA discovery prefix: `homeassistant/`.

Accepted command payloads: `STATUS`, `DIAGNOSTICS`, `RESTART`. No other
payloads are actions. The display responds to STATUS/DIAGNOSTICS on its own
`/status` topic. RESTART reboots only the LilyGO display.

## Gateway alert transport

The gateway currently returns bounded event provenance in `DEBUG 8`; the
LilyGO can display `POSSIBLE_CHANNEL_LEAK` from that response. Out-of-channels
requires a stable explicit gateway alert before the display can report it; do
not infer it from READY traffic or a missing assignment. Define that gateway
message before relying on the alert.

## Home Assistant discovery

Publish retained discovery for a single device with health entities for Wi-Fi
RSSI, MQTT connectivity, gateway availability, spa telemetry freshness/age,
last display update, and active alert. State values and status JSON are
non-retained. HA availability uses the LilyGO's own availability topic.
