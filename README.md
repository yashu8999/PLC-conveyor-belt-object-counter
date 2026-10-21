# PLC-Based Conveyor Belt Object Counter

<p align="center">
  <strong>An industrial automation prototype for automatic object counting using a Mitsubishi FX5U PLC, photoelectric sensing, and HMI monitoring.</strong>
</p>

<p align="center">
  <img src="Photos/mitsubishi-fx5u-plc-hmi-training-kit.jpeg" alt="Mitsubishi FX5U PLC and HMI training kit" width="700">
</p>

## Project Information

| Item | Details |
|---|---|
| Project title | PLC Based Conveyor Belt Object Counter |
| Department | Electronic & Computer Science |
| Institute | St. John College of Engineering & Management |
| University | University of Mumbai |
| Academic year | 2026–2027 |
| Project guide | Ms. Hiral Patel |
| Project type | Engineering academic prototype |

### Project Team

| Student | Enrollment number |
|---|---|
| Jatin S. Tandel | EU1237009 |
| Preet S. More | EU1237036 |
| Shivam D. Sogale | EU1237044 |
| Yash D. Upadhyay | EU1237041 |

## Overview

Manual counting of products on a conveyor can be slow, repetitive, and vulnerable to operator fatigue and tally errors. This project proposes a compact conveyor system that detects passing objects and counts them automatically using industrial control hardware.

A photoelectric/infrared sensor detects an object on the conveyor and sends a digital signal to a Mitsubishi FX5U Programmable Logic Controller (PLC). The PLC uses edge-triggered ladder logic to update the count. The count is intended to be shown on both an external five-digit digital counter and a Human-Machine Interface (HMI). START, STOP, RESET, and Emergency Stop controls support operation and manual intervention.

The design is intended for a benchtop educational prototype and lightweight objects, rather than heavy industrial loads.

## Objectives

- Design and assemble a compact conveyor mechanism.
- Detect passing objects using a 12–24 V DC photoelectric/infrared sensor.
- Program object counting in Mitsubishi GX Works3 using ladder logic.
- Prevent duplicate counts using a rising-edge pulse and suitable input filtering.
- Display the count on the PLC-connected HMI and external five-digit counter.
- Add motor control, reset functionality, status indication, and emergency stopping.
- Document the hardware, wiring, program logic, testing, limitations, and possible extensions.

## Main Features

- **Automatic object detection:** Contactless optical sensing.
- **PLC-based control:** Mitsubishi FX5U industrial controller.
- **Edge-triggered counting:** `PLS M10` pulse logic and `INC D0` count increment, as specified in the synopsis.
- **Dual count display:** HMI value and external five-digit digital counter.
- **Batch target monitoring:** The design uses data register `D10` for a target value and output `Y3` for a target alert.
- **Operator controls:** START, STOP, RESET, and Emergency Stop.
- **Status and alert outputs:** RUN indication and a buzzer/red alert lamp are included in the proposed design.

## System Architecture

### Material-handling path

```text
Objects placed on conveyor
          |
          v
PVC / rubber conveyor belt
          |
          v
Photoelectric / IR sensor
          |
          v
Mitsubishi FX5U PLC
          |
          +--------------------+
          |                    |
          v                    v
External 5-digit counter     HMI dashboard
```

### Control and power path

```text
230 V AC supply
       |
      2-pole MCB
       |
       +--------------------------+
       |                          |
       v                          v
12 V DC, 5 A SMPS            24 V DC supply
       |                          |
       v                          +--> PLC and sensor
24 V interface relay              +--> control interface
       |
       v
12 V DC conveyor motor

START / STOP / RESET / E-STOP --> PLC inputs
PLC outputs --------------------> motor relay, RUN lamp,
                                  external counter pulse,
                                  target alarm
```

> **Note:** This is a high-level functional representation based on the project synopsis. Verify the actual wiring and output types against the PLC model, hardware manuals, and the assembled prototype before applying power.

## Working Principle

1. The mains supply feeds the power-supply stages through the specified protective circuit.
2. Pressing the START button requests conveyor operation through the PLC logic and motor interface relay.
3. The motor drives the conveyor belt and transports objects past the sensing point.
4. When an object is detected, the photoelectric sensor changes the state of PLC input `X0`.
5. The ladder program uses a rising-edge pulse (`PLS M10`) to create a one-scan counting event.
6. The `INC D0` instruction increments the running count in data register `D0`.
7. PLC output `Y2` generates a pulse for the external five-digit counter, while the HMI reads the count from `D0`.
8. The RESET control clears the count according to the configured ladder logic.
9. STOP or Emergency Stop stops conveyor operation. The Emergency Stop should use an appropriately designed, hardwired safety circuit; PLC logic alone must not be treated as a safety-rated function.

## Hardware and Components

The synopsis specifies the following major components. Confirm the exact model and ratings of every part used in the final build.

