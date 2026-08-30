# Benchmark entry — board 20 of 32

[metadata.json](metadata.json) is the supplied catalogue entry for this board,
preserved byte for byte from the seed pack. It is the same record that appears
in `boards_index.json` in
[PCBA_AutoDesignAndTest_Bench](https://github.com/pentolope/PCBA_AutoDesignAndTest_Bench), and the two must agree.

| | |
|---|---|
| Repository | `PCBA_Solar_MPPT_Controller` |
| Board id | `solar_mppt_controller` |
| Category | power-energy |
| Difficulty | 4 / 5 |
| Brief detail | 2 / 5 |
| Likely layer count | 4 |
| Primary stressors | buck power stage, current sensing, wide voltage range, thermal |

`difficulty` is how hard the board is. `detail` is how much of it the brief
states — and a low `detail` is not a low bar. A detail-1 brief leaves the
architecture open on purpose, and an agent that fills the silence with invented
user requirements has failed the board more thoroughly than one that designs it
badly.

This is a power-energy board at difficulty 4/5 with a deliberately thin brief (detail 2/5), so it tests architecture derivation as much as execution: the agent must derive an entire converter and control architecture from a few sentences of intent and then defend it. The metadata stressors — buck power stage, current sensing, wide voltage range, thermal — target the hard parts of that execution on the likely 4-layer stackup, where a tight switching commutation loop and high-current copper must coexist with low-level analog current sensing across an input/output ratio set by a nominal 12 V panel and a 3S or 4S lithium pack. It also tests restraint, because the brief withholds the panel's operating window, the charge profile, the sense method and accuracy, the pack configuration it targets, and the protection scheme, and an agent that quietly invents them has failed the brief rather than satisfied it.

## What goes here

Compact results only: metrics, verdicts, and the commit each was measured at.
The evidence for a result is the artefact the toolkit recomputes, not a summary
of it.

Routing search output, candidate pools, build trees and field-solver dumps do
**not** go here. They are ignored by [.gitignore](../.gitignore) and are
regenerated from what is committed. Thirty-two repositories share one benchmark
clone; weight here is paid thirty-two times.

## Protocol

The attempt protocol is defined once, in the umbrella repository, so that
thirty-two boards cannot drift into thirty-two protocols. See
[PCBA_AutoDesignAndTest_Bench/BENCHMARK.md](https://github.com/pentolope/PCBA_AutoDesignAndTest_Bench/blob/main/BENCHMARK.md).
