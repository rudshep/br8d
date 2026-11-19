br8d-er: One who adopts the br8d-ing philosophy with a higher repurpose. We love you.

# The br8d-er guide

Got a cable in a drawer and an idea for another module? br8d is a draft way to give the wires a shared layout, so you can reuse suitable connectors without inventing every pinout from scratch. This guide combines a maker's journey with a connector catalogue: find the thing you have, check what each contact would carry, and make the electronics at both ends agree.

> **Draft, not a build-verified standard.** The connector mappings, protection levels and example component values need review and bench tests. The [connection guide](br8d-connection-guide.md) is the source for the newer names and mappings used here; the [architecture](br8d-architecture.md) describes the current electrical rules and open design work. Where this guide proposes something beyond those rules, it is marked as open.

**A plug that fits is not proof of compatibility.** A br8d port using a USB, RJ45, audio or other familiar connector is not automatically a USB, Ethernet, headphone or other conventional port. A stock device needs a correctly designed interface or bridge. Don't connect valuable equipment to an untested prototype.

## Find your way around

- [Start with a project](#start-with-a-project): MIDI, CV, an effect and a growing modular rig.
- [Read the catalogue](#read-the-catalogue): pair order, names, dashes and shields.
- [Connector catalogue](#connector-catalogue): [audio and CV](#audio-and-control-voltage), [speakers](#speakers), [MIDI and digital audio](#midi-and-digital-audio), [lighting](#lighting), [USB](#usb), [network](#network), [serial and buses](#serial-and-buses), [headers and power](#headers-and-power). Search with **Ctrl-F** for a connector, protocol, contact or `br8d` name.
- [Cable choices](#cable-choices): twin flex, shielded multicore, looms and custom connectors.
- [Bring-up checklist and bench seed](#bring-up-checklist): a practical starting point, not a finished circuit.
- [Not mapped and open decisions](#not-mapped-and-open-decisions): things that still need a decision or don't fit this guide.

## Start with a project

Imagine you've built a CV-controlled effect for a modular synth. Audio runs on ordinary patch leads. You want the effect to follow a MIDI sequencer and take CV from the synth, but there's no MIDI cable in the drawer—only a four-conductor AUX lead.

Build a **bridge**, not a magic cable: the bridge connects to the sequencer's conventional MIDI socket and the synth's CV output. It converts those signals for a separately designed br8d link; a matching interface on the effect translates them back into signals its circuit can use. Both boxes have local power.

| First link: 4-pole 3.5 mm TRRS | Intended use | Check first |
|---|---|---|
| A: tip / ring 1 | MIDI-derived data | The bridge handles the conventional MIDI current loop; the link needs its own agreed electrical signal. |
| V: ring 2 / sleeve | Differential CV, **not power** | This use of V is an **open proposal**, not an approved exception to the architecture's power rules. Both ends need the same documented profile and appropriate protection. |

The [TRRS mapping](#audio-and-control-voltage) itself is provisional. Check the actual cable's continuity and both port pinouts; don't substitute a phone headset or an ordinary headphone port. A three-pole plug can short the V contacts.

When the effect works, perhaps it becomes the first piece of a larger modular tribute: a filter, envelope, sequencer and a few delightfully odd control boxes. A MIDI/CV bridge can translate MIDI to CV and gate, and measured CV and gate back to MIDI. Conventional ports remain conventional on the equipment side; br8d-aware electronics do the adapting.

As the rack grows, a keyed ribbon or CAT cable gives you more conductors. You might document a **different** four-pair link: A for MIDI-derived data, B for differential CV, C for gate/clock, and V for power if both ends are designed for it. It is not interchangeable with the first two-pair profile. Write down the signal levels, directions and pinout for each link, then test them.

## Read the catalogue

Each row maps a connector's **own contacts** to logical br8d conductors. It does not say that the connector automatically converts a protocol or that an ordinary device is safe to connect.

| Logical br8d pin | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| Conductor | A+ | A− | B+ | B− | C+ | C− | V+ | V− |

- This is pair order, as on a 2x4 ribbon, **not RJ45 physical pin order**. See [RJ45 wire colours](#rj45-wire-colours).
- Contact names and familiar colours are given where useful. A **dash (—)** means no contact is mapped there; leave it unconnected. A gatekeeper still senses its br8d conductor.
- A contact may appear more than once: for example, a stereo jack sleeve is the return for both channels. The table shows both channel returns; it does not make them isolated.
- **Shield is not a ninth signal pin.** An asterisk (`*`) marks a contact proposed for the br8d shield. A dash means no separate shield is mapped, or the outer contact is a signal return, as with coax, RCA, TS and unbalanced TRS. Shield/chassis bonding is open; the working assumption is a bond to chassis at **most one end** (`ENV-7`).
- `br8di…` names identify an interface; `br8do…` names identify a connector or cable form. Suffixes distinguish variants. The connection guide records `br8dimidi`, `br8diaux`, `br8diphono`, `br8dispeaker-pow`, `br8dispeaker-pas`, `br8didmx`, `br8di485`, `br8diled`, `br8dilan`, `br8dicom`, `br8diqwic`, `br8docat`, `br8doflat`, `br8doA`, `br8doB` and `br8doC` as chosen names. Other interface names and suffix tags below are proposals. The earlier maker guide's `br8doaux`, `br8dobac` and `br8doqwiic` are broader or older labels; use the catalogue's more specific names when documenting a mapping.
- **‡** marks an explicitly open mapping. Other entries are draft mappings too, not tested or approved designs. Colours are conventions, not a substitute for checking the contacts.

## Connector catalogue

### Audio and control voltage

Search: analog audio, CV, gate, clock, Eurorack, modular, guitar, instrument, headphone, AUX, jack, TRS, TRRS, RCA, phono, turntable, XLR, balanced, unbalanced.

| Name | Connector and use | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| `br8dicv` | 3.5 mm TS modular CV, gate or clock | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8diaux-TS` | 3.5 mm TS mono; also 2.5 mm | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8diaux` | 3.5 mm TRS stereo AUX, headphones or line | Tip (left) | Sleeve | Ring (right) | Sleeve | — | — | — | — | — |
| `br8diaux-TRSbal` | 3.5 mm TRS balanced mono | Tip (hot) | Ring (cold) | — | — | — | — | — | — | Sleeve* |
| `br8diaux-TRRS` | 3.5 mm TRRS 4-pole; `br8dopatch` lead ‡ | Tip | Ring 1 | — | — | — | — | Ring 2 | Sleeve | — |
| `br8dijack-TS` | ¼-inch / 6.35 mm TS mono guitar or instrument | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8dijack` | ¼-inch TRS stereo headphones or line | Tip (left) | Sleeve | Ring (right) | Sleeve | — | — | — | — | — |
| `br8dijack-TRSbal` | ¼-inch TRS balanced mono | Tip (hot) | Ring (cold) | — | — | — | — | — | — | Sleeve* |
| `br8dirca-mono` | RCA single plug, mono or line | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8dirca` | RCA stereo pair | Left tip (white) | Left sleeve | Right tip (red) | Right sleeve | — | — | — | — | — |
| `br8dirca-AV` | RCA composite A/V trio | Left tip (white) | Left sleeve | Right tip (red) | Right sleeve | Video tip (yellow) | Video sleeve | — | — | — |
| `br8diphono` | RCA phono, turntable | Left tip (white) | Left sleeve | Right tip (red) | Right sleeve | — | — | — | — | Earth lug* |
| `br8dixlr` | XLR-3 balanced microphone or line | Pin 2 (hot) | Pin 3 (cold) | — | — | — | — | — | — | Pin 1* |

- **TRRS / `br8dopatch`:** the sleeve connects first on insertion. With a properly designed provider, a 3-pole plug shorts ring 2 to sleeve and the roughly 0 Ω reading is not a valid power signature. Contacts also sweep past one another during insertion; don't assume an untested port handles that safely. CTIA and OMTP phone headsets swap ring 2 and sleeve. Polarity-blind power handling is intended to tolerate the swap, but a headset is not a br8d lead.
- **¼-inch insert jacks:** TRS inserts may use tip and ring for send and return rather than stereo left and right. Check the equipment pinout and declare a separate profile; don't assume the stereo row applies.
- **RCA:** left is usually white, sometimes black; right is red; yellow is composite video. Putting video on C is proposed, not approved. RCA sleeves are signal returns, not br8d shield contacts.
- **Phono:** a moving-magnet cartridge produces only a few millivolts and expects about 47 kΩ. A 10 kΩ series limiter would load it; a suitable low-resistance gated path is needed at level 2 or above. Mapping the separate turntable earth lug to shield is proposed and still needs approval.
- **XLR and phantom power:** audio XLR uses pin 2 hot, whereas DMX uses pin 3 Data+. Phantom power can place +48 V on pins 2 and 3 relative to pin 1: a near-zero differential reading does not mean there is no hazardous common-mode voltage. The front end needs to handle the fault; an RS-485 transceiver's enable pin does not isolate its bus pins (`GK-10`).

### Speakers

Search: speaker, passive, powered, amplifier, Speakon, NL8, subwoofer, woofer, midrange, tweeter.

| Name | Connector and use | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| `br8dispeaker-pas` | Speakon NL8, passive speaker | Pin 1+ (sub) | Pin 1− (sub) | Pin 2+ (low) | Pin 2− (low) | Pin 3+ (mid) | Pin 3− (mid) | Pin 4+ (high) | Pin 4− (high) | — |
| `br8dispeaker-pow` | Powered speaker, any suitable 4-pair br8d connector; cells are signals, **not connector contacts** | Left+ | Left− | Right+ | Right− | Sub+ | Sub− | Power+ | Power− | — |

- Speaker-level signals can exceed the draft envelope. Even 60 V peak is about 225 W into 8 Ω. A passive speaker has no br8d signature; a signature or a probe with a mute relay would need its own design. Treat this as level 4 or 5 territory, not a beginner DIY port.
- **Open:** the passive NL8 mapping assigns V to the high-frequency speaker pair rather than power. A rule exempting non-power uses of V from power rules is proposed, not approved. NL4 and NL2 slot allocations are also undecided.
- For a powered speaker, select a connector from the catalogue and use its contact map; also specify the audio levels, power and endpoint electronics. The row above does not prescribe an NL8, RJ45 or ribbon contact pinout.

### MIDI and digital audio

Search: MIDI DIN-5, TRS Type A, TRS Type B, S/PDIF, SPDIF, AES3, AES3id, word clock, coax, BNC, optical, TOSLINK, ADAT.

| Name | Connector and use | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| `br8dimidi` | MIDI DIN-5, 180° | Pin 4 (source) | Pin 5 (sink) | — | — | — | — | — | — | Pin 2* |
| `br8dimidi-TRSA` | 3.5 mm TRS MIDI Type A; also 2.5 mm | Ring (source) | Tip (sink) | — | — | — | — | — | — | Sleeve* |
| `br8dimidi-TRSB` | 3.5 mm TRS MIDI Type B | Tip (source) | Ring (sink) | — | — | — | — | — | — | Sleeve* |
| `br8dispdif` | S/PDIF coax, RCA (orange) | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8dispdif-BNC` | S/PDIF coax, 75 Ω BNC | Centre | Shell | — | — | — | — | — | — | — |
| `br8dispdif-TS` | S/PDIF coax, 3.5 mm TS | Tip | Sleeve | — | — | — | — | — | — | — |
| `br8diaes` | AES3 balanced, 110 Ω XLR-3 | Pin 2 (+) | Pin 3 (−) | — | — | — | — | — | — | Pin 1* |
| `br8diaes-BNC` | AES3id unbalanced, 75 Ω BNC | Centre | Shell | — | — | — | — | — | — | — |
| `br8diwclk` | Word clock, 75 Ω BNC | Centre | Shell | — | — | — | — | — | — | — |

- **MIDI** is a roughly 5 mA current loop, one direction per cable. DIN pin 4 is the source and pin 5 the sink. For two-way MIDI use two ports, or explicitly design a profile with one direction on A and the other on B. Type A and Type B swap tip and ring: check the adapter type.
- A legacy MIDI OUT source pin can sit near 5 V behind 220 Ω; a br8d node must treat that as foreign. MIDI shield bonding is conventionally at the OUT end only; a MIDI IN socket does not connect DIN pin 2. The shield contact in the table is not an instruction to bond both ends.
- Coax shells carry the signal return, not a separate br8d shield. TOSLINK and ADAT are optical and have no electrical contact mapping here.

### Lighting

Search: DMX512, DMX XLR-5, DMX XLR-3, RS-485, addressable LED strip, WS2812, APA102, SK9822, JST-SM.

| Name | Connector and use | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| `br8didmx` | DMX512 XLR-5 | Pin 3 (Data 1+) | Pin 2 (Data 1−) | Pin 5 (Data 2+) | Pin 4 (Data 2−) | — | — | — | — | Pin 1* |
| `br8didmx-XLR3` | DMX512 XLR-3 fixtures | Pin 3 (Data+) | Pin 2 (Data−) | — | — | — | — | — | — | Pin 1* |
| `br8diled` | Addressable LEDs, JST-SM or similar | Data (green or white) | Clock (APA102/SK9822 only) | — | — | — | — | 5 V (red) | GND (black or white) | — |

- DMX and audio XLR swap the A+ and A− contact numbers. Don't mistake a DMX fixture for a microphone input; account for phantom-power faults in the interface.
- WS2812 uses data only: leave A− open. APA102 and SK9822 use data and clock. The draft V supply is about 500 mA; long strips need a separately designed power injector. Verify the actual LED connector pinout and colours.

### USB

Search: USB-A, USB-B, mini-B, micro-B, USB-C, USB 2.0, USB 3, SuperSpeed, VBUS, CC, SBU.

| Name | Connector and use | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| `br8doA` | USB-A 2.0 plug or socket | Pin 3 D+ (green) | Pin 2 D− (white) | — | — | — | — | Pin 1 VBUS (red) | Pin 4 GND (black) | Shell* |
| `br8doA-3` | USB-A 3.x | Pin 3 D+ (green) | Pin 2 D− (white) | Pin 9 SSTX+ (orange) | Pin 8 SSTX− (purple) | Pin 6 SSRX+ (yellow) | Pin 5 SSRX− (blue) | Pin 1 VBUS | Pin 4 GND | Shell* |
| `br8doB` | USB-B 2.0, square | Pin 3 D+ | Pin 2 D− | — | — | — | — | Pin 1 VBUS | Pin 4 GND | Shell* |
| `br8doB-3` | USB-B 3.x SuperSpeed | Pin 3 D+ | Pin 2 D− | Pin 9 SSRX+ | Pin 8 SSRX− | Pin 6 SSTX+ | Pin 5 SSTX− | Pin 1 VBUS | Pin 4 GND | Shell* |
| `br8doB-mini` | USB mini-B, 5-pin | Pin 3 D+ | Pin 2 D− | — | — | — | — | Pin 1 VBUS | Pin 5 GND | Shell* |
| `br8doB-micro` | USB micro-B, 5-pin | Pin 3 D+ | Pin 2 D− | — | — | — | — | Pin 1 VBUS | Pin 5 GND | Shell* |
| `br8doC` | USB-C, USB 2.0 cable, 2-pair profile | A6 / B6 D+ | A7 / B7 D− | — | — | — | — | A4, A9, B4, B9 VBUS | A1, A12, B1, B12 GND | Shell* |
| `br8doC-4` | USB-C, full-featured cable, 4-pair profile ‡ | A6 / B6 D+ | A7 / B7 D− | A2 TX1+ ‡ | A3 TX1− ‡ | B11 RX1+ ‡ | B10 RX1− ‡ | A4, A9, B4, B9 VBUS | A1, A12, B1, B12 GND | Shell* |

- A br8d port is **not a USB port**. A real USB host can drive VBUS and data; a br8d node must treat these as foreign. USB-shaped hardware alone does not provide USB behaviour or protection.
- For USB-A/B 3.x, the proposed mapping puts host-to-device on B and device-to-host on C. Pins 8/9 are host SSTX on A and device SSRX on B; pins 5/6 are the opposite. Mini/micro ID and USB 3.x pin 7 GND_DRAIN are unmapped.
- Start with the USB-C 2-pair profile for USB 2.0-only cables. The B/C SuperSpeed mapping is **open**: plug flipping changes the lanes and needs a detection rule. SBU1/SBU2 are unmapped; analog audio adapter mode is not covered.
- A USB-C source normally waits for Rd on CC before driving VBUS. A br8d port does not automatically power a USB-C sink or draw from a USB-C source. The draft br8d provider leaves CC unconnected and does not drive VBUS before recognising a signature; USB behaviour requires a separate design.

### Network

Search: RJ45, CAT5, CAT5e, CAT6, CAT7, Ethernet, LAN, PoE, M12 D-coded, T568A, T568B, crossover.

| Name | Connector and use | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| `br8docat` / `br8dilan` | RJ45 on UTP/STP CAT5/5e/6/7 | Pin 1 | Pin 2 | Pin 3 | Pin 6 | Pin 4 | Pin 5 | Pin 7 | Pin 8 | Shielded shell* |
| `br8dilan-M12` | M12 D-coded 4-pin industrial Ethernet | Pin 1 (white/orange) | Pin 3 (orange) | Pin 2 (white/green) | Pin 4 (green) | — | — | — | — | Housing* |

- Use known straight-through cables and matching endpoints. A br8d RJ45 port is not automatically Ethernet or PoE; don't plug it into a network switch on that assumption.
- The RJ45 mapping aligns with passive PoE mode B (+ on 4/5, − on 7/8). 1000BASE-T uses all four pairs; a link also carrying this br8d power allocation is limited to 10/100 Ethernet, subject to a suitable Ethernet and power design.
- M12 is a connector, not a protection rating. Industrial protection is beyond the maker-level target; the architecture places it at level 5.

#### RJ45 wire colours

Terminate both ends to the **same** standard. T568A at one end and T568B at the other makes a crossover that swaps A and B. These are cable-side colours; verify a cable rather than relying on colour alone.

| RJ45 pin | br8d conductor | T568B | T568A |
|---|---|---|---|
| 1 | A+ | White/orange | White/green |
| 2 | A− | Orange | Green |
| 3 | B+ | White/green | White/orange |
| 6 | B− | Green | Orange |
| 4 | C+ | Blue | Blue |
| 5 | C− | White/blue | White/blue |
| 7 | V+ | White/brown | White/brown |
| 8 | V− | Brown | Brown |

### Serial and buses

Search: FTDI, TTL serial, UART, RS-232, DB9, DE-9, CAN, CAN bus, RS-485, Qwiic, STEMMA QT, I2C, I²C, Grove.

| Name | Connector and use | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| `br8dicom-FTDI` | 6-pin FTDI TTL-232R serial header, host side | Pin 4 TXD (orange) | Pin 5 RXD (yellow) | Pin 6 RTS# (green) | Pin 2 CTS# (brown) | — | — | Pin 3 VCC (red) | Pin 1 GND (black) | — |
| `br8dicom-DB9` | DE-9 / DB9 RS-232, DTE (PC) side | Pin 3 TXD | Pin 2 RXD | Pin 7 RTS | Pin 8 CTS | Pin 4 DTR | Pin 6 DSR | — | Pin 5 GND | Shell* |
| `br8dican` | DE-9 / DB9 CAN, CiA 303-1 | Pin 7 CAN_H | Pin 2 CAN_L | — | — | — | — | Pin 9 CAN_V+ (optional) | Pin 3 CAN_GND | Pin 5 CAN_SHLD* |
| `br8di485` | RS-485 terminal block | D+ (non-inverting) | D− (inverting) | — | — | — | — | — | GND (common) | Shield terminal* |
| `br8diqwic` | Qwiic / STEMMA QT, JST-SH 4-pin, I²C | Pin 3 SDA (blue) | Pin 4 SCL (yellow) | — | — | — | — | Pin 2 3.3 V (red) | Pin 1 GND (black) | — |
| `br8digrove` | Grove 4-pin, I²C or signal | Pin 2 (white) | Pin 1 (yellow) | — | — | — | — | Pin 3 VCC (red) | Pin 4 GND (black) | — |

- **FTDI:** the cable/host drives TXD and RTS#; the module drives RXD and CTS#. VCC can be 5 V or 3.3 V depending on the cable. A provider grants only a voltage the node's signature permits.
- **RS-232:** DTE and DCE swap TXD and RXD; the row is for DTE. Typical levels are about ±5 to ±15 V. DCD (pin 1) and RI (pin 9) are unmapped. V+ is empty and V− is a reference, not a power pair; how the V power rules apply is open.
- **CAN:** CAN_V+ is optional, nominally 7–13 V when supplied by the bus. Check the actual bus and interface.
- **RS-485:** vendor A/B labels are inconsistent. The table uses D+ for the non-inverting line, called B in TIA-485 naming. Check the transceiver and terminal labels. V− is a common reference here, not a power supply.
- **Qwiic / STEMMA QT:** SDA and SCL are single-ended, open-drain I²C signals referenced to V−. Qwiic is **3.3 V only**: never grant 5 V. It needs a 3.3 V power class or deliberate forced mode with a 3.3 V ceiling (`EMIT-5`). A stock Qwiic board is not automatically br8d-protected.
- **Grove:** voltage can be 3.3 V or 5 V; identify the actual device, connector pinout and permitted voltage. For I²C, the lines are single-ended and open-drain, referenced to V−.

### Headers and power

Search: 2x4, 2x5, IDC, ribbon, keyed header, flat cable, DC barrel jack, centre positive, centre negative, power supply.

| Name | Connector and use | 1 A+ | 2 A− | 3 B+ | 4 B− | 5 C+ | 6 C− | 7 V+ | 8 V− | Shield |
|---|---|---|---|---|---|---|---|---|---|---|
| `br8doflat` | 2x4 shrouded IDC header, 2.54 mm ribbon | Pin 1 (red stripe) | Pin 2 | Pin 3 | Pin 4 | Pin 5 | Pin 6 | Pin 7 | Pin 8 | — |
| `br8doflat-10` | 2x5 shrouded IDC header, 2.54 mm ribbon ‡ | Pin 1 (red stripe) | Pin 2 | Pin 3 | Pin 4 | Pin 5 | Pin 6 | Pin 7 | Pin 8 | Pins 9 and 10* ‡ |
| `br8dipwr` | 5.5 × 2.1 mm DC barrel jack, power only | — | — | — | — | — | — | Centre | Sleeve | — |

- **Ribbon:** use a keyed shroud and check continuity. Rotating a 2x4 plug 180° maps pin n to pin 9−n, placing V in the A slot. Reserving 2x5 pins 9 and 10 for shield is an open proposal.
- **Barrel:** centre-positive is common, but centre-negative exists. The proposed polarity-blind V front end is intended to handle either; verify the supply and circuit before connecting. Typical 9–24 V adapters need a suitable wide-input V-W front end at level 2 or above, not a 5 V-only input.

## Cable choices

The catalogue maps contacts. These cable forms are useful starting points too, but each build still needs a pin table and matching electronics.

| Cable or connector | Possible job | What to check |
|---|---|---|
| 4-pole AUX, `br8dopatch` | Two-pair control link | [TRRS row](#audio-and-control-voltage) is provisional; A and V are the available pairs. |
| CAT and RJ45, `br8docat` | Rack-to-desk link | Four pairs; use a known straight-through map, not an assumed Ethernet connection. |
| USB-A, B or C, `br8doA` / `br8doB` / `br8doC` | Short links using available cables | Check the cable actually carries the required conductors. A USB-shaped br8d port is not automatically USB. |
| Qwiic, `br8diqwic` | Small I²C control board via an appropriate interface | 3.3 V only; a stock board is not automatically protected. |
| Keyed 2x4 ribbon, `br8doflat` | Internal rack link | Keying, pin 1 and both ends' mapping. |
| Twin flex, `br8doflex` | One differential signal, signal and return, or a separately designed power-only link | One pair cannot also provide a second independent pair. |
| Shielded multicore, `br8doshield` | Possible low-noise run between stations | Shield, chassis and ground rules remain open; treat as experimental. |
| Unshielded loom, `br8doloom` | Custom internal bundle | Label every conductor and publish a pin table. |
| M12, `br8dilan-M12` / `br8dom12` | Rugged connector or future road case | Use the appropriate pinout; a rugged plug alone does not provide industrial protection. |
| Another connector, `br8dox` | One-off or community profile | Document the contacts, cable, signal limits and protection before use. |

## Bring-up checklist

1. **Write the profile.** List connector, cable, pair assignments, direction, signal ranges, current and whether V carries power. Mark proposals and unresolved exceptions.
2. **Check both ends and the cable.** Physical fit is not electrical compatibility. Use the right bridge for a conventional MIDI, CV, USB, Qwiic or other device; check continuity and orientation.
3. **State the protection level.** Level 0 is not protected. Levels 1 and above are draft design targets, not verified ratings. The architecture targets ±60 V DC between any two conductors or a conductor and ground, and transients up to 100 V for 1 ms; suitable hardware and published testing are still needed. Don't rely on these targets as a guarantee.
4. **Start with local power.** Add cable power only once the provider, node, signature, current limit and V front end are designed for the link. Forced power without a signature must be deliberate and indicated.
5. **Test before connecting valuable gear.** Use current-limited supplies and known pin maps. Follow the [architecture bench-test plan](br8d-architecture.md#7-bench-test-plan) and record failures as well as successes.

### Bench seed: 5 V power and serial

This is a simple **provisional, unverified** provider/node example, not a reference circuit. It uses single-ended 3.3 V serial referenced to V−, so it needs the DC supply and a shared reference. For a self-powered link without V, consider differential signalling instead.

| Conductor | Purpose | Direction / draft value |
|---|---|---|
| V+ / V− | Power | Provider to node; 5 V, up to 500 mA **[PROVISIONAL]** |
| A+ | Serial TX | Provider drives, 3.3 V logic referenced to V− |
| A− | Serial RX | Node drives, 3.3 V logic referenced to V− |

| Connector | V+ / V− | A+ / A− |
|---|---|---|
| RJ45 | 7 / 8 | 1 / 2 |
| USB-A or USB-B | VBUS / GND | D+ / D− |
| 4-pole 3.5 mm TRRS ‡ | Ring 2 / sleeve | Tip / ring 1 |
| Keyed 2x4 IDC | 7 / 8 | 1 / 2 |

Use matching br8d ports and a known straight-through cable, not ordinary USB or Ethernet ports. The earlier seed suggests 115200 baud and up to about 3 m as starting points to verify, not guarantees.

1. The node presents a signature across V **before** its rectifier. The draft candidate is 15 kΩ ±1%, at least 0.25 W, marked sacrificial **[PROVISIONAL]**.
2. The node's V front end suits its protection level. The level 1 example uses a fuse, bidirectional TVS, Schottky bridge and regulator; check bridge and cable voltage drops against the node's input range.
3. Each A conductor needs the appropriate limiter, clamp, sense tap and gate before the UART. The earlier level 1 example suggests about 10 kΩ 1206 series resistors and at least 1 MΩ sense resistors; verify the parts and signal rate on the bench.
4. The provider starts with V off and signal pins high-impedance. It listens first, then probes V only after a quiet reading. The draft probe is 1 V through 15 kΩ, read by an ADC.
5. If the signature is recognised, the provider ramps V to 5 V with an initial 100 mA limit, potentially rising toward 500 mA after startup **[PROVISIONAL]**. On loss of signature or supply, or a sustained current-limit fault, it switches V off and returns to a defensive state.

As rough starting estimates, the architecture gives loop resistance drops of 0.17 V per metre per amp for 24 AWG copper and 0.43 V per metre per amp for 28 AWG. Measure the actual cable and load, and account for bridge drop. For longer runs, consider differential serial such as RS-485 with a properly rated or gated transceiver.

## Not mapped and open decisions

| Connector or case | Why it is not mapped here |
|---|---|
| RJ11 / RJ12 telephone | Ring voltage can be about 90 V RMS, beyond the draft ±60 V envelope. |
| Mains / IEC | Mains voltages are outside the envelope. |
| Speaker banana plugs / binding posts | Speaker power can exceed the envelope; no general mapping is proposed. |
| Speakon NL4 / NL2 | Need a decision on which slots a two-way or one-way speaker uses. |
| TOSLINK / ADAT | Optical, not electrical contacts. |
| HDMI / DisplayPort / Thunderbolt | Out of scope. |
| Eurorack ±12 V bus power header | The rails are not a pair and this bus is out of scope. |
| Combo XLR / TRS jack | Use the mapping appropriate to the plug and its actual signal; one socket does not define one universal map. |

The connection guide's decisions are still open: proposed names and tags; RCA video on C; the phono earth lug on shield; whether a profile may use V for something other than power and how power rules then apply; USB-C SuperSpeed B/C lanes; and 2x5 shield pins. The TRRS pinout and shield bonding also remain provisional. Document your choices rather than treating them as approved.

The goal is to make the next module more fun to add—not to pretend that a cable can replace a circuit, a pinout or a bench test.
