# br8d Architecture, draft 0.1

**Scope of this version:** physical layer, port front end (the "gatekeeper"), protection levels and connector profiles. Probing, discovery, signature classes, protocol negotiation and power negotiation will extend this document. The fictional maker example lives in the [maker guide](br8d-maker-guide.md).

**Conventions.** MUST, SHOULD and MAY follow RFC 2119. Rules carry IDs (for example `EMIT-1`) so that guides and issues can cite them. Two tags mark soft spots:

- **[PROVISIONAL]** means a value was chosen to let design proceed. It needs bench verification.
- **[OPEN]** means undecided. The text gives a working assumption.

All identifiers are lowercase (`br8d`, `br8docat`, `br8dilan`). Prose may capitalise at the start of a sentence.

---

## 0. How to use this document (for makers and for agents)

This document defines the electrical rules and connector profiles. For a fictional example of those ideas in a modular synth project, see the [maker guide](br8d-maker-guide.md). When writing a guide for a specific project:

1. Ask for or infer: **connector and cable**, **protection level (0 to 5)**, **role** (provider, node or both), **pairs needed** and **voltage and current budget**. Levels 4 and 5 require certified or industrial designs beyond this maker-focused draft.
2. Take rules only from Part 2 and pin maps only from Part 6.
3. Never invent component values or thresholds that are not in this document. Where a value is tagged **[PROVISIONAL]** or **[OPEN]**, tell the maker and recommend a bench check (Part 7).
4. Always include the maker's level in the guide's first lines, and state what that level does *not* protect against.
5. Warn that survival ratings depend on the actual parts chosen. Part numbers in this document are classes of part, not endorsements.

---

## 1. Concepts

### 1.1 What br8d is

br8d is a cabling convention for connecting two nodes with 1 to 4 pairs of wires, over any suitable connector and cable. It specifies:

- a **pair structure** (A, B, C, V),
- an **electrical envelope** (what a port must survive, and what it may emit),
- a **gatekeeper** (a front end that defends the port and later releases it to the circuit),
- **protection levels**, and
- **connector profiles**.

It does not specify the protocol. Two ends that share a br8d port negotiate protocol between themselves (this is later work). A br8d port also doesn't promise that the other end speaks br8d. Any foreign device, such as a PoE switch, a headphone amp or a USB charger, may be on the other end of the cable, and the port must be safe against all of them.

### 1.2 Pairs

| Pair | Name | Slot # | Conductors | Convention |
|---|---|---|---|---|
| 1 | **A** | 1 | A+, A- | Data. Differential, or TX/RX for single-ended serial. Low power |
| 2 | **B** | 2 | B+, B- | Data. Differential, or TX/RX for single-ended serial. Low power |
| 3 | **C** | 3 | C+, C- | Control channel. Can be the second rail for high-voltage power when joined with V |
| 4 | **V** | 4 | V+, V- | Power. 5V by default, polarity-blind, 60V-tolerant |

A **slot** is a position on the connector. A pair is identified by its slot, not by what is in it. A cable may put the wrong thing in a slot (a V pair in the A slot, say). The gatekeeper must stay safe when that happens.

### 1.3 Naming grammar

| Vowel | Meaning | Examples |
|---|---|---|
| `o` | **over**: cable or connector | `br8docat`, `br8doflex`, `br8doflat`, `br8dopatch`, `br8doaux`, `br8dobac`, `br8doqwiic` |
| `i` | **interface**: what runs on the wires | `br8dilan`, `br8dispi`, `br8ditwi`, `br8di485`, `br8didmx`, `br8dicom`, `br8diled` |
| `e` | **equipment**: things that speak br8d | `br8degate`, `br8dehub`, `br8deswitch`, `br8debox`, `br8dematrix` |

### 1.4 The port model

Every conductor, on any connector and at any level, passes through four stages:

```
 connector ──► [1 LIMIT] ──► [2 CLAMP] ──► [3 SENSE] ──► [4 GATE] ──► consumer
                series        shunt         high-Z tap     normally-     MCU pin,
                R / fuse      TVS/diode     (never drives) open switch   line driver,
                                                                         PHY, etc.
```

