# RJX Training Kit — PCB Layout Reference Manual

**Document type:** Engineering reference  
**Stackup:** 4-layer SIG / GND / GND / SIG  
**DocID:** RJX-PCB-LAYOUT-REF  
**Source:** Converted from the ST-style reference manual DOCX

## Revision history

| Revision | Date | Description |
|---|---|---|
| 1.1 | 2026-03-06 | Added clickable training video links and references section. |
| 1.0 | 2026-02-28 | Initial release: consolidated layout rules, video takeaways, and schematic-specific guidance for starting PCB design. |

## 1. Introduction

This document provides a practical PCB layout reference for the RJX Training Kit design. It is written as a working manual for creating the first PCB layout from the latest KiCad 9 schematic. The focus is signal integrity, EMC/EMI robustness, power integrity, and manufacturable implementation on a 4-layer SIG/GND/GND/SIG stackup.

### 1.1 Purpose

The purpose of this manual is to define layout intent, priorities, and verifiable rules so that the PCB implementation matches the electrical intent of the schematic and behaves predictably in bring-up and testing.

### 1.2 Scope

- Applies to PCB layout of the RJX Training Kit using a 4-layer SIG/GND/GND/SIG stackup.
- Covers placement, routing, planes, decoupling, CAN routing, battery charger power-path layout, and thermal considerations.
- Does not include enclosure/mechanical integration beyond basic placement and connector accessibility considerations.

### 1.3 Reference videos (training sources)

The design guidance in this manual is aligned with the following training videos supplied by the project owner. They are referenced by number throughout the document:

