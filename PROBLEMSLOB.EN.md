# 🧾 Triggerboard Pin Orientation Issue

## 1. Summary

During the initial hardware validation of the trigger input, it was discovered that the 5-pin JST connector footprint had been implemented with the pin numbering mirrored in the PCB layout.

The intended and functional mapping was:

| Pin | Correct Function |
|---|---|
| 1 | Nozzle |
| 2 | Poppet |
| 3 | Auto |
| 4 | Trigger |
| 5 | +5V |

However, the PCB was routed as:

| Pin | Actual Routed Function |
|---|---|
| 1 | +5V |
| 2 | Trigger |
| 3 | Auto |
| 4 | Poppet |
| 5 | Nozzle |

This created a 5↔1 and 4↔2 inversion, while Pin 3 remained correct.

---

## 2. How the issue was identified

A direct functional test was performed by shorting the expected trigger pad to the expected +5V pad on the JST footprint using tweezers.

- No trigger response was detected when using the expected orientation.
- When the JST plug was physically reversed, the FCU successfully detected the trigger.

This confirmed that the board routing was electrically correct, but the pin numbering on the JST footprint did not match the FCU standard.

---

## 3. Root cause

The JST footprint used in Fusion 360 had a mismatch between the physical orientation of Pin 1 and the logical Pin 1 assumed during schematic-to-PCB linking.

In practice:

- the physical orientation marker (chamfered corner / pin-1 indicator) did not match the assumed board reference,
- internal board nets for VCC, Trigger, Auto, Poppet, and Nozzle were routed to the wrong connector pins,
- the problem was not obvious earlier because the footprint silkscreen lacked a clear Pin 1 indicator,
- all nets were still valid and routed successfully,
- schematic labeling did not clearly reflect the physical connector orientation.

---

## 4. Technical impact

Because of the reversed mapping:

- the Trigger line was not connected to its proper FCU input, so the trigger did not register,
- debug LEDs connected to the Poppet / Nozzle lines would have read the wrong signals,
- routing confusion occurred because some traces were weighted as if they were power or signal lines without matching the intended physical mapping,
- despite the issue, the board remained electrically functional and the failure mode was limited to pin mapping rather than circuit design itself.

---

## 5. Correction and outcome

The fix was to correct the JST connector footprint orientation so the board pin numbering reflected the actual FCU mapping.

This ensured:

- correct trigger detection,
- accurate signal routing for Auto / Poppet / Nozzle,
- consistent physical and logical connector behavior,
- a reliable board that matches the expected hardware standard.

---

## 6. Final note

The issue was not caused by a circuit-level failure, but by a footprint orientation mismatch between the PCB implementation and the connector’s physical pin numbering. Once corrected, the triggerboard operated as expected.