1. **LIMIT** caps current and energy (resistor, fuse, PTC).
2. **CLAMP** caps voltage (TVS, diodes).
3. **SENSE** is a high-impedance tap (through very large resistors, as in a multimeter) that always watches and never drives.
4. **GATE** connects the wire to a consumer. A consumer is whatever sits behind the gate: an MCU pin, a level converter, a line driver, an Ethernet PHY and so on. The gate is open by default, with no firmware involved.

The gatekeeper does not carry the signal. It decides whether the signal may reach the consumer. The gate is a switch for a *known, expected* signal going to a *known* sink.

### 1.5 Modes and states

**Pair modes** (what the port is doing with a pair):

| Mode | Meaning |
|---|---|
| **DEFEND** | The default. Gate open. Only LIMIT, CLAMP and SENSE are live |
| **PROBE** | A probe source (harmless Thevenin source, see `EMIT-1`) touches the pair so the MCU can measure impedance |
| **ARMED** | The MCU has presented the pair to its consumer ("patch it through") |

**Pair status** (the gatekeeper reports it, passively, per pair):

| Status | Meaning |
|---|---|
| **UNSAFE** | Voltage exceeds the pair's trip reference. The pair is tripped and latched |
| **PROBABLY SAFE** | Within the pair's trip reference and no hazard seen. This does *not* mean the pair is quiet |
| **SAFE TO PROBE** | PROBABLY SAFE and also quiet (differential and common-mode both at or below about 1V), so a probe source may touch it |

The statuses are nested: SAFE TO PROBE implies PROBABLY SAFE, and UNSAFE excludes both.

**MCU judgements** (the node decides, because they depend on the module):

| Judgement | Meaning |
|---|---|
| **SAFE** | "This pair carries what this module expects, and my consumer can take it." SAFE means different things to different modules |
| **ARM** | The MCU's command to close the gate, in effect "patch it through" |

**Who decides what:**

| Decision | Decided by | Speed |
|---|---|---|
| UNSAFE | Gatekeeper hardware (passive) | Microseconds |
| PROBABLY SAFE, SAFE TO PROBE | Gatekeeper hardware (passive) | Fast |
| SAFE | MCU firmware | Slow, with dwell time |
| ARM | MCU firmware, subject to hardware veto | Slow |

**Lease.** The permission to stay ARMED expires unless the MCU keeps renewing it. The renewal is called a *lease* (see `MCU-4`).

### 1.6 Fast to NOT SAFE, slow to SAFE

The two decisions are different jobs with opposite requirements. A false trip is an annoyance, so trip on a single bad reading, in hardware, as fast as possible. A false permit can destroy a board, so permit only after sustained good readings, in firmware, with a hardware veto.

The design constraint is a **race budget**. The trip path (comparator, gate driver, FET turn-off) must finish before the LIMIT and CLAMP stages run out of ride-through (the time before the series resistor or TVS reaches its energy limit). That budget is microseconds for a FET gate and milliseconds for a fusible 1206 resistor. It is easy to meet and must be checked in every design.

---

## 2. Normative rules

### 2.1 Structure (`STR`)

- **STR-1.** A br8d port consists of up to 8 conductors arranged as up to 4 pairs, named A (1), B (2), C (3) and V (4). Each pair has a + and a - conductor.
- **STR-2.** A port MUST implement at least 1 pair and MAY implement up to 4. The profile of a port is the set of pairs it implements.
- **STR-3.** In a 1-pair profile the pair is either A (data, node self-powered) or V (power only). A 1-pair A profile has no common reference conductor, so single-ended signalling is not possible. Use differential signalling.
- **STR-4.** Any pair MAY carry any signal. The conventions in Section 1.2 are defaults, not requirements.
- **STR-5.** Pairs MUST be terminated in their own slots. A designer MUST NOT assume that other equipment does the same. Termination errors are handled by the electrical rules (`ENV-1`, `GK-3`).
- **STR-6.** The V pair is polarity-blind at level 1 and above (rectified, see `PWR-2`). At level 0 it MAY be polarity-sensitive, but the project MUST say so. For A, B and C, the designer MUST declare whether polarity is fixed or detected.

### 2.2 Electrical envelope (`ENV`)

