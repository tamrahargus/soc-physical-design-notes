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
This is one of the most critical steps in SoC-level physical design. A good floorplan minimizes congestion, supports clean clocking and power delivery, and simplifies integration of analog and hard IP.

### 🔹 Key Goals
- Define **core area** vs. IO ring
- Place **macros and hard IP** (e.g., PLLs, SRAM, analog blocks)
- Reserve channels for **clock, power, and high-speed routing**
- Assign **power domains** and PG straps
- Balance **density** and **aspect ratio** for timing and routing

### 🔹 Macro Placement Strategy
- Place **SRAMs near datapaths** to reduce delay and congestion
- **Analog IP** should be kept away from noisy digital logic
- **PLLs and clock sources** should be placed centrally or near the main clock tree sink
- Ensure **macro-to-macro spacing** for routing channels and filler insertion

### 🔹 Pad Ring & IO Planning
- Plan for **pad orientation** and ESD strategy (placement of clamp cells, ring continuity)
- Align pad cells and analog pads with IO muxing logic or retimers
- Keep critical control signals (like reset or test) physically close to their destinations

### 🔹 Block-Level Partitioning
- Assign **floorplan regions** to child blocks or P&R partitions
- Define **keep-out zones** around sensitive IP (e.g., analog, RF, high-current)
- Interface logic should have **clean hitpoints and buffer zones**

### 🔹 Common Challenges
- Over-constraining the floorplan too early leads to timing failure later
- Congestion near macros (especially SRAMs) due to limited routing channels
- Power strap coverage gaps due to irregular aspect ratios or analog block avoidance
- Not enough margin for **fill, antenna, or test structures**

### 🔹 What to Call Out in Interviews
- How you’ve placed and routed analog IP or compiler memory in large SoCs
- Strategies you’ve used for power domain separation and planning
- What you do when a block breaks the plan (e.g., late ECO, shifted IP, unbalanced congestion)

---

## 4. Placement
- Placing standard cells and macros within the floorplan
- Ensures timing, power, and congestion goals are achievable
- **Checks**: tie cell insertion, placement DRCs, initial clock tree planning

---

## 5. Clock Tree Synthesis (CTS)

CTS is the process of distributing the clock signal across the chip with minimal skew and controlled latency. This stage directly impacts timing closure and power, making it one of the most sensitive and iterative parts of the flow.

### 🔹 Primary Goals
- **Minimize clock skew** between timing-critical endpoints
- Balance **latency** across domains (especially in multi-clock SoCs)
- Insert appropriate **buffers and inverters** to drive clock load
- Enable clock gating where possible for **power reduction**

### 🔹 Flow Overview
- Clock sources defined at RTL (e.g., PLL output)
- Tool builds clock tree using buffers and gating cells
- Inserts **leaf cells** to drive registers and endpoints
- Adjusts tree dynamically to fix hold/setup violations
- **Tools used**: Innovus, ICC2, PrimeTime for analysis

### 🔹 Interview Talking Points
- What is clock skew and how do you control it?
- What’s the difference between useful skew and clock latency?
- What are the challenges when building CTS near analog IP?
- How does CTS interact with placement and congestion?

### 🔹 PDE-Relevant Insight
- **Clock tree layout overlaps with floorplanning**—buffer placement may violate spacing or metal density if not planned early
- **High-fanout clocks** (like scan enable or test mode) need extra buffer staging or shielding
- Clock routes may need **shielding** (ground rails on both sides) to reduce crosstalk, especially in analog-mixed designs
- PDEs often review clock tree reports to validate that tool-built clocks didn’t break block-level assumptions

### 🔹 Common Issues
- Inverter chains not aligning with expected skew targets
- Cross-domain clocking issues not caught in early constraints
- Hold violations after CTS requiring post-route fixing
- Clock gating cells inserted too far from register clusters

---

*CTS is where logical design and physical constraints collide. Understanding how layout affects skew, clock load, and tree shape can make or break timing closure.*


---

## 6. Routing
- Signal routing and via placement across metal layers
- Follows design rules and tries to minimize congestion, delay
- **Critical**: PG routing is often separate and must be verified

---

## 7. Signoff (DRC/LVS/STA/EM/IR)

This stage validates the physical design against foundry and electrical rules. Every check here must pass before a tape-in can be considered production-worthy. Failure at this stage = expensive silicon re-spins.

---

### Types of Signoff Checks

#### DRC – Design Rule Check
- Ensures layout obeys foundry rules (spacing, width, via arrays, enclosure)
- **Tool**: Calibre or Pegasus
- Common issues: spacing violations near corners, min-area metal, via-to-via spacing

####  LVS – Layout Versus Schematic
- Verifies that layout connectivity matches netlist (no shorts, opens, or wrong devices)
- Hierarchical LVS requires clean top/block boundary interface
- **Tool**: Calibre LVS or PVS

#### STA – Static Timing Analysis
- Checks if design meets setup/hold timing across PVT corners
- Requires full SPEF/parasitic extraction post-route
- **Tool**: PrimeTime, Tempus

#### EM/IR – Electromigration & IR Drop
- Validates PG metal widths vs current demand
- Detects IR drop hotspots, especially around SRAMs, pads, and PLLs
- **Tool**: Redhawk, Voltus, or PowerSI

---

###  From a Layout Perspective

- Filler cell insertion can impact DRC density rules if not spread carefully
- Metal fill for density balance must avoid coupling sensitive analog routes
- Clock nets may need shielding and spacing to pass noise margin checks
- Final PG continuity must be **visually verified** if tools insert fills or jumper routes

---

### Tape-In Prep Checklist

- [ ] DRC clean (Calibre or Pegasus)
- [ ] LVS clean at full hierarchy
- [ ] All corners STA met
- [ ] EM/IR within limits
- [ ] Antenna effects resolved (with jumper or diode insertion)
- [ ] PG net continuity verified across IO and macro domains
- [ ] All ECOs closed and signed off (no dangling nets or weak hold fixes)


---

## 8. GDSII Export (Tape-In)
- Final layout output for manufacturing
- Includes filler cell insertion, final metal fill, antenna fix
- Often goes through multiple QA passes before tape-in

---

*More detailed notes will be added per stage with tool quirks and layout interaction points.*
