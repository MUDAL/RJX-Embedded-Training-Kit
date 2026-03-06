# RJX Training Kit – Parts Research Priorities (Footprint-Locking Checklist)

These items affect footprints *and* board mechanics, so choose them early.

## 1) Barrel jack (J1)
- [ ] Choose the exact barrel jack part (pin spacing varies a lot)
- [ ] Confirm: horizontal vs vertical, switched pin usage
- [ ] Match the PCB footprint to the chosen jack
- [ ] Verify mechanical: body outline, keepout, edge clearance, connector access

## 2) Battery connector(s) (J4/J5)
- [ ] Confirm connector family: **JST-PH 2.0 mm** (or alternative)
- [ ] Choose orientation: vertical vs right-angle
- [ ] Confirm cable entry direction and strain relief
- [ ] Confirm polarity marking and mating connector/cable
- [ ] Match the PCB footprint to the exact connector variant

## 3) Headers (J2/J3) + 2-pin connectors (J6/J7/J8)
- [ ] Confirm pitch: **2.54 mm**
- [ ] Decide header style:
  - [ ] Open pin header
  - [ ] Boxed/shrouded (keyed) header
  - [ ] Locking/friction header
- [ ] Match PCB footprints to the chosen header style(s)
- [ ] Verify mechanical: clearance, accessibility, orientation consistency

## 4) Jumpers (JP1–JP4)
- [ ] Decide jumper implementation:
  - [ ] 2-pin header + removable shunt (user-friendly)
  - [ ] Solder jumper (smaller/cheaper)
- [ ] If shunt jumpers: choose shunt size (2.54 mm) and header height
- [ ] Match footprints (jumper headers vs solder jumpers)
- [ ] Verify placement for easy access (especially if used in labs)

## 5) Switches (SW1/SW2)
- [ ] Decide switch type:
  - [ ] THT 6×6 mm tact switch (`SW_PUSH_6mm` style)
  - [ ] SMT low-profile tact switch
- [ ] Confirm actuator height and feel (important for training use)
- [ ] Match footprint to exact switch variant
- [ ] Verify mechanical: clearance, edge distance, labeling space

## 6) Crystal (Y1) + load caps (C16/C17)
- [ ] Choose crystal package (e.g., **SMD 3225** or other)
- [ ] Confirm required load capacitance (CL) from crystal datasheet
- [ ] Choose load capacitors:
  - [ ] Dielectric: **C0G/NP0**
  - [ ] Voltage rating acceptable
- [ ] Match footprint to crystal package
- [ ] Ensure placement rules will be followed (very short traces near MCU)

## 7) Power resistors / shunt (R13 = 10 mΩ, R18 = 33 Ω)
- [ ] Confirm worst-case dissipation for R13 (shunt) and R18 (discharge resistor)
- [ ] Decide package based on power/thermal needs:
  - [ ] Start with **2512** for margin
  - [ ] Downsize only if thermal math supports it
- [ ] Match footprints to chosen packages
- [ ] Plan copper area/thermal spreading around these parts
