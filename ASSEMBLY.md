# Montagem — Corne V4 (Kea) + TENSTAR nRF52840 + ZMK

## Controladores
| Etiqueta | Firmware | Bootloader |
|---|---|---|
| LEFT  | `corne_left` (central, Studio) | nice!nano UF2 0.6.0, S140 6.1.1 |
| RIGHT | `corne_right` (periférico) | idem (serial `C1EDBA3F6905580A`) |

Não troque de lado sem `settings_reset` nos dois.

## Orientação no socket Pro Micro
O TENSTAR segue o pinout Pro Micro. **Oriente pelos rótulos, não pelo USB:**
- Coluna com `RAW/B+ · GND · RST · VCC` do controlador ↔ furos `RAW · GND · RST · VCC` da PCB.
- Coluna com `D1/TX · D0/RX · GND · GND` ↔ furos correspondentes.
- Repita a orientação do RP2040 antigo **somente se** os rótulos GND/RST/VCC dele coincidirem com os do TENSTAR (alguns RP2040 "Pro Micro" têm pinout diferente).
- Na dúvida, confira o esquema da Kea Workshop antes de soldar. Não energize se GND/VCC não baterem.

## OLED (SSD1306 128×32, I²C 0x3C)
Socket fêmea já existe; só encaixar. Antes de ligar, conferir: GND↔GND, VCC↔VCC, SCL↔SCL, SDA↔SDA.
Pinagem invertida (VCC/GND trocados) queima o display.

## Bateria (só depois dos testes por USB)
LiPo 1S 3,7 V: `+ → B+`, `- → B-`. Meça a polaridade com multímetro; cor de fio não garante.
Sem chave: fica sempre conectada; ZMK apaga OLED em 30 s e entra em deep sleep em 15 min (qualquer tecla acorda).

## TRRS
Não conecte o cabo TRRS com o build wireless.

## Primeiro boot (ordem)
1. LEFT por USB, sem bateria: teclas da esquerda digitam.
2. RIGHT por USB: só liga (não aparece como teclado).
3. Os dois energizados, reset juntos → split forma.
4. Bluetooth: parear com o host pelo LEFT (Lower + BT1..BT5; Lower + Ctrl = BT_CLR).
5. OLED: ambos acendem, legíveis, apagam após 30 s.
6. Deep sleep após 15 min; tecla acorda e reconecta.
7. Bateria: ligar LiPo, testar sem USB; depois USB carrega sem esquentar.

## Keymap — teclas de serviço (layer Lower, linha de baixo)
`BOOT` / `RST` nas extremidades de cada lado; `UNLCK` (Studio) em Lower + C, posição 4 da esquerda.
Studio: https://zmk.studio com LEFT no USB → `UNLCK` para liberar edição.

## Recuperação de pairing/split
Para cada metade problemática: gravar `settings_reset` → reboot → gravar firmware da metade. Depois resetar as duas juntas.
