# Assembly — Corne V4 (Kea) + TENSTAR nRF52840 + ZMK

## Controllers
| Label | Firmware | Bootloader |
|---|---|---|
| LEFT  | `corne_left` (central, Studio) | nice!nano UF2 0.6.0, S140 6.1.1 |
| RIGHT | `corne_right` (peripheral) | same (serial `C1EDBA3F6905580A`) |

Do not swap sides without running `settings_reset` on both.

## Orientation in the Pro Micro socket
The TENSTAR follows the Pro Micro pinout. **Use the labels, not the USB connector, for orientation:**
- The controller column labeled `RAW/B+ · GND · RST · VCC` matches the PCB holes labeled `RAW · GND · RST · VCC`.
- The column labeled `D1/TX · D0/RX · GND · GND` matches the corresponding holes.
- Reuse the old RP2040's orientation **only if** its GND/RST/VCC labels match the TENSTAR's (some "Pro Micro" RP2040 boards have a different pinout).
- If in doubt, check the Kea Workshop schematic before soldering. Do not power the board if GND/VCC do not match.

## OLED (SSD1306 128×32, I²C 0x3C)
The female socket is already installed; just plug it in. Before powering on, check: GND↔GND, VCC↔VCC, SCL↔SCL, SDA↔SDA.
Reversed pinout (VCC/GND swapped) will damage the display.

## Battery (only after USB testing)
1S 3.7 V LiPo: `+ → B+`, `- → B-`. Measure the polarity with a multimeter; wire color is not a guarantee.
There is no power switch, so it remains connected; ZMK turns off the OLED after 30 seconds and enters deep sleep after 15 minutes (any key wakes it).

## TRRS
Do not connect the TRRS cable when using the wireless build.

## First boot (order)
1. LEFT over USB, without the battery: the left keys type.
2. RIGHT over USB: it only powers on (it does not appear as a keyboard).
3. Power both halves and press reset on both → the split pair forms.
4. Bluetooth: pair with the host through LEFT (Lower + BT1..BT5; Lower + Ctrl = BT_CLR).
5. OLED: both displays light up, remain legible, and turn off after 30 seconds.
6. Deep sleep after 15 minutes; a key press wakes and reconnects.
7. Battery: connect the LiPo and test without USB; then connect USB and confirm it charges without heating up.

## Keymap — service keys (Lower layer, bottom row)
`BOOT` / `RST` are at the outer edges of each side; `UNLCK` (Studio) is on Lower + C, fourth position from the left.
Studio: https://zmk.studio with LEFT connected over USB → `UNLCK` to unlock editing.

## Pairing/split recovery
For each problematic half: flash `settings_reset` → reboot → flash that half's firmware. Then reset both halves together.
