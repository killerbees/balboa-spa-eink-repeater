# Agent notes

- This device is a read-only spa display. It must never publish spa control
  commands to `balboa/waveshare`.
- Its own maintenance commands use `balboa/spa-eink/command`; accept only the
  documented non-retained command vocabulary.
- Do not commit Wi-Fi or MQTT credentials. Use a local ignored secrets file.
- Distinguish MQTT reachability from fresh spa telemetry. The TP500-style link
  icon represents fresh RS-485-derived spa status, not Wi-Fi association.
- Do not infer commands, ACKs, or physical state from gateway-local events.
- The physical board is LilyGO T5 V2.3.1, marked 2.13-inch; the panel is
  unmarked. DKE timed out. B74 produced a stable image with inverted drawing
  colors, but that does not identify the panel part number.
- The operator approved the restomod layout and event-driven refreshes. Keep
  refreshes coalesced, skip unchanged frames, and preserve the 16-second
  minimum interval between physical e-paper redraws.
- Layout direction: restomod, no borders or icon labels, three status icons in
  a vertical column on each side, centered temperature and set point, with
  space for short alert/notification/priming messages.
- Keep this project read-only toward spa controls. The display may request
  STATUS/DEBUG data and must never publish spa-control commands.
