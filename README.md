# **EXPERIMENT 1**

## **DC, AC & Transient Analysis of a Common Source (CS) Amplifier**

### **TSMC 180nm CMOS Technology | LTSpice Simulation**

---

## **INTRODUCTION**

Analog amplifiers are fundamental building blocks in electronic and VLSI systems. Among MOSFET amplifier configurations, the **Common Source (CS) amplifier** is one of the most widely used structures because of its simple implementation, good voltage gain, and high input impedance.

In a Common Source amplifier:

- The **input signal** is applied at the gate terminal  
- The **output** is taken from the drain terminal  
- The **source** terminal is connected to ground  

The source terminal is common to both input and output circuits, which gives this configuration its name. The CS amplifier is the MOSFET counterpart of the Common Emitter amplifier in BJT circuits.

The CS configuration primarily provides:

- Voltage amplification  
- 180° phase inversion between input and output  
- High input impedance  

---

## **BIASING REQUIREMENT**

For proper amplification, the MOSFET must operate in the **saturation region**. If the transistor moves into cutoff or triode region during signal variation, distortion occurs and linear amplification is not maintained.

The condition for saturation is:

VDS ≥ (VGS − VT)


Where:

- VGS = Gate-to-Source voltage  
- VDS = Drain-to-Source voltage  
- VT = Threshold voltage  

To obtain maximum symmetrical output swing, the drain voltage is generally set near half of the supply voltage:

VDS ≈ VDD / 2


This operating point is referred to as the **Q-point**. Proper selection of Q-point ensures stable and distortion-free operation.

---

## **WORKING PRINCIPLE**

When a small AC signal is applied at the gate:

- An increase in VGS increases the drain current (ID)  
- The increased ID causes a larger voltage drop across RD  
- The drain voltage decreases  

Similarly:

- A decrease in VGS reduces ID  
- The voltage drop across RD decreases  
- The drain voltage increases  

Thus, the output voltage varies in the opposite direction of the input signal, resulting in a 180° phase shift.

The small-signal voltage gain of a Common Source amplifier is approximately:

Av = −gm × RD


Where:

- gm = Transconductance of the MOSFET  
- RD = Drain resistance  

The negative sign indicates phase inversion.

---

Where:

- gm = Transconductance of the MOSFET  
- RD = Drain resistance  

The negative sign indicates phase inversion.

---

## **CIRCUIT DESCRIPTION**

The basic Common Source amplifier used in this experiment consists of:

- NMOS transistor from TSMC 180nm technology library  
- Drain resistor (RD)  
- Supply voltage (VDD = 1.2V)  
- Input signal applied at the gate  
- Output taken from the drain  
- Source terminal connected to ground  

This circuit structure is implemented in LTSpice to perform:

- DC operating point analysis  
- Transient response analysis  
- AC frequency response analysis  

<img width="556" height="459" alt="Screenshot 2026-02-23 191931" src="https://github.com/user-attachments/assets/d99f7464-a76e-45be-a0be-6d4d2f14f299" />

## **3. DESIGN CALCULATIONS**

### **3.1 GIVEN PARAMETERS**

Technology: TSMC 180nm  
Supply Voltage, VDD = 1.2 V  
Power Constraint ≤ 0.4 mW  
Channel Length, Ln = 360 nm  
Threshold Voltage, VT ≈ 0.36 V  
Electron Mobility, μn = 273.81 × 10^-4 m²/V·s  
Load Capacitor, CL = 0.5 pF  
Gate Oxide Thickness, tox = 4.1 × 10^-9 m  

---

## **3.2 POWER CONSTRAINT**

The total power consumed by the circuit is given by:

P = VDD × ID  

Maximum allowed power:

P ≤ 0.4 mW  

Therefore,

ID ≤ (0.4 × 10^-3) / 1.2  

ID ≤ 333 µA  

To ensure safe operation within the limit and maintain controlled biasing, I selected:

ID = 200 µA  

Power dissipated:

P = 1.2 × 200 µA  
P = 0.24 mW  

Since 0.24 mW < 0.4 mW, the power constraint is satisfied.

---

## **3.3 BIAS POINT SELECTION**

For maximum symmetrical output swing:

VDS ≈ VDD / 2  

