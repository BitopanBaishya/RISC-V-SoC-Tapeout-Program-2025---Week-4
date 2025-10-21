# Day 3: CMOS Switching Threshold and Dynamic Simulations
 
The focus of today is to bridge static and dynamic behavior in CMOS circuits. We begin by exploring the switching threshold of an inverter through SPICE-based Voltage Transfer Characteristics (VTC) and understand how transistor sizing shifts this balance point. Then, we extend into transient analysis, observing the inverter’s real-time switching behavior, rise and fall delays, and how sizing ratios shape timing symmetry and performance.

---

## 📜 Table of Contents
[1. Setting up the SPICE Deck](#1-setting-up-the-spice-deck)<br>
[2. SPICE VTC Simulations of MOSFET](#2-spice-vtc-simulations-of-mosfet)<br>
[3. Outputs](#3-outputs)<br>
[4. Analysis of VTC Simulations](#4-analysis-of-vtc-simulations)<br>
[5. Relationship between (W/L) Ratio and Vm of a MOSFET](#5-relationship-between-frac-wl-ratio-and-v_m-of-a-mosfet)<br>
[6. Transient Analysis of CMOS Inverter](#6-transient-analysis-of-cmos-inverter)<br>
[7. Lab: Transient Analysis of CMOS Inverter](#7-lab-transient-analysis-of-cmos-inverter)<br>

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
       <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/19a7671ed5175a8c3312529d8b69fb9b757c95c4/Day%203/Images/vtc_ThresholdVoltage.png" alt="" width="1000"/>
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

---

## 6. Transient Analysis of CMOS Inverter

### <ins>1. What is Transient Analysis?</ins>
Transient analysis in SPICE is used to observe the time-domain behavior of a circuit — how voltages and currents change with time when an input signal varies.<br>
For a CMOS inverter, this analysis helps us understand:
- How fast the output switches from high to low and vice versa,
- The propagation delay of the inverter,
- The asymmetry between rise and fall times, and
- The effect of device sizing on switching performance.

*In simple words, transient analysis lets us see the inverter “in motion”, as it responds to a pulsed input — this is critical for timing optimization, delay estimation, and clock path balancing in VLSI circuits.*

### <ins>2. Why is Transient Analysis Necessary?</ins>
When designing digital systems, static analysis (like DC transfer curves) only shows where the inverter switches logically — it doesn’t reveal how long it takes to do so.<br>
Transient analysis provides this missing link. It helps determine:
- **Rise Delay** – time taken for output to rise from low to high when input goes low (in case of an inverter).
- **Fall Delay** – time taken for output to fall from high to low when input goes high (in case of an inverter).
- **Switching Threshold** – voltage at which both transistors conduct equal current.

These parameters together define speed, symmetry, and performance of a CMOS inverter.

---

## 7. Lab: Transient Analysis of CMOS Inverter

### <ins>1. Experimental Setup</ins>
We performed transient analysis for five different sizing ratios of PMOS to NMOS.<br><br>
Base case dimensions:
| Parameter  | NMOS    | PMOS    |
| ---------- | ------- | ------- |
| Width (W)  | 0.36 µm | 0.42 µm |
| Length (L) | 0.15 µm | 0.15 µm |

From this, for the base case,

$$
\frac{W_p}{W_n} = \frac{0.42}{0.36} \approx 1.167
$$

For the next four cases, the PMOS width was increased such that $$\frac {Wp}{Wn}$$ was 2×, 3×, 4×, and 5× of the first case.<br>
The dimensions of all the five cases are summarized below:
| Case | $$W_p$$ (µm) | $$L_p$$ (µm) | $$W_n$$ (µm) | $$L_n$$ (µm) | $$(W_p/L_p)$$ | $$(W_n/L_n)$$ |
| :--: | :--------: | :--------: | :--------: | :--------: | :---------: | :---------: |
|   1  |    0.42    |    0.15    |    0.36    |    0.15    |     1.167    |     1.00    |
|   2  |    0.84    |    0.15    |    0.36    |    0.15    |     2.33    |     1.00    |
|   3  |    1.26    |    0.15    |    0.36    |    0.15    |     3.50    |     1.00    |
|   4  |    1.68    |    0.15    |    0.36    |    0.15    |     4.67    |     1.00    |
|   5  |    2.10    |    0.15    |    0.36    |    0.15    |     5.83    |     1.00    |

The SPICE netlists for all cases can be found in [this](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/tree/f23ad7f323361fd6c0199031875800046ec6bcbd/Day%203/SPICE%20Netlists) folder.

### <ins>2. Simulation Details</ins>
- Each circuit was simulated in SPICE using a pulse input of 4 ns period (high = 1.8 V).<br>
- The rise and fall delays were measured at the 0.9 V level (50% of $$V_{DD}$$).
- All simulation commands remain identical to those used in Day 1, except for the final command inside ngspice, which should be:
  ```
  plot out vs time in
  ```
  This plots the output and input waveforms against time.

### <ins>3. Results and Observations</ins>
The outputs of the SPICE simulations for Transient Analysis of the above five cases are as follows:
<div align="center">
 <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/9981dfcffc02a9f0337b5b2827b96e5868c85c8d/Day%203/Images/CMOS_TransientAnalysis.png" alt="" width="1000"/>
 <p align="center"><em>Transient Analysis of the 5 different variations of CMOS, and determination of Rise and Fall Delays</em></p>
</div>

The observations from the graphs has been presented in the following table:
| Case | $$(W_p/L_p)$$ | $$(W_n/L_n)$$ | Rise Delay (ps) | Fall Delay (ps) |
| :--: | :---------: | :---------: | :-------------: | :-------------: |
|   1  |     1.167    |     1.00    | 615 | 260 |
|   2  |     2.33    |     1.00    | 332 | 280 |
|   3  |     3.50    |     1.00    | 233 | 286 |
|   4  |     4.67    |     1.00    | 179 | 289 |
|   5  |     5.83    |     1.00    | 147 | 291 |

### <ins>4. Analysis of Results</ins>
From the transient plots (attached above):
- Increasing $$W_p$$ improves the rise time (output rising faster).
- Fall time remains largely unaffected since NMOS is unchanged.
- As a result, the inverter transitions become more symmetric for a certain $$\frac {W_p}{W_n}$$ ratio.

  There exists a specific PMOS sizing for which rise and fall delays become equal — this is a critical design point for clock buffers and timing-critical logic.

### <ins>5. Practical Insights</ins>
- Faster rise → larger PMOS
- Faster fall → smaller PMOS (or larger NMOS)
- $$\text{Balanced design} \rightarrow t_{PLH} \approx t_{PHL}$$
- Real chips often target $$\frac{W_p}{W_n} \approx$$ 2.5 to 3 for equal delays.








