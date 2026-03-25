

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

---

## Observations

- The transient gain (≈19.78 dB) and AC gain (≈20.04 dB) closely match, confirming correct simulation.
- The theoretical gain (≈23.73 dB) is slightly higher because analytical models ignore second-order MOSFET effects.
- LTspice includes channel length modulation, body effect, and parasitic capacitances, which reduce the gain slightly.
- The designed drain current of **200 µA** was successfully achieved (simulated value ≈ **200.21 µA**), confirming accurate biasing.
- Source degeneration (**RS = 1 kΩ**) improves stability and linearity but reduces voltage gain.
- The amplifier shows a wide bandwidth of approximately **190 MHz**, suitable for high-frequency analog applications.

---

## Conclusion

The source-degenerated common source amplifier was successfully designed and simulated using **TSMC 180 nm CMOS technology in LTspice**.  
The circuit achieves a drain current of **≈200 µA**, a midband gain of **≈20 dB**, and a **−3 dB bandwidth of approximately 190 MHz**.

The close agreement between transient and AC results validates the design. Source degeneration improves amplifier **stability and linearity** while maintaining **reasonable gain and wide bandwidth**.







---

# Experiment 2B — Cascode Amplifier

<p align="center">

<img src="https://img.shields.io/badge/Technology-TSMC%20180nm-blue">
<img src="https://img.shields.io/badge/Tool-LTspice-orange">
<img src="https://img.shields.io/badge/Circuit-Cascode%20Amplifier-green">

</p>

> **Design and Simulation of a Cascode MOSFET Amplifier**

---

## Aim

To design and simulate a **Cascode MOSFET amplifier** using TSMC 180 nm CMOS models in LTspice and analyze its gain, operating point, and frequency response.

---

## Theory

A **cascode amplifier** is formed by stacking a **common source transistor** with a **common gate transistor**.

The configuration improves amplifier performance by:

- Increasing output resistance
- Reducing Miller effect
- Increasing voltage gain
- Improving bandwidth

The cascode structure isolates the input node from the output node, minimizing the effect of gate-drain capacitance.

---

## Circuit Description

The cascode amplifier consists of three MOSFETs:

| Transistor | Function |
|-----------|-----------|
| M1 | Input common-source transistor |
| M2 | Common-gate transistor (cascode stage) |
| M3 | Current source transistor |

The output is taken at the **drain of M1 / source of M2**.

---

## Design Specifications

| Parameter | Value |
|-----------|------|
| Technology | TSMC 180 nm |
| Supply Voltage VDD | 1.2 V |
| Target Drain Current ID | 200 µA |
| Overdrive Voltage Vov | 0.25 V |
| Channel Length L | 180 nm |
| NMOS Threshold Voltage VTHn | 0.36 V |
| PMOS Threshold Voltage VTHp | 0.39 V |

---

## Power Verification

P = VDD × ID

P = 1.2 × 200 µA

P = 0.24 mW


✔ Within power budget

---

## Output Voltage Selection

For symmetrical operation,

Vout = VDD/2 + VS

Vout = 1.2/2 + 0.3

Vout ≈ 0.9 V


---

## Gate-Source Voltage

VGS = VTHn + Vov

VGS = 0.36 + 0.25

VGS = 0.61 V


---

## Source Voltage Assumption

Assume

VS = 0.3 V

Therefore

VG = VGS + VS

VG = 0.61 + 0.3

VG = 0.91 V


---

## Saturation Condition

For NMOS operation in saturation

VDS ≥ VGS − VTH

0.6 ≥ 0.61 − 0.36

0.6 ≥ 0.25


✔ Condition satisfied

The transistor operates in the **saturation region**.

---

## Transistor Width Calculation

Using

ID = (1/2) μnCox (W/L) (Vov)²

Rearranging

W = (2 × ID × L) / (μnCox × Vov²)

Substituting values

