# Architecture — Solar MPPT Battery Charger

**A worksheet, not a design.** Every line below is a question this board has to
answer, and none of them is answered here. Nothing in this file is a
recommendation, and the order of the sections carries no preference.

The questions were derived from [the brief](../BRIEF.md) and from what this
board is meant to stress in the benchmark:

- buck power stage
- current sensing
- wide voltage range
- thermal

Those are the places where a wrong answer shows up in copper.

Answer them in this file as the design is made, each answer carrying the
evidence that supports it, and record the corresponding choice against its
`OPEN-nn` entry in [board/requirements.md](../board/requirements.md). An answer
without evidence is a guess wearing a document's clothes — and this benchmark is
allowed to refuse an unsupported claim rather than invent one.

## Panel input and operating window

- What open-circuit voltage, short-circuit current, and operating-voltage range is this board designed to accept from a nominal 12 V panel, and what evidence supports those numbers?
- What happens electrically when the panel voltage sits below the minimum tracking point, or above the designed ceiling?
- Which pack configuration is the input window designed against, and if more than one is served, does the window have to change between them?
- What panel-side transient and mis-connection conditions must the input survive, and where in the chain is each handled?
- How is panel power measured accurately enough for the tracking algorithm to act on it?

## MPPT power stage topology and control

- Which converter topology is chosen, and what specifically makes it the right answer for a nominal 12 V panel feeding the chosen pack configuration?
- Can the chosen topology deliver the required conversion direction across the full output range it is asked to serve, or does the input/output ratio cross over somewhere in the operating envelope?
- Does the MPPT loop run in MCU firmware, in a dedicated controller, or split between them, and where does control authority sit when firmware is not running?
- What switching frequency is chosen, and what trades (magnetics size, loss, EMI, loop bandwidth) drove it?
- What are the inner regulation loops, and how does the MPPT tracking loop sit above them?
- How will the tracking algorithm's convergence and stability be shown rather than asserted?

## Battery charge management

- Is the board built for a 3S pack, a 4S pack, or both, and what evidence and trade drove that choice?
- If both are served, how is the pack configuration established — hardware strap, firmware setting, measurement, or something else — and what does that cost?
- What charge regulation and termination behaviour does the design implement, and what per-cell limits does it enforce?
- How does the board behave with a deeply discharged pack, a disconnected pack, or a pack connected before the panel?
- Is reverse current from the battery back into the converter possible in any state, and what prevents it?
- What does the board do when the battery is at target and the panel is still producing?

## Current and voltage instrumentation

- What sensing method is chosen for each of the two current measurements, and what does the accuracy and bandwidth the control scheme needs demand of that choice?
- Where in the current path is each current measurement taken, and what does that placement mean for what the measurement actually represents?
- What conditioning chain follows the sense element, and what is the end-to-end error budget from element tolerance and drift through amplifier offset to ADC reference?
- What common-mode voltage does each sense node see across the full operating window, and does the chosen conditioning survive it?
- What bandwidth does each measurement need — for control, for protection, and for reporting — and are those the same signal path?
- How are the sense returns routed so that high-current drops in the power return do not appear in the measurement?
- Is any calibration step required in production, and what access does it need?

## MCU, firmware control, and housekeeping power

- What ADC channels, timer/PWM outputs, and comparator or protection inputs does the control scheme demand of the MCU?
- Which rails does the board generate internally, from which source, and what powers the MCU when the panel is dark?
- What is the power-up and power-down sequence, and what is the state of the switching devices during each?
- What happens to the power stage if firmware hangs or the MCU resets mid-conversion?
- Is the MCU programmed through the communications/debug port or through separate access?

## Connectors, protection, and high-current interfaces

- Which fault and stress modes is each of the panel and battery connectors protected against, and by what mechanism at what point in the chain?
- What connector types are selected, what wire size and current do they accept, and how are they keyed against mis-connection?
- How does the protection behave at full rated current without stealing headroom or accuracy from the sense chain?
- Is any protection element in the main current path, and what does it cost in dissipation there?
- How do the connector bodies constrain placement and copper routing on the board?

## Switching-loop layout and stackup

- Which physical loop is the high-di/dt commutation loop for the chosen topology, and how small is it actually made?
- Is the metadata's likely 4-layer count adopted, what layer roles does the chosen stackup assign, and what copper weight do the panel-side and battery-side currents require at the duty they actually run at?
- How are the switching node and its return kept away from the sense and MCU analog nodes?
- Where are the reference points for the controller, the sense chain, and the power return joined?
- What via count and placement carries the high current between layers, and what evidence sizes it?
- Does the layout leave room for the inductor and connectors without pushing the switching loop apart?

## Thermal design

- What is the worst-case dissipation, itemised per component, at the operating point that produces it?
- What ambient, airflow, and mounting conditions is the design assumed to work in, and who decided those?
- By what path does that heat leave the design — board copper and plane connection, thermal vias, a heatsink, enclosure conduction, airflow, or some combination — and what temperature rise does the chosen path produce?
- Which single part is the thermal limit, and how much margin does it have?
- How does the thermal solution interact with the demand to keep the switching loop tight and the connectors near the edge?

## Communications and debug port

- What electrical interface and protocol does the port use, and what is it expected to carry — telemetry, configuration, firmware, or all three?
- Is the port referenced to the battery return, and what does that mean for a connected host?
- What protection does the port itself need, given it may be plugged in while the power stage is running?
- What connector is used, and where does it sit relative to the power connectors?

## Mechanical envelope and placement

- What board outline and dimensions are chosen, and what drove them?
- Where do the panel connector, battery connector, inductor, and debug port sit, and what placement conflicts did that resolve?
- Are there mounting holes or keepouts that an enclosure or heatsinking demands, and what fixes them?
- Is any component tall enough or heavy enough to constrain assembly or the opposite side of the board?

## Validation and bring-up

- What test points give access to the switching node, both sense chains, and the internal rails without disturbing the layout?
- What is the bring-up order for a first article, and what is powered first?
- Which claims — efficiency, tracking, sense accuracy, temperature rise — will be measured, and which are only simulated?
- What simulation is run on the power stage and the control loop, and what must it show before layout is committed?
- How is the roughly 5 A charge capability demonstrated rather than asserted?

## Repository and toolkit boundary

- What configuration for this board lives here, and what generic capability belongs in the shared PCBA_AutoDesignAndTest toolkit?
- Which generated search, routing, and simulation outputs are disposable, and which are promoted and committed?
- How are the open decisions in this scaffold tracked from open to decided, with evidence attached?
- What in the design would have to change if the assumed panel operating window turned out wrong?

## Answers still owed

All of them. See [status.md](status.md).
