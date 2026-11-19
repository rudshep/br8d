# br8d maker guide: one effect, then a whole modular tribute

> **A fictional maker story, not a build-verified design.** The connector choices and protection levels below are examples. Draft values and profiles still need the checks listed in the [architecture document](br8d-architecture.md).

This story borrows the spirit of hands-on synth making: start with a small circuit, get a sound out of it, then let the next idea turn into another module. It is not a description of Look Mum No Computer's actual equipment or a claim that the creator uses or endorses br8d. At the time this guide was drafted (18 November 2026), the channel's newest listed upload was [“I MADE AN ANALOG POLY SYNTH COMPLETELY ON BREADBOARD…”](https://www.youtube.com/watch?v=jHz3ho6LUOc). For more modular-synth context, see the [DIY modular setup walkthrough](https://www.youtube.com/watch?v=lJROZv3y270) and [DIY Kosmo synth voice](https://www.youtube.com/watch?v=B5siZ7_x8C8).

## The first two boxes

Our maker already has a modular synth voice with CV and gate outputs, plus a MIDI sequencer. They build a small CV-controlled effect module to add echo, grit, and a ridiculous amount of resonance to the synth. Audio still travels over ordinary modular audio patch leads; br8d is for the control link between the boxes.

The effect is fun, but the maker wants to sync it from a MIDI sequencer and also let the analog synth's CV influence it. There is one snag: no MIDI cable in the studio. There *is* a four-conductor AUX lead, a pile of parts, and an idea.

### A bridge, not a magic cable

The maker adds a small bridge that plugs directly into the sequencer's MIDI socket and accepts a CV input from the synth voice. It translates MIDI onto pair A and the synth's CV into a differential signal on pair V. A br8d-aware interface on the effect module receives those signals and converts CV to the range its circuit expects. The bridge handles the conventional MIDI electrical interface. The AUX lead carries only the agreed br8d signals:

| Pair | Use on this first link | Important detail |
|---|---|---|
| **A** | MIDI data from the bridge | The bridge converts between the sequencer's MIDI electrical interface and the signal used inside the br8d link. |
| **V** | CV as a differential signal | V is **not power** on this link. Both modules use their own power supplies. |

This uses the draft four-pole 3.5 mm profile: A is tip/ring 1 and V is ring 2/sleeve. The profile is provisional, so the maker checks the actual lead and port pinout before connecting anything.

On the other side of the effect module, ordinary modular audio and CV jacks still do their usual jobs. A short modular patch lead takes the synth's CV output to the bridge's CV input. A br8d cable does not turn a standard MIDI, CV, headphone, or audio port into a br8d port. The bridge and module electronics make the signals compatible; the cable only carries the agreed conductors.

## The converter appears

Once MIDI sync is working, the maker builds a two-way MIDI/CV bridge:

- MIDI from the sequencer becomes the CV and gate signals expected by the analog modules.
- CV and gate can be measured and translated into MIDI messages for the sequencer.
- The conventional MIDI and CV connectors stay on the bridge's equipment side. Its br8d port is a separate, documented interface.

The maker labels each connection with its role, pair map, signal range, and protection level. They keep the first tests local and current-limited, and don't use a prototype to protect expensive equipment. A wrong cable or foreign device is still a real possibility; the draft's protection goals are not verified ratings.

## From a useful effect to the Modular Emu

Now the effect is the start of a much larger project: a lovingly recreated **Modular Emu** tribute, one oscillator, filter, envelope, sequencer, and oddball control box at a time. Some modules are built on breadboards first; others are adapted from equipment the maker already owns.

The useful br8d idea is not that every box suddenly speaks every protocol. The maker adds a bridge where an existing MIDI, CV, or I2C device needs to meet a br8d link, and reuses a documented wiring profile for the new modules. The audio path can remain ordinary patch cables while control, data, and (where designed for it) power take a different route.

The first two-pair control link has one profile. When the rack grows, the maker documents a separate four-pair profile for the CAT and ribbon links:

| Link profile | A | B | C | V |
|---|---|---|---|---|
| First AUX link | MIDI data | Unused | Unused | Differential CV; no cable power |
| Four-pair rack link | MIDI data | Differential CV | Gate/clock | Power, only if both endpoints are designed for it |

These are two declared profiles, not interchangeable pin maps. Each link must use the same profile at both ends. The pair names describe the wires; each project still has to say what they carry.

## The cable drawer gets a starring role

The system grows across a workbench, a rack, and a second station. These are different connector profiles—not a promise that every plug fits every port or that ordinary devices are interchangeable.

| Connector or cable | Its job in the story | What to keep in mind |
|---|---|---|
| **4-pole AUX lead** (`br8doaux`) | The first MIDI-and-CV link, because it is already in the drawer. | Four conductors give two pairs. The draft pin map is provisional; a phone headset or ordinary headphone port is not a br8d link. |
| **RJ45 with CAT5/5e/6/7** (`br8docat`) | A longer link between the rack and desk station. | Use a declared straight-through pair map. This does not make the modules Ethernet devices, and do not plug the link into a network switch. |
| **USB-A, USB-B, or USB-C connectors and cables** (`br8dobac`) | Convenient short links between a controller pod and a module. | These are br8d ports using USB-shaped connectors, not USB ports. USB-C is limited to the defined A/V pair profile here; its other lanes remain open work. Never connect ordinary USB equipment on the assumption that the port will understand it. |
| **Qwiic 4-pin cable** (`br8doqwiic`) | A small I2C control board gets its own proposed br8d profile. | The candidate map is A for SDA/SCL and V for 3.3 V/GND. This profile needs review and testing; a stock Qwiic board is not automatically br8d-protected. Use an appropriate bridge or protected interface. |
| **Keyed 2x4 header and ribbon** (`br8doflat`) | A tidy internal link between modules in the same rack. | Keep the keyed header and the documented pin order. A reversed or mismatched ribbon can put the wrong pair in a slot. |
| **Twin flex** (`br8doflex`) | A simple one-pair connection for a differential control signal or a separately designed power-only link. | One pair can carry a differential signal or a signal with its return; it does not provide another pair for an extra signal. |
| **Shielded multicore** (`br8doshield`) | A possible low-noise run between stations. | Shield, chassis, and ground rules are still open in the architecture. Treat this as experimental until those rules are defined and tested. |
| **Unshielded loom** (`br8doloom`) | A custom bundle inside the growing rack. | Label the conductors and publish a pin table; a bundle is not automatically a br8d profile. |
| **M12** (`br8dom12`) | A tempting future connector for a road case or rugged stage setup. | This is outside the current maker-level target; the architecture places industrial protection at level 5. Don't treat the connector alone as rugged protection. |
| **Another connector from the parts bin** (`br8dox`) | A one-off bridge for a useful cable the maker already owns. | Define and document its pin table before use. |

The growing rig uses each cable where it makes sense. A two-pair lead is not asked to do the job of an eight-conductor ribbon, and a USB-shaped connector is not treated as a USB protocol. Connector choice is flexible; the pair map and the electronics at both ends are not optional.

## A maker's bring-up checklist

1. **Write the link profile first.** List the connector, cable, pair assignments, signal direction, voltage/current range, and whether power is present.
2. **Check both ends.** A conventional MIDI, CV, or Qwiic device needs a correctly designed bridge or interface; physical fit is not electrical compatibility.
3. **Choose and state the protection level.** Level 0 is not protected. Levels 1 and above have draft survival targets, but published reference circuits and test results are still missing.
4. **Power locally for the first tests.** Don't add cable power until the provider, node, signature, current limit, and V front end are designed as described in the architecture.
5. **Test before connecting valuable gear.** Use a current-limited supply, a known pin map, and the [architecture bench-test plan](br8d-architecture.md#7-bench-test-plan). Record what worked and what failed.

The aim is to make the next module more fun to add—not to pretend that a draft connector standard can remove the need to check a circuit. br8d gives the maker a place to write down the wiring choices once, then build on them as the Modular Emu grows.

## Bench seed: 5 V power and serial

This simpler link is a useful way to try the provider/node idea before adding a custom MIDI/CV interface. It is retained from the earlier architecture seed, but its values are provisional and it has no published reference circuit or test result. In particular, do not treat the draft's level 1 survival target as demonstrated.

| Signal | Pair | Direction | Draft value |
|---|---|---|---|
| Power | V | Provider to node | 5 V, up to 500 mA **[PROVISIONAL]** |
| Serial, provider to node | A+ | Provider drives | 3.3 V logic, referenced to V- |
| Serial, node to provider | A- | Node drives | 3.3 V logic, referenced to V- |

This single-ended serial example requires a DC supply so V- can be the signal reference. A self-powered link without V should use differential signalling instead. The earlier seed suggests 115200 baud and runs up to about 3 m, but both are starting points to verify with the actual cable, gates, and protection parts—not guarantees.

The matching connector maps in the draft are:

| Connector | 5 V (V+ / V-) | Serial (A+ / A-) |
|---|---|---|
| RJ45 | 7 / 8 | 1 / 2 |
| USB-A or USB-B | VBUS / GND | D+ / D- |
| 4-pole 3.5 mm | Ring 2 / Sleeve | Tip / Ring 1 |
| Keyed 2x4 IDC | 7 / 8 | 1 / 2 |

These are br8d connector profiles, not ordinary Ethernet or USB ports. Use straight-through, known cables and matching br8d ports at both ends.

### Node and provider sequence

1. The **node** presents a signature resistor across V before its rectifier. The draft candidate is 15 kΩ ±1%, rated at least 0.25 W and marked as sacrificial **[PROVISIONAL]**.
2. The node's V front end is designed for its protection level. The level 1 example uses a fuse, bidirectional TVS, Schottky bridge and regulator; the bridge drop and cable resistance must fit the node's input range.
3. Each A conductor gets a limiter, clamp, sense tap and gate before reaching the UART. The earlier level 1 example uses about 10 kΩ, 1206 series resistors and ≥1 MΩ sense resistors; these values and the signal rate need bench verification.
4. The **provider** starts with V switched off and signal pins high-impedance. It listens first. Only after a quiet reading does it probe V for the expected signature.
5. The draft probe is 1 V through 15 kΩ, read by an ADC. If the signature is recognized, the provider ramps V to 5 V with a 100 mA initial current limit, then may raise it toward 500 mA after the node starts **[PROVISIONAL]**.
6. If the signature, supply, or link is lost, the provider turns V off and returns both ends to their defensive state. Forced power without a signature must be deliberate and indicated.

At level 0, the probe and signature can be handled in firmware, but the port is **not protected**. Higher levels need the hardware behavior and testing described in the [architecture](br8d-architecture.md); use its [bench-test plan](br8d-architecture.md#7-bench-test-plan) before connecting valuable equipment.

### Cable length and variants

The draft estimates loop resistance drops of about 0.17 V per metre per amp for 24 AWG copper (typical CAT cable) and 0.43 V per metre per amp for 28 AWG copper (thin USB or ribbon cable). These are rough estimates: subtract the bridge drop too, and check the actual cable and load.

For longer data runs, the architecture suggests differential serial such as RS-485, with a transceiver rated for the fault environment or placed behind the gate. A one-pair link can also be A for differential data or V for power only. These choices still need a defined profile and a bench check.