W = (2 × 200×10⁻⁶ × 180×10⁻⁹) / (230×10⁻⁶ × 0.25²)

W ≈ 5.008 µm


---

## PMOS Width Calculation

W = (2 × ID × L) / (μpCox × Vov²)

W ≈ 9.98 µm


---

## Final Transistor Dimensions

| Device | Width (µm) | Length (µm) |
|------|------|------|
| NMOS M1 | 10 µm | 180 nm |
| NMOS M3 | 6.5 µm | 180 nm |
| PMOS M2 | 4.62 µm | 180 nm |


---

## LTspice Circuit

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/circuit%202.png?raw=true)

The circuit was implemented using **TSMC 180 nm CMOS models** in LTspice.

The amplifier consists of:

- M1 : Common source transistor  
- M2 : Cascode transistor  
- M3 : Current source transistor  

The output is taken at the **drain node of M1 / source node of M2**.

---

## Simulation Setup

| Simulation | LTspice Command |
|-----------|----------------|
| Operating Point | .op |
| DC Sweep | .dc v3 0 2 |
| AC Analysis | .ac dec 20 1 100G |
| Transient Analysis | .tran 5m |

The TSMC model library is included using

.include tsmc018.lib

---

## Component List

| Component | Value | Description |
|-----------|------|-------------|
| V1 | 1.2 V | VDD supply |
| V2 | 1.2 V | PMOS source bias |
| V3 | SINE(0.91 10m 1K) AC 1 | Input signal |
| V4 | 0.56 V | PMOS gate bias |
| V5 | 0.61 V | NMOS bias voltage |
| M1 | NMOS | Amplifying transistor |
| M2 | PMOS | Cascode load transistor |
| M3 | NMOS | Current source transistor |

---

## DC Operating Point — LTspice Result

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c2%20operating%20point.png?raw=true)

| Node | Voltage |
|------|--------|
| V(n001) | 1.2 V |
| V(n002) | 1.2 V |
| V(n003) | 0.2308 V |
| V(n004) | 0.61 V |
| V(vin) | 0.91 V |
| V(vout) | 0.9597 V |

---

## Operating Point Observation

The simulated output voltage is approximately

Vout ≈ 0.96 V

which is close to the theoretical value of

Vout ≈ 0.9 V

The drain current obtained from simulation is

ID ≈ 200 µA

which confirms that the biasing conditions are correctly established.


---

## Transient Analysis

### Input Signal

| Parameter | Value |
|-----------|-------|
| Signal Type | Sinusoidal |
| Frequency | 1 kHz |
| Amplitude | 10 mV |
| DC Bias | 0.91 V |

---

### Input Waveform

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c2%20input%20.png?raw=true)

The input signal is a small sinusoidal waveform centered around **0.91 V** with an amplitude of **10 mV**.

---

### Output Waveform

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c2%20output%20.png?raw=true)

The output waveform shows an amplified version of the input signal.  
The waveform is **phase inverted**, which is expected behavior for a **common-source based amplifier configuration**.

---

### Input and Output Waveforms

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c2%20in%20and%20out%20wave%20forms%20.png?raw=true)

---

### Voltage Gain Calculation

Measured values from transient analysis

| Parameter | Value |
|-----------|-------|
| Vin (peak-to-peak) | 19.16 mV |
| Vout (peak-to-peak) | 16.38 mV |

Voltage gain

Av = Vout(pp) / Vin(pp)

Av = 16.38 / 19.16

Av ≈ 0.85 V/V


Voltage gain in decibels

Av(dB) = 20 log10(0.85)

Av(dB) ≈ -1.60 dB


---

---

## AC Analysis

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c2%20ac%20analysis.png?raw=true)

The AC analysis was performed using the LTspice command

.ac dec 20 1 100G

This analysis determines the **frequency response and bandwidth** of the cascode amplifier.

---

### Frequency Response — LTspice Cursor Measurement

