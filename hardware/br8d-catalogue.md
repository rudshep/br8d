# br8d adapter module catalogue

Find your connector: container (heading 1), application (heading 2), family
(heading 3), then the module (heading 4). Module names link to their proposed
folders. The maps are proposals; nothing here is built yet. The size token
(`1u` or `2u`) gives the board width in 1U units.

# Analogue audio and CV

## 3.5 mm sockets: AUX and CV

### [br8d-i-aux](./br8d-i-aux/)

- Maps: TS: A: tip / sleeve. TRS stereo: A: tip (left) / sleeve; B: ring
  (right) / sleeve. TRS balanced: A: tip / ring; sleeve to a separate shield.
  TRRS (provisional): A: tip / ring 1; V: ring 2 / sleeve.
- Use 3.5 mm sockets for these; a 2.5 mm socket needs its own name and footprint.
- TRRS needs a `br8d-o-patch` lead. The sleeve makes contact first, and the
  contacts can short during insertion. A three-pole plug shorts V and is not a
  valid power signature. CTIA and OMTP headsets swap ring 2 and sleeve, so a
  headset is not a br8d lead.

#### [br8d-i-aux-1ts-br0-1u-A](./br8d-i-aux/1ts-br0-1u-A/)

One TS socket on A.

#### [br8d-i-aux-1ts-br0-1u-B](./br8d-i-aux/1ts-br0-1u-B/)

One TS socket on B.

#### [br8d-i-aux-2ts-br0-2u-AB](./br8d-i-aux/2ts-br0-2u-AB/)

Two TS sockets, one per pair.

#### [br8d-i-aux-4ts-br0-2u-ABCV](./br8d-i-aux/4ts-br0-2u-ABCV/)

Four TS sockets, one per pair. V signal use is open.

#### [br8d-i-aux-4ts-io1-2u-ABCV](./br8d-i-aux/4ts-io1-2u-ABCV/)

Shared AUX/CV I/O with level 1 protection. Circuit and power are undecided.

#### [br8d-i-aux-1trs-br0-1u-AB](./br8d-i-aux/1trs-br0-1u-AB/)

One stereo TRS socket: left on A, right on B.

#### [br8d-i-aux-2trs-br0-2u-ABCV](./br8d-i-aux/2trs-br0-2u-ABCV/)

First stereo socket on A/B, second on C/V. Tip is the first pair's +, ring the
second pair's +, and the sleeve joins both returns. V signal use is open.

#### [br8d-i-aux-1TRSbal-br0-1u-A](./br8d-i-aux/1TRSbal-br0-1u-A/)

One balanced TRS socket on A.

#### [br8d-i-aux-4TRSbal-br0-2u-ABCV](./br8d-i-aux/4TRSbal-br0-2u-ABCV/)

Four balanced TRS sockets, one per pair. Sleeves go to a separate shield.

#### [br8d-i-aux-1trrs-br0-1u-AV](./br8d-i-aux/1trrs-br0-1u-AV/)

One TRRS socket, provisional map as above.

### [br8d-i-cv](./br8d-i-cv/)