- **ENV-1. Survive.** At level 1 and above, a port MUST survive **±60V DC between any two of its conductors, in either polarity** (this includes pair-to-pair and conductor-to-ground), indefinitely or until a designated sacrificial part opens, and must additionally tolerate **transients up to 100V** (hot-plug ringing) for at least 1ms. "Survive" means that, after the event, only parts marked as sacrificial (`SAC-1`) may need replacing, and there is no fire, smoke or lasting damage to anything else.
- **ENV-2.** At level 0, a port SHOULD survive the ENV-1 event briefly, or with minimal user-fixable damage. A level 0 port MUST NOT be described as "protected".
- **ENV-3.** An endpoint MUST expect any voltage from -60V to +60V to appear on any pair at any time, and MUST NOT assume a pair's content or its polarity.
- **ENV-4.** Voltage ratings for AC are peak values. 60V peak is about 42V RMS.
- **ENV-5.** V- is *not* ground when the supply is AC. A node MUST NOT assume that V- is ground unless the supply is DC. Single-ended signals referenced to V- are valid only on DC supplies.
- **ENV-6. Parts rating.** A node that accepts more than 24V on V SHOULD use parts rated for 100V, because hot-plug ringing can reach nearly twice the supply voltage.
- **ENV-7. Shield and ground.** [OPEN] Shield, chassis and ground bonding rules for shielded cable (`br8doshield`) are undecided. The working assumption is that the shield is a separate, unswitched conductor that is never used for signalling and is bonded at most at one end.

### 2.3 Emit rule (`EMIT`)

- **EMIT-1. Probe limit.** Until the port has recognised its peer, it MUST NOT drive any conductor beyond **probe level**: open-circuit voltage ≤ 1V, short-circuit current ≤ 1mA, with slew-limited edges. The probe SHOULD touch one pair at a time.
- **EMIT-2. Hi-Z by default.** While unpowered, in reset or in brownout, and until the first stage of probing, every br8d conductor MUST be high-impedance behind its limiter. At level 0 this means firmware leaves the pins as inputs until it has finished listening.
- **EMIT-3. No back-drive.** A port MUST NOT drive a conductor that its sense stage reads above probe level. It MUST NOT drive a rail it detects on the other side.
- **EMIT-4. Grant rule.** A port MAY raise a voltage only after recognising the peer, only to a level the peer's signature says it can survive, and never above its own limit.
- **EMIT-5. Forced mode.** A provider MAY power or talk to a signatureless peer only in an explicit, per-port **forced mode** with a hard ceiling (3.3V, and no V rail unless the maker has chosen it), set deliberately (config or jumper) and indicated (for example an LED). Forced mode MUST NOT be negotiated automatically.
- **EMIT-6.** A signatureless ("dumb") node MUST be self-powered, or be powered from a forced-mode port. Two dumb nodes connected to each other are outside the spec.

### 2.4 Gatekeeper (`GK`)

