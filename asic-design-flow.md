# ASIC Design Flow

This section covers the standard RTL-to-GDSII flow in physical design. It’s a high-level overview of the key stages and what matters most from a layout and PDE perspective.

---

## 1. RTL Design
- The starting point: synthesizable Verilog or VHDL
- Includes clock and reset design, constraints, and hierarchy definition
- **What PDEs care about**: ensuring the RTL is physically aware (multi-VT cells, area targets)

---

## 2. Logic Synthesis
- Translates RTL into gate-level netlist
- Uses standard cell libraries and tech constraints (timing, power)
- **Tool examples**: Synopsys Design Compiler, Cadence Genus
- **Why it matters**: bad synthesis = hard place and route = timing closure hell

---

## 3. Floorplanning
- Defining block placement, aspect ratio, IO ring, pad locations
- Assigning hard macros (PLL, SRAM, analog IP), power domains
- **SoC layout concern**: keep high-activity logic near power straps

---

## 4. Placement
- Placing standard cells and macros within the floorplan
- Ensures timing, power, and congestion goals are achievable
- **Checks**: tie cell insertion, placement DRCs, initial clock tree planning

---

## 5. Clock Tree Synthesis (CTS)
- Builds clock networks to balance skew and latency
- Inserts buffers, defines clock gating cells
- **Goal**: min skew, min power, meet timing closure

---

## 6. Routing
- Signal routing and via placement across metal layers
- Follows design rules and tries to minimize congestion, delay
- **Critical**: PG routing is often separate and must be verified

---

## 7. Signoff (DRC/LVS/STA/EM/IR)
- DRC: Design Rule Check
- LVS: Layout vs. Schematic
- STA: Static Timing Analysis (pre/post-route)
- EM/IR: Electromigration and IR drop checks
- **Tool examples**: Calibre, PrimeTime, Redhawk, Voltus

---

## 8. GDSII Export (Tape-In)
- Final layout output for manufacturing
- Includes filler cell insertion, final metal fill, antenna fix
- Often goes through multiple QA passes before tape-in

---

*More detailed notes will be added per stage with tool quirks and layout interaction points.*
