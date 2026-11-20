# br8d variant naming convention

**Draft.** A short name should identify what a board is for, what connectors it
has, and what it does. The details still belong in the board's documentation.

## Naming a variant

Use a descriptive family and connector layout, followed by function, level
and occupied pairs. Keep the name short: don't repeat a connector or mapping
already clear elsewhere in the name. The usual pattern is:

```text
<family>-<layout>[-<map>]-<function><level>-<size>-<pairs>
```

This is a guide, not a rigid grammar. For example, use
`br8d-i-aux-1ts-br0-1u-A`, not `br8d-i-aux-TS-1ts-br0-1u-A`, and
`br8d-i-aux-1TRSbal-br0-1u-A`, not `br8d-i-aux-TRSbal-1trs-br0-1u-A`.
The guide's longer profile names can remain as references without being
repeated in board names.

- **Family** identifies the interface or connector profile, such as
  `br8d-i-cv`. These are the names used in the
  [adapter catalogue](../hardware/br8d-catalogue.md).
- **Connector layout** gives the quantity and type of external connectors.
  For example, `1ts` is one TS socket, `4ts` is   four TS sockets, `4TRSbal` is four TRS sockets with a pair on tip and ring,
  and `2trrs` is two TRRS sockets. `TRSbal` describes the contact map; it
  does not guarantee a balanced signal. Use familiar connector names where
  possible, such as `usbA`, `usbC`, `xlr3m` or `xlr3f`; `m` and `f` indicate
  male and female contacts.
- **Map**, when needed, distinguishes layouts with the same connectors but
  different wiring or behaviour. For example, `3rcaAV1trs-mirror` mirrors
  stereo audio to the TRS connector. The full contact map remains in the
  catalogue or design documentation.
- **Function** describes what the board does: `br` is a breakout and `io` is
  an input/output module. Other function codes are to be decided when needed.
- **Protection level** is the number following the function code, from `0` to
  `5`. Function and protection are separate choices: a layout can have, for
  example, both a `br0` and a `br1` variant.
- **Size** is the board width in 1U units: `1u` for a 1U board (20.38 mm) or
  `2u` for a 2U board (40.76 mm). A 4U custom build is not a standard module.
  Use lowercase `u` throughout.
- **Pairs** lists the occupied br8d slots in A, B, C, V order, even when only
  one conductor of a pair is connected. `A`, `AV` and `ABCV` are examples.
  A separate shield is not a pair and does not appear in this suffix.

Every name keeps its family prefix, even when the connector name already says
what it is. Examples are `br8d-i-xlr3f-br0-1u-A` (one female XLR-3),
`br8d-cat-rj45-br0-1u-ABCV` (one RJ45 socket), `br8d-i-rca-2rca-br0-2u-AB`
and `br8d-o-usb-1usbC-br0-1u-AV`. Keep any distinction needed to tell designs
apart: `1TRSA` and `1TRSB` have different MIDI wiring, and stereo `1trs` is not
the same map as `1TRSbal`.

Use hyphens throughout; don't use `/` or `#`. In the proposed hardware tree,
the family folder holds one folder per module, with the name after the family
prefix, for example `hardware/br8d-i-cv/4ts-br0-2u-ABCV/`. Use the catalogue
link rather than guessing the path. Pair allocation alone does not specify a
pin map or a connector's signal direction.

The whole name identifies a **variant** of the family. A **revision** is a
change to a particular variant, not another function or connector layout.
How revisions are labelled has not yet been decided.

## Shared designs and inheritance

A family can inherit a design from another family when the physical board and
its contact map are the same. The application name or packaging can differ;
it does not change the board. Matching CV TS variants inherit the AUX TS
design through levels 0-2, with CV specialization starting at level 3. For
example, `br8d-i-cv-1ts-br0-1u-A` uses the
`br8d-i-aux-1ts-br0-1u-A` board, possibly with a CV label. Level 1/2 designs
remain proposals and need verification for both applications.

The [catalogue](../hardware/br8d-catalogue.md#design-inheritance) records
the proposed parent-child relationships. Sharing files, such as with symlinks,
is for later; the relationship alone does not establish signal compatibility
or a protection rating.

## Example

`br8d-i-cv-4ts-br0-2u-ABCV` means:

- `br8d-i-cv`: the CV family.
- `4ts`: four TS sockets.
- `br0`: an unprotected, directly wired breakout, with no added circuitry.
- `2u`: a 2U board.
- `ABCV`: the board uses all four pairs.

As a proposed layout, the four sockets could each use one br8d pair: A, B,
C and V. Using V for a signal rather than power is still an open decision; the
name does not approve the mapping.

`br8d-i-cv-4ts-io1-2u-ABCV` would use the same connector layout with CV input/output
circuitry and level 1 protection. It is an example name, not a claim that such
a design exists or has been verified.

## What the name does not say

Document the exact pin map, connector and header, direction of each signal,
electrical limits, protection, PCB dimensions and mounting separately.
For example, `4trs` alone does not say which contacts carry signals or shield.
The 1U x 1U PCB size and north-facing connector are part of the design
description, not currently part of the name.

Composite boards may need a map qualifier when connectors share signals or
some contacts are unused. The current pair suffix describes one br8d header
only; naming and allocation for multiple independent headers remain open.

`br0` describes an unprotected breakout board; it does not claim that the
complete port is architecture-compliant at level 0. The current
[architecture](../br8d-architecture.md#5-protection-levels) specifies
additional requirements for a level 0 port. Higher protection levels are
design targets, not verified ratings: each design must document and test its
actual protection.