| Parameter | Value |
|-----------|-------|
| Cursor Frequency | 678.55 MHz |
| Magnitude | −4.326 dB |
| Phase | 132.63° |

---

### Midband Gain

From the AC plot the midband gain is approximately

Av ≈ -1.88 dB


---

### Bandwidth

The −3 dB bandwidth occurs near

f(-3dB) ≈ 678.55 MHz


---

### AC Analysis Observation

The cascode amplifier exhibits a **very wide bandwidth of approximately 678 MHz**.

This improvement in frequency response is due to the **cascode configuration**, which significantly reduces the **Miller capacitance effect** between the input and output nodes.

Although the voltage gain is relatively small, the circuit demonstrates excellent **high-frequency performance**, making it suitable for **wideband analog applications**.

---

## Results and Conclusion

### Simulation Results Summary

| Analysis | Parameter | Value |
|--------|--------|--------|
| DC Analysis | Drain Current (ID) | ≈ 200 µA |
| DC Analysis | Output Voltage (Vout) | ≈ 0.96 V |
| Transient Analysis | Vin(p-p) | ≈ 19.16 mV |
| Transient Analysis | Vout(p-p) | ≈ 16.38 mV |
| Transient Gain | Av = Vout/Vin | ≈ 0.85 V/V |
| Gain in dB | 20 log10(Av) | ≈ −1.88 dB |
| AC Analysis | −3 dB Bandwidth | ≈ 678.55 MHz |

---

### Key Observations

- The cascode amplifier successfully maintains the MOSFETs in the **saturation region**, ensuring proper amplification.
- The **drain current of approximately 200 µA** confirms correct biasing conditions.
- Transient analysis shows a **small signal amplification with stable waveform behavior**.
- The **cascode configuration reduces the Miller capacitance**, improving the high-frequency response.
- AC analysis demonstrates a **very wide bandwidth of approximately 678 MHz**, which is significantly higher than a simple common source amplifier.

---

### Conclusion

The cascode amplifier was successfully designed and simulated using **TSMC 180 nm CMOS technology in LTspice**.  
The circuit achieves proper biasing with a drain current close to the target **200 µA** and an output voltage around **0.96 V**.

Although the voltage gain is relatively small (approximately **−1.88 dB**), the cascode configuration significantly improves the **frequency response and bandwidth**, achieving a −3 dB bandwidth of approximately **678 MHz**.

This demonstrates that the cascode amplifier is highly suitable for **high-frequency analog applications**, where bandwidth and stability are more critical than large voltage gain.

---



# Circuit 3  
## Common-Source Amplifier with PMOS Active Load & Source Degeneration  
### TSMC 0.18 µm — LTspice Simulation Report  

---

## 1. Introduction  

This report presents the design and simulation of a **CMOS common-source amplifier** implemented using **TSMC 0.18 µm technology**.  

The design focuses on achieving a balance between **gain, linearity, and bias stability** using MOS-based techniques.

Key techniques used:
- **Source degeneration (M3)** → improves linearity and stabilizes operating point  
- **PMOS active load (M2)** → provides high output resistance for better gain  

### Circuit Topology  

- **M1 (NMOS)** → Main amplifying transistor (common-source stage)  
- **M2 (PMOS)** → Diode-connected active load  
- **M3 (NMOS)** → Diode-connected source degeneration  

---

## 2. Diode-Connected MOSFET  

A diode-connected MOSFET is formed by shorting the **gate and drain terminals**, resulting in:  

V_GS = V_DS  

### Key Characteristics  

- Operates in **saturation region** when V_GS > V_TH  
- Acts as a **nonlinear resistive element**  
- Provides **self-biasing and improved stability**  

### Small-Signal Model  

- Equivalent resistance ≈ **1/g_m**  
- In parallel with **output resistance (r_o)**  

👉 Effective resistance ≈ **1/g_m**  

---

## 3. Design Specifications  

The circuit is designed under the following conditions:

