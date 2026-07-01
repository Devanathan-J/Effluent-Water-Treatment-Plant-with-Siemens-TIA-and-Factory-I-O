Demonstration Video Link:https://drive.google.com/file/d/1ptVHT0u3w82y4y3ldyzapwxQC-b-0Qhu/view?usp=sharing



> Remember to push the reset button in factory io if you are not restarting the plcsim but restarting the factory io , inorder to resume the system properly vis versa for force stop button too if it is not responding. 

>------------MORE DETAILS-------------<

Problem Statement/Topic :Effluent / Water Treatment Sequencing
AIM
Control a multi-stage effluent treatment process - collection, pH-correction dosing, aeration/mixing, settling, and discharge - driven by level and pH, with pump sequencing and protective interlocks.
REAL-TIME SCENARIO
An effluent treatment plant receives wastewater into a collection tank, corrects pH by dosing acid or alkali, aerates/mixes in a reaction tank, allows settling, then discharges clarified water only if it is within limits. Levels and pH govern the stage transitions.
SEQUENCE OF OPERATION
1.	The collection tank fills via the inlet; at high level a transfer pump moves water to the reaction tank.
2.	Read pH and dose acid or alkali (pulsed dosing pump) until pH is within band (e.g., 6.5-8.5).
3.	Run the aerator/mixer for a set time, then stop and allow settling for a set time.
4.	If final pH is within limits, open discharge / run the discharge pump; otherwise recirculate or hold and 	raise an alarm.
5.	Repeat per batch, logging treated volume and pH.

CONTROL LOGIC (BACK-END)
•	Stage sequencer: Fill -> Transfer -> Dose -> Aerate -> Settle -> Test -> Discharge.
•	pH control: dose acid if pH high, alkali if low, with a deadband.
•	Timers for aeration and settling; batch counter with pH/volume logging.
INTERLOCKS & SAFETY
•	No transfer when the reaction tank is high.
•	No discharge unless pH is within limits AND the discharge permit is present.
•	Dosing only during the reaction stage with the mixer running; pumps must not run dry; E-Stop drops all outputs.
ALARMS & FAULTS
•	pH out-of-limits (discharge blocked).
•	Tank high / overflow.
•	Dosing-pump fault or dosing-time exceeded (chemical fault).
•	Aeration/settling timeout.
•	Discharge blocked.
HMI / SCADA (FRONT-END)
•	Full plant mimic (tanks, pumps, dosing, aerator) with live levels and pH.
•	Current-stage indicator and pH trend.
•	Dosing status, batch log (pH in/out, volume, time).
•	Alarm + event log, manual device control, mode.
FACTORY IO / IMPLEMENTATION NOTES
Build a custom multi-tank scene or simulate levels and pH in the PLC; this is strongest as a SCADA, sequencing and analog-control exercise.
OPERATING MODES
Auto (batch), Manual (individual devices), Reset.
STRETCH GOALS
•	Continuous (non-batch) mode.
•	PID dosing on pH.
•	Turbidity/DO sensors.
•	Daily compliance report.
•	Multi-tank cascade.
