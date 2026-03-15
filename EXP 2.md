

# Source-Degenerated Common Source Amplifier

<p align="center">

<img src="https://img.shields.io/badge/Technology-TSMC%20180nm-blue">
<img src="https://img.shields.io/badge/Tool-LTspice-orange">
<img src="https://img.shields.io/badge/Course-Linear%20Integrated%20Circuits-green">
<img src="https://img.shields.io/badge/Institution-NIE%20Mysuru-purple">

</p>

<p align="center">
Design and Simulation of a Source-Degenerated Common Source MOSFET Amplifier
</p>



## Table of Contents

- [Aim](#aim)
- [Theory](#theory)
- [Design Specifications](#design-specifications)
- [Power Verification](#power-verification)
- [Transistor Width Calculation](#transistor-width-calculation)
- [DC Bias Design](#dc-bias-design)
- [LTspice Circuit](#ltspice-circuit)
- [Width Optimization](#width-optimization)
- [DC Operating Point](#dc-operating-point)
- [Transient Analysis](#transient-analysis)
- [AC Analysis](#ac-analysis)
- [Theoretical Gain Calculation](#theoretical-gain-calculation)
- [Results Summary](#results-summary)
- [Observations](#observations)
- [Conclusion](#conclusion)


# Experiment 2A — Source-Degenerated Common Source Amplifier

**Technology:** TSMC 180nm | **Tool:** LTspice | **Course:** Linear Integrated Circuits (BEC405D)

---

## Aim

To design and simulate a source-degenerated common source MOSFET amplifier using TSMC 180nm CMOS models in LTspice operating at a drain current of 200 µA with maximum symmetrical output voltage swing.

---

## Theory

A source-degenerated CS amplifier places a resistor RS between the MOSFET source terminal and ground. This introduces local negative feedback which improves linearity, reduces sensitivity to device variations and stabilizes the bias point.

**Small-Signal Voltage Gain:**

```
Av = -gm × (ro1 || ro2) / (1 + gm × RS)
```

**Key Equations:**

| Parameter | Formula |
|-----------|---------|
| Drain Current | ID = (1/2) × µCox × (W/L) × Vov² |
| Transconductance | gm = 2ID / Vov |
| Output Resistance | ro = 1 / (λ × ID) |
| Voltage Gain | Av = -gm × (ro1 ∥ ro2) / (1 + gm × RS) |

---

## Design Specifications

| Parameter | Value |
|-----------|-------|
| Technology | TSMC 180nm |
| Supply Voltage VDD | 1.2 V |
| Target Drain Current ID | 200 µA |
| Overdrive Voltage Vov | 0.25 V |
| Channel Length L | 180 nm |
| NMOS Threshold Voltage VTHn | 0.36 V |
| PMOS Threshold Voltage VTHp | 0.39 V |
| NMOS µnCox | 230 µA/V² |
| PMOS µpCox | 100 µA/V² |
| Power Budget | ≤ 0.4 mW |

---

## Power Verification

```
P = VDD × ID = 1.2 × 200µA = 0.24 mW   ✓ (within 0.4 mW budget)
```

---

## Transistor Width Calculation

**Formula:**
```
W = (2 × ID × L) / (µCox × Vov²)
```

**NMOS:**
```
Wn = (2 × 200×10⁻⁶ × 180×10⁻⁹) / (230×10⁻⁶ × 0.25²)
   = 7.2×10⁻¹¹ / 1.4375×10⁻⁵
   = 5.008 µm
```

**PMOS:**
```
Wp = (2 × 200×10⁻⁶ × 180×10⁻⁹) / (100×10⁻⁶ × 0.25²)
   = 7.2×10⁻¹¹ / 6.25×10⁻⁶
   = 11.52 µm
```

> PMOS width is larger than NMOS due to lower hole mobility.

**Theoretical Dimensions:**

| Transistor | W (µm) | L (µm) | W/L |
|-----------|--------|--------|-----|
| NMOS | 5.008 | 0.18 | 27.8 |
| PMOS | 11.52 | 0.18 | 64.0 |

---

## DC Bias Design

**Output Voltage (for maximum symmetric swing):**
```
Vout = VDD/2 + VS = 1.2/2 + 0.2 = 0.8 V
```

**Source Voltage:**
```
VS = 0.2 V
```

**Source Degeneration Resistor:**
```
RS = VS / ID = 0.2 / 200µA = 1 kΩ
```

**NMOS Gate Bias:**
```
VGS = VTHn + Vov = 0.36 + 0.25 = 0.61 V
VG  = VGS + VS   = 0.61 + 0.2  = 0.81 V
```

**PMOS Gate Bias:**
```
VSG  = |VTHp| + Vov = 0.39 + 0.25 = 0.64 V
VGp  = VDD - VSG    = 1.2 - 0.64  = 0.56 V
```

**Saturation Verification:**

| Device | Condition | Check | Status |
|--------|-----------|-------|--------|
| NMOS | VDS ≥ Vov | 0.6 ≥ 0.25 | ✅ |
| NMOS | VD ≥ VG − VTH | 0.8 ≥ 0.45 | ✅ |
| PMOS | VSD ≥ Vov | 0.4 ≥ 0.25 | ✅ |

**DC Operating Point Summary:**

| VS | VDS | Vout | VGn | VGp | ID | RS | Vov |
|----|-----|------|-----|-----|----|----|-----|
| 0.2 V | 0.6 V | 0.8 V | 0.81 V | 0.56 V | 200 µA | 1 kΩ | 0.25 V |

---

## LTspice Circuit

[![LTspice Circuit](circuit%201.png)](https://github.com/Dhureen-07/LIC-EXP-1/blob/main/circuit%201%20.png?raw=true)

```

**Component List:**

| Component | Value | Description |
|-----------|-------|-------------|
| V4 | 1.2 V | VDD supply |
| V2 | 1.2 V | PMOS source supply |
| V3 | 0.56 V | PMOS gate bias |
| V1 | SINE(0.81 10m 1K) AC 1 | Input signal |
| R1 | 1 kΩ | Source degeneration resistor |
| M1 — CMOSN | W = 36.3 µm / L = 180 nm | NMOS amplifying transistor |
| M2 — CMOSP | W = 36.3 µm / L = 180 nm | PMOS active load |

---

## Width Optimization

Theoretical widths are a starting point. Parasitic effects and non-ideal TSMC model behavior require adjustment to achieve ID = 200 µA in simulation.

| Device | Theoretical W | Adjusted W | Reason |
|--------|--------------|------------|--------|
| NMOS | 5.008 µm | 36.3 µm | Increased to compensate for model non-idealities and achieve target ID |
| PMOS | 11.52 µm | 36.3 µm | Matched to NMOS for equal current delivery under TSMC 180nm model |

---

## DC Operating Point — LTspice .op Result

| Node | Voltage | Description |
|------|---------|-------------|
| V(n001) | 1.2 V | VDD |
| V(n002) | 1.2 V | PMOS source |
| V(n003) | 0.56 V | PMOS gate |
| V(n004) | 0.200211 V | NMOS source VS across R1 |
| V(vin) | 0.81 V | NMOS gate |
| V(vout) | 0.46477 V | Output node — drain of M1 |
| I(R1) | 200.211 µA | Drain current |
| Id(M1) | 200.211 µA | NMOS drain current |
| Id(M2) | −200.211 µA | PMOS drain current |

**Verification Against Design:**

| Parameter | Design | Simulated | Status |
|-----------|--------|-----------|--------|
| ID | 200 µA | 200.211 µA | ✅ |
| VS | 0.2 V | 0.200211 V | ✅ |
| VGS = V(vin) − V(n004) | 0.61 V | 0.609789 V | ✅ |
| Vov = VGS − VTHn | 0.25 V | 0.249789 V | ✅ |
| VDS = V(vout) − V(n004) | 0.6 V | 0.264559 V | Note |
| VDS ≥ Vov — saturation | — | 0.265 ≥ 0.25 | ✅ |
| VSD(PMOS) = VDD − V(vout) | 0.4 V | 0.735 V ≥ 0.25 | ✅ |

> Simulated Vout = 0.465 V is lower than the design target of 0.8 V. At the adjusted transistor widths the TSMC 180nm PMOS model settles at a different current-voltage point due to higher-order device effects. Both MOSFETs remain in saturation.

---

## Transient Analysis

**Input Signal:**

| Parameter | Value |
|-----------|-------|
| Type | Sinusoidal |
| Frequency | 1 kHz |
| Amplitude | 10 mV |
| DC Bias | 0.81 V |

**Waveform Measurements:**

| Signal | Vmax | Vmin | Vpp |
|--------|------|------|-----|
| V(vin) — Input | 820 mV | 800 mV | 20 mV |
| V(vout) — Output | 565 mV | 370 mV | 195 mV |

**Voltage Gain:**
```
Av = Vout(pp) / Vin(pp) = 195 mV / 20 mV = 9.75 V/V

Av(dB) = 20 × log10(9.75) = 19.78 dB
```

> Output is phase-inverted (180°) relative to input — expected for a common source configuration.

---

## AC Analysis

**Frequency Response — LTspice Cursor Measurement:**

| Parameter | Value |
|-----------|-------|
| Cursor Frequency | 190.62839 MHz |
| Cursor Magnitude | 17.037731 dB |
| Phase at Cursor | 129.515° |
| Midband Gain (cursor + 3 dB) | 20.038 dB |
| −3 dB Gain | 17.038 dB |
| Bandwidth f(−3dB) | 190.628 MHz |

---

## Theoretical Gain Calculation

**Small-Signal Parameters:**

| Parameter | Formula | Value |
|-----------|---------|-------|
| gm | 2 × 200µA / 0.25 | 1.6 mS |
| λ (TSMC 180nm) | Chosen | 0.1 V⁻¹ |
| ro1 (NMOS) | 1 / (0.1 × 200µA) | 50 kΩ |
| ro2 (PMOS) | 1 / (0.1 × 200µA) | 50 kΩ |

**Step-by-Step:**
```
ro1 ∥ ro2  =  50k ∥ 50k            =  25 kΩ
Denominator = 1 + gm × RS          =  1 + (1.6m × 1k)  =  2.6
Numerator   = gm × (ro1 ∥ ro2)     =  1.6m × 25k       =  40

|Av| = 40 / 2.6 = 15.38 V/V

Av(dB) = 20 × log10(15.38) = 23.73 dB
```

---

## Results Summary

| Analysis | Gain (V/V) | Gain (dB) | Bandwidth |
|---------|-----------|-----------|-----------|
| Transient — Practical | 9.75 | 19.78 dB | — |
| AC Analysis | — | 20.038 dB | 190.628 MHz |
| Theoretical | 15.38 | 23.73 dB | — |

---

## Observations

- Transient gain (19.78 dB) and AC gain (20.038 dB) are in close agreement with a difference of 0.26 dB confirming simulation consistency.
- Theoretical gain (23.73 dB) is higher than simulated values because analytical models use simplified second-order approximations. LTspice accounts for channel-length modulation, body effect, gate-oxide capacitance and other non-ideal MOSFET behaviour inherent to the TSMC 180nm model.
- Target drain current of 200 µA is achieved with high accuracy (200.211 µA) confirming correct biasing.
- Source degeneration resistor RS = 1 kΩ reduces gain relative to a standard CS amplifier but significantly improves bias stability, thermal robustness and linearity.
- Bandwidth of 190.628 MHz confirms the circuit is suitable for wideband analog signal processing.

---

## Conclusion

The source-degenerated common source amplifier was successfully designed and simulated using TSMC 180nm CMOS models with VDD = 1.2 V. The circuit achieves a drain current of 200.211 µA, a midband gain of 20.038 dB and a −3 dB bandwidth of 190.628 MHz. The close agreement between transient and AC simulation results validates the design approach. The difference from the theoretical gain reflects the accuracy of the full-physics MOSFET model in LTspice compared to simplified analytical approximations.

---

**USN:** 4NI24EC043  
**Course:** Linear Integrated Circuits — BEC405D  
**Institution:** NIE Mysuru
