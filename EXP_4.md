# Experiment 4 — Differential Amplifier Analysis

## Aim

To design and simulate three MOSFET differential amplifier configurations using LTspice by performing DC, Transient, and AC analyses, and to compare their performance based on gain, bandwidth, and power efficiency.

---

## Introduction

A differential amplifier is one of the most fundamental building blocks in analog circuit design. It amplifies the difference between two input signals while **rejecting common-mode noise**, making it indispensable in precision and noise-sensitive applications.

MOSFET-based differential amplifiers are extensively used in integrated circuits — including operational amplifiers, comparators, and analog front-ends — owing to their **high gain, superior noise immunity, and area efficiency** in CMOS processes.

This experiment analyzes three distinct MOSFET differential amplifier configurations in LTspice using the **TSMC 180nm process**. Each configuration is evaluated through DC operating point analysis, Transient analysis, and AC frequency response to characterize gain, bandwidth, and power dissipation.

---

## Theory

### Differential Amplification Principle

A differential amplifier amplifies the difference between two input signals while rejecting any signal common to both inputs — a property known as **Common-Mode Rejection (CMR)**.

The differential input voltage is defined as:

$$v_{id} = v_{in1} - v_{in2}$$

A basic MOS differential pair consists of **two matched MOSFETs (M1 and M2)** whose sources are tied together and biased by a constant tail current source $I_{SS}$. The drains are connected to load elements (resistive or active).

---

### Current Steering

When a differential input is applied, the tail current $I_{SS}$ is steered between M1 and M2:

- If $v_{in1} > v_{in2}$ → M1 conducts more, M2 conducts less
- If $v_{in2} > v_{in1}$ → M2 conducts more, M1 conducts less

> **Key Point:** Both transistors must remain in the **saturation region** for linear, small-signal operation.

---

### Small-Signal Differential Gain

For small differential inputs where both transistors are in saturation, the voltage gain is:

$$\boxed{A_v = g_m \cdot R_{out}}$$

where:
- $g_m$ — transconductance of the MOSFET
- $R_{out}$ — effective output resistance of the amplifier

The transconductance is given by:

$$g_m = \frac{2I_D}{V_{ov}}$$

where:
- $I_D$ — DC drain current
- $V_{ov} = V_{GS} - V_{TH}$ — overdrive voltage

---

### Linear Operating Range

The circuit behaves as a linear amplifier only when:

$$|v_{id}| < \sqrt{2} \cdot V_{ov}$$

> **Key Point:** When $|v_{id}| > \sqrt{2} \cdot V_{ov}$, one transistor cuts off and the amplifier enters **non-linear (large-signal) operation**.

---

### Impact of Load Configuration

The choice of load directly determines amplifier performance:

| Load Type | Output Resistance | Voltage Gain |
|---|---|---|
| Resistive Load | Moderate | Moderate |
| Active Load (PMOS) | High | High |
| Current Mirror Load | Very High | Maximum |

> **Key Point:** Replacing resistive loads with active or current mirror loads significantly increases $R_{out}$ and therefore the differential gain $A_v$.

---

## Given Specifications

| Parameter | Value |
|---|---|
| Supply Voltage $V_{DD}$ | $+0.9\ \text{V}$ |
| Supply Voltage $V_{SS}$ | $-0.9\ \text{V}$ |
| Channel Length $L$ | $360\ \text{nm}$ |
| Maximum Power Dissipation $P$ | $\leq 1.5\ \text{mW}$ |
| Common-Mode Input Voltage $V_{inCM}$ | $0\ \text{V}$ |
| Common-Mode Output Voltage $V_{oCM}$ | $0\ \text{V}$ |
| Tail Node Voltage $V_P$ | $-0.7\ \text{V}$ |
| Load Capacitance $C_L$ | $10\ \text{pF}$ |
| Process Technology | TSMC 180nm CMOS |


## Circuit 1: Differential Amplifier with Resistive Load

### Working Principle

The circuit consists of two matched NMOS transistors **M1** and **M2** forming a differential pair. Their sources are tied to a constant tail current source $I_{SS}$, and their drains are connected to resistive loads $R_D$ referenced to $V_{DD}$.

When a differential input voltage is applied:

$$v_{id} = v_{in1} - v_{in2}$$

the tail current $I_{SS}$ is steered between M1 and M2 based on the input difference:

- $v_{in1} > v_{in2}$ → M1 draws more current, M2 draws less
- $v_{in2} > v_{in1}$ → M2 draws more current, M1 draws less

The resulting change in drain current produces a proportional voltage drop across $R_D$, generating differential output voltages at the drain nodes.

> **Key Point:** For $|v_{id}| < \sqrt{2}\,V_{ov}$, both transistors remain in saturation and the circuit operates as a **linear amplifier**. Beyond this range, one transistor cuts off and the response becomes non-linear.

### Voltage Gain

For small-signal differential operation, the voltage gain is:

$$\boxed{A_v = -g_m R_D}$$

The single-ended output resistance seen at each drain is simply $R_D$, which limits the achievable gain. This is the primary trade-off of a resistive load topology — simplicity at the cost of moderate gain.

### Limitations

- Gain is bounded by $R_D$, which cannot be made arbitrarily large without violating the saturation condition
- Higher $R_D$ reduces headroom for output voltage swing
- Power dissipation scales directly with $I_{SS}$ and $R_D$


## Circuit Diagram

