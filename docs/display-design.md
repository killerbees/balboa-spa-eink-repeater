# Display design

This is a read-only restomod inspired by the TP500, not a pixel-for-pixel
reproduction. There are no borders or labels around the icons. The temperature
uses a bold face in the center column, with its set point below; three stateful
icon slots run down each side, leaving space for short messages around the
temperature. The set point uses `C` rather than the degree glyph to avoid a
missing-font box, and `PRIME PUMPS` appears below the set point.

```text
        CHANNEL ALERT
 [READY]        38.0        [HIGH]
 [HEAT]      SET 38.0 C     [LIGHT]
 [FAN]       SPA NOTE 28    [SPA LINK]
```

The side slots change icon in place: Ready/Rest (including READY-IN-REST);
heat on/off; jets fan on/off;
high/low range; light on/off; and fresh/stale spa link. The jets use a fan or
propeller symbol. The Wi-Fi glyph means
fresh spa telemetry through the gateway, not the LilyGO's Wi-Fi association.
Filter-cycle indicators are omitted from the main screen; they remain available
in diagnostics if needed.

The center area prioritizes invalid-channel alert, possible channel leak,
requested diagnostics, and stale-link message. With fresh spa data, temperature
and set point remain visible; a bell and spa notification use the bottom line.
Known notification codes use their short labels; unknown codes are shown in hex
as `SPA NOTE NN`. The notification takes precedence over `PRIME PUMPS` when
both need the bottom line. Otherwise `PRIME PUMPS` appears there while priming
is active. An invalid channel assignment recommends a spa-pack reset without
claiming that channel exhaustion is proven.

The approved layout uses live MQTT state. Updates are coalesced, unchanged
images are skipped, and physical redraws are limited to one every 16 seconds.
When spa telemetry is stale, both temperatures show `--`; the six equipment
icons keep their last observed states, the lower-right link icon turns off, and
the bottom line shows a bell with `GATEWAY OFFLINE`, `SPA LINK LOST`, or
`WAITING FOR SPA` as appropriate.

Icons are monochrome BMPs selected from Home Assistant's MDI set; their SVG
sources and license are in [`assets/mdi`](../assets/mdi/).
