# br8d Connection Guide, draft 0.1

How common connectors map onto the eight br8d conductors. This is a companion to [br8d-architecture.md](br8d-architecture.md). Where the two disagree, the architecture document wins, and this guide should be corrected.

## How to read the table

- **br8d pins** run in pair order: 1 = A+, 2 = A−, 3 = B+, 4 = B−, 5 = C+, 6 = C−, 7 = V+, 8 = V−. This is the *logical* order (the same as a 2x4 header or ribbon). It is not the physical RJ45 pin order. The RJ45 row shows 1, 2, 3, 6, 4, 5, 7, 8.
- **Cells** give the connector's own contact name (Tip, Ring, Pin 4, and so on) and, where a convention exists, the wire or plug colour in brackets. Where a signal's role matters it is added in brackets too (Left, Hot, Source).
- **A dash (—)** means the connector has nothing for that br8d pin. Leave the pin unconnected. The gatekeeper still senses it.
- **One contact can fill several br8d pins.** A sleeve used as a return for both channels appears under pins 2 and 4. Each channel keeps its own return.
- **Shield column.** It is **not** a ninth pin. It is a connection point for the br8d shield conductor.
  - An asterisk (`*`) means "connect this contact to the br8d shield".
  - A dash means the connector has no separate shield contact, or the outer conductor is the signal return (coax, RCA, TS and TRS jacks), so it is *not* tied to the br8d shield.
  - Bonding rule (working assumption, `ENV-7`): the shield is bonded to chassis at most at one end.
- **Names.**
  - The first column gives the br8d interface name where one exists (`br8di…`). Where the connector itself is the identity (USB, RJ45), it gives the connector name (`br8do…`).
  - A tag on the back, such as `-TS`, separates connectors that share a name.
  - Names you chose: `br8dimidi`, `br8diaux`, `br8diphono`, `br8dispeaker-pow`, `br8dispeaker-pas`, `br8didmx`, `br8di485`, `br8diled`, `br8dilan`, `br8dicom`, `br8diqwic`, `br8docat`, `br8doflat`, `br8doA`, `br8doB`, `br8doC`.
  - **All others are proposals:** `br8dicv`, `br8dijack`, `br8dirca`, `br8dixlr`, `br8dispdif`, `br8diaes`, `br8diwclk`, `br8dican`, `br8digrove`, `br8dipwr` and the tags.

## The table

