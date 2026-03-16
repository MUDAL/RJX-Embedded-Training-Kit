# RJX Training Kit – Parts Research Priorities (Footprint-Locking Checklist)

These items affect footprints *and* board mechanics, so choose them early.

## 1) Barrel jack (J1)
- [x] Narrow connector family to **5.5 mm OD barrel jack**
- [x] Prefer plug size: **5.5 mm × 2.1 mm**
- [x] Choose mechanical style: **right-angle, through-hole**
- [x] Prefer **3-pin switched jack** style
- [x] Select the exact connector part number: **Tensility 54-00133**
- [x] Confirm current rating is **≥ 2 A continuous**
- [x] Confirm polarity: **center-positive**
- [x] Confirm switched pin mapping against schematic usage
- [x] Match the PCB footprint to the chosen jack
- [ ] Verify final board-edge placement and connector clearance

**Decision note:**  
Use **Tensility 54-00133**, a **5.5 mm × 2.1 mm**, **right-angle**, **through-hole**, **3-contact switched** barrel jack.  
The selected KiCad horizontal barrel-jack footprint matches the part’s pinout and mechanical layout.  
Pin mapping is aligned across the **datasheet**, **footprint**, and **schematic symbol**:
- **Pin 1 / A** = center contact (**+5V**)
- **Pin 3 / C** = sleeve contact (**GND**)
- **Pin 2 / B** = switched contact

## 2) Battery connector(s) (J4/J5)
- [x] Confirm connector family: **JST-PH 2.0 mm**
- [x] Choose exact variant: **right-angle / side-entry THT**
- [x] Select the exact manufacturer part number: **JST S2B-PH-K-S**
- [ ] Confirm cable entry direction and strain relief
- [ ] Confirm polarity marking and mating connector/cable
- [x] Match the PCB footprint to the exact connector variant
- [ ] Verify mechanical: board-edge access, cable clearance, insertion/removal space

**Decision note:**  
Use **JST S2B-PH-K-S**, a **JST PH series, 2.0 mm pitch, right-angle / side-entry THT** connector for J4/J5.

## 3) Headers (J2/J3) + 2-pin connectors (J6/J7/J8)

### J2 / J3 main headers
- [x] Confirm pitch: **2.54 mm**
- [x] Decide header style: **open pin header**
- [x] Choose orientation: **vertical THT**
- [x] Confirm format: **1x32**
- [ ] Select exact manufacturer part number
- [x] Match PCB footprint to the chosen header style
- [ ] Verify mechanical: clearance, accessibility, orientation consistency

**Decision note:**  
Use **open, vertical, through-hole 1x32, 2.54 mm pin headers** for J2 and J3.

### J6 / J7 / J8 auxiliary connectors
- [x] Confirm pitch: **2.54 mm**
- [x] Decide header style: **open pin header**
- [x] Choose orientation: **vertical THT**
- [x] Confirm format: **1x02**
- [ ] Select exact manufacturer part number
- [x] Match PCB footprint to the chosen header style
- [ ] Verify mechanical: clearance, accessibility, orientation consistency

**Decision note:**  
Use **open, vertical, through-hole 1x02, 2.54 mm pin headers** for J6, J7, and J8.

## 4) Jumpers (JP1–JP4)
- [x] Decide jumper implementation: **2-pin header + removable shunt**
- [x] Choose pitch: **2.54 mm**
- [x] Choose orientation: **vertical THT**
- [x] Select the exact header part number: **TE Connectivity 826629-2**
- [x] Select the jumper/shunt part number: **Würth Elektronik 609002115121**
- [x] Match footprints to the chosen header
- [ ] Verify placement for easy access (especially for lab use)

**Decision note:**  
Use **TE Connectivity 826629-2** for the 2-pin headers and **Würth Elektronik 609002115121** for the removable shunts.

## 5) Switches (SW1/SW2)
- [x] Decide switch type: **surface-mount tactile button**
- [x] Confirm actuation style: **top-actuated**
- [x] Confirm mounting style: **surface-mount, gull-wing**
- [x] Choose preferred candidate: **C&K PTS647SM70SMTR2 LFS**
- [x] Confirm body size: **4.5 mm × 4.5 mm**
- [x] Confirm actuator height: **7.0 mm**
- [ ] Confirm user feel is acceptable in the final PCB placement
- [x] Match footprint to the exact switch variant
- [ ] Verify mechanical: clearance, edge distance, labeling space

