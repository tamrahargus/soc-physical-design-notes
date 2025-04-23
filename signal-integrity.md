# Signal Integrity (SI)

Signal integrity (SI) refers to how cleanly signals propagate through a chip without being distorted, delayed, or corrupted by noise, interference, or parasitics. In advanced SoCs, especially those with analog or high-speed blocks, SI awareness is critical.

---

## Core SI Concerns in Physical Design

### Crosstalk
- Occurs when a switching signal induces noise on a nearby quiet net
- More prominent in long, parallel routes—especially on mid-level metals
- Fix with: spacing, shielding (GND rails), and layer changes

### Ground Bounce
- Multiple simultaneous switching outputs (SSO) can cause local ground levels to rise temporarily
- Affects IO blocks and large buses—especially during functional switching
- Requires good decoupling and strong PG grid

### Power/Ground Noise
- IR drop and Ldi/dt noise can affect signal thresholds
- High di/dt causes power spikes and GND collapse in localized areas
- Controlled with power grid straps, decaps, and careful switching balance

### Reflection & Impedance Discontinuity
- In high-speed digital (SerDes, memory), improper termination or stubs cause signal bounce
- Addressed by routing length control, impedance matching, and simulation

---

## PDE-Relevant Mitigations

- Place **shielding** (GND or VSS) around sensitive analog nets
- Avoid routing clocks or high-speed data lines adjacent to noisy digital busses
- Keep **reset and test signals** well buffered and routed in low-congestion zones
- Use wider metals for long signals to reduce delay variation and coupling

---

## SI-Aware Layout Behavior

- Choose **routing layers intentionally**—avoid M3 for critical analog signals if it's noisy
- Bundle **clock nets** with spacing and ground shielding
- Recognize that **macro placement** can cause coupling if analog and digital domains are too close
- **Redhawk/Voltus** analysis often flags coupling after fill insertion or PG stitching
