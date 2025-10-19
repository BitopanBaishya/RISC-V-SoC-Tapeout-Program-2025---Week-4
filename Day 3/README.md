# Day 3: CMOS Switching Threshold and Dynamic Simulations
 
The focus of today is 

---

## 📜 Table of Contents
[1. Setting up the SPICE Deck](#1-setting-up-the-spice-deck)<br>
[2. SPICE VTC Simulations of MOSFET](#2-spice-vtc-simulations-of-mosfet)<br>
[3. Outputs](#3-outputs)<br>
[4. Analysis of VTC Simulations](#4-analysis-of-vtc-simulations)<br>
[5. Relationship between (W/L) Ratio and Vm of a MOSFET](#5-relationship-between-frac-wl-ratio-and-v_m-of-a-mosfet)<br>


---

## 1. Setting up the SPICE Deck
Before running simulations, a SPICE deck must be prepared. The SPICE deck contains:
- **Component connectivity**: How NMOS, PMOS, load capacitor and other compopnents are connected.
- **Component values**: Transistor sizes (W/L), load capacitance, power supply (VDD) and other components values.
- **Input and measurement nodes**: Points where Vin is applied and Vout is measured.

  <div align="center">
       <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/bc7aa4ea80879a151fffcf9a5184fea5c7d24cb3/Day%203/Images/CMOS_Netlist.png" alt="" width="600"/>
       <p align="center"><em>Example of a CMOS SPICE Netlist</em></p>
  </div>

---

## 2. SPICE VTC Simulations of MOSFET
We ran the VTC simulations for two PMOS width configurations while keeping the NMOS constant:
| Case | Wpmos   | Wnmos   | L (both) |
| ---- | ------- | ------- | -------- |
| A    | 0.44 µm | 0.36 µm | 0.15 µm  |
| B    | 0.84 µm | 0.36 µm | 0.15 µm  |

The SPICE Netlists used for these simulations are [this](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/036af8a994b1d91dbd8db13b311a2b2dd8d7b631/Day%203/SPICE%20Netlists/day3_inv_vtc_Wp044_Wn036.spice) and [this](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/036af8a994b1d91dbd8db13b311a2b2dd8d7b631/Day%203/SPICE%20Netlists/day3_inv_vtc_Wp084_Wn036.spice).

---

## 3. Outputs
The outputs from the above SPICE simulations are as follows:
<div align="center">
  <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/1ae29535e99030d51bdd2f1da9d609e03c8a5ee6/Day%203/Images/day3_inv_vtc_Wp044_Wn036_VTC.png" alt="" width="1000"/>
  <p align="center"><em>Voltage Transfer Characteristics for the MOSFET with Wpmos=0.44 µm & Wnmos=0.36 µm</em></p>
</div>
<div align="center">
  <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/1ae29535e99030d51bdd2f1da9d609e03c8a5ee6/Day%203/Images/day3_inv_vtc_Wp084_Wn036_VTC.png" alt="" width="1000"/>
  <p align="center"><em>Voltage Transfer Characteristics for the MOSFET with Wpmos=0.84 µm & Wnmos=0.36 µm</em></p>
</div>

---

## 4. Analysis of VTC Simulations
*Observations:*
1. Shape Robustness:
   * The overall shape of the VTC remains largely unchanged even with different PMOS sizes, although slight lateral displacements have been observed.
   * This demonstrates the robustness of CMOS inverters, which contributes to their widespread use in digital circuits.
2. $$\text{Switching Threshold } (V_m):$$
   * $$\text{Definition: } V_m \text{ is the input voltage at which the output equals the input } (V_{in} = V_{out}),\text{ i.e., the inverter is at its midpoint switching point.}$$
   * $$\text{At this point, both NMOS and PMOS are typically in saturation, and there is a high chance of direct current from } V_{DD} \text{ to } V_{SS}.$$
   * $$\text{Graphical Method: Draw a 45° line on the VTC and find the intersection with the curve } (V_{in} = V_{out}).$$
   * For example, Switching Threshold for the above two SPICE simulations are as follows:
     <div align="center">
       <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/e34b89f39a0dca5e8336ee7e06575a3481064658/Day%203/Images/vtc_ThresholdVoltage.png" alt="" width="1000"/>
       <p align="center"><em>Determination of Threshold Voltage from the Voltage Transfer Characteristics</em></p>
     </div>

---

## 5. Relationship between $$(\frac {W}{L})$$ Ratio and $$V_m$$ of a MOSFET
The switching threshold voltage $$V_m$$ of a CMOS inverter depends on the relative driving strengths of the PMOS and NMOS transistors. These strengths are determined by their respective (W/L) ratios and mobility constants.

### <ins>1. Finding $$V_m$$ for known (W/L) ratios</ins>
The drain current for a MOSFET (neglecting channel length modulation, i.e., (assuming λ≈0) is given by

$$
I<sub>D</sub> = \mu_n C_{ox} \frac{W}{L} \left[ (V<sub>G</sub> - V<sub>t</sub>) V<sub>DSsat</sub> - \frac{V<sub>DSsat</sub>^2}{2} \right]
$$

For the NMOS and PMOS transistors in the CMOS inverter at the switching point:

$$
I_{DS,N} = k_n \left[ (V_m - V_{t,N})V_{DSsat,N} - \frac{V_{DSsat,N}^2}{2} \right]
$$

$$
I_{DS,P} = k_p \left[ (V_m - V_{DD} - V_{t,P})V_{DSsat,P} - \frac{V_{DSsat,P}^2}{2} \right]
$$

At the switching threshold $$V_m$$, both transistors conduct equal and opposite currents, hence:

$$
I<sub>DSP</sub> + I<sub>DSN</sub> = 0 \quad \text{or} \quad I<sub>DSP</sub> = -I<sub>DSN</sub>
$$

Solving this relation gives:

$$
V<sub>m</sub> = \frac{R_R \cdot V_{DD}}{1 + R}
$$

where

$$
R = \frac{k_p V_{DSsatP}}{k_n V_{DSsatN}} 
  = \frac{ (\frac{W_p}{Lp}) k'_p V_{DSsat}} {(\frac{W_n}{L_n}) k'_n V_{DSsatN}}
$$

This equation indicates that $$V_m$$ depends on both the device geometry (W/L) and the process parameters (k', $$V_{DSsat}$$) of the PMOS and NMOS transistors.

### <ins>2. Finding the required (W/L) ratio for a fixed $$V_m$$</ins>
If the desired switching threshold $$V_m$$ is known, we can determine the required sizing ratio of the PMOS and NMOS transistors.<br>
Starting once again from the current equality condition:

$$
I_{DSP} = -I_{DSN}
$$

Rewriting in terms of device parameters:

$$
\frac{k_p V_{DSsatP}}{k_n V_{DSsatN}} 
= \frac{\left(\frac{W_p}{L_p}\right) k'_p V_{DSsatP}}
       {\left(\frac{W_n}{L_n}\right) k'_n V_{DSsatN}}
$$

Rearranging, we get:

$$
\frac{W_p/L_p}{W_n/L_n} = \frac{k'_n V_{DSsatN} (V_m - V_{tN}) - \frac{V_{DSsatN}^2}{2}}{k'_p V_{DSsatP} (-V_m + V_{DD} + V_{tP}) - \frac{V_{DSsatP}^2}{2}}
$$

This gives the required PMOS-to-NMOS size ratio to achieve a desired inverter switching threshold $$V_m$$.<br>
In simpler terms, this relation tells us how much larger the PMOS must be than the NMOS to balance their currents and center the switching threshold near $$\frac{V_{DD}}{2}$$.

### <ins>3. Interpretations</ins>
- Increasing the PMOS width $$W_P$$ shifts $$V_m$$ higher (towards $$V_{DD}$$.
- Decreasing $$W_p$$ shifts ##V_m$$ lower (towards 0V).
- A balanced design typically has $$\frac{W_p}{W_n}≈ 2-3 due to the lower hole mobility in PMOS.


