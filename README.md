# Solar MPPT Battery Charger

An MPPT charger taking a nominal 12 V PV panel to a 3S or 4S lithium pack at up to roughly 5 A, with input/output I and V measurement, an MCU, and a communications/debug port.

This repository holds a **solar MPPT battery charger**: a nominal 12 V photovoltaic panel charging a 3S or 4S lithium battery pack at up to roughly 5 A. The brief fixes the endpoints and the instrumentation — input and output current and voltage measurement, an MCU, protected battery and panel connectors, and a communications/debug port — and then explicitly hands the power architecture back to the design agent: "Choose the exact MPPT power topology and controller implementation."

At brief detail 2/5 this is a sparse brief. Almost everything between the panel terminals and the battery terminals is undetermined: converter topology, switching frequency, magnetics and switches, sense method and accuracy, MCU part, connector selection, what "protected" protects against, whether the board serves 3S, 4S or both, board outline, and stackup are all open. The brief names the areas it wants stressed — high-current routing, analog current sensing, switching-loop layout, thermal management, and placement around bulky connectors and inductors — but names no parts, dimensions, tolerances, or operating limits, and none should be assumed. Every number in the eventual design is a decision the design agent must make, justify against cited evidence, and record.

> **This board has not been designed.** There is no schematic, no layout and no
> part selection here — only the brief, a reading of the brief, and the
> scaffolding a design run needs. That is the intended state of this repository,
> not a gap in it.

## What the brief fixes, and what it leaves open

The brief pins down 13 requirements and deliberately leaves
19 decisions to whoever designs the board. The `Source` column says
which is which: `brief` is quoted from [BRIEF.md](BRIEF.md), `metadata` comes
from the benchmark catalogue, and `open` means the brief does not fix it.

| Aspect | Value | Source |
|---|---|---|
| Function | MPPT solar charger (panel to battery) | brief |
| Input source | Nominal 12 V photovoltaic panel | brief |
| Battery pack | A 3S or 4S lithium battery pack — the brief names both configurations and narrows to neither | brief |
| Charge current | Up to roughly 5 A into the pack (a ceiling; no duty cycle or continuity stated) | brief |
| Instrumentation | Input and output current measurement, and input and output voltage measurement | brief |
| Digital control | An MCU on board (part and family not fixed) | brief |
| Power connectors | Protected battery connector and protected panel connector | brief |
| Comms/debug | A communications/debug port (interface not named) | brief |
| MPPT topology and controller | Explicitly delegated to the design agent by the brief | brief |
| Areas the benchmark should stress | High-current routing, analog current sensing, switching-loop layout, thermal management, placement around bulky connectors/inductors | brief |
| Likely layer count | 4 | metadata |
| Category / difficulty / brief detail | power-energy; difficulty 4; detail 2 | metadata |
| Primary stressors | buck power stage, current sensing, wide voltage range, thermal | metadata |
| Board outline, size, mounting | Not fixed by the brief — design agent's choice | open |

The full split, with the verbatim brief text substantiating every fixed
requirement, is in [board/requirements.md](board/requirements.md) and
machine-readably in [board/requirements.json](board/requirements.json).

**Missing details are design freedom, not permission to fabricate unstated user
requirements.** A choice the brief left open is recorded as a decision, with its
reasoning — never promoted into a requirement.

## Benchmark position

| | |
|---|---|
| Benchmark id | 20 of 32 |
| Category | power-energy |
| Difficulty | 4 / 5 |
| Brief detail | 2 / 5 |
| Likely layer count | 4 |
| Primary stressors | buck power stage, current sensing, wide voltage range, thermal |

This is a power-energy board at difficulty 4/5 with a deliberately thin brief (detail 2/5), so it tests architecture derivation as much as execution: the agent must derive an entire converter and control architecture from a few sentences of intent and then defend it. The metadata stressors — buck power stage, current sensing, wide voltage range, thermal — target the hard parts of that execution on the likely 4-layer stackup, where a tight switching commutation loop and high-current copper must coexist with low-level analog current sensing across an input/output ratio set by a nominal 12 V panel and a 3S or 4S lithium pack. It also tests restraint, because the brief withholds the panel's operating window, the charge profile, the sense method and accuracy, the pack configuration it targets, and the protection scheme, and an agent that quietly invents them has failed the brief rather than satisfied it.

This repository is one of thirty-two. The suite, the protocol and the results
live in [PCBA_AutoDesignAndTest_Bench](https://github.com/pentolope/PCBA_AutoDesignAndTest_Bench).

## Repository layout

| Path | Contents |
|---|---|
| `BRIEF.md` | the supplied brief — authoritative, preserved byte for byte, never edited |
| `board/requirements.md` | what the brief fixes, what it leaves open, and where decisions get recorded |
| `board/requirements.json` | the same split, machine-readable, each fixed requirement bound to brief text |
| `board/manifest.template.json` | the toolkit's minimum manifest, pre-filled for this board |
| `board/toolchain.json` | where this board's build finds KiCad and the router |
| `benchmark/metadata.json` | the supplied catalogue entry — category, difficulty, detail, stressors |
| `docs/architecture.md` | the decisions this board must make, as questions, unanswered |
| `docs/sources.md` | the classes of evidence the design will have to cite |
| `docs/status.md` | what exists, what does not, and what is deliberately absent |
| `candidates/` | disposable search output, ignored by Git |
| `.claude/skills/` | the accountability-review skill [CLAUDE.md](CLAUDE.md) requires before a push |
| `tooling/PCBA_AutoDesignAndTest` | the shared verification/routing/release toolkit, as a pinned submodule |

## Getting the repository

The toolkit is a submodule and carries KiCad Routing Tools as a submodule of its
own, so clone recursively:

```bash
git clone --recursive https://github.com/pentolope/PCBA_Solar_MPPT_Controller.git
```

```bash
git submodule update --init --recursive
```

## Designing the board

Generic verification, routing and release logic is **not** written here. It is
consumed from `tooling/PCBA_AutoDesignAndTest`, which is board-agnostic by
construction and must stay that way; this repository owns the board and nothing
else. Start from
[the toolkit's onboarding guide](tooling/PCBA_AutoDesignAndTest/examples/onboarding.md),
and see [CLAUDE.md](CLAUDE.md) for the rules a design run works under.

```bash
python3 tooling/PCBA_AutoDesignAndTest/run.py preflight
```

## Brief integrity

`BRIEF.md` SHA-256 `d62213d57c66897eb6756f8d70a4fdc41221aed55d7afc257cbc82ba56b40d05`

Every quotation in `board/requirements.json` is bound to those exact bytes. If
the brief ever changes, the bindings are stale by construction — which is the
point of recording the digest.
