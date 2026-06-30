
# Effluent / Water Treatment Sequencing System

PLC-based automation project for a multi-stage wastewater treatment process, developed during the **SCoE-MIT Summer Internship 2026** (Siemens Centre of Excellence, MIT Campus, Anna University, Chennai) under the theme **PLC with HMI**.

## Overview

This project automates the sequential operations of an Effluent Treatment Plant (ETP) — collection, transfer, pH correction, aeration/mixing, settling, and discharge — using a Siemens S7-1200 PLC program, simulated controller execution, and a 3D virtual SCADA/HMI front-end.

The system continuously monitors tank levels and pH, and automatically drives pumps, valves, and a mixer through each treatment stage while enforcing safety interlocks and alarm handling for abnormal conditions.

## Tools Used

| Tool | Purpose |
|---|---|
| Siemens TIA Portal V15.1 | Ladder logic development, S7-1200 CPU/I-O configuration, tag management |
| PLCSIM V15.1 | Virtual S7-1200 controller execution (no physical hardware) |
| Factory I/O | 3D plant mimic acting as the SCADA/HMI interface |

## System Architecture

```
Input Devices  →  PLC (S7-1200 CPU)  →  Output Devices
(level sensors,     - Sequencing logic     (valves, dosing
 pH sensor,          - pH dosing control     pumps, mixer,
 push buttons,       - Timers / interlocks   discharge valve,
 E-Stop)             - Alarm & fault logic   indicators, siren)
                            ↕
                    SCADA / HMI (Factory I/O)
                    Visualization & operator control
```

## Key Features

- **Sequential batch control**: collection → transfer → pH correction → aeration → settling → discharge → reset
- **Analog pH-based dosing**: acid/alkali pumps triggered by continuous comparison against pH limits (target pH = 7)
- **Timer-driven stages**: 45 s transfer, 10 s mixing, 20 s settling, 40 s post-discharge reset
- **Safety interlocks**: overflow protection, discharge blocking on out-of-range pH, dry-run protection, Emergency Stop with manual delatch
- **Alarm management**: overflow alarm/siren, long-wait/pH-misbehaviour alarm (30-minute watchdog)
- **Manual reset** function for returning the system to its initial state

## PLC Program Structure

The ladder logic program is organized into 13 networks in TIA Portal:

1. Factory I/O integration (FC9000 communication block)
2. Start/Stop/Mode selection and inlet valve control
3. Transfer valve activation via level sensor
4. Reaction tank readiness (Tank2Ready)
5. pH correction dosing (acid/alkali)
6. Discharge permission bit and mixer indicator
7. Mixer and discharge valve sequencing (timers)
8. Post-discharge reset logic
9. Safety measures and alarm logic
10. Manual reset button
11. Long-wait time / pH misbehaviour monitoring
12. Emergency Stop function
13. Analog signal conversion (REAL → DINT) for pH value

## I/O Tag Summary

| Tag | Type | Description |
|---|---|---|
| Start / Stop / Estop | DI | Operator controls |
| Auto_Man | DI | Mode selector |
| Coll_High / React_High | DI | Tank level sensors |
| Discharge_Permit | DI | External discharge permission |
| Overflow_tank0 / tank1 | DI | Overflow detection |
| pH_Value | AI | Reaction-tank pH |
| Inlet_Valve | DO | Raw inlet control |
| Transfer_Pump | DO | Collection → reaction transfer |
| Acid_Dose / Alkali_Dose | DO | Dosing pumps |
| Mixer | DO | Aeration & mixing |
| Discharge_Pump | DO | Treated-water discharge |
| Beacon (Active) | DO | Status light |

## Results

The system was validated end-to-end in PLCSIM with Factory I/O under both normal and fault conditions:

- Correct sequencing through filling, transfer, dosing, aeration, settling, and discharge
- Acid/alkali dosing pumps responded correctly to simulated pH variations and stopped at pH = 7
- Mixing (10 s) and settling (20 s) timers correctly gated discharge
- Overflow and out-of-range pH conditions correctly triggered alarms and blocked discharge
- Emergency Stop immediately halted operations and required manual delatch to resume
- Manual reset correctly cleared all status bits for the next batch

## Future Work

- Continuous (non-batch) treatment mode
- PID-based pH control instead of ON/OFF dosing
- Turbidity and dissolved oxygen (DO) sensor integration
- Automated daily/shift-wise compliance reporting
- Multi-tank cascade operation for higher capacity
- Deployment on physical S7-1200/S7-1500 hardware
- Remote/cloud-based SCADA monitoring

## Author

**Devanathan J**
Department of Electronics and Communication Engineering
Meenakshi College of Engineering, Chennai

Completed as part of the SCoE-MIT Summer Internship 2026, in collaboration with the Department of Instrumentation Engineering, MIT Campus, Anna University, Chennai.

## References

1. Bolton, W., *Programmable Logic Controllers*, 7th Edition, Newnes Publications, 2021.
2. Petruzella, F.D., *Programmable Logic Controllers*, 5th Edition, McGraw-Hill Education, 2020.
3. Webb, J.W. and Reis, R.A., *Programmable Logic Controllers: Principles and Applications*, 6th Edition, Pearson Education, 2015.
4. Siemens AG, *SIMATIC S7-1200 Programmable Controller System Manual*, 2023.
5. Siemens AG, *TIA Portal Programming Manual*, 2023.
6. Siemens AG, *WinCC Unified Engineering Manual*, 2023.
7. CODESYS GmbH, *CODESYS Development System V3 User Manual*, 2024.
8. OPC Foundation, *OPC Unified Architecture (OPC UA) Specification*, 2023.
9. Real Games, *Factory I/O User Guide and Documentation*, 2024.
10. Siemens Centre of Excellence (SCoE-MIT), *Training Materials on PLC, HMI, OPC UA Communication and Industrial Automation Systems*, MIT Campus, Anna University, Chennai, 2026.
