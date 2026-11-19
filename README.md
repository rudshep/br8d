# br8d: a stable able cable

**br8d (pronounced "braid") is a shared way to connect modules with the wires and cables you already have.**

Building a modular synth in a home studio means making a lot of small decisions: which connector to use, which pins carry which signals, and how one module talks to another. As the project spreads across racks and workstations, every new interconnect scheme can become another compatibility problem.

br8d is an open draft for a common interconnect layout. It aims to let you choose a suitable cable from your parts bin, keep the same basic wiring convention between modules, and decide in the module's electronics and firmware what the signals mean. That way, you can spend less time reinventing cables and more time building the system.

The goal is simple: **use 2 to 8 conductors for a link, agree what each pair does, and keep the module-specific behavior in the modules.**

Some examples:
**br8docat** uses off-the-shelf CAT5/5e/6/7 cable and RJ45 sockets. It doesn't make your module Ethernet-capable; both ends still need a matching, documented br8d design.

**br8dobac** uses USB-A, B or C connector styles for br8d links, so you can reuse compatible cables from the parts bin. A br8d port is not a USB port unless it also implements USB.

**br8doaux** can reuse an inexpensive 4-pole AUX cable to link modules carrying analog audio, control, or other signals the endpoints are designed for. Someone may plug headphones, powered speakers, or an old charger into a similar-looking jack, so the port needs suitable protection. The protection design is still a draft, not a guarantee that nothing will be damaged.

**br8doqwiic** is a proposed br8d profile for the familiar 4-pin Qwiic connector and I2C signals. The pin map and protected interface still need review and testing; a standard Qwiic board is not automatically br8d-protected.

> **Status: draft 0.1.** The rules and designs are still being developed. Many component values and timings are provisional, and reference circuits and test results have not been published yet. The protection levels below describe design targets, not verified guarantees. Read the [architecture document](br8d-architecture.md) before building a port that depends on its protection.

## What br8d helps with

- **Stop starting from scratch.** Reuse a consistent pair layout instead of inventing a new pinout for every connection.
- **Make use of cables you have.** Choose a suitable cable and connector that are already available to you; br8d does not require one special connector.
- **Grow the system more easily.** Keep the wiring convention consistent as modules move between racks or stations.
- **Keep the signal choice flexible.** Pairs can carry audio, control, data, or power when the electronics at both ends are designed for those signals. br8d does not dictate the protocol.
- **Plan for wiring mistakes.** The design includes a front end intended to limit unexpected electrical conditions before they reach a module's circuitry.

For example, a synth builder might reserve one pair for audio, another for control or data, and a pair for power. Those assignments are choices for that system, not rules imposed by br8d.

Depending on the chosen profile, a link might use CAT cable, a USB-style cable, a 4-pole audio lead, or keyed ribbon cable. Using one of these does **not** make the port Ethernet, USB, or a headphone connection: use a known pair map at both ends.

## What br8d is

br8d is a **cabling convention with a port safety design**. It describes:

- up to four pairs of conductors, named **A, B, C, and V**;
- electrical expectations for a port;
- a front end that can monitor and protect the module-side circuitry; and
- example ways to map the pairs onto different connectors.

Each pair has two conductors, so a link can use 2, 4, 6, or 8 conductors. You can use fewer than four pairs when your project needs fewer.

| Pair | Common use | Think of it as |
|---|---|---|
| **A** | Data or other signals | First pair |
| **B** | More data or signals | Second pair |
| **C** | Control or another signal | Third pair |
| **V** | Power | Fourth pair |

These are defaults, not fixed assignments: any pair can carry a different signal if the modules are designed for it and the wiring is documented.

## How the safety idea works

A br8d port is designed to **listen before it connects a cable to sensitive circuitry**. Its front end limits current, clamps voltage, monitors the wires, and uses a gate that normally keeps the module's circuit disconnected until the port is allowed to connect it.

The draft calls this approach **“fast to not safe, slow to safe”**: hardware should disconnect quickly when it detects a hazard, while the module takes time to decide that a connection is suitable. A fault on one pair is intended to be handled separately from the other pairs.

These are design goals, not a promise that any br8d board is safe in every situation. Protection depends on the circuit, the parts, and testing. In particular, the draft's higher protection levels still need reference designs and bench results.

## Choose a protection level

The draft describes levels so makers can be clear about how much protection a port actually has.

| Level | Intended for | In brief |
|---|---|---|
| **0** | Your own bench prototypes | Basic current-limiting resistors and careful firmware. Not protected; mistakes can damage the board. |
| **1** | Shared DIY projects | A passive hardware front end with replaceable parts intended to take the first hit. |
| **2** | More capable DIY builds | Level 1 plus active monitoring and more flexible signal handling. |
| **3** | Smart modules | Adds active discovery and negotiation; these details are future work. |
| **4–5** | Consumer or industrial products | Requires certification or industrial-grade design beyond this maker-focused draft. |

**Important:** Levels 1 and above have a draft target for surviving ±60 V DC between any two conductors or between a conductor and ground, plus transients up to 100 V for 1 ms. These targets have not been demonstrated by published reference circuits or test results. Do not treat them as verified ratings. State the level and its limits for every port you build.

## A first link

Before wiring modules together:

1. Pick a connector and a cable that suit the project.
2. Decide which pairs you need and write down what each one carries.
3. Use the same pair mapping at both ends, and make sure each module's circuitry is designed for the signal and power it will receive.
4. Decide and document the protection level for each port.
5. Test the link on the bench before connecting it to the rest of your system.

For a fictional example that follows a synth project from its first effects module to a growing modular system, see the [br8d-er guide](br8d-er-guide.md).

## What br8d does not do

- **It is not a protocol.** The modules decide how audio, control, or data is encoded and understood.
- **It is not a universal adapter.** A cable that physically fits may connect unrelated signals. br8d does not make different devices communicate.
- **It does not make every cable or port interchangeable.** Use a suitable cable with a known wiring map, and design both ends for the signals they carry.
- **It is not a certification or a verified safety rating.** The draft describes design goals; each implementation needs suitable parts and testing.

## Help shape the draft

br8d is at an early stage, and maker feedback is especially useful:

- Build and test a reference circuit, then share what happened.
- Suggest connector and cable profiles that are useful in real projects.
- Point out unclear rules, missing cases, or compatibility problems.
- Share how a common interconnect could help your modular build.

Open an issue with corrections, questions, or test results.

## License

To be decided.
