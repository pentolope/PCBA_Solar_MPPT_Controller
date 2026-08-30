# PCBA_Solar_MPPT_Controller — Solar MPPT Battery Charger

**Benchmark ID:** 20  
**Difficulty:** 4/5  
**Brief detail:** 2/5  
**Category:** power-energy  
**Likely layer count:** 4  
**Primary stressors:** buck power stage, current sensing, wide voltage range, thermal

## Design brief

Design an MPPT solar charger for a nominal 12 V photovoltaic panel charging a 3S or 4S lithium battery pack at up to roughly 5 A. Include input/output current and voltage measurement, an MCU, protected battery and panel connectors, and a communications/debug port. Choose the exact MPPT power topology and controller implementation. The benchmark should stress high-current routing, analog current sensing, switching-loop layout, thermal management, and placement around bulky connectors/inductors.

## Benchmark intent

This brief is intentionally one member of a heterogeneous PCBA-autodesign benchmark. Treat stated requirements as authoritative; where the brief leaves choices open, make and document reasonable engineering decisions rather than inventing hidden user requirements. The repository should remain a consumer of the shared `PCBA_AutoDesignAndTest` toolkit rather than accumulating board-specific logic in the toolkit.
