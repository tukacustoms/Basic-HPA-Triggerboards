🧾 Triggerboard Pin Orientation Issue — Discovery & Correction Notes
1. Summary of the Issue
During initial hardware validation of the trigger input, it was discovered that the 5-pin JST connector footprint was implemented with the pin numbering reversed (mirrored) in the PCB layout.
Instead of the functional layout being:        
1 → Nozzle  
2 → Poppet  
3 → Auto  
4 → Trigger  
5 → +5V                  
the PCB was routed as:
1 → +5V  
2 → Trigger  
3 → Auto  
4 → Poppet  
5 → Nozzle 

This created a 5↔1, 4↔2 inversion, while Pin 3 remained correct.

2. How the Issue Was Identified
A direct test was performed by shorting the expected trigger pad to the expected +5V pad on the JST footprint using tweezers.
No trigger response occurred with the correct orientation.


When the JST plug was physically reversed, the FCU successfully detected the trigger.


This confirmed that the board routing was functionally correct, but pin numbering on the JST footprint did not match the FCU standard.

3. Root Cause
The JST footprint used in Fusion 360 had the following mismatch:
The physical orientation of Pin 1 (typically indicated by a chamfered corner or marker)
 did not match the assumed logical Pin 1 used during schematic-to-PCB linking.


As a result, internal board nets (VCC, Trigger, Auto, Poppet, Nozzle) were routed to incorrect pins.
This misalignment was not previously visible because:
The footprint silkscreen lacked a Pin 1 indicator.


All nets were valid and routing completed successfully.


Schematic labeling did not reflect physical orientation.



4. Technical Impact
Because of reversed mapping:
Trigger Line was not connected to its proper FCU input → trigger did not register.


Debug LEDs connected to Poppet/Nozzle lines would have been reading incorrect pins.


Incorrect routing weights caused confusion (e.g., thicker traces for power vs thinner for I/O).


Despite this, the board was electrically functional.
 The failure mode was purely pin mapping, not circuitry.
