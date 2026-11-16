# Display design

Target the TP500 main-screen hierarchy rather than reproducing its segmented
LCD electrically: one large current-temperature value, compact status glyphs,
and a right-side column for range/filter indicators. The LilyGO is read-only.

```text
┌──────────────────────────────┐
│                           )))│  Fresh spa telemetry glyph; crossed out when stale
│                              │
│          38.0 °C             │  Current temperature, large
│          SET 38.0 °C         │  Target temperature, smaller extra
│                              │
│ HEAT   READY   LIGHT   JETS 1│  Only equipment present on this spa
└──────────────────────────────┘
```

The link glyph is the TP500-style Wi-Fi symbol repurposed to mean fresh
connectivity to the spa through the gateway. It does not mean the LilyGO has
Wi-Fi association; that belongs in HA health sensors. A crossed-out glyph is
shown until fresh gateway spa telemetry arrives and whenever it becomes stale.

A spa notification or gateway alert can replace the large temperature area
while active, with a short label and readable explanation. Return to the normal
screen only when the source clears the alert or its explicit expiry elapses.
Notification precedence, exact alert text, and refresh policy remain to be
finalized against live schema and physical screen behavior.

The ESPHome display refresh is event-driven and coalesces bursts for 750 ms to
avoid repeated full-screen refreshes. Confirm portrait/landscape rotation, font
legibility, and ghosting on the actual panel before calling the layout done.
