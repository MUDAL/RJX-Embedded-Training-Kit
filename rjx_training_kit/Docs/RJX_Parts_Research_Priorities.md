# RJX Training Kit – Parts Research Priorities (Footprint-Locking Checklist)

These items affect footprints *and* board mechanics, so choose them early.

## 1) Barrel jack (J1)
- [x] Narrow connector family to **5.5 mm OD barrel jack**
- [x] Prefer plug size: **5.5 mm × 2.1 mm**
- [ ] Select the exact THT connector part number
- [ ] Confirm current rating is **≥ 2 A continuous**
- [ ] Confirm polarity: **center-positive**
- [ ] Confirm switched pin usage (if applicable)
- [ ] Match the PCB footprint to the chosen jack
- [ ] Verify mechanical: body outline, keepout, edge clearance, connector access

**Decision note:**  
For the RJX Training Kit, **5.5 mm × 2.1 mm** is the current preferred choice for the 5 V / 2 A input.  
Only switch to **5.5 mm × 2.5 mm** if the intended external adapter is already fixed to that plug size.

## 2) Battery connector(s) (J4/J5)
- [x] Confirm connector family: **JST-PH 2.0 mm**
- [x] Choose exact variant: **right-angle / side-entry THT**
- [ ] Confirm cable entry direction and strain relief
- [ ] Confirm polarity marking and mating connector/cable
- [ ] Select the exact manufacturer part number
- [ ] Match the PCB footprint to the exact connector variant
- [ ] Verify mechanical: board-edge access, cable clearance, insertion/removal space

**Decision note:**  
Use a **JST PH series, 2.0 mm pitch, right-angle / side-entry THT** connector for J4/J5.

## 3) Headers (J2/J3) + 2-pin connectors (J6/J7/J8)
- [ ] Confirm pitch: **2.54 mm**
- [ ] Decide header style:
  - [ ] Open pin header
  - [ ] Boxed/shrouded (keyed) header
  - [ ] Locking/friction header
- [ ] Match PCB footprints to the chosen header style(s)
- [ ] Verify mechanical: clearance, accessibility, orientation consistency

## 4) Jumpers (JP1–JP4)
- [x] Decide jumper implementation: **2-pin header + removable shunt**
- [x] Choose pitch: **2.54 mm**
- [x] Choose orientation: **vertical THT**
- [ ] Select the exact header part number
- [ ] Select the jumper/shunt part number
- [ ] Match footprints to the chosen header
- [ ] Verify placement for easy access (especially for lab use)

**Decision note:**  
Use **2-pin, 2.54 mm pitch, vertical THT pin headers with removable shunts** for JP1–JP4.

## 5) Switches (SW1/SW2)
- [x] Decide switch type: **surface-mount tactile button**
- [x] Confirm actuation style: **top-actuated**
- [x] Confirm mounting style: **surface-mount, gull-wing**
- [x] Choose preferred candidate: **C&K PTS647SM70SMTR2 LFS**
- [x] Confirm body size: **4.5 mm × 4.5 mm**
- [x] Confirm actuator height: **7.0 mm**
- [ ] Confirm user feel is acceptable in the final PCB placement
- [ ] Match footprint to the exact switch variant
- [ ] Verify mechanical: clearance, edge distance, labeling space

**Decision note:**  
Current selected candidate for **SW1** and **SW2** is **C&K PTS647SM70SMTR2 LFS**, a top-actuated SMT tactile switch with a **4.5 mm × 4.5 mm** body and **7.0 mm** actuator height.

## 6) Crystal (Y1) + load caps (C16/C17)
- [x] Choose crystal frequency: **8 MHz**
- [x] Choose crystal package direction: **SMD 3225**
- [x] Confirm STM32 HSE compatibility direction from ST oscillator guidance
- [ ] Select the exact crystal MPN
  - [ ] Abracon
  - [ ] ECS
  - [ ] Epson
  - [ ] NDK
  - [ ] Micro Crystal
- [ ] Confirm crystal load capacitance (CL)
- [ ] Confirm ESR / oscillator-drive suitability
- [ ] Recalculate C16/C17 based on selected CL and estimated stray capacitance
- [ ] Choose C16/C17 dielectric: **C0G/NP0**
- [ ] Match footprint to the exact crystal package
- [ ] Ensure placement rules will be followed (very short traces near MCU)

**Decision note:**  
Use an **8 MHz, fundamental-mode, SMD 3225 crystal** for Y1.  
Do **not** finalize C16/C17 until the exact crystal’s **CL** is selected and the capacitor values are recalculated.

## 7) Power resistors / shunt (R13 = 10 mΩ, R18 = 33 Ω)
- [ ] Confirm worst-case dissipation for R13 (shunt) and R18 (discharge resistor)
- [ ] Decide package based on power/thermal needs:
  - [ ] Start with **2512** for margin
  - [ ] Downsize only if thermal math supports it
- [ ] Match footprints to chosen packages
- [ ] Plan copper area/thermal spreading around these parts