- **Technology:** TSMC 0.18 µm CMOS  
- **Supply Voltage (VDD):** 1.2 V  
- **Target Drain Current (ID):** ~200 µA  
- **Input Signal:** Small-signal sine wave (~10 mV amplitude)  
- **Biasing:** DC bias ensures saturation operation  

### Circuit Schematic  

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/l.png?raw=true)

| Parameter | Symbol | Value |
|-----------|--------|-------|
| Drain Current | I_D | 200 µA |
| Overdrive Voltage | V_OV | 0.2 V |
| Supply Voltage | V_DD | 1.2 V |
| NMOS Threshold | V_THn | 0.366 V |
| PMOS Threshold | \|V_THp\| | 0.39 V |
| Channel Length Modulation | λ | 0.1 V⁻¹ |
| Channel Length | L | 0.18 µm |
| NMOS Mobility | µn | 273.809 cm²/Vs |
| PMOS Mobility | µp | 115.689 cm²/Vs |
| Oxide Thickness | T_ox | 4.1 nm |
| Relative Permittivity | εr | 3.9 |


### 3.5 Operating Point (Simulation Results)


![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c3%20dc%20operating%20point.png?raw=true)


| Node/Device | Parameter | Simulated Value |
|-------------|----------|-----------------|
| V(n001) | VDD | 1.2 V |
| V(n002) | Bias Voltage | 0.56 V |
| V(n003) | Bias Node | 1.2 V |
| V(n004) | Source Voltage (M1) | 0.536 V |
| V(vin) | Input Voltage | 1.22 V |
| V(vout) | Output Voltage | 0.5901 V |
| Id(M1) | Drain Current M1 | 200.66 µA |
| Id(M2) | Drain Current M2 | -200.66 µA |
| Id(M3) | Drain Current M3 | 200.66 µA |


## 4. Device Width Calculation

### 4.1 Theoretical Width Formula

From the saturation drain current equation:

```
ID = (1/2) µ Cox (W/L) VOV²

W = (2 ID L) / (µ Cox VOV²)
```

### 4.2 NMOS Width (M1 and M3) — VOV = 0.2 V

```
Wn = (2 × 200×10⁻⁶ × 0.18×10⁻⁶) / (0.02738 × 8.42×10⁻³ × (0.2)²)

Numerator   = 7.2×10⁻¹¹
Denominator = 9.22×10⁻⁶

Wn ≈ 7.81 µm  (theoretical)
```

> Note: Theoretical calculation gives a minimum device size. In simulation, larger widths are used to set the correct bias point and current.

### 4.3 PMOS Width (M2) — VOV = 0.2 V

```
Wp = (2 × 200×10⁻⁶ × 0.18×10⁻⁶) / (0.01157 × 8.42×10⁻³ × (0.2)²)

Numerator   = 7.2×10⁻¹¹
Denominator = 3.90×10⁻⁶

Wp ≈ 18.5 µm  (theoretical)

```

> Note: Theoretical calculation gives a minimum device size. In simulation, larger widths are used to set the correct bias point and current.

### 4.3 PMOS Width (M2) — VOV = 0.2 V

```
Wp = (2 × 200×10⁻⁶ × 0.18×10⁻⁶) / (0.01157 × 8.42×10⁻³ × (0.2)²)

Numerator   = 7.2×10⁻¹¹
Denominator = 3.90×10⁻⁶

Wp ≈ 18.5 µm  (theoretical)
```

### 4.4 Final Device Sizing — Simulation-Optimized (Trial & Error)

The theoretical widths were used as a starting point. Final widths were iteratively tuned in LTspice to achieve the target I_D = 200 µA, correct DC operating point, and symmetric operation.