**Decision note:**  
Use **C&K PTS647SM70SMTR2 LFS**, a top-actuated SMT tactile switch with a **4.5 mm × 4.5 mm** body and **7.0 mm** actuator height.  
Use the **exact manufacturer land pattern** or a verified equivalent that matches the datasheet dimensions.

## 6) Crystal (Y1) + load caps (C16/C17)
- [x] Choose crystal frequency: **8 MHz**
- [x] Choose crystal package direction: **SMD 3225**
- [x] Confirm STM32 HSE compatibility direction from ST oscillator guidance
- [x] Select the exact crystal MPN: **ECS-80-8-33B2Q-CVY-TR3**
- [x] Confirm crystal load capacitance (CL): **8 pF**
- [x] Confirm ESR / oscillator-drive suitability
- [x] Recalculate C16/C17 based on selected CL and estimated stray capacitance
- [x] Choose initial load capacitors: **10 pF / 10 pF, C0G/NP0**
- [x] Match footprint to the exact crystal package
- [ ] Ensure placement rules will be followed (very short traces near MCU)

**Decision note:**  
Use **ECS-80-8-33B2Q-CVY-TR3**, an **8 MHz, fundamental-mode, SMD 3225 crystal** with **8 pF load capacitance**.  
Initial design choice for the load capacitors is **C16 = 10 pF** and **C17 = 10 pF**, using **C0G/NP0 dielectric**.  
Use the **exact ECS footprint** if available, otherwise a verified standard **3225 4-pad crystal footprint**.

### Why lower-CL crystals are preferred here

For the STM32 HSE oscillator, crystal compatibility is not checked by frequency alone.  
ST’s oscillator guidance also requires checking oscillator-drive margin using:

`gm_crit = 4 × ESR × (2πF)² × (C0 + CL)²`

and then comparing it against the MCU oscillator capability using gain margin:

`Gain Margin = gm / gm_crit`

A practical target is **Gain Margin > 5**.

Because `gm_crit` depends on **`(C0 + CL)²`**, increasing **CL** increases the required critical transconductance **quadratically**.  
That means, for the same MCU, frequency, and ESR:

- larger **CL** → larger **gm_crit**
- larger **gm_crit** → lower gain margin
- lower gain margin → less startup margin / harder oscillator drive

So for the STM32L431 HSE, a **lower-CL crystal** (for example **8 pF**) is generally a cleaner fit than a higher-CL crystal (for example **18 pF**), assuming other parameters are comparable.

Also, the crystal’s specified **CL** must be matched by the external load network. ST’s load equation is:

`CL = (CL1 × CL2) / (CL1 + CL2) + Cstray`

If `CL1 = CL2 = C`, this simplifies to:

`CL = C/2 + Cstray`

So:

`C = 2 × (CL - Cstray)`

For **CL = 8 pF** and a reasonable stray-capacitance estimate of about **3 pF**, the initial capacitor choice becomes:

`C = 2 × (8 pF - 3 pF) = 10 pF`

**Practical implication for this project:**  
Prefer an **8 MHz, 3225, low-CL crystal** with acceptable ESR, then calculate **C16/C17** from the selected crystal’s load capacitance rather than copying values from another STM32 board.

## 7) Power resistors / shunt (R13 = 10 mΩ, R18 = 33 Ω)
- [x] Confirm initial package direction: **2512**
- [ ] Confirm worst-case dissipation for R13 (shunt) and R18 (discharge resistor)
- [ ] Decide whether downsizing is justified by thermal math
- [x] Match footprints to chosen packages
- [ ] Plan copper area/thermal spreading around these parts

**Decision note:**  
Use **2512** as the initial package choice for both **R13 (10 mΩ)** and **R18 (33 Ω)**.  
Only downsize after confirming dissipation and thermal margin.

## 8) Remaining pre-layout checks
- [ ] Verify board-edge placement and keepout for J1
- [ ] Verify cable clearance and insertion/removal space for J4/J5
- [ ] Verify user access to JP1–JP4 shunts
- [ ] Verify switch spacing / labeling / accessibility for SW1 and SW2
- [ ] Verify crystal placement close to MCU with short symmetric traces
- [ ] Confirm all selected footprints match datasheet mechanical drawings