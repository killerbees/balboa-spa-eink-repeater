# Display design

Target the TP500 main-screen hierarchy rather than reproducing its segmented
LCD electrically: one large current-temperature value, compact status glyphs,
and a right-side column for range/filter indicators. The LilyGO is read-only.

```text
┌──────────────────────────────┐
│                           )))│  Fresh spa telemetry glyph; crossed out when stale
│                              │
│       38.0 °C      HIGH      │  Current temperature, range at right
│       SET 38.0 °C   F1  --   │  Target and filter indicators
│                      F2  ON  │
│                              │
│ HEAT READY/REST  LIGHT JETS ON│  Equipment state summary
└──────────────────────────────┘
```

The link glyph is the TP500-style Wi-Fi symbol repurposed to mean fresh
connectivity to the spa through the gateway. It does not mean the LilyGO has
Wi-Fi association; that belongs in HA health sensors. A crossed-out glyph is
shown until fresh gateway spa telemetry arrives and whenever it becomes stale.

A spa notification or gateway alert can replace the main temperature area
while active, with a short label and readable explanation. Return to the normal
screen only when the source clears the alert or its explicit expiry elapses.
Notification/alert precedence and exact layout still need validation on the
physical panel.

The ESPHome display refresh is event-driven and coalesces bursts for 750 ms to
avoid repeated full-screen refreshes. Confirm portrait/landscape rotation, font
legibility, and ghosting on the actual panel before calling the layout done.
