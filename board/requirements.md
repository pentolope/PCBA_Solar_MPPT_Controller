# Requirements — Solar MPPT Battery Charger

Two lists. The difference between them is the whole point of this file.

A **fixed requirement** is something [BRIEF.md](../BRIEF.md) asks for. Each one
below quotes the brief text that substantiates it; if a statement cannot be
quoted, it is not a requirement here. An **open decision** is a choice the brief
deliberately left to whoever designs this board.

> Missing details are design freedom, not permission to fabricate unstated user
> requirements.

Promoting a decision into a requirement is the failure this file exists to
prevent. Record a choice under the decision it answers, with the reasoning that
made it — never by adding it to the list above.

Bound to `BRIEF.md` SHA-256 `d62213d57c66897eb6756f8d70a4fdc41221aed55d7afc257cbc82ba56b40d05`.

## Fixed by the brief

### REQ-01 — The board is an MPPT solar charger — maximum power point tracking is a required function, not an optional mode.

Brief text:

> Design an MPPT solar charger for a nominal 12 V photovoltaic panel

### REQ-02 — The input source is a nominal 12 V photovoltaic panel.

Brief text:

> for a nominal 12 V photovoltaic panel charging a 3S or 4S lithium battery pack

### REQ-03 — The output is a lithium battery pack in a 3S or 4S configuration. The brief names both and narrows to neither, so the chemistry class and the two candidate pack configurations are fixed, while which configuration(s) a given board serves — and how that is established — is the design agent's decision to make and record (OPEN-05).

Brief text:

> charging a 3S or 4S lithium battery pack at up to roughly 5 A.

### REQ-04 — The charger must handle charge current up to roughly 5 A into the pack. This is a stated ceiling on the charge current; the brief states no duty cycle, no continuity, and no current figure for the panel side.

Brief text:

> lithium battery pack at up to roughly 5 A. Include input/output current and voltage measurement

### REQ-05 — Current measurement is required on both the input (panel) side and the output (battery) side.

Brief text:

> Include input/output current and voltage measurement, an MCU, protected battery and panel connectors

### REQ-06 — Voltage measurement is required on both the input (panel) side and the output (battery) side.

Brief text:

> Include input/output current and voltage measurement, an MCU, protected battery and panel connectors

### REQ-07 — The board must include an MCU.

Brief text:

> current and voltage measurement, an MCU, protected battery and panel connectors, and a communications/debug port.

### REQ-08 — Both the battery connector and the panel connector must be protected; protection is a stated requirement on the connector interfaces. The brief says "connectors" and names no connector style, family or part.

Brief text:

> an MCU, protected battery and panel connectors, and a communications/debug port.

### REQ-09 — The board must include a communications/debug port.

Brief text:

> protected battery and panel connectors, and a communications/debug port. Choose the exact MPPT power topology

### REQ-10 — The design agent is required to select and pin down the MPPT power topology and the controller implementation — the choice is delegated but making and recording it is mandatory.

Brief text:

> Choose the exact MPPT power topology and controller implementation.

### REQ-11 — The brief states the areas the benchmark should stress: high-current routing, analog current sensing, switching-loop layout, thermal management, and placement around bulky connectors/inductors. The design is expected to engage those areas as the brief's stated intent for this board; the sentence names areas of stress, not a prohibition on any particular design choice.

Brief text:

> The benchmark should stress high-current routing, analog current sensing, switching-loop layout, thermal management, and placement around bulky connectors/inductors.

### REQ-12 — Where the brief leaves a choice open, the design agent must make and document a reasonable engineering decision rather than invent an unstated user requirement to justify it.

Brief text:

> Treat stated requirements as authoritative; where the brief leaves choices open, make and document reasonable engineering decisions

### REQ-13 — This repository must remain a consumer of the shared PCBA_AutoDesignAndTest toolkit; board-specific logic belongs here, not in the toolkit.

Brief text:

> The repository should remain a consumer of the shared `PCBA_AutoDesignAndTest` toolkit rather than accumulating board-specific logic in the toolkit.

## Open — the design agent decides