- **V1:** [EEVblog #176 – Lithium Ion/Polymer Battery Charging Tutorial](https://www.youtube.com/watch?v=A6mKd5_-abk)
- **V2:** [KatKimShow – Ohmic/Linear Region Operation of MOSFET](https://youtu.be/psm6z11gQHM?si=_SXVJzHC-8dOen4S)
- **V3:** [Jonathan Currie – Power Supplies: Heat Sinking and Thermal Considerations](https://youtu.be/62B8vy_SmIw?si=c1s2OIpJ7ubwtUgC)
- **V4:** [EEVBlog – Electronics Thermal Heatsink Design](https://youtu.be/8ruFVmxf0zs?si=be-NMTI2Bkt7ZpK1)
- **V5:** [W2AEW – Transmission Line Termination for Digital and RF Signals](https://youtu.be/g_jxh0Qe_FY?si=Ww_zA6snRCSIAuPj)
- **V6:** [W2AEW – Power Supply Decoupling and Filtering](https://youtu.be/9EaTdc2mr34?si=7K7oemsxKq56t8Vc)
- **V7:** [LC Oscillator](https://youtu.be/nh4q7mIhLrY?si=ASXD_tC_YMh5RTSi)
- **V8:** [Transmission Line Return Current](https://youtu.be/CDqref32tPU?si=Ev1lt_kskHVetqsp)
- **V9:** [Rick Hartley – How to achieve proper grounding](https://www.youtube.com/live/ySuUZEjARPY?si=D93KltMv5KR9p3M2)
- **V10:** [Phil’s Lab – PCB stack up and build up](https://youtu.be/QAOEtfvCaMw?si=8B5PLt3KHyhGxZyl)

### 1.4 Abbreviations

| Abbreviation | Meaning |
|---|---|
| EMC/EMI | Electromagnetic compatibility / interference |
| SI | Signal integrity |
| PI | Power integrity |
| CC/CV | Constant current / constant voltage charging profile |
| LDO | Low-dropout linear regulator |
| TVS | Transient voltage suppressor (ESD protection) |
| GND | Ground (0 V reference plane) |

## 2. System overview (layout-relevant)

The RJX Training Kit integrates a microcontroller core with power regulation, battery management, sensors, user interface peripherals, and CAN connectivity. Layout should preserve clean return paths and isolate high-current loops from sensitive references.

### 2.1 Functional blocks

| Block | Main functions | Primary layout concern |
|---|---|---|
| Input + 3.3 V regulation | 5 V barrel input; LDOs for +3V3 and +3V3A | Short high-current loops; thermal spreading; low-impedance ground return |
| MCU core | STM32L431 + crystal + SWD | Decoupling loop inductance; crystal cleanliness; controlled return paths |
| Battery management | Li-ion charger/power-path; battery connectors; gauge | Compact BAT/OUT loops; sense routing; keep away from fast/noisy loops |
| Interfaces | CAN; SPI; I2C/SMBus | Pair integrity; avoid stubs; continuous plane reference |
| UI loads | LCD, 7-seg, buzzer, LEDs, jumpers | Keep switching currents local; avoid coupling into analog/clock region |

### 2.2 Design targets

- 4-layer stackup: Top signal, Inner1 solid GND, Inner2 solid GND, Bottom signal.
- Continuous reference planes (L2/L3) under fast-edge signals: CAN, SPI, SWD, clock.
- Low-noise implementation with controlled return currents and minimized loop areas.
- Manufacturable constraints based on the selected PCB vendor’s standard 4-layer process.

## 3. Stackup and plane strategy

### 3.1 Recommended stackup

| Layer | Type | Primary use | Notes |
|---|---|---|---|
| L1 (Top) | Signal | Placement and routing | Prefer short routes; keep critical nets here when possible |
| L2 (Inner1) | GND plane | Reference for L1 | Keep solid and uninterrupted; avoid splits/voids |
| L3 (Inner2) | GND plane | Reference for L4 | Keep solid and uninterrupted; avoid splits/voids |
| L4 (Bottom) | Signal | Secondary routing | Use for escape routing and secondary interconnects |

### 3.2 Plane rules (mandatory)

- Do not route signals on L2 or L3 unless there is a justified exception.
- Do not create ground plane splits under fast-edge nets (V5, V8, V9).
- If a signal transitions between L1 and L4, provide a nearby GND stitching via (V8).
- Stitch GND near board edges and near external connectors to control EMI and provide low-impedance returns (V9).

## 4. Placement rules

### 4.1 Global placement priorities

1. Place the MCU first. Reserve a clean local region for crystal + decoupling + SWD access.
2. Place power regulation next (5 V entry, LDOs). Place input/output capacitors immediately adjacent to regulator pins.
3. Place the battery charger/power-path IC close to the battery connector(s) and high-current components (V1).
4. Place the CAN transceiver close to the CAN connector(s). Place termination/jumper close to connector end (V5).
5. Place high-current/noisy UI loads away from the crystal/analog domain; keep their returns local.

### 4.2 “Must-be-close” placement clusters

- Regulator clusters: Regulator + Cin + Cout + local GND vias (tight loop).
- MCU cluster: VDD decouplers at pins + bulk cap nearby + crystal and load caps at pins.
- Charger cluster: charger + BAT/OUT caps + shunt/sense components on shortest path.
- CAN cluster: transceiver + connector + termination option; minimal stubs.

## 5. Power integrity and decoupling

### 5.1 Decoupling rules (mandatory)

- Place 100 nF decouplers as close as possible to each IC power pin group (V6).
- Connect each decoupler to GND with a short via directly to L2 or L3.
- Place a bulk capacitor (few µF to 10 µF) per rail per local region.
- Use pours/wide traces for +5 V, +3V3, +3V3A, BAT, and OUT rails; avoid long thin bottlenecks.

### 5.2 LDO layout + thermal (V3, V4)

- Keep Cin and Cout adjacent to LDO pins; route with shortest possible traces.
- Provide copper area on output and GND pins for heat spreading.
- Use multiple thermal vias from regulator GND/copper region to L2/L3.
- Validate worst-case dissipation: `P ≈ (VIN − VOUT) × IOUT`.

### 5.3 Charger/power-path layout (V1)

- Treat BAT/OUT and charger current paths as a high-current loop: short, wide copper; minimal vias.
- Place charger capacitors at the pins; connect to GND plane with short vias.
- Keep gauge/sense and I2C traces away from the noisiest current loop areas where possible.

## 6. Signal integrity and EMC/EMI

### 6.1 Return current control (V8, V9)

- Route fast-edge signals over continuous GND reference (L2 for L1, L3 for L4).
- Do not cross plane voids; reroute instead of accepting return-path detours.
- When changing layers, add a nearby GND stitching via to keep the return loop tight.

### 6.2 CAN routing rules (V5, V8)

- Route CANH/CANL as a differential pair: same layer when possible, same number of vias, similar length.
- Keep the pair tightly coupled; avoid running through the crystal region or near high-current switching loops.
- Avoid stubs. Put termination resistor/jumper near connector end so enabling termination makes the PCB behave as an endpoint.
- If CAN goes off-board, reserve footprint/space for ESD/TVS at the connector in a future revision (recommended).

### 6.3 SPI, SWD, and clock routing

- Keep SWDIO/SWCLK short and referenced to GND; avoid stubs and unnecessary vias.
- Keep SPI traces short; add series source resistors if needed for long traces (V5).
- Crystal traces must be very short and symmetric; keep away from noisy loops (V7).

## 7. Thermal design guidance

Thermal design is addressed using the thermal resistance method described in V3 and V4: estimate dissipation and ensure copper area and via stitching maintain junction temperatures within limits.

### 7.1 Practical rules

- Give regulators and power resistors a copper pour region for heat spreading.
- Use thermal vias to connect hot copper to inner planes (heat + impedance).
- Keep temperature-sensitive parts away from hot regions when feasible.

## 8. Implementation workflow (KiCad 9)

### 8.1 Setup

1. Select PCB vendor and obtain standard 4-layer stackup parameters (dielectric thicknesses, copper weights).
2. KiCad: **Board Setup → Stackup**. Configure layers Top/GND/GND/Bottom and set thickness values.
3. KiCad: **Board Setup → Design Rules**. Create net classes: Power, CAN diff pair, Fast digital (SPI/SWD/clock), General.
4. Create zones: L2 = GND, L3 = GND. Ensure zones produce solid planes (no unnecessary cutouts).

### 8.2 Routing order (recommended)

1. Route power first: 5 V entry, LDO outputs, BAT/OUT high-current paths.
2. Route CAN differential pair next and verify minimal stubs and clean reference.
3. Route MCU essentials: crystal, reset/boot, SWD.
4. Route remaining buses (SPI/I2C), then remaining GPIO/UI nets.
5. Run DRC early/often; fix return-path issues before cosmetic cleanup.

## 9. Verification checklist

### 9.1 Pre-route placement review

- MCU decouplers close to VDD pins; direct GND vias present.
- Crystal and load caps adjacent to MCU pins; clear of noisy routing corridors.
- Regulators: Cin/Cout tight; copper area available; multiple GND vias planned.
- Charger: BAT/OUT loop compact; sense routing separated from power loop.
- CAN: transceiver close to connector; termination option positioned; pair corridor reserved.

### 9.2 Post-route review

- L2/L3 planes remain solid with no unnecessary splits under fast-edge nets.
- Layer transitions on fast nets include nearby GND stitching vias.
- CAN pair symmetric (length/vias) and free of long stubs.
- Power pours/traces meet current assumptions; no thin bottlenecks.
- DRC passes; zones fill correctly; no accidental isolated copper islands on GND planes.

## 10. Training video links

For convenience, the full training video list is repeated here as a dedicated reference section:

1. [EEVblog #176 – Lithium Ion/Polymer Battery Charging Tutorial](https://www.youtube.com/watch?v=A6mKd5_-abk)
2. [KatKimShow – Ohmic/Linear Region Operation of MOSFET](https://youtu.be/psm6z11gQHM?si=_SXVJzHC-8dOen4S)
3. [Jonathan Currie – Power Supplies: Heat Sinking and Thermal Considerations](https://youtu.be/62B8vy_SmIw?si=c1s2OIpJ7ubwtUgC)
4. [EEVBlog – Electronics Thermal Heatsink Design](https://youtu.be/8ruFVmxf0zs?si=be-NMTI2Bkt7ZpK1)
5. [W2AEW – Transmission Line Termination for Digital and RF Signals](https://youtu.be/g_jxh0Qe_FY?si=Ww_zA6snRCSIAuPj)
6. [W2AEW – Power Supply Decoupling and Filtering](https://youtu.be/9EaTdc2mr34?si=7K7oemsxKq56t8Vc)
7. [LC Oscillator](https://youtu.be/nh4q7mIhLrY?si=ASXD_tC_YMh5RTSi)
8. [Transmission Line Return Current](https://youtu.be/CDqref32tPU?si=Ev1lt_kskHVetqsp)
9. [Rick Hartley – How to achieve proper grounding](https://www.youtube.com/live/ySuUZEjARPY?si=D93KltMv5KR9p3M2)
10. [Phil’s Lab – PCB stack up and build up](https://youtu.be/QAOEtfvCaMw?si=8B5PLt3KHyhGxZyl)

## Appendix A. Video-to-rule traceability matrix

| Video | Key idea | Applied in |
|---|---|---|
| V1 | CC/CV charging; current-loop control | 5.3, 4.1, 9.1 |
| V2 | Linear dissipation risk | 7, 4 placement separation |
| V3/V4 | Thermal resistance; copper as heatsink | 5.2, 7 |
| V5 | Transmission lines; termination | 6.2, 6.3 |
| V6 | Decoupling loop inductance | 5.1 |
| V7 | Oscillator parasitics | 6.3 |
| V8 | Return current path control | 3.2, 6.1, 9.2 |
| V9 | Grounding/loop control | 3.2, 6.1, 9 |
| V10 | Fab-realistic stackup | 3.1, 8.1 |