| Component | Purpose / specification in synopsis |
|---|---|
| Mitsubishi FX5U PLC | Main controller; processes sensor and operator inputs |
| HMI touchscreen | Displays live count, status, and batch target |
| Photoelectric / IR sensor | 12–24 V DC object detection |
| DC geared motor | 12 V DC, approximately 100 RPM |
| Conveyor belt | PVC/rubber belt, approximately 600 × 180 mm |
| Conveyor chassis | MDF or aluminium frame; approximately 700 mm long |
| Rollers and bearings | Two rollers; four 608ZZ bearings |
| External digital counter | 24 V, five-digit pulse counter |
| 12 V SMPS | 12 V DC, 5 A motor supply |
| 24 V DC supply | PLC and sensor/control supply |
| Interface relay | Isolates PLC control from the motor power path |
| Operator controls | START, STOP, RESET, Emergency Stop |
| Indicator and alarm devices | RUN lamp, red target lamp, 24 V buzzer |
| Protection and wiring | 2-pole MCB, grounding, terminals, ferrules, and suitable wiring |

## PLC I/O and Register Mapping

The following mapping is reproduced from the synopsis. The final hardware wiring and program must be checked against the actual PLC variant and trainer-panel terminals.

| Address | Signal / device | Function |
|---|---|---|
| `X0` | Photoelectric sensor | Detects passing objects |
| `X1` | Green START push button (NO) | Starts/latches conveyor run request |
| `X2` | Red STOP push button (NC) | Stops conveyor |
| `X3` | RESET push button (NO) | Clears count |
| `X4` | Emergency Stop (NC) | Emergency stop input/interlock |
| `Y0` | Motor interface relay | Controls conveyor motor power path |
| `Y1` | Green RUN lamp | Indicates motor running |
| `Y2` | Transistor output | Pulse to external digital counter |
| `Y3` | Red lamp / buzzer | Batch-target alert |
| `D0` | Data register | Current object count |
| `D10` | Data register | Configured batch target |
| `M10` | Internal relay | One-scan edge-triggered count pulse |
| `M20` | Internal bit | HMI reset request, as described in synopsis |

## Software and Tools

- **GX Works3** — PLC ladder-logic development and transfer to the Mitsubishi FX5U.
- **HMI configuration software compatible with the installed HMI** — create the dashboard and map tags to PLC registers.
- **Ethernet communication** — HMI-to-PLC communication as specified in the synopsis.
- **Electrical test equipment** — multimeter and suitable workshop tools for safe verification.

This repository is primarily project documentation and hardware photographs unless PLC source files, HMI project files, and test records are added. Proprietary project files should only be included when available and permitted to share.

## Mechanical Design Specifications

| Parameter | Synopsis specification |
|---|---|
| Conveyor chassis length | Approximately 700 mm |
| Conveyor belt | Approximately 600 mm long × 180 mm wide |
| Conveyor bed width / height | 180 mm / approximately 150 mm |
| Rollers | Two, approximately 25 mm diameter and 180 mm face length |
| Bearings | Four 608ZZ bearings (8 mm ID × 22 mm OD × 7 mm width) |
| Motor | 12 V DC geared motor, approximately 100 RPM |
| Intended objects | Lightweight items, such as plastic objects or caps |

The synopsis includes a belt-size description and a separate chassis length. These refer to different parts of the assembly.

## Power and Safety

The synopsis describes a 230 V AC input, a 2-pole 6 A MCB, a 12 V DC 5 A motor supply, and a separate 24 V DC supply for PLC and sensor/control circuits.

- Mains wiring must be performed and checked by a qualified person.
- Use a correctly rated enclosed AC–DC power supply, appropriate overcurrent protection, and protective earthing.
- Keep mains wiring physically separated from low-voltage signal wiring.
- Verify supply polarity, sensor output type, PLC sinking/sourcing configuration, relay ratings, and counter input compatibility before connection.
- Provide a suitable hardwired Emergency Stop arrangement that removes motor energy independently of ordinary software operation.
- Disconnect power before changing wiring or servicing the conveyor.
- Do not operate the mechanism with exposed moving parts or loose clothing near the belt and rollers.

This repository is educational documentation, not a substitute for a formal machine risk assessment or electrical design review.

## Budget Estimate

The synopsis estimates approximately **₹5,600** for procured items, assuming that high-value laboratory equipment such as the PLC and HMI are available through the college lab kit.

| Item | Approximate estimate (₹) |
|---|---:|
| 12 V DC geared motor | 350 |
| 12 V, 5 A SMPS | 500 |
| Photoelectric / IR sensor | 600 |
| 24 V relay and socket | 200 |
| START / STOP / RESET buttons | 450 |
| Emergency Stop button | 250 |
| Five-digit digital counter | 1,350 |
| Belt and rollers | 800 |
| Chassis material | 600 |
| Panel enclosure and hardware | 500 |
| **Estimated total** | **5,600** |

These are synopsis estimates, not verified current prices or final expenditure. Update the table to reflect actual purchases and any components supplied by the laboratory.

## Testing and Validation Plan

Record actual results in a table or a separate `docs/test-results.md` file. Do not treat target values as measured results unless tests have been performed and documented.

