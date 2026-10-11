# Design and Simulation of a Low Dropout (LDO) Voltage Regulator

A complete LDO voltage regulator designed and simulated in **Cadence Virtuoso** (Spectre simulator): a two-stage CMOS op-amp error amplifier, a PMOS pass transistor, a resistive feedback network, and a PMOS current-mirror bias block.

> Open-ended assignment for the **Analog and Mixed Signal Design Lab**, Department of Electronics and Telecommunication Engineering, MKSSS's Cummins College of Engineering for Women, Pune.

---

## Overview

Linear regulators waste power as heat when the input-output voltage difference is large. An LDO keeps the output regulated even when the input is only slightly above the output. This project builds the full regulator at transistor level and verifies DC, AC and transient behaviour.

**Design targets**

| Parameter | Target |
|---|---|
| Input supply (Vdd) | 1.8 V |
| Output voltage (Vout) | ~1.13 V (set by R0 = 3 kΩ, R1 = 9 kΩ) |
| Pass transistor | PMOS, W = 20 µm, L = 400 nm |
| Error amplifier | Two-stage op-amp |
| Op-amp DC gain | > 38 dB |
| Phase margin | > 45° |
| Load | C0 = 10 µF, R3 = 750 Ω |

---

## Architecture

The regulator is a closed feedback loop made of four blocks, each built as its own Virtuoso schematic cell and instantiated in the top-level `entire_ldo_main` schematic.

1. **Bias / current source**: PMOS current mirror (PM0, PM1: W = 15 µm, L = 400 nm) with a 100 kΩ resistor, providing `idc_out` to the op-amp.
2. **Two-stage op-amp (error amplifier)**
   - Stage 1: NMOS differential pair (NM3, NM4: W = 15 µm, L = 400 nm) with PMOS current-mirror load (PM2, PM3: W = 2.1 µm, L = 400 nm)
   - Stage 2: PMOS common-source stage (PM1: W = 28 µm, L = 400 nm) with NMOS current source (NM1: W = 30 µm, L = 400 nm)
   - Miller compensation capacitor (C1 = 10 pF)
3. **PMOS pass transistor (PM0)**: source to Vdd, drain to Vout, gate driven by the op-amp output.
4. **Feedback network and load**: R0 = 3 kΩ and R1 = 9 kΩ divider from Vout to GND feeds the op-amp; C0 = 10 µF and R3 = 750 Ω form the load.

**Output voltage:** `Vout = Vref × (1 + R0/R1)`. With the op-amp reference (VN) at about 851 mV, `Vout ≈ 0.851 × 1.333 ≈ 1.134 V`, which matches the simulation.

---

## Simulations

Run with Cadence Spectre via ADE-L at T = 27 °C:

| Analysis | Purpose | Settings |
|---|---|---|
| AC | Op-amp gain, phase margin, unity-gain bandwidth | 10 Hz to 1 GHz |
| Transient | Output start-up, settling and stability | 0 to 100 ms |

---

## Results

| Parameter | Target | Achieved |
|---|---|---|
| Output voltage | ~1.13 V | ~1.1346 V |
| Input voltage | 1.8 V | 1.8 V (stable) |
| Op-amp DC gain (initial sizing) | > 38 dB | 38.73 dB |
| Op-amp DC gain (optimised sizing) | > 38 dB | 61.67 dB |
| Phase margin (initial sizing) | > 45° | 66.33° |
| Phase margin (optimised sizing) | > 45° | 58.08° |
| Settling time | Fast and stable | ~20 ms |
| Output ripple after settling | Very low | < 0.03 mV |

---

## Repository Structure

```
.
├── README.md
├── docs/
│   ├── LDO_Report.pdf           # Full lab report
│   └── LDO_Presentation.pptx    # Presentation slides
├── schematics/                  # Screenshots of bias, op-amp and top-level schematics
├── simulation_results/          # Bode plots, ADE-L outputs, transient waveforms
└── .gitignore
```

---

## Tools and Technology

- Cadence Virtuoso (schematic entry, ADE-L, Visualization & Analysis XL)
- Cadence Spectre simulator
- Cadence GPDK process libraries (gpdk090 / gpdk130 device models)

> The Cadence PDK and Virtuoso library files are licensed and are **not** included in this repository. Only documentation, screenshots and results are shared.

---

## Key Learnings

- LDO architecture and the role of each building block
- Hierarchical schematic design in Virtuoso (sub-circuit instantiation)
- AC simulation to extract gain, phase margin and unity-gain bandwidth
- Transient simulation to verify settling and regulation quality
- MOSFET sizing, current mirrors and Miller compensation in analog IC design

---

## Team

- Rucha Diwan
- Richa Ghumare
- Krutika Pingale
- Mahek Shah

Department of Electronics and Telecommunication Engineering, MKSSS's Cummins College of Engineering for Women, Pune.

---

## License

This project is for academic purposes. Add a license of your choice (for example MIT) if you want others to reuse the documentation.
