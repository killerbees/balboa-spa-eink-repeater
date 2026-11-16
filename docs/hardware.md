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

The unit is marked `T5 V2.3.1 2.13`, but its panel itself is unmarked. The
initial DKE profile timed out during a full refresh. The B74 profile rendered a
stable geometric test and a full white frame after drawing colors were reversed
(`Color::BLACK` background, `Color::WHITE` ink). The live renderer has since
been flashed using this profile. This is empirical evidence that B74 is usable
on this unit, not proof of the panel's part number. Do not switch profiles
based on the board marking alone.

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
3. Compile using `ESPHOME_BUILD_PATH` outside the repository build directory.
4. Flash over USB and verify the physical image and refresh behavior.
5. Validate MQTT inputs, Home Assistant discovery, and alert handling. The
   display has no RS-485 connection and publishes no spa-control commands.

The repeater has no RS-485 wiring and never sends spa-control commands.
