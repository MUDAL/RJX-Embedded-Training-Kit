# RJX Training Kit – BOM Metadata Checklist (Purchasing-Ready)

Use this checklist to make BOM exports consistently orderable and CM-friendly.

## Symbol fields to standardize in KiCad
- [ ] **Manufacturer**
- [ ] **MPN** (Manufacturer Part Number)
- [ ] (Optional) **Supplier**
- [ ] (Optional) **Supplier_PN**
- [ ] (Optional) **DNP** / Populate (Yes/No)
- [ ] **Notes** (ratings and constraints)

## What to record in Notes (by part type)
### Capacitors
- [ ] Capacitance, tolerance
- [ ] Voltage rating (e.g., 10 V / 16 V / 25 V)
- [ ] Dielectric (X7R / X5R / C0G/NP0)
- [ ] Case size (e.g., 0603) if not implied by footprint

### Resistors (general)
- [ ] Resistance, tolerance
- [ ] Power rating (especially if not a standard 0603)
- [ ] Case size (0603/0805/1206/2512)

### Shunt / low-ohm sense resistors
- [ ] Resistance value and tolerance
- [ ] Power rating and temperature coefficient (TCR if relevant)
- [ ] Package and Kelvin connection requirement (if used)

### Regulators / ICs
- [ ] Exact ordering code (package + temperature grade)
- [ ] Key required external components (caps, resistors) and any “do not substitute” notes

### Connectors / switches / mechanical parts
- [ ] Exact series + variant (vertical/right-angle, keyed, pitch)
- [ ] Mating part number (recommended)
- [ ] Any height/mechanical constraints

## Export / review
- [ ] Export grouped BOM and confirm **no blanks** in Manufacturer/MPN for orderable parts
- [ ] Confirm footprints match selected packages (no “placeholder” footprints left)
- [ ] Add at least one approved alternate MPN for long lead-time items (connectors, MCU, PMIC/charger, CAN transceiver)
