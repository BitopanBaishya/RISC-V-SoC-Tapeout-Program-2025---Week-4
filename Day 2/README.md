# Day 2: Velocity Saturation and Basics of CMOS Inverter VTC
 
Today, we journey deeper into the transistor world, exploring how shrinking channel lengths reshape MOSFET behavior. We start by uncovering the phenomena of short-channel effects and velocity saturation, phenomena that dominate modern nanometer-scale devices. Through SPICE simulations, we compare long-channel and short-channel MOSFETs to see these effects in action. Finally, we step into the digital domain, understanding the fundamentals of CMOS inverter operation and constructing the Voltage Transfer Characteristic (VTC), the cornerstone of digital circuit analysis.

---

## 📜 Table of Contents
[1. Introduction: The Era of Short Channels](#1-introduction-the-era-of-short-channels)<br>
[2. Lab: Comparison between Short-channel and Long-channel MOSFET Behaviour](#2-lab-comparison-between-short-channel-and-long-channel-mosfet-behaviour)<br>
[3. Theoretical Framework — Regions of Operation & Velocity Saturation](#3-theoretical-framework--regions-of-operation--velocity-saturation)<br>
[4. CMOS Voltage Transfer Characteristics (VTC)](#4-cmos-voltage-transfer-characteristics-vtc)<br>

---

## 1. Introduction: The Era of Short Channels
In classical MOSFET theory, the channel length L (the distance between the source and drain) is assumed to be long enough that the electric field and carrier motion can be modeled smoothly across it. This assumption breaks down as we scale down transistor dimensions to the deep submicron and nanometer regime — commonly referred to as lower technology nodes (e.g., 180 nm, 90 nm, 45 nm, etc.).<br>
In these smaller geometries, the channel length becomes comparable to the depletion region widths, and the electric fields become much stronger. As a result, new physical phenomena begin to dominate transistor behavior — these are collectively known as **Short-Channel Effects (SCEs)**.

### <ins>1. What Are Short-Channel Effects?</ins>
Short-channel effects are deviations from the ideal MOSFET behavior observed in long-channel devices.<br>
They arise due to:
- High electric fields in the channel
- Drain-induced barrier lowering (DIBL)
- Velocity saturation of carriers
- Reduction in threshold voltage (V<sub>TH</sub>)

As a consequence:
- The drain current no longer follows the quadratic law,
- The device saturates earlier,
- And even for the same W/L ratio, the I<sub>D</sub> differs between long- and short-channel devices.

In simple terms — as we go to lower nodes, transistors switch faster, but they lose some of their ideality. SPICE simulations are a perfect tool to visualize these effects.

---

## 2. Lab: Comparison between Short-channel and Long-channel MOSFET Behaviour
On Day-1, we had performed a SPICE simulation of a MOSFET with L=2 µm and W=5 µm ([see here](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/main/Day%201/README.md#3-lab-1---nmos-idvgs-spice-simulation)), which was a long-channel MOSFET simulation. Here, we will further run a SPICE simulation for a short-channel MOSFET, keeping the W/L ratio constant, and then analyze the two cases. 

### <ins>1. Parameters of the two MOSFETS</ins>
The parameters of the two MOSFETs are as follows:

| Parameter | Long-Channel Device | Short-Channel Device |
|----------|----------|----------|
| Width (W) | 5 µm | 0.625 µm |
| Length (L) | 2 µm | 0.25 µm |
| W/L Ratio | 2.5 | 2.5 |
| V<sub>DS</sub> | 1.8 V | 1.8 V |
| Sweep Variable | V<sub>GS</sub> | V<sub>GS</sub> |

### <ins>2. SPICE Netlist for the Short Channel MOSFET</ins>
The SPICE Netlist for the short-channel MOSFET used in this simulation is not present in the `design` directory by default. You may find it [here](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/73d8ccd61b456750df6772c754da6be42f90a34a/Day%202/SPICE%20Netlists/day2_nfet_idvds_L025_W0625.spice) to download it and add it in the `design` directory before SPICE simulations.<br>
After that, run the SPICE simulations for this file following the same steps as in the Day-1 simulation, and obtain the I<sub>D</sub> vs V<sub>GS</sub> characteristics.

### <ins>3. The Outputs</ins>
<div>
  <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/2b0bec34903e8d7d90f692e37952f504e40dfea5/Day%202/Images/day2_nfet_idvds_L2_W5.spice_IDvsVD.png" alt="NMOS Structure" width="1000"/>
  <p align="center"><em>Figure 1: I<sub>D</sub> vs V<sub>GS</sub> characteristics for long-channel MOSFET (W=5 µm, L=2 µm).</em></p>
</div>
<div>
  <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/2b0bec34903e8d7d90f692e37952f504e40dfea5/Day%202/Images/day2_nfet_idvds_L025_W0625.spice_IDvsVD.png" alt="NMOS Structure" width="1000"/>
  <p align="center"><em>Figure 2: I<sub>D</sub> vs V<sub>GS</sub> characteristics for short-channel MOSFET  (W=0.625 µm, L=0.25 µm).</em></p>
</div>

### <ins>4. Analysis of the Output Characteristics</ins>
- **Observation 1**<br>
  For long-channel (higher node) devices:
  * The I<sub>D</sub>–V<sub>GS</sub> relationship is quadratic,
  * Current follows the classic square-law model:

    $$
    I_D = k_n \frac{(V_{GS} - V_T)^2}{2}
    $$

  For short-channel (lower node) devices:
  * The I<sub>D</sub>–V<sub>GS</sub> curve becomes more linear.

- **Observation 2**: Reduction in Peak Current<br>
  At lower technology nodes, the peak current in the saturation region reduces because of velocity saturation.<br>
  This causes:
  * Earlier saturation of I–V curves
  * Reduced drive current
  * Loss of quadratic dependence on V<sub>GS</sub>

Hence, identical W/L ratios no longer guarantee identical transistor performance — a critical insight in modern CMOS design.

---

## 3. Theoretical Framework — Regions of Operation & Velocity Saturation

### <ins>1. Regions of Operation</ins>
In higher technology nodes (long-channel devices), a MOSFET primarily operates in three distinct regions:
1. **Cutoff Region** – The transistor is OFF when $$V_{GS} < V_{T}$$
2. **Linear or Resistive Region** – For small $$\( V_{DS} \)$$, the device behaves like a voltage-controlled resistor.
3. **Saturation Region** – When $$\( V_{DS} \geq V_{GS} - V_{T} \)$$, the current saturates and ideally follows a quadratic relationship with $$\( V_{GS} \)$$.

However, as we move to lower technology nodes (short-channel devices), a fourth region emerges — the Velocity Saturation Region.

### <ins>2. The Velocity Saturation Effect</ins>
In long-channel transistors, the carrier velocity $$(\( v \))$$ increases linearly with the applied electric field $$(\( \varepsilon \))$$:

$$
v = \mu_n \varepsilon
$$

But in short-channel devices, as the electric field grows stronger, the velocity no longer increases indefinitely.  
Instead, it saturates at a constant value $$\( v_{sat} \)$$.<br>
This behavior can be expressed as:

$$
v_n =
\begin{cases}
\frac {\mu_n \varepsilon} {( 1 + \frac{\varepsilon}{\varepsilon_c})}, & \text{for } \varepsilon \leq \varepsilon_c \\
v_{sat}, & \text{for } \varepsilon \geq \varepsilon_c
\end{cases}
$$

where $$\( \varepsilon_c \)$$ is the critical electric field, marking the transition between linear and saturated velocity regions.<br>
For continuity at $$\( \varepsilon = \varepsilon_c \)$$:

$$
\varepsilon_c = \frac{2 v_{sat}}{\mu_n}
$$

### <ins>3. Modified Drain Current Equation</ins>
Incorporating this velocity-dependent behavior into the drain current model gives:

$$
I_D = \frac{\mu_n C_{ox}}{1 + (\frac{V_{DS}}{\varepsilon_c L})} \cdot \frac{W}{L} \left[ (V_{GS} - V_T) V_{DS} - \frac{V_{DS}^2}{2} \right]
$$

Here, the term $$\( \frac{1}{1 + (V_{DS} / \varepsilon_c L)} \)$$ accounts for the reduction in carrier velocity at high fields, leading to early current saturation in short-channel devices.<br>
To simplify the expression, we define:

$$
V_{GT} = V_{GS} - V_T
$$

This is known as the **gate overdrive voltage.**

### <ins>4. Unified Drain Current Model</ins>
A more compact, general equation that captures all regions of MOSFET operation is:

$$
I_D = k_n \left[ (V_{GT} \cdot V_{min}) - \frac{V_{min}^2}{2} \right] \left( 1 + \lambda V_{DS} \right)
$$

where:
- $$k_n = \mu_n C_{ox} \frac{W}{L}$$
- $$V_{min} = \min \left( V_{GT}, V_{DS}, V_{DSsat} \right)$$
- $$\lambda \text{ is the channel-length modulation parameter}$$
- $$V_{DSsat} \text{ is the drain saturation voltage, a technology-dependent constant determined by the foundry.}$$

This single expression flexibly models all regions based on which voltage dominates.

### <ins>5. Region-wise Behavior</ins>

#### **<ins>A. Cutoff Region</ins>**:
When $$\( V_{GT} < 0 \)$$:

$$
I_D = 0
$$

The device is fully off.
>[!NOTE]
> The Unified Drain Current Model equation does not cover the Cutoff region. It only defines the rest of the three regions (as explained below), in the case of short-channel MOSFETs.

#### **<ins>B. Linear (or Resistive) Region</ins>**:
When $$\( V_{DS} \)$$ is the smallest term, the transistor operates as a resistor:

$$
I_D = k_n \left[ (V_{GT} \cdot V_{DS}) - \frac{V_{DS}^2}{2} \right] \left( 1 + \lambda V_{DS} \right)
$$

This region shows ohmic or resistive behavior, where $$\( I_D \)$$ increases linearly with $$\( V_{DS} \)$$.

#### **<ins>C. Saturation Region (Long Channel)</ins>**:
When $$\( V_{GT} \)$$ becomes the limiting factor:

$$
I_D = k_n.\frac{V_{GT}^2}{2} \left( 1 + \lambda V_{DS} \right)
$$

This corresponds to the square-law region, where $$\( I_D \propto V_{GT}^2 \)$$.

#### **<ins>D. Velocity Saturation Region (Short Channel Only)</ins>**:
When $$\( V_{DSsat} \)$$ becomes the smallest voltage — a phenomenon seen only in short-channel devices — the drain current becomes:

$$
I_D = k_n \left[ (V_{GT} \cdot V_{DSsat}) - \frac{V_{DSsat}^2}{2} \right] \left( 1 + \lambda V_{DS} \right)
$$

Expanding $$\( k_n \)$$, we get:

$$
I_D = \mu_n C_{ox} \frac{W}{L} \left[ (V_{GT} \cdot V_{DSsat}) - \frac{V_{DSsat}^2}{2} \right] \left( 1 + \lambda V_{DS} \right)
$$

This region is governed by velocity saturation, where the carrier velocity is limited by $$\( v_{sat} \)$$ and the current grows linearly with $$\( V_{GT} \)$$ instead of quadratically.

---

## 4. CMOS Voltage Transfer Characteristics (VTC)

### <ins>1. MOSFET as a Switch</ins>
A MOSFET behaves as an electronic switch:
- **OFF state**: When $$\( |V_{GS}| < |V_t| \)$$, the MOSFET exhibits very high resistance (effectively infinite), and very little current flows.
- **ON state**: When $$\( |V_{GS}| > |V_t| \)$$, the MOSFET conducts, with a finite ON resistance allowing current to flow.

*This switching property is the foundation for digital CMOS logic.*

### <ins>2. CMOS Structure</ins>
CMOS stands for Complementary MOSFET. It combines a PMOS and an NMOS transistor in a complementary configuration:
- **PMOS Source**: Connected to $$\( V_{DD} \)$$
- **NMOS Source**: Connected to $$\( V_{SS} \)$$ (typically 0 V)
- **Gates**: Both connected to input $$\( V_{in} \)$$
- **Drains**: Both connected together at output $$\( V_{out} \)$$
- **Load Capacitance** $$\( C_L \)$$: Represents any load at the output (e.g., wiring, gate of another transistor).

Operation:
- Vin = VDD (High)
  * **NMOS**: $$\( V_{GS} = V_{DD} \rightarrow \text{ON} \)$$
  * **PMOS**: $$\( V_{GS} = 0 \rightarrow \text{OFF} \)$$
  * **Result**: Direct path from $$\( V_{out} \)$$ to $$\( V_{SS} \)$$, discharging $$\( C_L \) → \( V_{out} = 0 \)$$
- Vin = 0 (Low)
  * **NMOS**: $$\( V_{GS} = 0 \rightarrow \text{OFF} \)$$
  * **PMOS**: $$\( V_{GS} = -V_{DD} \rightarrow \text{ON} \)$$
  * **Result**: Direct path from $$\( V_{DD} \)$$ to $$\( V_{out} \)$$, charging $$\( C_L \) → \( V_{out} = V_{DD} \)$$

<div align="center">
  <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/c79f35e8da5b335548ddcaf57cb56d8a9d7e369b/Day%202/Images/CMOS_SwitchModel.png" alt="Alt Text" width="600"/>
</div>

### <ins>3. Naming Conventions</ins>
<div align="center">

| Symbol | Description |
|--------|-------------|
| G      | Gate        |
| S      | Source      |
| D      | Drain       |
| VgsN   | NMOS gate-to-source voltage |
| VgsP   | PMOS gate-to-source voltage |
| VdsN   | NMOS drain-to-source voltage |
| VdsP   | PMOS drain-to-source voltage |
| IdsN   | NMOS drain current |
| IdsP   | PMOS drain current |

</div>

<div align="center">
  <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/408119844c9223ff634587c941043005d5365431/Day%202/Images/CMOS_NamingConventions.png" alt="Alt Text" width="500"/>
</div>

Observations:
- $$V_{gsN} = V_{in} - V_{SS} = V_{in}$$
- $$V_{dsN} = V_{out}$$
- $$V_{gsP} = V_{in} - V_{DD} $$
- $$V_{dsP} = V_{out} - V_{DD} $$
- $$I_{dsP} = - I_{dsN}$$

### <ins>4. Id vs Vds Characteristics</ins>
- **NMOS**: Standard $$\( I_D \)$$ vs $$\( V_{DS} \)$$ curve with varying $$\( V_{GS} \)$$
- **PMOS**: Mirror of NMOS, appearing in the 3rd quadrant due to negative currents and voltages.

<div align="center">
  <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/9202e70a2ac9cd1b487cba4f582219da7c781656/Day%202/Images/CMOS_VIcharacteristics.png" alt="Alt Text" width="600"/>
</div>

However, in real circuits, we only observe $$\( V_{in} \)$$ and $$\( V_{out} \)$$. To model CMOS behavior, the curves must be translated in terms of these external voltages.

### <ins>5. Conversion to Vin vs Vout</ins>
Steps to obtain CMOS Voltage Transfer Curve:
1. Prepare PMOS load curve:
   * $$\text{Start with PMOS: } I_{dsP} \text{ vs } V_{dsP}$$
   * $$\text{Convert gate voltage: } V_{gsp} \rightarrow V_{in} = V_{gsp} + V_{DD}$$
   * $$\text{Replace drain voltage: } V_{dsP} \rightarrow V_{out} = V_{dsP} + V_{DD}$$
   * $$\text{Result: PMOS load curve in terms of } V_{in} \text{ and } I_{dsP}$$
2. Prepare NMOS load curve:
   * $$\text{For NMOS: } V_{gsN} = V_{in} \text{ and } V_{dsN} = V_{out}$$
   * $$\text{Directly map } I_{dsN} \text{ vs } V_{out}$$
3. Superimpose PMOS and NMOS curves:
   * $$\text{Find points where PMOS and NMOS currents match for each } V_{in}$$
   * $$I_{dsP} = -I_{dsN} \quad \Rightarrow \quad \text{the corresponding } V_{out} \text{ gives the CMOS output}$$
4. Result:
   * $$\text{Plotting } V_{out} \text{ vs } V_{in} \text{ gives the Voltage Transfer Characteristic (VTC) of the CMOS inverter.}$$
   * $$\text{This curve indicates the switching threshold and the regions where PMOS or NMOS dominate conduction.}$$

     <div align="center">
       <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/dc6bfe9004db606d1ae5bde70629e683fabc5408/Day%202/Images/PMOS%26NMOS_Superimpose.png" alt="NMOS Structure" width="600"/>
       <p align="center"><em>Superimposition of PMOS and NMOS I-V characteristics</em></p>
     </div>
     <div align="center">
       <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/dc6bfe9004db606d1ae5bde70629e683fabc5408/Day%202/Images/CMOS_VTC.png" alt="NMOS Structure" width="600"/>
       <p align="center"><em>Voltage Transfer Characteristics (Vin vs Vout)</em></p>
     </div>