- **GK-1. Four stages.** Every conductor MUST pass through LIMIT, CLAMP, SENSE and GATE as in Section 1.4. At level 0 the LIMIT is a series resistor, the CLAMP is the MCU's own pin diodes plus optional ESD diodes, the SENSE is the MCU's ADC or input pin and the GATE is firmware (high-impedance pin). Levels 1 and above MUST implement all four stages in hardware.
- **GK-2. Defend by default.** At level 1 and above, GATE MUST default to open, with no firmware, when unpowered, in reset and in brownout.
- **GK-3. Per-pair status.** At level 1 and above, the gatekeeper MUST provide a status per pair (`PAIR_SAFE_n`: PROBABLY SAFE or not) and SHOULD provide SAFE TO PROBE. Status is derived passively and MUST NOT depend on firmware.
- **GK-4. Band code.** At level 2 and above the gatekeeper MUST report to the MCU, per pair, the voltage band (Section 3), a polarity or AC flag and a common-mode band. At level 1 the bands are fixed in hardware.
- **GK-5. Per-pair trip.** Tripping is **per pair**, not global. A pair trips when its voltage (differential or common-mode) exceeds its **trip reference T_n**. A tripped pair is latched UNSAFE, its gate is forced open **passively**, and it overrides ARM for that pair. Only the MCU can clear the latch, and only after the pair has read at or below T_n for the quiet dwell (`MCU-5`).
- **GK-6. Reference.** T_n is set by the MCU (level 2 and above) or by hardware strap (level 1). After reset or lease expiry T_n MUST return to its default, **band B2** (about 5.5V). T_n MUST NOT be settable above the pair's **ceiling C_n**, which is a hardware limit chosen by the designer to match what is behind that pair's gate, and which the MCU cannot override.
- **GK-7. ALL_SAFE.** The gatekeeper MUST provide `ALL_SAFE`, a passive AND of every implemented pair's `PAIR_SAFE_n`. It is for power grants and other things that need a clean port. It MUST NOT open or close the gate of any other pair. A pair that is not tripped keeps working while another pair is tripped. A profile MAY define a **strict** option in which an unsafe V pair also blocks the signal pairs. The default is not strict.
- **GK-8. Sense.** Sense circuits SHOULD measure each pair differentially, plus common-mode, because floating peers (transformer-isolated Ethernet, PoE) have no relationship to local ground. Level 0 and 1 MAY measure each wire against local ground, with the stated limitation that floating peers may read ambiguously.
- **GK-9. Asymmetry.** Trip MUST be fast (comparator and hardware). Permit MUST be slow: a pair becomes SAFE or ARMED only after a quiet dwell and hysteresis (`MCU-5`).
- **GK-10. Consumers.** A consumer sees the wire only through an open-by-default gate, or sits behind a limiter that survives the fault. A chip with exposed pins (RS-485, RS-232, level translators, MCU pins) MUST be either rated for the full ±60V, or sit behind a gate. Enabling a transceiver (for example RS-485 DE/RE) does not isolate its bus pins.

### 2.5 MCU responsibilities (`MCU`)

- **MCU-1.** The MCU decides SAFE and decides ARM. A node MUST NOT ARM a pair unless the pair's gatekeeper status is PROBABLY SAFE or better, the MCU has judged it SAFE for the intended use and the lease is valid.
- **MCU-2. Veto only.** The MCU can only add vetoes to the gatekeeper. It MUST NOT be able to force a gate open against the gatekeeper's status.
- **MCU-3. Gate logic.** For each pair, `GATE_n = PAIR_SAFE_n AND ARM_n AND LEASE_OK`, implemented in hardware at level 1 and above.
- **MCU-4. Lease.** The MCU renews the lease by **toggling** a pin (a heartbeat edge) into a charge pump or retriggerable monostable. It does not hold a level, so a stuck-high pin cannot hold the gates open. If the lease expires, all gates open, T_n returns to default and the port returns to DEFEND. Timeout: 50 to 250ms **[PROVISIONAL]**.
- **MCU-5. Re-arm.** After an unplug, bounce, a change in sensed impedance or voltage, or a trip, the pair returns to DEFEND. Before probing or arming again it needs a **quiet dwell** of 50 to 200ms **[PROVISIONAL]**, and after a trip a hold-off of at least 1s **[PROVISIONAL]**.
- **MCU-6. Listen first.** The MCU MUST read the sense stage of every pair before any probe. If any pair is outside its allowed band, that pair stays in DEFEND.
- **MCU-7. Never trust the peer.** The MCU MUST NOT assume the peer is protected, is br8d or behaves.
- **MCU-8. Wrong slot.** If a pair reads a steady voltage in a slot where none is expected (for example 5V on A because a V pair was terminated in the A slot), the gatekeeper reports PROBABLY SAFE but not SAFE TO PROBE, and the MCU reads the band and decides. Which of these cases a node supports (for example "5V on A" as a power input) is up to the node.

### 2.6 Power (`PWR`)