| Test | What to verify | Record |
|---|---|---|
| Sensor detection | Each object is detected reliably | Objects passed / detections |
| Count increment | One count per object | Reference count vs PLC `D0` |
| Duplicate-pulse prevention | No repeated counts while an object remains in the sensing zone | Observed duplicate counts |
| Display synchronisation | HMI and external counter agree | HMI count / external count |
| Reset | RESET clears the count as intended | Before / after values |
| STOP function | Conveyor stops when STOP is activated | Observed response |
| Emergency Stop | Safety circuit removes motor energy | Observed response and inspection |
| Batch target | Alarm activates at configured target | Target / actual activation count |
| Continuous run | Operation remains stable over repeated cycles | Duration, object count, faults |

The synopsis states a target of 100% counting accuracy over 500 cycles and describes expected performance. Report these as **targets or synopsis-reported results** until your own test records substantiate them.

## Limitations

- Intended for a small benchtop demonstration and lightweight objects, not heavy pallets.
- Diffuse optical sensing may need adjustment for surface colour, shape, reflectivity, and ambient conditions.
- Transparent objects may require a different sensor arrangement.
- Count accuracy depends on object spacing, belt speed, sensor positioning, PLC input response, and ladder-logic filtering.
- The current project focuses on counting; automatic sorting and rejection are outside the initial scope.
- HMI functionality depends on correct communication configuration and tag mapping.

## Future Scope

- Add a pneumatic diverter or sorting mechanism.
- Add machine vision for colour, shape, or label inspection.
- Log batch totals and production statistics.
- Add MQTT / Node-RED connectivity for optional remote monitoring.
- Add configurable recipes, shift reports, and fault diagnostics.
- Improve enclosure design, cable management, and maintainability.

## Repository Structure

A suggested layout for this GitHub repository is:

```text
PLC-Based-Conveyor-Belt-Object-Counter/
├── README.md
├── Photos/
│   ├── mitsubishi-fx5u-plc-hmi-training-kit.jpeg
│   ├── control-push-buttons.jpeg
│   ├── 12v-5a-power-supply.jpeg
│   ├── dc-motor.jpeg
│   ├── sensor-component.jpeg
│   ├── conveyor-belt-material-1.jpeg
│   ├── conveyor-belt-material-2.jpeg
│   ├── conveyor-frame-parts.jpeg
│   ├── conveyor-support-pipes.jpeg
│   ├── connecting-wires.jpeg
│   ├── display-module.jpeg
│   └── terminal-connectors.jpeg
├── docs/
│   ├── synopsis.pdf
│   ├── block-diagram.png
│   ├── system-flowchart.png
│   ├── wiring-diagram.png
│   └── test-results.md
└── plc-program/
    └── README.md
```

This is a recommended structure. Add only files that actually exist, and rename image references in this README if your repository uses different filenames. The PLC and HMI source files can be added to `plc-program/` if they are available and shareable.

## Getting Started

This repository can be browsed as a project-documentation resource immediately. To reproduce the physical system:

1. Review the synopsis and verify the final component models.
2. Fabricate and align the chassis, rollers, bearings, belt, and motor.
3. Have the power and safety circuits reviewed before energising the equipment.
4. Wire the PLC inputs and outputs according to the verified terminal map.
5. Develop and test the ladder program in GX Works3.
6. Configure HMI communication and register mapping.
7. Calibrate sensor sensitivity and test with known object counts.
8. Record results, limitations, and changes from the original design.

No software installation command is provided because this is a PLC hardware project rather than a conventional web application. GX Works3 and HMI configuration tools must be obtained through their respective official channels and compatible with your hardware.

## Project Documentation

Place the full synopsis PDF in `docs/synopsis.pdf` and add diagrams or test results when ready. Suggested documentation:

- `docs/block-diagram.png`
- `docs/system-flowchart.png`
- `docs/wiring-diagram.png`
- `docs/test-results.md`
- `plc-program/README.md`

## References

1. Mitsubishi Electric Corporation, *MELSEC iQ-F FX5U User's Manual (Hardware)*, JY997D55301, 2021.
2. Mitsubishi Electric Corporation, *MELSEC iQ-F FX5 Programming Manual (Program Design)*, JY997D55701, 2022.
3. Frank D. Petruzella, *Programmable Logic Controllers*, 5th ed., McGraw-Hill Education, 2016.
4. W. Bolton, *Programmable Logic Controllers*, 6th ed., Newnes / Elsevier, 2015.
5. International Electrotechnical Commission, *IEC 61131-3: Programmable Controllers — Programming Languages*, 2013.
6. Omron Corporation, *Industrial Sensors Technical Guide: Photoelectric Sensors*, 2021.

Additional references are listed in the academic synopsis.

## Acknowledgement

We thank our project guide, **Ms. Vishakha Rane**, and the Department of Electronic & Computer Science at St. John College of Engineering & Management for their guidance and support.

---

<p align="center">
  <em>PLC Based Conveyor Belt Object Counter · Academic project · 2026–2027</em>
</p>