| Transistor | Role | Theoretical W (µm) | Final W (µm) | L (µm) |
|------------|------|--------------------|--------------|--------|
| M1 (NMOS) | Common-source amplifier | 7.81 | 47 | 0.18 |
| M2 (PMOS) | Active load (diode-connected) | 18.5 | 38.7 | 0.18 |
| M3 (NMOS) | Source degeneration (diode-connected) | 7.81 | 27 | 0.18 |

**Key Observations:**

- The final widths are significantly higher than theoretical values, indicating that practical TSMC model behavior requires **larger device sizing to achieve the target current**.

- M1 (47 µm) is sized larger than M3 (27 µm), ensuring **strong amplification while maintaining controlled source degeneration**.

- M2 (38.7 µm) provides an effective active load, helping achieve a **balanced output voltage (~0.59 V)** close to mid-supply.

- The drain currents (~200.66 µA) across M1, M2, and M3 confirm **proper current mirroring and bias consistency**.

- The output voltage being near half of VDD indicates **good biasing and symmetric signal swing capability**.

- Source degeneration via M3 improves **stability and linearity**, even though it slightly reduces gain.


## 5. Small-Signal Analysis  

### 5.1 Transconductance (g_m)

```
g_m = 2I_D / V_OV  
    = (2 × 200×10⁻⁶) / 0.2  
    = 2 mS
```

---

### 5.2 Output Resistance (r_o)

```
r_o = 1 / (λ I_D)  
    = 1 / (0.1 × 200×10⁻⁶)  
    = 50 kΩ
```

---

### 5.3 Voltage Gain (Theoretical)

Considering source degeneration due to diode-connected M3  
(effective resistance ≈ 1/g_m3):

```
A_v = − (g_m1 × r_o2) / (1 + g_m1/g_m3)  
    = − (2 mS × 50 kΩ) / (1 + 1)  
    = − 50 V/V
```

```
|A_v| = 50 V/V  
Gain (dB) = 20 log(50) = 33.97 dB
```

---

## 6. DC Transfer Characteristic  

<img width="2982" height="746" alt="DC Transfer Curve" src="https://github.com/user-attachments/assets/3d79393e-33bf-479d-a17d-05f77b2756e6" />

The DC sweep (V₃: 0 → 2 V) illustrates the input–output relationship of the amplifier.

- Linear operating region: **~0.9 V to 1.2 V (input)**  
- Output swing: **~1.17 V → ~0.56 V**  
- Midpoint corresponds to the **quiescent bias point**

This confirms that the circuit is properly biased for **maximum symmetrical signal swing**.


## 7. Transient Analysis  

### Input Waveform  

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c3%20input.png?raw=true)

The input is a small sinusoidal signal centered around **~1.12 V** with an amplitude of approximately **±10 mV**.

---

### Output Waveform  

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c3%20output.png?raw=true)

The output signal varies between **~0.67 V and ~0.88 V** (≈ **200 mV peak-to-peak**), showing clear amplification.  
The signal is **phase inverted**, confirming common-source operation.

---

### Input and Output Waveforms  

![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c3%20both%20.png?raw=true)

The output waveform is an **amplified and inverted version** of the input, centered around the DC operating point.  
This verifies proper biasing and expected amplifier behavior.


### 7.1 Simulation Cursor Readings  

| Cursor | Time | V(vout) |
|--------|------|---------|
| Cursor 1 (min) | 2.2566 ms | 674.23 mV |
| Cursor 2 (max) | 2.7516 ms | 876.36 mV |
| Difference (Δ) | 0.4951 ms | 202.13 mV |

---

### 7.2 Practical Gain Calculation  

```
ΔVout = 876.36 − 674.23 = 202.13 mV  (peak-to-peak)

Vin amplitude = 10 mV  

Av (practical) = (202.13 mV / 2) / 10 mV  
               ≈ 10.1 V/V
```

---

### 7.3 Gain Comparison  

| Parameter | Theoretical | Practical (Simulation) |
|-----------|-------------|------------------------|
| Av (V/V) | 50 V/V | ~10.1 V/V |
| Av (dB) | 33.97 dB | ~20.08 dB |

