# Sources — Solar MPPT Battery Charger

The evidence this board's design will have to cite. **Classes of document, not
documents:** the specific parts are not chosen yet, so naming a datasheet here
would be choosing one.

A number that reaches the board carries its provenance: source, document id or
URL, retrieval date, units, and the condition it applies under. A number without
that is not evidence, and no live network lookup may change a validation or
release result.

| Kind of source | What the design needs from it |
|---|---|
| Photovoltaic module datasheet / panel electrical characteristics | The brief says only "nominal 12 V" panel. Voc, Isc, Vmp, Imp and their temperature coefficients define the real input window the converter and its protection must cover. |
| Lithium cell and pack charging data | Per-cell voltage and current limits, and termination behaviour, set the output target for whichever of the 3S or 4S configurations the board serves, and the regulation the charger must enforce. |
| Converter controller datasheet and its layout/reference-design guidance | Whatever controller implementation is chosen supplies the compensation, timing, and layout constraints the switching-loop layout has to satisfy. |
| Power inductor datasheet | Saturation current, DCR, and core loss at the chosen frequency and ripple decide whether the magnetics support the roughly 5 A charge current at the resulting inductor current, and how much heat they add. |
| Power switching device and rectifier datasheets | On-resistance, gate charge, safe operating area, and junction-to-ambient thermal resistance drive both the loss budget and the thermal design. |
| Current-sense element and amplifier datasheets for the sensing method chosen | Sense-element tolerance and temperature coefficient, amplifier offset, drift, common-mode range and bandwidth are the terms in the sense-accuracy error budget, whichever sensing method the agent selects. |
| MCU datasheet and ADC electrical characteristics | ADC resolution, reference accuracy, sampling rate, timer/PWM capability, and package thermal data determine whether the MCU can close the intended loops. |
| Voltage reference and rail regulator datasheets | Measurement accuracy is bounded by the reference, and the housekeeping rails must start and hold across the panel and battery operating range. |
| Connector datasheets | Current rating, wire gauge acceptance, keying, retention, and body dimensions constrain both the roughly 5 A interface and the placement around bulky connectors. |
| Conductor sizing and temperature-rise reference data | Trace width, copper weight, and via count for the high-current paths must be justified against current-capacity curves rather than asserted. |
| Fabricator capability and stackup documentation | Minimum trace/space, available copper weights, dielectric thicknesses, and via options bound what the high-current and switching-node layout can do on the stackup finally chosen. |
| Creepage, clearance, and protection-device data | Whatever implements the required connector protection needs rated clamping, interrupt, or standoff data, plus the spacing rules for the voltages present. |

## Recording a source, once one is chosen

Replace the class with the actual document — manufacturer, part number, revision
and date — and state the fact taken from it, in the units the document uses.
Keep the class row: it says why the document was needed.

JLCPCB-wide process limits are **not** recorded here. They live in the toolkit's
`profiles/jlcpcb/`, with their own provenance; this board records only its own
tighter targets and its own selected options. A limit copied into two places is
a rival threshold, and the toolkit has a gate that says so.
