

# Source-Degenerated Common Source Amplifier

<p align="center">

<img src="https://img.shields.io/badge/Technology-TSMC%20180nm-blue">
<img src="https://img.shields.io/badge/Tool-LTspice-orange">
<img src="https://img.shields.io/badge/Course-Linear%20Integrated%20Circuits-green">
<img src="https://img.shields.io/badge/Institution-NIE%20Mysuru-purple">

</p>

<p align="center">
<p align="center">
<b>Design and Simulation of a Source-Degenerated Common Source MOSFET Amplifier</b>
</p>
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

![image alt](https://github.com/Dhureen-07/LIC-EXP-1/blob/main/circuit%201%20.png?raw=true)

| Component  | Value                    | Description                  |
| ---------- | ------------------------ | ---------------------------- |
| V4         | 1.2 V                    | VDD supply                   |
| V2         | 1.2 V                    | PMOS source supply           |
| V3         | 0.56 V                   | PMOS gate bias               |
| V1         | SINE(0.81 10m 1K) AC 1   | Input signal                 |
| R1         | 1 kΩ                     | Source degeneration resistor |
| M1 (CMOSN) | W = 36.3 µm / L = 180 nm | NMOS amplifying transistor   |
| M2 (CMOSP) | W = 36.3 µm / L = 180 nm | PMOS active load             |

 ##  Width Optimization

 The **theoretical transistor widths** provide an initial estimate.  
 However, due to **parasitic effects** and **non-ideal behavior of the TSMC 180 nm MOSFET model**, the widths are **iteratively adjusted during simulation**.

 This optimization ensures the circuit achieves the **target drain current of 200 µA**.

 | Device | Theoretical Width | Adjusted Width | Reason                                            |
| ------ | ----------------- | -------------- | ------------------------------------------------- |
| NMOS   | 5.008 µm          | 36.3 µm        | Increased width to achieve required drain current |
| PMOS   | 11.52 µm          | 36.3 µm        | Matched with NMOS for balanced current operation  |

**DC Operating Point — LTspice .op Result**
| Parameter                        | Design Value | Simulated Value | Status     |
| -------------------------------- | ------------ | --------------- | ---------- |
| Drain Current ID                 | 200 µA       | 200.211 µA      | Verified   |
| Source Voltage VS                | 0.2 V        | 0.200211 V      | Verified   |
| Gate-Source Voltage VGS          | 0.61 V       | 0.609789 V      | Verified   |
| Overdrive Voltage Vov            | 0.25 V       | 0.249789 V      | Verified   |
| Drain-Source Voltage VDS         | 0.6 V        | 0.264559 V      | Acceptable |
| Saturation Condition (VDS ≥ Vov) | —            | 0.265 ≥ 0.25    | Satisfied  |
| PMOS Condition (VSD ≥ Vov)       | —            | 0.735 ≥ 0.25    | Satisfied  |

![image alt](https://github.com/Dhureen-07/LIC-EXP-1/blob/main/dc%20operating%20point%20.png?raw=true)

**Transient Analysis**

**Input Signal Parameter**

| Parameter   | Value      |
| ----------- | ---------- |
| Signal Type | Sinusoidal |
| Frequency   | 1 kHz      |
| Amplitude   | 10 mV      |
| DC Bias     | 0.81 V     |

**Waveform Measurements**

| Signal        | Maximum Voltage | Minimum Voltage | Peak-to-Peak Voltage |
| ------------- | --------------- | --------------- | -------------------- |
| Vin (Input)   | 820 mV          | 800 mV          | 20 mV                |
| Vout (Output) | 565 mV          | 370 mV          | 195 mV               |


**Voltage Gain Calculation**

Av = Vout(pp) / Vin(pp)

Av = 195 mV / 20 mV

Av = 9.75 V/V

**Voltage gain in decibels**

Av(dB) = 20 log10(9.75)

Av(dB) = 19.78 dB

**Input Waveform**

Vin = SINE(0.81 V, 10 mV, 1 kHz)
Input signal centered at 0.81 V with a peak amplitude of 10 mV.

![image alt](https://github.com/Dhureen-07/LIC-EXP-1/blob/main/c1%20input.png?raw=true)

**Output Waveform**

Vout shows an amplified signal with approximately 195 mV peak-to-peak voltage.
The waveform is inverted due to the common source configuration.

![image alt](https://github.com/Dhureen-07/LIC-EXP-1/blob/main/c1%20output.png?raw=true)

**Input and Output Waveforms Together**

The output waveform is a phase-inverted amplified version of the input signal.
Measured gain ≈ 9.75 V/V (≈ 19.78 dB).

![image alt](https://github.com/Dhureen-07/LIC-EXP-1/blob/main/c1%20both%20in%20and%20out.png?raw=true)

**AC Analysis**
Frequency Response

The AC analysis determines the frequency response and bandwidth of the amplifier.

LTspice Cursor Measurements

| Parameter         | Value       |
| ----------------- | ----------- |
| Cursor Frequency  | 190.628 MHz |
| Cursor Magnitude  | 17.038 dB   |
| Phase at Cursor   | 129.515°    |
| Midband Gain      | 20.038 dB   |
| −3 dB Gain        | 17.038 dB   |
| Bandwidth (f−3dB) | 190.628 MHz |

![image alt](https://github.com/Dhureen-07/LIC-EXP-1/blob/main/c1%20ac%20analysis.png?raw=true)

**Frequency Response Observation**

The amplifier exhibits a midband gain of approximately 20 dB with a −3 dB bandwidth of 190.628 MHz.
The wide bandwidth indicates that the circuit is capable of operating in high-frequency analog signal processing applications.

**Theoretical Gain Calculation**
Small-Signal Parameters

| Parameter                   | Formula            | Value   |
| --------------------------- | ------------------ | ------- |
| Transconductance gm         | gm = 2ID / Vov     | 1.6 mS  |
| Channel-Length Modulation λ | Assumed            | 0.1 V⁻¹ |
| Output Resistance (NMOS)    | ro1 = 1 / (λ × ID) | 50 kΩ   |
| Output Resistance (PMOS)    | ro2 = 1 / (λ × ID) | 50 kΩ   |


**Step-by-Step Gain Calculation**

ro1 || ro2 = 50 kΩ || 50 kΩ
           = 25 kΩ

Numerator = gm × (ro1 || ro2)
          = 1.6 mS × 25 kΩ
          = 40

Denominator = 1 + gm × RS
            = 1 + (1.6 mS × 1 kΩ)
            = 2.6

|Av| = 40 / 2.6
|Av| = 15.38 V/V

**Voltage Gain in Decibels**

Av(dB) = 20 log10 (15.38)

Av(dB) ≈ 23.73 dB

**Results Summary**
| Analysis                | Gain (V/V) | Gain (dB) | Bandwidth   |
| ----------------------- | ---------- | --------- | ----------- |
| Transient Analysis      | 9.75       | 19.78 dB  | —           |
| AC Analysis             | —          | 20.038 dB | 190.628 MHz |
| Theoretical Calculation | 15.38      | 23.73 dB  | —           |

**Observations**

The transient gain (19.78 dB) and AC gain (20.038 dB) are closely matched, confirming the correctness of the simulation setup.

The theoretical gain (23.73 dB) is slightly higher than the simulated gain because theoretical models neglect several second-order MOSFET effects.

LTspice simulation includes channel-length modulation, body effect, and parasitic capacitances, which slightly reduce the gain.

The target drain current of 200 µA was successfully achieved with a simulated value of 200.211 µA, confirming accurate biasing.

The source degeneration resistor (RS = 1 kΩ) improves stability and linearity but reduces the overall voltage gain.

The amplifier demonstrates a high bandwidth of approximately 190 MHz, making it suitable for wideband analog applications.


**Conclusion**

The source-degenerated common source amplifier was successfully designed and simulated using TSMC 180 nm CMOS technology in LTspice. The circuit achieves a drain current of 200.211 µA, a midband gain of approximately 20 dB, and a −3 dB bandwidth of 190.628 MHz.

The close agreement between transient and AC simulation results validates the correctness of the design methodology. The difference between theoretical and simulated gain highlights the impact of realistic MOSFET device effects included in the TSMC 180 nm model.

The design demonstrates how source degeneration improves amplifier stability and linearity while maintaining a reasonable gain and wide bandwidth.
