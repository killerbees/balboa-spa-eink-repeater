# MQTT contract

The display consumes the Waveshare MQTT schema v2 documented at gateway commit
[`45745667`](https://github.com/killerbees/Balboa_Spa_Controller_ESP32_RS485/blob/45745667af31cdcda17cd18a83abd6ec8bab0bc4/docs/mqtt-command-state-schema.md).
The gateway has no published release tag; this accessory versions independently
against that interface. It does not subscribe to or publish spa-control entity
command topics.

## Gateway inputs

- `balboa/waveshare/availability`: ESPHome online/offline state.
- `balboa/waveshare/sensor/spa_current_temperature/state`
- `balboa/waveshare/sensor/spa_target_temperature/state`
- `balboa/waveshare/select/spa_heating_mode/state`
- `balboa/waveshare/fan/spa_jets/state` and `.../speed_level/state`
- `balboa/waveshare/switch/spa_light/state`
- `balboa/waveshare/status`: typed JSON CONFIG/STATUS/DEBUG responses, used for
  fresh spa snapshots and RS-485 diagnostics.

The display requests `STATUS 5` for a current spa snapshot and, on demand and
once per minute, `STATUS 8` plus `DEBUG 8` for RS-485 counters and bounded event
provenance. Requests are non-retained. It validates JSON `schema_version`,
`message`, and selector fields; unknown fields/messages are ignored. A topic's
arrival alone is not proof of fresh spa telemetry.

The TP500-style spa-link glyph is on only while the gateway is available and
fresh same-boot spa status is arriving. MQTT online by itself does not light it.

## Display-device command and status topics

- Command input: `balboa/spa-eink/command` (non-retained).
- Device status: `balboa/spa-eink/status` (non-retained JSON).
- Availability: `balboa/spa-eink/availability` (retained birth/LWT).
- HA discovery prefix: `homeassistant/`.

Accepted command payloads are `STATUS`, `DIAGNOSTICS`, and the current-boot
token form `RESTART <token>`. Obtain the token from the device's own STATUS JSON
as `reset_command`; this prevents a retained restart payload from rebooting each
time the device reconnects. DIAGNOSTICS sets the local overlay until its HA
switch is turned off. It requests gateway RS-485 counters and bounded event
provenance. The overlay shows LilyGO MQTT status plus gateway registration,
RS-485 RX/TX counters, event loss, poll coalescing, and pending events. E-paper
updates are event-driven, skip unchanged images, and are limited to one per 16
seconds. No spa-control commands are accepted. The display responds on its own
`/status` topic. RESTART reboots only the LilyGO display.

Examples (substitute broker flags/credentials for the local network):

```sh
mosquitto_pub -t balboa/spa-eink/command -m STATUS
mosquitto_sub -t balboa/spa-eink/status
mosquitto_pub -t balboa/spa-eink/command -m DIAGNOSTICS
mosquitto_pub -t balboa/spa-eink/switch/display_diagnostics_overlay/command -m OFF
```

To restart, first read the `reset_command` field from `/status`, then publish
that exact payload. Never retain commands.

STATUS publishes the cached device health and spa snapshot immediately, then
requests a fresh gateway spa snapshot when the gateway is online and publishes
again when that response arrives. The JSON includes spa temperatures, heating
mode/range, jets speed, light, heating/priming/filter flags, link freshness,
gateway registration/counters, alert information, display age, and a
current-boot restart token. Missing readings are JSON `null`.

## Gateway alert transport

The gateway currently returns bounded event provenance in `DEBUG 8`; the
LilyGO can display `POSSIBLE_CHANNEL_LEAK` from that response for two minutes.
A rejected invalid-channel assignment (`REJECTED`, frame type `0xBF02`, detail
`6`, raw channel above `0x2F`) is shown with a spa-pack reset recommendation.
The event proves an out-of-range assignment was rejected; pool exhaustion is a
likely explanation, not directly proven by this event. Do not infer exhaustion
from READY traffic or a missing assignment.

## Home Assistant discovery

The YAML enables retained ESPHome discovery for one device, including health
entities for LilyGO Wi-Fi RSSI, MQTT connectivity, gateway availability, spa
telemetry freshness/age, and active alert, plus observed spa values. State
values and status JSON are non-retained. HA availability uses the LilyGO's own
availability topic. Gateway-sourced numeric state sensors are configured to
expire after 90 seconds without a refresh; the Spa Link entity separately
reports stale spa telemetry. The broker has returned the retained discovery
configs and live health states; Home Assistant's rendered device and entity
behavior still needs confirmation.