---

> The reduced gain compared to theory is mainly due to non-ideal effects such as finite output resistance (r_o), channel-length modulation, and the impact of source degeneration.


## 8. AC Analysis (Frequency Response)

<img width="2999" height="1617" alt="AC Response" src="https://github.com/Dhureen-07/LIC-Experiments/blob/main/c3%20ac.png?raw=true" />

**Simulation:** `.ac dec 10 100 10G` | Input: `AC 1`

---

### 8.1 Bode Plot Cursor Readings

| Cursor | Frequency | Magnitude | Phase | Group Delay |
|--------|-----------|-----------|-------|-------------|
| Cursor 1 (−3 dB region) | 396.59 MHz | 17.81 dB | −130.27° | 230.77 ps |
| Cursor 2 (midband) | 994.94 kHz | 20.87 dB | 179.84° | 437.85 ps |

---

### 8.2 Key Frequency Parameters

```
Midband gain     = 20.87 dB  
3 dB level       = 20.87 − 3 = 17.87 dB  
f3dB (approx)    ≈ 396 MHz
```

| Parameter | Value |
|-----------|-------|
| Midband Gain | 20.87 dB (≈ 11.05 V/V) |
| Lower Cutoff Frequency (fL) | ~0 Hz (DC coupled) |
| Upper Cutoff Frequency (fH) | ~396 MHz |
| Bandwidth | ~396 MHz |

---

### 8.3 Unity Gain Bandwidth (UGB)


![image alt](https://github.com/Dhureen-07/LIC-Experiments/blob/main/c3%20unity%20gain%20.png?raw=true)


```
UGB ≈ Gain × Bandwidth  
    ≈ 11.05 × 396 MHz  
    ≈ 4.38 GHz
```

| Parameter | Value |
|-----------|-------|
| Unity Gain Bandwidth (UGB) | ~4.38 GHz |

---

### 8.4 Summary

- High midband gain (~20.87 dB) confirms strong amplification  
- Bandwidth limited to ~396 MHz due to parasitic effects  
- UGB in GHz range indicates good high-frequency performance  


## 9. Summary of Results  

| Analysis | Parameter | Value |
|----------|-----------|-------|
| DC | Input bias (Vin) | ~1.12 V |
| DC | Output bias (Vout) | ~0.762 V |
| DC | Drain current (ID) | ~200.46 µA |
| DC | Source voltage (VS1) | ~0.569 V |
| Small-Signal | gm | ~2 mS |
| Small-Signal | ro | ~50 kΩ |
| Gain | Theoretical | 50 V/V (33.97 dB) |
| Gain | Practical (Transient) | ~10.1 V/V (~20.08 dB) |
| Gain | AC Midband Gain | ~20.87 dB |
| Frequency | fH (3 dB bandwidth) | ~396 MHz |
| Frequency | Unity Gain Bandwidth (UGB) | ~4.38 GHz |
| Sizing | W(M1) | 47 µm |
| Sizing | W(M2) | 38.7 µm |
| Sizing | W(M3) | 27 µm |

---

## 10. Key Observations  

- The DC operating point confirms **proper biasing near mid-supply**, enabling symmetric signal swing.  

- The practical gain (~10 V/V) is lower than theoretical due to:
  - Finite output resistance (r_o)  
  - Channel-length modulation  
  - Source degeneration effect  

- The AC midband gain (~20.87 dB) closely matches the transient-based gain, validating consistency.  

- The bandwidth (~396 MHz) is limited by **parasitic capacitances at the output node**.  

- The unity gain bandwidth (~4.38 GHz) indicates **good high-frequency performance**.  

- Larger device widths (compared to theoretical) are required to achieve the target current due to **short-channel effects in 0.18 µm technology**.  

- Unequal sizing of M1, M2, and M3 ensures a balance between **gain, stability, and linearity**.  

---
