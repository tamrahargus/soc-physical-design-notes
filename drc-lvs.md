# DRC & LVS Strategy

Design Rule Checks (DRC) and Layout Versus Schematic (LVS) are the last gatekeepers before tape-in. Passing these cleanly is required for signoff, and layout engineers are often the first (and sometimes only) line of defense.

---

## Design Rule Checks (DRC)

Checks physical layout against foundry-defined constraints:
- **Spacing** (min distance between layers, vias, metals)
- **Width** (min/max metal width, poly, diffusion)
- **Enclosure** (e.g., via-to-metal, contact-to-poly)
- **Density** (metal fill, dummy shapes, coverage zones)
- **Antenna** (excessive gate area exposed during fabrication)

### Tools
- **Calibre DRC** (industry standard)
- **Pegasus** (faster signoff checks in some flows)

### PDE-Relevant DRC Challenges
- Fill-induced DRCs post-routing
- Straps violating width/spacing on thick metals
- PG overlaps near pad cells causing notch violations
- IO pad pitch violations after late ECOs

---

## LVS – Layout vs Schematic

Ensures that the netlist extracted from layout matches the design schematic:
- Verifies **connectivity**, **device sizes**, and **hierarchy integrity**
- Identifies **opens, shorts, missing connections**, or unintended merges

### Common LVS Mismatches
- **Extra shorts** from fill overlap or auto-routing
- **Missing connections** from incorrect pin shapes or orientation
- **Device mismatch** due to drawn W/L or incorrect diffusion

### PDE-Relevant LVS Scenarios
- Hierarchical LVS runs may miss errors at macro boundaries if pins aren’t clean
- Analog blocks require **custom pins and precise placement**
- LVS may pass locally but fail at top if net naming isn’t preserved

---

## Interview Real Talk

> “I’ve cleaned up hundreds of DRCs—spacing, enclosure, fill-related. On the LVS side, I’ve handled shorts from misaligned pins and seen parasitic nets appear due to misdrawn vias. You learn to check pins, labels, and well ties before even running the tool.”

---

### Pro Tip

Always rerun DRC and LVS after:
- Routing changes
- PG net merges
- Fill insertion
- Late-stage ECOs

DRC and LVS failures at tape-in = delay + budget bleed. Catch it early, fix it clean, sleep at night.

---

*PDEs who understand what breaks in layout don’t just push blocks—they prevent rework.*