VDS = 1.2 / 2  
VDS = 0.6 V  

Since the source terminal is grounded:

VGS = VG − VS  

Assuming gate bias:

VGS = 0.9 − 0  
VGS = 0.9 V  

From the technology library:

VT ≈ 0.36 V  

The overdrive voltage is:

VOV = VGS − VT  

VOV = 0.9 − 0.36  
VOV = 0.54 V  

Now checking saturation condition:

VDS ≥ VOV  

0.6 V ≥ 0.54 V  

Since the condition is satisfied, the MOSFET operates in the saturation region, which is required for proper amplification.

---

## **3.4 CALCULATION OF DRAIN RESISTANCE (RD)**

Using KVL:

VDD − ID × RD = VDS  

1.2 − (200 × 10^-6) × RD = 0.6  

200 × 10^-6 × RD = 0.6  

RD = 0.6 / (200 × 10^-6)  

RD = 3 kΩ  

---

## **3.5 CALCULATION OF TRANSISTOR WIDTH (W)**

The drain current equation in saturation is:

ID = (1/2) × μnCox × (W/L) × (VOV)^2  

Using calculated values:

ID = 200 × 10^-6 A  
VOV = 0.54 V  
L = 180 nm = 180 × 10^-9 m  

Substituting and solving:

W ≈ 1.07 µm  

---

## **FINAL DESIGN VALUES**

ID = 200 µA  
RD = 3 kΩ  
Calculated Width (W) ≈ 1.07 µm  
VDS = 0.6 V  
Power Dissipation = 0.24 mW  

All design constraints are satisfied, and the device operates in saturation, ensuring proper voltage amplification.

DC Analysis

<img width="1905" height="406" alt="image" src="https://github.com/user-attachments/assets/13bcb416-476d-4253-9617-2314ef5b152d" />

---

## **DC OPERATING POINT VERIFICATION**

From LTSpice `.op` analysis:

- VDD = 1.2 V  
- VGS = 0.9 V  
- VDS ≈ 0.5987 V  
- ID ≈ 200 µA  

Since VDS ≈ VDD/2 (≈ 0.6 V), the Q-point is properly centered.

Also,

VOV = VGS − VT = 0.9 − 0.36 = 0.54 V  

Because VDS (0.5987 V) > VOV (0.54 V), the MOSFET operates in the **saturation region**.

Power dissipation:

P = VDD × ID = 1.2 × 200 µA = 0.24 mW  

Since 0.24 mW < 0.4 mW, the power constraint is satisfied.
The amplifier is correctly biased and ready for AC and transient analysis.

## **Transient Analysis**

Transient analysis is performed to observe the time-domain response of the amplifier and verify signal amplification and phase inversion under small-signal operation.

Input Signal (Vin)

<img width="1908" height="402" alt="image" src="https://github.com/user-attachments/assets/d8d18763-a18a-4e1f-85bd-64121bcb0f06" />

Output Signal (Vout)

<img width="1909" height="413" alt="image" src="https://github.com/user-attachments/assets/27b1af48-a834-4f9a-b4e7-a75145fbf888" />

## **Simulated Voltage Gain (From Transient Response)**

### Measured Peak-to-Peak Values

Vin(p-p) = 909.5633 mV − 890.42676 mV  
Vin(p-p) = 19.13654 mV  

Vout(p-p) = 616.4211 mV − 581.1320 mV  
Vout(p-p) = 35.2891 mV  

---

### Simulated Gain

Av = Vout(p-p) / Vin(p-p)

Av = (35.2891 × 10^-3) / (19.13654 × 10^-3)

Av ≈ 1.844  

Gain in dB:

Av(dB) = 20 log10(1.844)

Av(dB) ≈ 5.31 dB  

---

## **Theoretical Gain (Using RD = 3kΩ)**

Given:

ID = 200 µA  
VOV = VGS − VT = 0.9 − 0.36 = 0.54 V  

gm = (2 × ID) / VOV  
gm = (2 × 200 × 10^-6) / 0.54  
gm ≈ 0.00074 S  

Av = gm × RD  
Av = 0.00074 × 3000  

Av ≈ 2.22  

Gain in dB:

Av(dB) = 20 log10(2.22)  
Av(dB) ≈ 6.93 dB  

