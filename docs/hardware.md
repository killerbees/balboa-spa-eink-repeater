# Hardware

## Supported target

The initial target is the LilyGO/TTGO T5 V2.3.1 with an ESP32, microSD slot,
and 2.13-inch 122 × 250 e-paper panel. The V2.3.1 board name alone does not
identify the e-paper controller. Read the panel marking or inspect the display
flex/cable before choosing an ESPHome `waveshare_epaper` model.

| Panel marking / family | ESPHome model |
| --- | --- |
| DEPG0213BN / DKE | `2.13in-ttgo-dke` |
| GDEM0213B74 | `2.13in-ttgo-b74` |
| GDEW0213M21 | Not yet verified in the current ESPHome driver list |

The YAML currently selects DKE only as a compile candidate. Do not flash until
the physical panel variant is confirmed. An incorrect driver can leave the
display blank or produce a damaged-looking refresh; it does not change the
read-only MQTT behavior.

References: [LilyGO T5 E-paper Series](https://github.com/Xinyuan-LILYGO/LilyGo-T5-Epaper-Series)
and [ESPHome Waveshare e-paper models](https://esphome.io/components/display/waveshare_epaper/).

## V2.3.1 pin map

| Function | ESP32 GPIO |
| --- | ---: |
| E-paper MOSI | 23 |
| E-paper SCLK | 18 |
| E-paper CS | 5 |
| E-paper DC | 17 |
| E-paper RESET | 16 |
| E-paper BUSY | 4 |
| microSD MOSI / SCK / MISO / CS | 15 / 14 / 2 / 13 |
| Button 1 | 39 |
| BOOT | 0 |

GPIO5 is a strapping pin and ESPHome warns about it. The microSD card uses
GPIO2; remove the card before USB firmware upload. GPIO12 power control belongs
to the later V2.4 board and is not part of this V2.3.1 profile.

## Safe bring-up

1. Confirm the board revision and panel marking.
2. Remove the microSD card before upload.
3. Compile with the matching panel model and inspect ESPHome output.
4. Flash and check the panel test/layout over USB before connecting it to Wi-Fi
   or MQTT.
5. Confirm MQTT subscriptions and Home Assistant discovery only after the
   screen renders correctly.

The repeater has no RS-485 wiring and never sends spa-control commands.