- Connector: 3.5 mm TS for modular CV, gate and clock.
- The single-socket CV boards are the AUX TS boards in a CV-labelled box, so
  they share a design (see [Design inheritance](#design-inheritance)).
- The TRS and TRRS CV layouts are new, not the AUX map. The four-TRS shield
  needs its own path, and tying the two-TRRS sleeves together would join B- and V-.

#### [br8d-i-cv-1ts-br0-1u-A](./br8d-i-cv/1ts-br0-1u-A/)

One socket on A. Same board as `br8d-i-aux-1ts-br0-1u-A`.

#### [br8d-i-cv-1ts-br0-1u-B](./br8d-i-cv/1ts-br0-1u-B/)

One socket on B. Same board as `br8d-i-aux-1ts-br0-1u-B`.

#### [br8d-i-cv-2ts-br0-2u-AB](./br8d-i-cv/2ts-br0-2u-AB/)

Two sockets, one per pair.

#### [br8d-i-cv-4ts-br0-2u-ABCV](./br8d-i-cv/4ts-br0-2u-ABCV/)

Four sockets, one per pair. V use is open.

#### [br8d-i-cv-4TRSbal-br0-2u-ABCV](./br8d-i-cv/4TRSbal-br0-2u-ABCV/)

Four TRS sockets, tip / ring to + / -, sleeves to a separate shield. Same map as
the balanced AUX board.

#### [br8d-i-cv-2trrs-br0-2u-ABCV](./br8d-i-cv/2trrs-br0-2u-ABCV/)

Two TRRS sockets: A on tip / ring 1 with B on ring 2 / sleeve; C on tip /
ring 1 with V on ring 2 / sleeve (proposed). Whether the sleeves also go to
shield is undecided. Not the AUX TRRS profile, so it needs its own parent.

#### [br8d-i-cv-4ts-io1-2u-ABCV](./br8d-i-cv/4ts-io1-2u-ABCV/)

Active CV I/O with level 1 protection. Circuit, power source and direction are
not designed yet.

## 6.35 mm jack

### [br8d-i-jack](./br8d-i-jack/)

- Maps: TS: A: tip / sleeve. TRS stereo: A: tip (left) / sleeve; B: ring
  (right) / sleeve. TRS balanced: A: tip / ring; sleeve to a separate shield.
- Insert jacks often use tip and ring for send and return. Those need their own
  profile.

#### [br8d-i-jack-1ts-br0-1u-A](./br8d-i-jack/1ts-br0-1u-A/)

One TS socket on A.

#### [br8d-i-jack-1ts-br0-1u-B](./br8d-i-jack/1ts-br0-1u-B/)

One TS socket on B.

#### [br8d-i-jack-1trs-br0-1u-AB](./br8d-i-jack/1trs-br0-1u-AB/)

One stereo TRS socket: left on A, right on B.

#### [br8d-i-jack-2trs-br0-2u-ABCV](./br8d-i-jack/2trs-br0-2u-ABCV/)

Two stereo sockets: first on A/B, second on C/V. Sleeves join their returns.
V use is open.

#### [br8d-i-jack-1TRSbal-br0-1u-A](./br8d-i-jack/1TRSbal-br0-1u-A/)

One balanced TRS socket on A.

#### [br8d-i-jack-1TRSbal-br0-1u-B](./br8d-i-jack/1TRSbal-br0-1u-B/)

One balanced TRS socket on B.

## RCA and turntable

### [br8d-i-rca](./br8d-i-rca/)

- Map: each socket is tip to + and sleeve to -. Left is usually white (sometimes
  black), right red, video yellow.
- The 2- and 4-socket boards serve as mono or stereo; the pairs decide which.

#### [br8d-i-rca-1rca-br0-1u-A](./br8d-i-rca/1rca-br0-1u-A/)

One RCA socket on A.

#### [br8d-i-rca-2rca-br0-2u-AB](./br8d-i-rca/2rca-br0-2u-AB/)

Two RCA sockets, one per pair. Stereo is left on A, right on B.

#### [br8d-i-rca-4rca-br0-2u-ABCV](./br8d-i-rca/4rca-br0-2u-ABCV/)

Four RCA sockets, one per pair. V use is open.

#### [br8d-i-rca-3rcaAV-br0-2u-ABC](./br8d-i-rca/3rcaAV-br0-2u-ABC/)

Composite video trio: left on A, right on B, video on C (proposed). Mixed
RCA and TRS layouts are under [Composite modules](#composite-modules).

### [br8d-i-phono](./br8d-i-phono/)

- Map: left on A, right on B, each tip / sleeve. Earth lug goes to a separate
  shield contact (proposed).
- A moving-magnet cartridge is sensitive to load. Pick the gain and loading stage
  in the endpoint, not on the breakout.

#### [br8d-i-phono-2rca-br0-2u-AB](./br8d-i-phono/2rca-br0-2u-AB/)

Two RCA sockets with a separate earth-lug connection.

## Balanced XLR

### [br8d-i-xlr](./br8d-i-xlr/)

- Map: pin 2 (hot) on + and pin 3 (cold) on -; pin 1 is shield.
- Audio XLR and DMX XLR swap pins 2 and 3 on A, so their boards are not
  interchangeable. Check the device before plugging in.
- Phantom power puts +48 V on both signal pins relative to pin 1. Ribbon mics
  and some older line gear don't like it, so switch phantom off unless the mic
  needs it.

#### [br8d-i-xlr-xlr3f-br0-1u-A](./br8d-i-xlr/xlr3f-br0-1u-A/)

Female XLR-3 on A.

#### [br8d-i-xlr-xlr3f-br0-1u-B](./br8d-i-xlr/xlr3f-br0-1u-B/)

Female XLR-3 on B.

#### [br8d-i-xlr-xlr3m-br0-1u-A](./br8d-i-xlr/xlr3m-br0-1u-A/)

Male XLR-3 on A.

#### [br8d-i-xlr-xlr3m-br0-1u-B](./br8d-i-xlr/xlr3m-br0-1u-B/)

Male XLR-3 on B.

# Speakers

Speaker outputs can carry tens of volts and enough current to damage a
small endpoint. Treat them as level 4-5 work and check the amplifier's output
before connecting anything.

## Passive speakers

### [br8d-i-speaker-pas](./br8d-i-speaker-pas/)

- Map (NL8): A: pin 1+ / 1- (sub); B: pin 2+ / 2- (low); C: pin 3+ / 3- (mid);
  V: pin 4+ / 4- (high).
- V carries a speaker signal, not power. The V power rules do not fit this
  case and need an exemption.
- The NL4, NL2, banana and binding-post boards below are proposals. Each
  connector's pair allocation for two-way and one-way speakers is still open.
- NL4 is likely too wide for 1U; check the datasheet. NL2 is proposed on 1U.

#### [br8d-i-speaker-pas-1nl8-br0-2u-ABCV](./br8d-i-speaker-pas/1nl8-br0-2u-ABCV/)

Speakon NL8, four pairs.

#### [br8d-i-speaker-pas-1nl4-br0-2u-AB](./br8d-i-speaker-pas/1nl4-br0-2u-AB/)

Speakon NL4 (proposed): 1+ / 1- on A, 2+ / 2- on B.

#### [br8d-i-speaker-pas-1nl2-br0-1u-A](./br8d-i-speaker-pas/1nl2-br0-1u-A/)

Speakon NL2 (proposed): 1+ / 1- on A.

#### [br8d-i-speaker-pas-1banana-br0-1u-A](./br8d-i-speaker-pas/1banana-br0-1u-A/)

One pair of 4 mm banana sockets: + on A+, - on A-.

#### [br8d-i-speaker-pas-2banana-br0-2u-AB](./br8d-i-speaker-pas/2banana-br0-2u-AB/)

Two banana pairs, one per speaker: left on A, right on B.

#### [br8d-i-speaker-pas-1bind-br0-1u-A](./br8d-i-speaker-pas/1bind-br0-1u-A/)

One pair of binding posts: red on A+, black on A-.

#### [br8d-i-speaker-pas-2bind-br0-2u-AB](./br8d-i-speaker-pas/2bind-br0-2u-AB/)

Two binding-post pairs, one per speaker.

## Powered speakers

### [br8d-i-speaker-pow](./br8d-i-speaker-pow/)

- Signal profile, not a connector map: A: left+ / left-; B: right+ / right-;
  C: sub+ / sub-; V: power+ / power-.
- No connector is chosen yet, so no variant is proposed. Pick a four-pair
  connector, then define levels, power and the endpoint before drawing a board.

# Digital audio and MIDI

## MIDI

MIDI is a 5 mA current loop, one direction per cable. Type A and Type B TRS
swap tip and ring, so check which one a lead is before connecting it. The
shield is conventionally bonded at the OUT end, so a MIDI IN socket does not
connect DIN pin 2. A legacy MIDI OUT is a foreign source, not a br8d peer.

### [br8d-i-midi](./br8d-i-midi/)

- DIN-5 (180 degrees): A: pin 4 (source) / pin 5 (sink); shield: pin 2.
- TRS Type A: A: ring (source) / tip (sink); shield: sleeve.
- TRS Type B: A: tip (source) / ring (sink); shield: sleeve.
- Each socket is one direction; the endpoint decides which role it plays.

#### [br8d-i-midi-1din5f-br0-1u-A](./br8d-i-midi/1din5f-br0-1u-A/)

One female DIN-5 on A.

#### [br8d-i-midi-1din5f-br0-1u-B](./br8d-i-midi/1din5f-br0-1u-B/)

One female DIN-5 on B; pin 2 to a separate shield.

#### [br8d-i-midi-2din5f-br0-2u-AB](./br8d-i-midi/2din5f-br0-2u-AB/)

Two female DIN-5 sockets, one per pair.

#### [br8d-i-midi-1TRSA-br0-1u-A](./br8d-i-midi/1TRSA-br0-1u-A/)

One 3.5 mm TRS Type A socket on A.

#### [br8d-i-midi-2TRSA-br0-2u-AB](./br8d-i-midi/2TRSA-br0-2u-AB/)

Two TRS Type A sockets, one per pair.

#### [br8d-i-midi-1TRSB-br0-1u-A](./br8d-i-midi/1TRSB-br0-1u-A/)

One 3.5 mm TRS Type B socket on A.

#### [br8d-i-midi-2TRSB-br0-2u-AB](./br8d-i-midi/2TRSB-br0-2u-AB/)

Two TRS Type B sockets, one per pair.

## S/PDIF, AES3 and word clock

Coax and balanced digital audio. Use 75 ohm BNC or the correct XLR for the
rate in use.

### [br8d-i-spdif](./br8d-i-spdif/)

- RCA (usually orange) or 3.5 mm TS: A: tip / sleeve.
- BNC, 75 ohm: A: centre / shell.

#### [br8d-i-spdif-1rca-br0-1u-A](./br8d-i-spdif/1rca-br0-1u-A/)

One RCA socket on A.

#### [br8d-i-spdif-2rca-br0-2u-AB](./br8d-i-spdif/2rca-br0-2u-AB/)

Two RCA sockets, one per pair.

#### [br8d-i-spdif-1ts-br0-1u-A](./br8d-i-spdif/1ts-br0-1u-A/)

One 3.5 mm TS socket on A.

#### [br8d-i-spdif-2ts-br0-2u-AB](./br8d-i-spdif/2ts-br0-2u-AB/)

Two 3.5 mm TS sockets, one per pair.

#### [br8d-i-spdif-1bnc-br0-1u-A](./br8d-i-spdif/1bnc-br0-1u-A/)

One BNC socket on A.

#### [br8d-i-spdif-1bnc-br0-1u-B](./br8d-i-spdif/1bnc-br0-1u-B/)

One BNC socket on B.

### [br8d-i-aes](./br8d-i-aes/)

- AES3 balanced, 110 ohm XLR-3: A: pin 2 (+) / pin 3 (-); shield: pin 1.
- AES3id, 75 ohm BNC: A: centre / shell.

#### [br8d-i-aes-1xlr3f-br0-1u-A](./br8d-i-aes/1xlr3f-br0-1u-A/)

Female XLR-3 on A.

#### [br8d-i-aes-1xlr3f-br0-1u-B](./br8d-i-aes/1xlr3f-br0-1u-B/)

Female XLR-3 on B.

#### [br8d-i-aes-1xlr3m-br0-1u-A](./br8d-i-aes/1xlr3m-br0-1u-A/)

Male XLR-3 on A.

#### [br8d-i-aes-1xlr3m-br0-1u-B](./br8d-i-aes/1xlr3m-br0-1u-B/)

Male XLR-3 on B.

#### [br8d-i-aes-1bnc-br0-1u-A](./br8d-i-aes/1bnc-br0-1u-A/)

One BNC socket on A.

#### [br8d-i-aes-1bnc-br0-1u-B](./br8d-i-aes/1bnc-br0-1u-B/)

One BNC socket on B.

### [br8d-i-wclk](./br8d-i-wclk/)

- Word clock, 75 ohm BNC: A: centre / shell.

#### [br8d-i-wclk-1bnc-br0-1u-A](./br8d-i-wclk/1bnc-br0-1u-A/)

One BNC socket on A.

#### [br8d-i-wclk-1bnc-br0-1u-B](./br8d-i-wclk/1bnc-br0-1u-B/)

One BNC socket on B.

## Optical: TOSLINK and ADAT

Light, not wire, so the board maps the transceiver's electrical side, not the
optical port. ADAT (eight channels at 48 kHz) uses the same TOSLINK transceiver.

### [br8d-i-toslink](./br8d-i-toslink/)

- Transceiver electrical side: A: TX data / GND; B: RX data / GND; V: VCC / GND.
- Check the module's logic and supply voltage (commonly 3.3 V or 5 V) before
  wiring to a node.

#### [br8d-i-toslink-1xcvr-br0-1u-ABV](./br8d-i-toslink/1xcvr-br0-1u-ABV/)

One TOSLINK transceiver module on the header.

# Lighting

## DMX512

### [br8d-i-dmx](./br8d-i-dmx/)

- XLR-5: A: pin 3 (Data 1+) / pin 2 (Data 1-); B: pin 5 (Data 2+) / pin 4
  (Data 2-); shield: pin 1.
- XLR-3 fixtures: A: pin 3 (Data+) / pin 2 (Data-); shield: pin 1.
- DMX swaps pins 2 and 3 on A relative to audio XLR. See
  [XLR](#balanced-xlr).

#### [br8d-i-dmx-1xlr5f-br0-1u-AB](./br8d-i-dmx/1xlr5f-br0-1u-AB/)

Female XLR-5, both data pairs.

#### [br8d-i-dmx-1xlr5m-br0-1u-AB](./br8d-i-dmx/1xlr5m-br0-1u-AB/)

Male XLR-5, both data pairs.

#### [br8d-i-dmx-1xlr3f-br0-1u-A](./br8d-i-dmx/1xlr3f-br0-1u-A/)

Female XLR-3 on A.

#### [br8d-i-dmx-1xlr3m-br0-1u-A](./br8d-i-dmx/1xlr3m-br0-1u-A/)

Male XLR-3 on A.

#### [br8d-i-dmx-1xlr3m-br0-1u-B](./br8d-i-dmx/1xlr3m-br0-1u-B/)

Male XLR-3 on B.

## Addressable LED

### [br8d-i-led](./br8d-i-led/)

- Connector: addressable LEDs, JST-SM or similar. Check the strip's pinout first.
- Map: A: data / clock; V: 5 V / GND. WS2812 uses data only, so A- is left open.
  APA102 and SK9822 use data and clock.
- Long strips need a power injector. The 500 mA V supply target is provisional.

#### [br8d-i-led-1jstsm3-br0-1u-AV](./br8d-i-led/1jstsm3-br0-1u-AV/)

Three contacts: data and power.

#### [br8d-i-led-1jstsm4-br0-1u-AV](./br8d-i-led/1jstsm4-br0-1u-AV/)

Four contacts: data, clock and power.

#### [br8d-i-led-2jstsm4-br0-2u-ABV](./br8d-i-led/2jstsm4-br0-2u-ABV/)

Two four-contact sockets on A and B share V power. Current is shared; neither
output is isolated.

# USB

## USB ports

### [br8d-o-usb](./br8d-o-usb/)

- A br8d port is only USB if the endpoint speaks USB. Real hosts and chargers
  are foreign sources.
- Profile names from the connection guide map to these boards: `br8d-o-A` /
  `-A-3`, `-B` / `-B-3`, `-B-mini`, `-B-micro`, `-C` / `-C-4`.
- USB 2.0 A and B: A: pin 3 D+ / pin 2 D-; V: pin 1 VBUS / pin 4 GND;
  shield: shell.
- USB-C: A: A6/B6 D+ / A7/B7 D-; V: A4, A9, B4, B9 VBUS / A1, A12, B1, B12 GND;
  shield: shell.
- Mini/micro ID and USB 3.x pin 7 GND_DRAIN are not mapped.

#### [br8d-o-usb-1usbA-br0-1u-AV](./br8d-o-usb/1usbA-br0-1u-AV/)

USB-A 2.0 receptacle.

#### [br8d-o-usb-1usbA3-br0-1u-ABCV](./br8d-o-usb/1usbA3-br0-1u-ABCV/)

USB-A 3.x receptacle. B: pin 9 SSTX+ / pin 8 SSTX-; C: pin 6 SSRX+ / pin 5 SSRX-.

#### [br8d-o-usb-1usbB-br0-1u-AV](./br8d-o-usb/1usbB-br0-1u-AV/)

USB-B 2.0 receptacle, square.

#### [br8d-o-usb-1usbB3-br0-1u-ABCV](./br8d-o-usb/1usbB3-br0-1u-ABCV/)

USB-B 3.x receptacle. B: pin 9 SSRX+ / pin 8 SSRX-; C: pin 6 SSTX+ / pin 5 SSTX-.
B is host-to-device and C device-to-host.

#### [br8d-o-usb-1usbBmini-br0-1u-AV](./br8d-o-usb/1usbBmini-br0-1u-AV/)

Mini-B receptacle. V: pin 1 VBUS / pin 5 GND.

#### [br8d-o-usb-1usbBmicro-br0-1u-AV](./br8d-o-usb/1usbBmicro-br0-1u-AV/)

Micro-B receptacle. V: pin 1 VBUS / pin 5 GND.

#### [br8d-o-usb-1usbC-br0-1u-AV](./br8d-o-usb/1usbC-br0-1u-AV/)

USB-C receptacle, USB 2.0 cable, two-pair profile. Do not drive VBUS before
the endpoint has identified itself; leave CC unconnected for now.

#### [br8d-o-usb-1usbC-br0-1u-ABCV](./br8d-o-usb/1usbC-br0-1u-ABCV/)

USB-C receptacle, full-featured cable, four-pair profile. B: A2 TX1+ / A3 TX1-;
C: B11 RX1+ / B10 RX1-.
- Open: the B/C SuperSpeed lanes swap when the plug is flipped. SBU is unmapped.

## Thunderbolt

Thunderbolt 3 and 4 use the USB-C connector. The power and USB 2.0 contacts
follow the [USB-C](#usb-ports) boards above. The high-speed lanes need a
dedicated design, so no header-based Thunderbolt board is proposed.

# Network and buses

## Ethernet

### [br8d-cat-rj45](./br8d-cat-rj45/)

- Profiles: `br8d-o-cat` connector, `br8d-i-lan` interface.
- Map: A: pin 1 / pin 2; B: pin 3 / pin 6; C: pin 4 / pin 5; V: pin 7 / pin 8.
  Shield: socket shell, if present.
- Straight-through cable in T568B at both ends. A crossover swaps A and B.
- Passive PoE mode B uses C and V as joined power rails, so they are not four
  independent pairs.

#### [br8d-cat-rj45-br0-1u-ABCV](./br8d-cat-rj45/br0-1u-ABCV/)

One RJ45 socket.

### [br8d-i-lan](./br8d-i-lan/)

- M12 D-coded, four-pin industrial Ethernet. Map: A: pin 1 / pin 3; B: pin 2 /
  pin 4; shield: housing.

#### [br8d-i-lan-1m12d4-br0-1u-AB](./br8d-i-lan/1m12d4-br0-1u-AB/)

One M12 D-coded four-pin connector. Choose contact gender and mounting for the
panel.

## Telephone

### [br8d-i-rj11](./br8d-i-rj11/)

- Map (6P contacts numbered 1-6 left to right, clip down): line 1 on pins 3
  (tip) and 4 (ring). RJ12 line 2 is on pins 2 and 5.
- Use: handset cords, phone-to-phone test leads, or your own line interface.
- Do not connect a board to a live telephone line. Lines can carry about 48 V DC,
  and ringing reaches about 90 V RMS. Telephone network connections are
  regulated in most countries.

#### [br8d-i-rj11-1rj11-br0-1u-A](./br8d-i-rj11/1rj11-br0-1u-A/)

One RJ11 socket; line 1 on A.

#### [br8d-i-rj11-1rj12-br0-1u-AB](./br8d-i-rj11/1rj12-br0-1u-AB/)

One RJ12 socket; line 1 on A, line 2 on B.

## Serial

### [br8d-i-com](./br8d-i-com/)

- FTDI TTL-232R six-pin, host side: A: pin 4 TXD / pin 5 RXD; B: pin 6 RTS# /
  pin 2 CTS#; V: pin 3 VCC / pin 1 GND.
- DE-9 RS-232, DTE (PC) side: A: pin 3 TXD / pin 2 RXD; B: pin 7 RTS / pin 8 CTS;
  C: pin 4 DTR / pin 6 DSR; V: - / pin 5 GND; shield: shell.
- VCC is 3.3 V or 5 V, depending on the device. RS-232 swings about +/-5 to
  +/-15 V; a TTL or FTDI-level port will not work with it.

#### [br8d-i-com-1ftdi6-br0-1u-ABV](./br8d-i-com/1ftdi6-br0-1u-ABV/)

One six-pin FTDI TTL-232R header, host side.

#### [br8d-i-com-1de9m-br0-1u-ABCV](./br8d-i-com/1de9m-br0-1u-ABCV/)

One male DE-9, DTE-style pins. DTE and DCE swap TXD and RXD. DCD and RI are unmapped.

## Field buses

### [br8d-i-can](./br8d-i-can/)

- DE-9 CAN, CiA 303-1: A: pin 7 CAN_H / pin 2 CAN_L; V: pin 9 CAN_V+ (optional)
  / pin 3 CAN_GND; shield: pin 5 CAN_SHLD.
- CAN_V+ is nominally 7-13 V when supplied.

#### [br8d-i-can-1de9m-br0-1u-AV](./br8d-i-can/1de9m-br0-1u-AV/)

One male DE-9. Specify the optional V+ connection when designing.

### [br8d-i-485](./br8d-i-485/)

- RS-485 terminal block: A: D+ / D- (vendor labels vary, and TIA calls D+ "B");
  V: - / GND (common); shield: shield terminal.
- V- is a common reference for the bus, not a power supply.

#### [br8d-i-485-1term4-br0-2u-AV](./br8d-i-485/1term4-br0-2u-AV/)

Four terminals: D+, D-, common and shield. Four 5.08 mm terminals are about
20 mm wide, so 2U leaves room for the enclosure clearance.

#### [br8d-i-485-2term4-br0-2u-AV](./br8d-i-485/2term4-br0-2u-AV/)

Two terminal blocks on the same bus and shield. Termination and topology are
external.

## Sensor buses

### [br8d-i-qwic](./br8d-i-qwic/)

- Qwiic / STEMMA QT, four-pin JST-SH. Map (provisional): A: pin 3 SDA / pin 4
  SCL; V: pin 2 3.3 V / pin 1 GND.
- Qwiic is **3.3 V only**. Plugging a 5 V device into it can damage the sensor,
  so use a 3.3 V port or a level-shifting bridge.

#### [br8d-i-qwic-1jstsh4-br0-1u-AV](./br8d-i-qwic/1jstsh4-br0-1u-AV/)

One four-pin JST-SH socket.

#### [br8d-i-qwic-2jstsh4-br0-2u-AV](./br8d-i-qwic/2jstsh4-br0-2u-AV/)

Two sockets on the same SDA, SCL, 3.3 V and GND lines. Check combined current
and pull-ups.

### [br8d-i-grove](./br8d-i-grove/)

- Grove four-pin: A: pin 2 / pin 1; V: pin 3 VCC / pin 4 GND. Check the device
  for 3.3 V or 5 V.

#### [br8d-i-grove-1grove4-br0-1u-AV](./br8d-i-grove/1grove4-br0-1u-AV/)

One four-pin Grove socket.

#### [br8d-i-grove-2grove4-br0-2u-AV](./br8d-i-grove/2grove4-br0-2u-AV/)

Two sockets sharing all four lines. Check voltage, current and pull-ups.

# Video and display

## Display sideband

Video itself runs on high-speed differential pairs that a 2x4 header can't
carry. These boards map the low-speed sideband: EDID, hot-plug and power.

### [br8d-i-hdmi](./br8d-i-hdmi/)

- HDMI 19-pin sideband: A: pin 16 DDC SDA / pin 15 DDC SCL; B: pin 19 HPD /
  pin 13 CEC; V: pin 18 +5 V / pin 17 GND.
- Sources supply about 50 mA on +5 V.

#### [br8d-i-hdmi-1hdmi19-br0-1u-ABV](./br8d-i-hdmi/1hdmi19-br0-1u-ABV/)

One HDMI 19-pin socket, sideband only.

### [br8d-i-dp](./br8d-i-dp/)

- DisplayPort 20-pin sideband: A: pin 15 AUX+ / pin 17 AUX- (differential, not
  single-ended); B: pin 18 HPD / pin 13 CONFIG1; V: pin 20 DP_PWR (3.3 V) /
  pin 19 GND.

#### [br8d-i-dp-1dp20-br0-1u-ABV](./br8d-i-dp/1dp20-br0-1u-ABV/)

One DisplayPort 20-pin socket, sideband only.

# Headers and power

## Ribbon headers

### [br8d-o-flat](./br8d-o-flat/)

- 2x4 keyed, shrouded IDC, 2.54 mm ribbon. Map: A: pin 1 / pin 2; B: pin 3 /
  pin 4; C: pin 5 / pin 6; V: pin 7 / pin 8. The red stripe marks pin 1.
- `br8d-o-flat-8` in this catalogue is this profile. A board here is a
  header-to-header breakout or pass-through. Rotation maps pin n to pin 9-n.

#### [br8d-o-flat-1idc8-br0-1u-ABCV](./br8d-o-flat/1idc8-br0-1u-ABCV/)

2x4 header-to-header pass-through.

#### [br8d-o-flat-2idc8-br0-2u-ABCV](./br8d-o-flat/2idc8-br0-2u-ABCV/)

Two parallel headers. All conductors are shared, not isolated.

#### [br8d-o-flat-1idc10-br0-1u-ABCV](./br8d-o-flat/1idc10-br0-1u-ABCV/)

2x5 to 2x4 pass-through. Pins 9 and 10 are proposed for shield, and the shield
path is still open.

## Eurorack power

### [br8d-i-eurorack](./br8d-i-eurorack/)

- Eurorack 10-pin IDC (Doepfer style): pin 1 -12 V (red stripe), pin 2 GND,
  pin 9 GND, pin 10 +12 V. The 16-pin variant with +5 V is not mapped.
- Map: A: -12 V / GND; V: +12 V / GND. The two GND returns join at the board.
- The ribbon can be fitted backwards on a loose header. Check the stripe before
  you power up, because a reversed rail can damage modules.

#### [br8d-i-eurorack-1idc10-br0-1u-AV](./br8d-i-eurorack/1idc10-br0-1u-AV/)

One 10-pin IDC for the Eurorack bus, two pairs on the header.

## DC power

### [br8d-i-pwr](./br8d-i-pwr/)

- 5.5 x 2.1 mm DC barrel, power only. Map: V: centre / sleeve.
- Centre-positive is common, but centre-negative exists. Check the supply's
  polarity and voltage.

#### [br8d-i-pwr-1dc5521-br0-1u-V](./br8d-i-pwr/1dc5521-br0-1u-V/)

One 5.5 x 2.1 mm barrel jack.

# Composite modules

A composite board combines more than one connector. Count independent pairs,
not sockets. A shared pair ties its contacts together, so it is not another
channel. One 2x4 header has only four pairs.

## RCA with TRS

### [br8d-i-rca](./br8d-i-rca/) (composite)

#### [br8d-i-rca-3rcaAV1trs-mirror-br0-2u-ABC](./br8d-i-rca/3rcaAV1trs-mirror-br0-2u-ABC/)

Left on A, right on B, video on C. TRS tip to A+, ring to B+, sleeve to A- and
B-. The TRS repeats the RCA stereo pair; it does not add channels.

#### [br8d-i-rca-3rcaAV1trs-mono-br0-2u-ABCV](./br8d-i-rca/3rcaAV1trs-mono-br0-2u-ABCV/)

Left on A, right on B, video on C. TRS tip / sleeve on V+ / V-, ring unused.
This keeps all three RCA signals but gives up the power pair.

`mirror` and `mono` distinguish layouts that the pair suffix alone can't describe.

# Not mapped

## Mains and IEC

Mains never goes on a br8d header. Use a ready-made IEC inlet with a
certified power module, and keep low-voltage DC on the [DC power](#dc-power)
board.

## Combo XLR/TRS sockets

A combo socket's contacts depend on which plug is inserted, so there is no
single map. Use the XLR-3 board for balanced XLR or the TRS board for jack.

## Cable forms

Twin flex (`br8d-o-flex`), shielded multicore (`br8d-o-shield`), unshielded
loom (`br8d-o-loom`), M12 (`br8d-o-m12`) and community connectors (`br8d-o-x`)
are cable or connector families, not defined maps. Twin flex carries one pair:
A for a signal, or V for power.

# Reference

## Reading the maps

Each module name is a proposed folder, relative to this file. Unless noted, the
board connects a keyed, shrouded 2x4 2.54 mm header to the named connector.
Folders sit under their family folder, with the name after the family prefix.

```text
Header pin:  1   2   3   4   5   6   7   8
Conductor:  A+  A-  B+  B-  C+  C-  V+  V-
```

`A: X / Y` means A+ connects to X and A- to Y; B, C and V follow the same
pattern. `-` means no external contact. Pin numbers are the connector's own
contacts. Check the footprint and cable with a continuity test before use.

A repeated contact joins its header conductors, so stereo sleeves share a return.
A **shield** listed in an entry is a separate contact, not a ninth signal pin.
Coax shells used as the signal return are not separate shields.

Names beginning `br8d-i-` are interfaces; `br8d-o-` names are connector or cable
forms. The safety and electrical limits are in the
[architecture](../br8d-architecture.md).

## Candidate variants

The entries are starting points, not a full list. A suffix such as `-2u-ABCV`
gives the size and occupied pairs, not a complete pin map. Unless noted, a
variant follows its family's map.

The boards target a 1U x 1U PCB (20.38 x 23.58 mm), connector at the north
edge. Large connectors such as XLR and Speakon may need a bigger board or a
panel mount. Check the part, header, clearances and enclosure before designing.

## Design inheritance

Inheritance is a **design relationship**: a child is a different application
name for a shared design, or a later specialization. A shared breakout must
have the same connector, pin map and populated circuitry. A label or box can
change the application name without changing the board.

- **AUX TS -> CV TS:** `br8d-i-cv` TS boards share the `br8d-i-aux` TS design at
  levels 0-2, and CV specialization starts at level 3. For example,
  `br8d-i-cv-1ts-br0-1u-A` uses the same board as `br8d-i-aux-1ts-br0-1u-A`.
- **AUX TRS -> CV TRS:** the four-TRS CV board has the same map as the balanced
  AUX board, so it can share it. The two-TRRS CV layout needs its own parent.
- **Other overlaps:** mono RCA -> S/PDIF RCA; AUX TS -> S/PDIF TS; balanced XLR
  -> AES XLR. S/PDIF and AES need the right impedance and bandwidth, so check the
  part before reusing a board.
- **Pin 2/3 swap:** audio XLR and DMX XLR swap pins 2 and 3 on A, so their
  boards are not interchangeable.

Share a file only when the physical design matches, including the shield path.

## Composite design notes

Three RCA sockets (left, right, video) use A, B and C. An independent stereo TRS
socket needs two more pairs, five in total. Alternatives:

- Mirror: TRS repeats RCA stereo. Do not connect independent outputs to both.
- Mono: keeps the three RCA signals but gives up TRS stereo and the power pair.
- Drop video to free C for a stereo TRS pair on C/V.
- Use two br8d headers for five pairs. This needs a multi-header naming and pin
  convention, because the `-ABCV` suffix can't say which header owns a pair.
