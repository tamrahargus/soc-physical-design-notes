# Timing Analysis

Timing analysis is about verifying that signals arrive within acceptable windows across the entire design, under all expected process, voltage, and temperature conditions. It’s one of the most critical tasks in signoff and tape-in prep.

---

## Key Terms

- **Setup Time**: Minimum time data must be stable *before* clock edge
- **Hold Time**: Minimum time data must stay stable *after* clock edge
- **Slack**: The difference between required time and actual arrival time
- **Skew**: Difference in clock arrival time at two endpoints
- **Jitter**: Short-term clock variations—can eat up slack budget

---

## Common Violations

### Setup Violation
- Data arrives too late
- Typically fixed by reducing logic depth or speeding up the clock path

### Hold Violation
- Data arrives too early and gets latched incorrectly
- Usually fixed by adding delay buffers in the data path

---

## Static Timing Analysis (STA)

- Uses delay models to analyze all timing paths without simulation
- Must be done **across all corners** (slow-slow, fast-fast, typical)
- Includes **on-chip variation (OCV)** and derating

### Tools
- **PrimeTime** (Synopsys)
- **Tempus** (Cadence)
- **GoldTime** (Siemens, rarely used in advanced flows)

---

## Layout Awareness in Timing

As a layout engineer or PDE, you may not run STA yourself, but:

- Macro placement impacts path length and logic depth
- Via counts and metal hops affect delay
- Clock tree placement directly affects skew and latency
- Filler cell insertion can add or block routing paths used for delay fix