- **PWR-1.** The V pair is the power pair. A node MAY include a permanent 60V-tolerant front end on V.
- **PWR-2. Rectified V.** A V front end at level 1 and above MUST be polarity-blind (a full-wave bridge or ideal-diode bridge), and so also AC-tolerant (up to 60V peak). Operating *from* AC is optional **[OPEN]**: a node MUST state whether it supports it, and MUST NOT depend on a stable ground when it does.
- **PWR-3. Signature.** A node that takes power from V MUST present a passive **signature** (a resistor across the V pair, *before* the rectifier). The signature is what lets a provider recognise the node. A node SHOULD pick a value outside these bands: about 1kΩ (USB-C Ra), about 5.1kΩ (USB-C Rd) and 19 to 26.5kΩ (PoE detection). The candidate value is **15kΩ ±1%** **[PROVISIONAL]**. A 10kΩ signature is avoided because it is a common audio input impedance.
- **PWR-4. Provider rule.** A provider MUST NOT energise V until it has recognised a signature (`EMIT-1`, `EMIT-4`), except in forced mode (`EMIT-5`). The first grant on V is 5V with a current limit, ramped, and monitored. Initial limit: 100mA, then up to 500mA **[PROVISIONAL]** after the node has come up.
- **PWR-5. Sag and loss.** If V sags, current limit is hit for longer than a debounce, or the signature disappears, the provider switches V off and returns to DEFEND.
- **PWR-6. UVLO.** A node's regulator MUST NOT start below about 7V on the rectified rail **if it also accepts high-voltage input**. A 5V-only node MUST run from about 4V. Below its start threshold, the front end MUST draw only microamps so it stays invisible to a 1V probe.
- **PWR-7. High-voltage mode.** C and V MAY be joined to carry up to 48V (passive PoE). In joined mode the two conductors of C are tied together as one rail, and the two conductors of V as the other. A node using joined mode MUST declare it, and while C is carrying power it cannot carry control unless the node implements true PoE (isolation or bias-tee).
- **PWR-8. Wake rail.** A node MAY harvest a small current from any pair that reads steady DC above about 4.5V, to wake its MCU. The wake current MUST be small and limited: **2mA [OPEN]** (10mA was proposed, but it would load foreign sources, and limiting 10mA at 60V needs an active limiter). Real power comes only from V (or joined C+V).
- **PWR-9. Foreign PoE.** 802.3af/at/bt switches will not power a node that has no 25kΩ PD signature. That is intended, and the node stays dead and safe. A node MAY implement a PD signature at level 2 and above.
- **PWR-10. Tie-break.** [OPEN] When two providers meet (hub to hub), they need a tie-break so that both don't try to drive V. Two providers that each probe a port first, and see no signature, do not drive V.

### 2.7 Levels (`LVL`)

- **LVL-1.** A product MUST state its protection level (0 to 5) for each port, and what that level does not protect against.
- **LVL-2.** Port capability is the minimum of both ends. Negotiation never goes above the weaker end.
- **LVL-3.** A level 3 node MUST also carry the passive signature, so that it can get power from a level 1 or 2 provider before it starts active signalling.
- **LVL-4.** Levels 1 and above MUST meet `ENV-1`. See Section 5 for the per-level requirements.

### 2.8 Sacrificial parts (`SAC`)

- **SAC-1.** Parts that are expected to fail in a fault (series resistors, fuses, TVS, buffer chips) MUST be marked on the board as sacrificial (silkscreen mark, for example `S`) and documented.
- **SAC-2.** Sacrificial parts MUST be replaceable by hand: through-hole, socketed, or SMD no smaller than 0805 (resistors) and SOIC (ICs).
- **SAC-3.** Sacrificial parts SHOULD fail open. Resistors usually do. ICs can fail short or leaky, so a sacrificial IC MUST sit **behind** a limiter and **outside** the gate, so that a failed one can't pass the fault on.
- **SAC-4.** A node SHOULD detect a failed limiter automatically at power-up (for example, by measuring continuity through each series resistor with the probe) and indicate it (fault LED).

---

## 3. Voltage bands

Bands are a **thermometer code** on |V|. Each band's edge is a nominal rail plus margin. A lower band is always included in a higher one.

| Band | |V| edge | Typical use |
|---|---|---|
| B0 | ≤ 1.0V | **Quiet**. Probe allowed |
| B1 | ≤ 3.6V | 3.3V logic |
| B2 | ≤ 5.5V | 5V logic and rail. **Default trip reference** |
| B3 | ≤ 13.5V | 12V |
| B4 | ≤ 26V | 24V |
| B5 | ≤ 40V | 36V |
| B6 | ≤ 57V | 48V class (PoE up to 57V) |
| B7 | > 57V | Hazard up to the 60V survive limit. Always UNSAFE |