### OPEN-01 — The MPPT power converter topology.

The brief explicitly delegates it ("Choose the exact MPPT power topology"). The metadata tags "buck power stage" as a stressor, but that is a benchmark stress label, not a stated user requirement — the topology decision and its justification are the design agent's.

*Decision:* **not yet made.**

### OPEN-02 — Where the MPPT control loop lives and what algorithm it uses — MCU firmware, a dedicated controller, or a split between them.

The brief says to choose the controller implementation and requires an MCU, but never says the MCU runs the tracking loop and names no algorithm.

*Decision:* **not yet made.**

### OPEN-03 — MCU selection: family, core, package, peripheral set, and ADC capability.

The brief requires "an MCU" and stops there. No architecture, vendor, speed, memory, or pin count is stated.

*Decision:* **not yet made.**

### OPEN-04 — The panel-side operating window the board is designed to: open-circuit voltage ceiling, short-circuit current, minimum tracking voltage, and behaviour outside that window.

The brief gives only "nominal 12 V" for the panel — a nominal rating, not a range — while the metadata names "wide voltage range" as a stressor. The actual design limits are a derived, documented decision.

*Decision:* **not yet made.**

### OPEN-05 — Which pack configuration the board serves — 3S, 4S, or both — and, if both, how the case is configured, detected, or selected; and the battery charge profile and limits: how charging is regulated and terminated, and the per-cell voltage limits enforced.

The brief writes "a 3S or 4S lithium battery pack" as a disjunction and never says one board must cover both. It states no chemistry variant, cell voltage, charge stages, or configuration mechanism, and this choice drives the output voltage range and therefore the required conversion direction.

*Decision:* **not yet made.**

### OPEN-06 — Current-sense method and topology for each of the two measurement points — sense element type (resistive, magnetic, DCR, sense-FET, or integrated in the converter), its placement in the current path, and the signal-conditioning chain into the ADC.

The brief requires input and output current measurement and calls the area "analog current sensing", but names no sense element and does not say how or where in the loop the measurement is taken.

*Decision:* **not yet made.**

### OPEN-07 — Accuracy, resolution, bandwidth, and calibration strategy for the current and voltage measurements.

The brief is silent on measurement performance; no tolerance, error budget, or update rate appears anywhere in it.

*Decision:* **not yet made.**

### OPEN-08 — Voltage-sense implementation on both rails — divider ratios, reference source, and how the sense nodes tolerate the full operating window.

The brief requires the measurement to exist but fixes no ranges, references, or input-protection approach for it.

*Decision:* **not yet made.**

### OPEN-09 — What the connector "protection" covers — which fault and stress modes are in scope, on which port, and by what mechanism.

The brief requires "protected battery and panel connectors" but names neither the threats being protected against nor any protective device. Converting the word into a specific protection part would be fabricating a requirement.

*Decision:* **not yet made.**

### OPEN-10 — Panel and battery connector selection: type, current rating, wire termination, polarity keying, and retention.

The brief names no connector family, style or part, only that these connectors exist and are protected — while also calling out placement around bulky connectors as a stress area.

*Decision:* **not yet made.**

### OPEN-11 — The communications/debug port's electrical interface, protocol, connector, and whether it also serves as the programming interface.

The brief writes "a communications/debug port" as a single item and names no interface standard, connector, or protocol.

*Decision:* **not yet made.**

### OPEN-12 — Switching frequency, inductor and switching-device selection, ripple targets, and efficiency targets.

The brief states none of these; they follow from the topology decision, which is itself delegated.

*Decision:* **not yet made.**

### OPEN-13 — Housekeeping/bias supply architecture — how the MCU and analog rails are generated, from which source (panel, battery, the debug port, or a combination), and start-up behaviour when the panel is dark or the battery is deeply discharged.

The brief is silent on internal rails, rail voltages, and start-up or sequencing behaviour, and it does not say which of the board's interfaces may act as a supply.

*Decision:* **not yet made.**

### OPEN-14 — Thermal strategy: the worst-case dissipation the board is designed to, ambient assumptions, allowed temperature rise, and by what path heat leaves the assembly.

