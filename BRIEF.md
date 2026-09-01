# PCBA_Solar_MPPT_Controller — Solar MPPT Battery Charger
## Design brief

Design an MPPT solar charger for a nominal 12 V photovoltaic panel charging a 3S or 4S lithium battery pack at up to roughly 5 A. Include input/output current and voltage measurement, an MCU, protected battery and panel connectors, and a communications/debug port. Choose the exact MPPT power topology and controller implementation. The benchmark should stress high-current routing, analog current sensing, switching-loop layout, thermal management, and placement around bulky connectors/inductors.

## Functional requirements

- One assembly shall charge both 3S and 4S packs, with the active configuration readable over the comms port.
- Charge shall be constant-current then constant-voltage, and pack limits shall override the maximum power point.
- Tracking shall re-acquire after an irradiance step, and the pack shall not back-discharge into the panel.

## Power stage and rails

- Rated output current shall be delivered continuously at maximum ambient within every part's derated ratings.
- The stage shall convert from cold-condition panel open-circuit voltage down to its topology's usable minimum.
- The inductor shall not saturate at any fault-limited peak, and the housekeeping rail shall hold up from either source.

## Measurement and control

- All four quantities shall reach the MCU without clipping at cold panel Voc, at 4S termination or at rated current plus ripple.
- Accuracy shall resolve the power difference between adjacent tracker operating points; the resolution shall be stated.
- Shunts shall be Kelvin-connected over the full sense common-mode range, and switching ripple shall not alias into the tracking loop.

## Protection and fault behaviour

- Reversed panel or battery connection shall neither damage the board nor present a sustained short, and either connector shall tolerate hot-plug with bounded inrush.
- Battery-terminal overcurrent, short circuit and overvoltage shall be bounded by hardware independent of firmware.
- Power-stage temperature shall be readable by the controller, with fold-back or shutdown above a stated limit.

## Layout, thermal and mechanical

- Each commutation loop shall close on adjacent layers with minimum enclosed area, excluding sense and feedback routing.
- Sense pairs shall run differentially clear of the switch node and inductor field, and power return shall not share copper with the analog reference return.
- Copper width, weight and via count along the panel-to-battery path shall carry rated current within a stated temperature rise, and heat-spreading copper shall remain continuous.
- Connector and inductor placement shall preserve mating and tooling clearance and keep insertion force off power-stage joints.

## Test and debug access

- The comms/debug port shall expose the four measurements, controller state and latched faults, and allow firmware update in place.
- Tracking shall be disableable at a fixed operating point, and charge inhibitable.
- Test points shall reach the switch node with a short ground return, both sense inputs and the rails.

## Open choices

- Power topology, and whether rectification is synchronous.
- Controller implementation (MCU-timed PWM, supervised dedicated controller, or hybrid) and the MCU itself.
- Current-sense method and location, comms/debug port type, 3S/4S selection mechanism, stack-up, outline and connector families.