| br8diX / br8doX | Connector (popular name) | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| **ANALOG AUDIO AND CONTROL VOLTAGE** | | | | | | | | | | |
| `br8dicv` | 3.5 mm TS, modular CV, gate, clock (Eurorack) | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8diaux-TS` | 3.5 mm TS mono (also 2.5 mm) | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8diaux` | 3.5 mm TRS stereo ("Aux", headphones, line) | Tip (Left) | Sleeve | Ring (Right) | Sleeve | — | — | — | — | — |
| `br8diaux-TRSbal` | 3.5 mm TRS balanced mono | Tip (Hot) | Ring (Cold) | — | — | — | — | — | — | Sleeve* |
| `br8diaux-TRRS` | 3.5 mm TRRS, 4-pole (the `br8dopatch` lead) ¹ | Tip | Ring 1 | — | — | — | — | Ring 2 | Sleeve | — |
| `br8dijack-TS` | ¼" (6.35 mm) TS mono (guitar, instrument) | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8dijack` | ¼" TRS stereo (headphones, stereo line, insert) | Tip (Left) | Sleeve | Ring (Right) | Sleeve | — | — | — | — | — |
| `br8dijack-TRSbal` | ¼" TRS balanced mono | Tip (Hot) | Ring (Cold) | — | — | — | — | — | — | Sleeve* |
| `br8dirca-mono` | RCA single plug (line, mono) | Tip | Sleeve (shell) | — | — | — | — | — | — | — |
| `br8dirca` | RCA stereo pair ² | Left (White) Tip | Left (White) Sleeve | Right (Red) Tip | Right (Red) Sleeve | — | — | — | — | — |
| `br8dirca-AV` | RCA trio, composite A/V ² | Left (White) Tip | Left (White) Sleeve | Right (Red) Tip | Right (Red) Sleeve | Video (Yellow) Tip | Video (Yellow) Sleeve | — | — | — |
| `br8diphono` | RCA phono, turntable ³ | Left (White) Tip | Left (White) Sleeve | Right (Red) Tip | Right (Red) Sleeve | — | — | — | — | Earth lug* |
| `br8dixlr` | XLR-3 balanced (mic, line) ⁴ | Pin 2 (Hot) | Pin 3 (Cold) | — | — | — | — | — | — | Pin 1* |
| **SPEAKERS** | | | | | | | | | | |
| `br8dispeaker-pas` | Speakon NL8, passive speaker ⁵ | Pin 1+ (Sub) | Pin 1− (Sub) | Pin 2+ (Low) | Pin 2− (Low) | Pin 3+ (Mid) | Pin 3− (Mid) | Pin 4+ (High) | Pin 4− (High) | — |
| `br8dispeaker-pow` | Powered speaker. Any 4-pair br8d connector (RJ45, 2x4, NL8). Cells show br8d signals, not contacts ⁵ | Left+ | Left− | Right+ | Right− | Sub+ | Sub− | Power+ | Power− | — |
| **DIGITAL AUDIO AND MIDI** | | | | | | | | | | |
| `br8dimidi` | MIDI DIN-5 (180°) ⁶ | Pin 4 (Current source) | Pin 5 (Current sink) | — | — | — | — | — | — | Pin 2* |
| `br8dimidi-TRSA` | 3.5 mm TRS MIDI, Type A (MMA standard, also 2.5 mm) ⁶ | Ring (Current source) | Tip (Current sink) | — | — | — | — | — | — | Sleeve* |
| `br8dimidi-TRSB` | 3.5 mm TRS MIDI, Type B (Korg, Arturia, Novation and others) ⁶ | Tip (Current source) | Ring (Current sink) | — | — | — | — | — | — | Sleeve* |
| `br8dispdif` | S/PDIF coax, RCA (Orange) ⁷ | Tip | Sleeve (shell) | — | — | — | — | — | — | — |
| `br8dispdif-BNC` | S/PDIF coax, BNC 75 Ω | Centre pin | Shell | — | — | — | — | — | — | — |
| `br8dispdif-TS` | S/PDIF coax, 3.5 mm TS | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8diaes` | AES3 balanced, XLR-3 (110 Ω) | Pin 2 (+) | Pin 3 (−) | — | — | — | — | — | — | Pin 1* |
| `br8diaes-BNC` | AES3id unbalanced, BNC (75 Ω) | Centre pin | Shell | — | — | — | — | — | — | — |
| `br8diwclk` | Word clock, BNC (75 Ω) | Centre pin | Shell | — | — | — | — | — | — | — |
| **LIGHTING** | | | | | | | | | | |
| `br8didmx` | DMX512, XLR-5 ⁴ | Pin 3 (Data 1+) | Pin 2 (Data 1−) | Pin 5 (Data 2+) | Pin 4 (Data 2−) | — | — | — | — | Pin 1* |
| `br8didmx-XLR3` | DMX512, XLR-3 (3-pin fixtures) ⁴ | Pin 3 (Data+) | Pin 2 (Data−) | — | — | — | — | — | — | Pin 1* |
| `br8diled` | Addressable LEDs (WS2812, APA102), JST-SM or similar ⁸ | Data (Green or White) | Clock (APA102/SK9822 only) | — | — | — | — | 5V (Red) | GND (Black or White) | — |
| **USB** | | | | | | | | | | |
| `br8doA` | USB-A 2.0 plug or socket ⁹ | Pin 3 D+ (Green) | Pin 2 D− (White) | — | — | — | — | Pin 1 VBUS (Red) | Pin 4 GND (Black) | Shell* |
| `br8doA-3` | USB-A 3.x ⁹ | Pin 3 D+ (Green) | Pin 2 D− (White) | Pin 9 SSTX+ (Orange) | Pin 8 SSTX− (Purple) | Pin 6 SSRX+ (Yellow) | Pin 5 SSRX− (Blue) | Pin 1 VBUS (Red) | Pin 4 GND (Black) | Shell* |
| `br8doB` | USB-B 2.0 (square) ⁹ | Pin 3 D+ (Green) | Pin 2 D− (White) | — | — | — | — | Pin 1 VBUS (Red) | Pin 4 GND (Black) | Shell* |
| `br8doB-3` | USB-B 3.x (SuperSpeed) ⁹ | Pin 3 D+ (Green) | Pin 2 D− (White) | Pin 9 SSRX+ | Pin 8 SSRX− | Pin 6 SSTX+ | Pin 5 SSTX− | Pin 1 VBUS (Red) | Pin 4 GND (Black) | Shell* |
| `br8doB-mini` | USB mini-B (5-pin) ⁹ | Pin 3 D+ | Pin 2 D− | — | — | — | — | Pin 1 VBUS | Pin 5 GND | Shell* |
| `br8doB-micro` | USB micro-B (5-pin) ⁹ | Pin 3 D+ | Pin 2 D− | — | — | — | — | Pin 1 VBUS | Pin 5 GND | Shell* |
| `br8doC` | USB-C, USB 2.0 cable (2-pair profile) ¹⁰ | A6 / B6 (D+) | A7 / B7 (D−) | — | — | — | — | A4, A9, B4, B9 (VBUS) | A1, A12, B1, B12 (GND) | Shell* |
| `br8doC-4` | USB-C, full-featured cable (4-pair profile) ‡ ¹⁰ | A6 / B6 (D+) | A7 / B7 (D−) | A2 (TX1+) ‡ | A3 (TX1−) ‡ | B11 (RX1+) ‡ | B10 (RX1−) ‡ | A4, A9, B4, B9 (VBUS) | A1, A12, B1, B12 (GND) | Shell* |
| **NETWORK** | | | | | | | | | | |
| `br8docat` | RJ45 on UTP/STP cable (CAT5, 5e, 6, 7). Also `br8dilan` ¹¹ | Pin 1 | Pin 2 | Pin 3 | Pin 6 | Pin 4 | Pin 5 | Pin 7 | Pin 8 | Shell* (shielded RJ45) |
| `br8dilan-M12` | M12 D-coded, 4-pin (industrial Ethernet) | Pin 1 (White/Orange) | Pin 3 (Orange) | Pin 2 (White/Green) | Pin 4 (Green) | — | — | — | — | Housing* |
| **SERIAL AND BUS** | | | | | | | | | | |
| `br8dicom-FTDI` | 6-pin FTDI/serial header (host side, TTL-232R cable) ¹² | Pin 4 TXD (Orange) | Pin 5 RXD (Yellow) | Pin 6 RTS# (Green) | Pin 2 CTS# (Brown) | — | — | Pin 3 VCC (Red) | Pin 1 GND (Black) | — |
| `br8dicom-DB9` | DE-9 RS-232, DTE (PC) side ¹³ | Pin 3 (TXD) | Pin 2 (RXD) | Pin 7 (RTS) | Pin 8 (CTS) | Pin 4 (DTR) | Pin 6 (DSR) | — | Pin 5 (GND) | Shell* |
| `br8dican` | DE-9 CAN (CiA 303-1) ¹³ | Pin 7 (CAN_H) | Pin 2 (CAN_L) | — | — | — | — | Pin 9 (CAN_V+, optional) | Pin 3 (CAN_GND) | Pin 5* (CAN_SHLD) |
| `br8di485` | RS-485 terminal block ¹⁴ | D+ (non-inverting) | D− (inverting) | — | — | — | — | — | GND (common) | Shield terminal* |
| `br8diqwic` | Qwiic / STEMMA QT, JST-SH 4-pin (I²C) ¹⁵ | Pin 3 SDA (Blue) | Pin 4 SCL (Yellow) | — | — | — | — | Pin 2 3.3V (Red) | Pin 1 GND (Black) | — |
| `br8digrove` | Grove 4-pin (I²C or signal) ¹⁵ | Pin 2 (White) | Pin 1 (Yellow) | — | — | — | — | Pin 3 VCC (Red) | Pin 4 GND (Black) | — |
| **HEADERS AND POWER** | | | | | | | | | | |
| `br8doflat` | 2x4 shrouded IDC header and ribbon, 2.54 mm ¹⁶ | Pin 1 (Red stripe) | Pin 2 | Pin 3 | Pin 4 | Pin 5 | Pin 6 | Pin 7 | Pin 8 | — |
| `br8doflat-10` | 2x5 shrouded IDC header and ribbon, 2.54 mm ‡ ¹⁶ | Pin 1 (Red stripe) | Pin 2 | Pin 3 | Pin 4 | Pin 5 | Pin 6 | Pin 7 | Pin 8 | Pin 9 and 10* |
| `br8dipwr` | DC barrel jack, 5.5 × 2.1 mm (power only) ¹⁷ | — | — | — | — | — | — | Centre pin | Sleeve | — |

`‡` marks provisional (open) mappings.

## RJ45 wire colours

Colours matter only on the cable side. Terminate **both ends to the same standard**. A T568A plug on one end and a T568B plug on the other is a crossover and swaps pairs A and B.

| RJ45 pin | br8d | T568B | T568A |
|---|---|---|---|
| 1 | A+ | White/Orange | White/Green |
| 2 | A− | Orange | Green |
| 3 | B+ | White/Green | White/Orange |
| 6 | B− | Green | Orange |
| 4 | C+ | Blue | Blue |
| 5 | C− | White/Blue | White/Blue |
| 7 | V+ | White/Brown | White/Brown |
| 8 | V− | Brown | Brown |

## Notes

1. **TRRS (`br8dopatch`).** A = Tip and Ring 1, V = Ring 2 and Sleeve. This is provisional (architecture 6.4).
   - The sleeve connects first on insertion, so V makes contact before the signal pair.
   - A 3-pole plug in a 4-pole socket shorts V. The provider reads about 0 Ω and stays off.
   - Phone headsets swap Ring 2 and Sleeve (CTIA versus OMTP). The polarity-blind V pair tolerates it, but don't use a phone headset as a br8d lead.
2. **RCA colours.** Left is conventionally White, though some older and pro gear uses Black. Right is Red. Yellow is composite **video**, which is why it sits on pair C and keeps audio on A and B. Orange is the conventional S/PDIF coax colour. The RCA shell is the signal return, so it is not tied to the br8d shield.
3. **Phono.** A moving-magnet cartridge produces a few millivolts into 47 kΩ.
   - A 10 kΩ series limiter would load and degrade it, so this profile needs a low-resistance gated path (level 2 and above).
   - I put the turntable **earth lug** on the shield. It is a chassis earth, not a signal return. Say if you'd prefer it unmapped.
4. **XLR and DMX.**
   - Audio XLR is Pin 2 hot, but DMX has Pin 3 as Data+. They swap A+ and A−.
   - DMX into a mic input, or the reverse, is a classic mistake.
   - **Phantom power** puts +48V on Pins 2 and 3 relative to Pin 1. The differential reading is about 0V, but the common-mode band reads about 48V, which is what common-mode reporting is for.
   - RS-485 chips dislike that voltage, so the DMX and RS-485 profiles at level 1 and above must tolerate it (`GK-10`).
5. **Speakers.**
   - **Level.** Speaker-level signals break the electrical envelope. 60V peak is about 225W into 8 Ω.
   - **V reassigned.** In `br8dispeaker-pas` the V pair carries High, not power, so V is not power in that profile and the V rules (`PWR-*`) don't apply. I propose a new rule: *a connector profile may assign V to a non-power use, and the V rules apply only where V carries power.* It needs your approval.
   - **Signature.** A passive speaker has no signature, so a provider would need one added or a probe with a mute relay. I'd treat it as level 4 or 5 and not DIY.
   - **NL4 and NL2.** Speakon NL4 and NL2 have fewer poles and are not mapped. They need a decision on which slots a 2-way or 1-way speaker uses.
6. **MIDI.** It is a 5mA current loop and **one direction per cable**.
   - A+ is the current source (+5V through 220 Ω, DIN Pin 4) and A− is the sink (DIN Pin 5).
   - Two-way MIDI uses two ports, or A for one direction and B for the other.
   - TRS Type A and Type B swap Tip and Ring, so a mismatched adapter does nothing but is harmless.
   - A legacy MIDI OUT socket sits at about 5V behind 220 Ω on its source pin, so a br8d node sees about 5V (band B2) on A and must treat it as foreign.
   - The shield is conventionally bonded at the OUT end only. A MIDI IN socket doesn't connect DIN Pin 2.
7. **TOSLINK and ADAT** are optical and have no electrical contacts, so they are not mapped.
8. **LEDs.** WS2812 uses Data only (leave A− open). APA102 and SK9822 use Data and Clock. The V pair carries about 500mA, so a long strip needs a separate power injector.
9. **USB colours.** USB 2.0: Red VBUS, White D−, Green D+, Black GND. USB 3.x: Blue SSRX−, Yellow SSRX+, Purple SSTX−, Orange SSTX+.
   - **Pair mapping.** B always carries the host-to-device pair and C the device-to-host pair, so type A and type B plugs share a pin map.
   - **Pins 8 and 9** are the host's SSTX on a type A plug and the device's SSRX on a type B plug. Pins 5 and 6 are the opposite.
   - **Unmapped.** The ID pin on mini and micro, and Pin 7 (GND_DRAIN) on USB 3.x.
   - **A br8d port is not a USB port.** A real host will drive VBUS and the data lines, which the node must treat as foreign.
10. **USB-C.**
    - Start with the 2-pair profile. It works on cheap USB 2.0-only cables.
    - **Open (‡).**
      - The SuperSpeed lanes (TX1 and RX1) are proposed for B and C.
      - A flipped plug moves them, so a flip-detection rule is needed.
      - SBU1 and SBU2 (a natural single-ended C pair) are unmapped because they are fragile.
    - **CC.** A USB-C source puts no voltage on VBUS until it sees Rd on CC, so a br8d port does not power a USB-C sink unless the node opts in. A br8d provider on USB-C leaves CC unconnected and does not drive VBUS until it recognises a signature.
    - **Audio adapter mode.** USB-C analog headphone adapters (both CC pins at Ra) are not covered.
11. **RJ45.** This matches passive PoE mode B (+ on 4,5 and − on 7,8). 1000BASE-T uses all four pairs, so a link that carries br8d power is 10/100. A crossover cable swaps A and B (see the colour table).
12. **FTDI header.** The cable (host) drives TXD and RTS#, and the module drives RXD and CTS#. This follows the starter rule that A+ and B+ are driven by the provider. VCC is 5V or 3.3V depending on the cable. The provider grants only a voltage the node's signature allows.
13. **DB9.**
    - **RS-232.**
      - Levels are about ±5 to ±15V, which is band B3.
      - Pin names are for a DTE (PC) port, and a DCE swaps TXD and RXD.
      - There is no power, so V+ is empty and V does not carry power on this profile, which is the same issue as note 5.
      - DCD (Pin 1) and RI (Pin 9) are unmapped.
    - **CAN.** CAN_V+ is optional and nominally 7 to 13V (V+ only if the bus supplies it).
14. **RS-485 labels.** The "A" and "B" labels are inverted between vendors. Here A+ is the **non-inverting** line (D+, "B" in TIA-485 naming).
15. **Qwiic and Grove are I²C connectors**, which makes them single-ended and open-drain with V− as the reference.
    - **Qwiic power is 3.3V only.** A br8d provider must never grant 5V to a Qwiic socket. It needs a 3.3V class, or forced mode with a 3.3V ceiling (`EMIT-5`).
    - **Grove** can be 5V or 3.3V, so the signature has to say which.
16. **Ribbon and headers.** A plug rotated 180° maps pin n to pin 9-n, which puts V in the A slot, so use a keyed shroud. In the 2x5, pins 9 and 10 are reserved for the shield, which is a proposal.
17. **Barrel jack.** Centre-positive is typical, but some adapters are centre-negative, and the polarity-blind V pair handles both. Most barrel adapters are 9 to 24V, so this is a wide-input V-W front end (level 2 and above), not a 5V port.

## Not mapped, and why

| Connector | Reason |
|---|---|
| RJ11 / RJ12 telephone | Ring voltage is about 90 V RMS, which exceeds the ±60V envelope (`ENV-1`) |
| Mains / IEC, speaker banana and binding posts | Far beyond 60V |
| Speakon NL4 and NL2 | Need a slot allocation decision (note 5) |
| TOSLINK, ADAT | Optical |
| HDMI, DisplayPort, Thunderbolt | Out of scope |
| Eurorack bus power header (±12V) | Rails are not a pair, and it is out of scope |
| Combo XLR/TRS jacks | Use the mapping of the plug that is inserted |

## To approve

1. The proposed names and tags (see the names note above).
2. Video on pair C in `br8dirca-AV`.
3. The phono earth lug on the shield.
4. The proposed rule in note 5: a profile may assign V to a non-power use, and the V rules apply only where V carries power.
5. The USB-C B and C pairs (TX1 and RX1) and the 2x5 shield pins.