The brief names thermal management as an area to stress but states no ambient, rise limit, airflow condition, heatsinking, or mounting arrangement, and prescribes no heat-removal mechanism.

*Decision:* **not yet made.**

### OPEN-15 — Board outline, dimensions, mounting-hole pattern, keepouts, and connector edge locations.

The brief states no mechanical envelope, enclosure, or mounting requirement at all.

*Decision:* **not yet made.**

### OPEN-16 — Stackup specifics: whether the likely 4 layers are used, layer roles, copper weights, dielectric thicknesses, and finished board thickness.

The metadata offers "4" as a likely layer count only. Copper weight in particular is a live decision because the board must carry the roughly 5 A charge current on the battery side plus whatever panel-side current the chosen topology and operating point imply, and nothing in the brief fixes either the conductor sizing or the duty over which that current flows.

*Decision:* **not yet made.**

### OPEN-17 — Ground and return-path strategy: how the power-stage return, the sense returns, and the MCU/analog reference are partitioned and joined.

The brief pairs high-current routing with analog current sensing as stressors but prescribes no grounding scheme.

*Decision:* **not yet made.**

### OPEN-18 — Fabrication and assembly constraints: fabricator, design-rule class, single- versus double-sided assembly, and any resulting placement restrictions.

The brief names no manufacturer, process, or DFM ruleset.

*Decision:* **not yet made.**

### OPEN-19 — Bring-up and test provisions: test points, in-circuit access to the switching node and sense chain, and any status indicators.

The brief mentions only a communications/debug port and says nothing about physical test access, instrumentation points, or user-visible status.

*Decision:* **not yet made.**

## Where a decision gets recorded

1. Set `chosen` and `rationale` on the matching entry in
   [requirements.json](requirements.json). **That file is the authoritative
   record**, and the only one the benchmark's scripts read: a decision written
   only in prose is invisible to `board_status.py` and to any result that
   counts how many decisions an attempt actually made.
2. Answer it under its `OPEN-nn` heading here as well, with the reasoning and
   the evidence that made the choice. This file is the readable copy; where the
   two disagree, the JSON is what happened.
3. Cite the datasheet or standard in [docs/sources.md](../docs/sources.md).

A choice recorded this way stays visibly a choice. That is what lets a later
reader tell this board's engineering apart from its brief.

## Where this board is most likely to be faked

Places where a design run would be tempted to assert something it cannot
substantiate:

- Claiming the roughly 5 A rating without conductor-sizing evidence — trace width, copper weight, and via count for the high-current paths called "adequate" rather than derived from current-capacity data and a stated temperature rise, and with the panel-side current never derived from the chosen topology at all.
- Inventing the panel's operating window. The brief gives a nominal 12 V rating only; Voc, Isc, and the "wide voltage range" stressor mean the input limits are a decision to justify and record, not a specification to quote.
- Treating the metadata stressor "buck power stage" as a stated requirement. The brief explicitly delegates topology, and a nominal 12 V panel feeding a 4S pack is exactly where an unexamined topology assumption breaks.
- Reading "a 3S or 4S lithium battery pack" as a mandate to support both in one board, or silently narrowing it to one, without stating which was chosen and why — the brief names both and settles neither, and the answer moves the output range, the charge configuration, and possibly the topology.
- Sense-accuracy claims without an error budget — quoting a resolution or accuracy figure that never accounts for sense-element tolerance and temperature coefficient, amplifier offset and drift, reference error, and common-mode range across the operating window; or fixing a sense element before the accuracy requirement is stated.
- Thermal assertions without numbers: no itemised dissipation, no ambient or airflow assumption, no temperature rise, and no identified thermal-limiting component — just "sufficient copper and thermal vias".
- Declaring the switching loop "minimised" without showing the actual commutation loop geometry, its return path, and how it stays tight once the bulky inductor and connectors claim their space.
- Reading "protected" connectors as satisfied by a chosen part, without naming which fault modes are covered on which port — the brief names neither the threats nor a mechanism, so both are the agent's to state.