Each pair also reports:

- a **polarity/AC flag** (sign of the reading, or whether it changes sign),
- a **common-mode band** (pair average against local ground), and
- (level 2 and up) a **floating** flag when the common-mode is not meaningful.

Bands that a live peer commonly produces (Ethernet idle pulses to about 2.5V, USB data at 3.3V, line audio to about ±12V) must not be treated as faults by default. A peer in B1 to B3 on a signal pair is a *foreign, live* peer: stay passive and observe.

---

## 4. The V pair and power

```
 V+ ─[fuse/PTC]─┬──┬────────┐
                │  │      ┌─┴─ bridge (4 Schottkys or ideal-diode bridge controller)
              [TVS]│      └─┬─
                │  │        │
 V- ─[fuse/PTC]─┴──┴────────┼── Vrect ──[UVLO]── regulator ──► rails
                   [signature R]
```

- The **signature resistor** sits across V+ and V- *before* the bridge. The bridge makes the node invisible to a 1V probe (the diodes don't conduct), so the resistor is the only thing the probe sees. It also gives leakage somewhere to go, which stops back-powering.
- The fuse (or PTC) is upstream of the TVS, so a sustained overvoltage opens the fuse rather than cooking the TVS.
- A TVS alone cannot survive sustained passive PoE. It needs a series element that opens.
- In the AC case, bulk capacitance must ride through the gaps between peaks. That can mean hundreds of µF at 100V. Document it as an option, not the default.
- **Local ground** is the lower of the two V wires after the bridge. Single-ended signals that are referenced to it keep working when a cable swaps V+ and V-, provided the supply is DC.
- The bridge costs 0.5 to 1V of drop. Account for this and cable resistance in the power budget.

**Two V front-end styles:**

| Style | Level | Idea |
|---|---|---|
| **V-5** (5V only) | 1 | Fuse, bidirectional TVS with standoff just above 5V, Schottky bridge, 5V-class regulator. Any source above about 7V clamps and opens the fuse. Fuse and TVS are sacrificial |
| **V-W** (wide input) | 2 and up | 100V-class parts, an OVP/eFuse controller, and a 60 to 100V input buck. Accepts 5V up to 48V |

Check TVS leakage near standoff voltage. It can disturb a high-impedance signature.

---

## 5. Protection levels

All levels 1 and above MUST survive ±60V (`ENV-1`).

| Level | Name | Hardware | Signature | Notes |
|---|---|---|---|---|
| **0** | Hobbyist | Wires to MCU pins. Series R per conductor (1 to 2.2kΩ minimum, or 10kΩ 1206 as sacrificial). Optional ESD array. Pins hi-Z until probe. V needs polarity protection (series Schottky or P-FET) | A node that takes power from V MUST still carry the signature (a single resistor) | Survives mistakes to about 5V. At 60V a 10k/1206 limiter passes about 6mA and about 0.36W, so it opens before the MCU is hurt. Slow signals only. Published projects MUST say "level 0" |
| **1** | Passive gatekeeper | Per-pair threshold sense (TL431-class reference, or comparator, fed through ≥ 1MΩ resistors), series limiter, clamp, gate (back-to-back N-FETs or a fault-protected switch), `PAIR_SAFE_n`, wired-AND `ALL_SAFE`. V front end V-5 | Passive (one resistor) | Fixed thresholds set by strap or resistor. ARM from an MCU pin or jumper. Slow signals through high-value limiters |
| **2** | Intelligent defence | Level 1 plus MCU-programmable T_n within the hardware ceiling C_n, per-pair differential and common-mode sensing, band code, latched trip, lease, wake rail, state machine. V front end V-W | Passive | "High-level, reasonable cost". Fast signals pass through the gate once ARMED |
| **3** | Active discovery | Level 2 plus signalling on C, a full measurement sweep (current source and mux across wires), floating or differential sensing. Dedicated chips are likely | Passive for cold start, then active | Negotiates protocol and power |
| **4** | Consumer | Certified. ESD, 60V survive, PD/PoE-like negotiation | Active | Up to 48V |
| **5** | Industrial | Isolation, surge protection, wide temperature range | Active | Per profile |

Reference circuits for levels 0 to 3 will be published per level, as a single-conductor stage, a signal pair and a V pair, each with a BOM and a bench-test result. **They don't exist yet.** The [maker guide](br8d-maker-guide.md) is an illustrative story, not a reference circuit.

---

## 6. Connector profiles

All profiles assume a **straight-through** cable (pin n to pin n). Crossover cables swap pairs.

### 6.1 RJ45 (`br8docat`)

| Pair | + | - | Notes |
|---|---|---|---|
| A | 1 | 2 | The 10/100BASE-TX "TX" pair |
| B | 3 | 6 | The 10/100BASE-TX "RX" pair |
| C | 4 | 5 | Control, or + rail in joined mode |
| V | 7 | 8 | Power. Polarity-blind |

- This mapping matches passive PoE "mode B" (+ on 4,5 and - on 7,8), so joined mode (C as the + rail and V as the - rail) works with that adapter. Joined mode needs a solder jumper or strap on the node to change how the bridge is wired.
- Passive PoE using pins 1,2 and 3,6 ("mode A") is not covered. It reads as high voltage on A and B and is treated as foreign.
- 1000BASE-T uses all four pairs. A `br8dilan` link that also carries power is 10/100 only.
- A crossover cable swaps A and B. Avoid it, or let the nodes detect and swap.
- Any category of cable works (CAT5, 5e, 6, 7). Use a pair per slot.

### 6.2 USB-A and USB-B (`br8dobac`, USB-A/USB-B connector variants)

| Pair | + | - |
|---|---|---|
| A | D+ | D- |
| V | VBUS | GND |

- This is a 2-pair profile. B and C are not available.
- A br8d port on a USB-A or B connector is **not** a USB port unless the node implements USB. A real USB host will put 5V on VBUS and 3.3V levels on D+/D-, and a br8d node must treat that as foreign.

### 6.3 USB-C (`br8dobac`, USB-C connector variant)

| Pair | Mapping |
|---|---|
| V | VBUS / GND |
| A | D+ / D- |
| B, C | **[OPEN]** The SuperSpeed pairs (full-featured cables only) and/or the SBU pins |

- Start with the 2-pair profile (A and V), which works on cheap USB 2.0-only USB-C cables.
- **Flip ambiguity.** USB-C is reversible. A and V are orientation-invariant (they are wired both sides). Any pair on the SuperSpeed lanes can end up in a different slot or crossed when the plug is flipped. The profile for B and C must say how the node detects this. **[OPEN]**
- **CC.** A USB-C source puts no voltage on VBUS until it sees Rd on a CC pin, so a br8d node does not get power from a USB-C host unless it opts in with an Rd. That is a decision about USB behaviour, and it is outside br8d. A br8d provider on a USB-C connector MUST NOT drive VBUS until it recognises a signature (`PWR-4`), and SHOULD leave CC unconnected.
- SBU pins are fragile. Don't expose them to 60V without a gate.

### 6.4 Four-pole 3.5mm (`br8dopatch` or br8doaux`) **[PROVISIONAL]**

| Pair | + | - |
|---|---|---|
| A | Tip | Ring 1 |
| V | Ring 2 | Sleeve |

- 2 pairs (A and V), 4 conductors.
- The sleeve connects first and the tip last when a plug is inserted, so V (ring 2 and sleeve) makes contact before the signal pair.
- During insertion, contacts sweep past each other and briefly short. This is harmless: no power is on V until the provider has recognised a signature (`EMIT-1`).
- A **3-pole** plug in a 4-pole socket shorts ring 2 to sleeve, so it shorts V. The provider sees about 0Ω, which is not a signature, so it stays off.
- CTIA and OMTP phone wiring differ (ring 2 and sleeve swapped). A straight 4-pole lead doesn't care. Don't use a phone headset as a br8d lead.
- A 3-pole TRS profile is **[OPEN]**.

### 6.5 2x4 header and ribbon (`br8doflat`)

| Pair | + | - |
|---|---|---|
| A | 1 | 2 |
| B | 3 | 4 |
| C | 5 | 6 |
| V | 7 | 8 |

- Use a **shrouded, keyed** 2.54mm header with IDC ribbon (wire n goes to pin n), so each pair is two adjacent wires.
- A plug rotated 180° maps pin n to pin 9-n, which puts V in the A slot and A in the V slot, and flips every pair's polarity. The key prevents this. If it still happens, the electrical rules cover it (`ENV-3`, `MCU-8`).

### 6.6 Twin flex (`br8doflex`)

One pair. Use it as a power-only link (V profile) or a differential data link (A profile). See `STR-3`.

### 6.7 Shielded multicore (`br8doshield`) and others

Shielded multicore cable (USB-style) is named `br8doshield` (suggested). Unshielded loom is `br8doloom`. Industrial circular connectors (for example M12) would be `br8dom12` at level 5. `br8dox` is reserved for connector profiles registered by the community: any cable and connector that isn't named here, with a declared pin table.

### 6.8 Qwiic (`br8doqwiic`) **[PROVISIONAL]**

Qwiic is a 4-pin JST-SH I2C connector. This proposed br8d profile keeps the familiar four signals and maps them onto two br8d pairs:

| Qwiic signal | br8d pair/conductor |
|---|---|
| Pin 1: GND | V- |
| Pin 2: 3.3V | V+ |
| Pin 3: SDA | A+ |
| Pin 4: SCL | A- |

This is a candidate pin map; verify connector orientation against the actual board and cable before use. SDA and SCL are ordinary single-ended I2C signals referenced to GND, not a differential bus. The br8d port still needs its own limiter, clamp, sense and gate. A standard Qwiic board is not automatically br8d-protected, and must not be exposed directly to the br8d electrical envelope. Use an appropriately designed bridge or protected interface. The profile and its electrical compatibility need review and bench testing.

---

## 7. Bench test plan

Before calling a design level 1 or above, test it. **Use a current-limited bench supply (start at about 100mA), eye protection, and a fire-safe surface.**

1. **Unpowered.** Apply ±60V (current-limited) between each pair of conductors, and from each conductor to ground. Check what survives. Only sacrificial parts may fail.
2. **Powered, DEFEND.** Repeat while the node is running. Check that gates stay open and that the MCU and consumers see nothing above their rating.
3. **Trip time.** Step 60V onto each pair and measure when the gate opens, compared with the limiter's ride-through (race budget, Section 1.6).
4. **Live peers.** Plug in a PoE switch port, a passive PoE injector, a USB host, a USB charger, a headphone amp and a line-level source. The port must never drive above probe level (measure with a scope on the node side) and must not trip on ordinary signals.
5. **Hot-plug ringing.** Plug a 48V supply through a cable, and scope the peak at the connector (it can approach twice the supply).
6. **Back-power.** With the node unpowered, apply 5V to a signal pair and check for no supply back-feed, and no half-boot.
7. **Fault detection.** Remove a sacrificial resistor and check that the fault indicator works.
8. **Report** parts, results, and what failed, so the reference circuit can be updated.

---

## 8. Open items and roadmap

**Open items:**

1. Signature value and probe method (15kΩ and the 1V-through-15kΩ probe are candidates).
2. Wake-rail current: 2mA or 10mA (`PWR-8`).
3. Four-pole 3.5mm: V assignment (ring 2 and sleeve is provisional), and a 3-pole profile.
4. USB-C: the B and C pairs, flip handling, and the optional Rd for sinks.
5. Hub-to-hub tie-break (`PWR-10`).
6. Shield, chassis and ground rules, and isolation (`ENV-7`).
7. Solder-jumper definition for joined C+V mode on RJ45.
8. Whether to operate from AC (survive is required; operate is optional).
9. All timings and thresholds marked **[PROVISIONAL]**.
10. Qwiic pin map and electrical compatibility.
11. Licence.

**Planned sections:**

- Signature classes and the probing sequence (including the tie-break).
- Discovery on the C pair and the active signature (level 3).
- Protocol negotiation: how two nodes pick `br8dilan`, `br8dispi`, `br8di485` and so on over the same pairs.
- Power negotiation above 5V.
- Reference circuits and BOMs for levels 0 to 3.
- Conformance tests.
