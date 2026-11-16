# Agent notes

- This device is a read-only spa display. It must never publish spa control
  commands to `balboa/waveshare`.
- Its own maintenance commands use `balboa/spa-eink/command`; accept only the
  documented non-retained command vocabulary.
- Do not commit Wi-Fi or MQTT credentials. Use a local ignored secrets file.
- Distinguish MQTT reachability from fresh spa telemetry. The TP500-style link
  icon represents fresh RS-485-derived spa status, not Wi-Fi association.
- Do not infer commands, ACKs, or physical state from gateway-local events.
- Verify the board revision and e-paper panel controller before selecting a
  driver or flashing firmware.
- Keep display refreshes event-driven and rate-limited; avoid rapid full-screen
  e-paper refreshes.
