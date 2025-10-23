# Day 4: CMOS Noise Margin Robustness Evaluation

Today we focus on the robustness of CMOS inverters against noise. By examining the Voltage Transfer Characteristics (VTC) for various PMOS sizes, we extract the noise margins — $$NM_H$$ and $$NM_L$$ — which quantify the circuit’s tolerance to voltage fluctuations. This evaluation helps identify how transistor sizing affects reliability, logic level stability, and overall inverter performance under real-world non-ideal conditions.

---

## 📜 Table of Contents
[1. What is Noise Margin and why it matters?](#1-what-is-noise-margin-and-why-it-matters)<br>
[2. Lab: Noise Margin Extraction from VTC Simulations](#2-lab-noise-margin-extraction-from-vtc-simulations)<br>

---

## 1. What is Noise Margin and why it matters?
A digital inverter does not switch perfectly instantaneously in the real world. The Voltage Transfer Characteristic (VTC) of a practical inverter is a continuous curve that maps input voltage $$V_{in}$$ to output voltage $$V_{out}$$. Because transitions are not ideal, we define noise margins to quantify how much unwanted perturbation (noise) the gate can tolerate while still producing a valid logic level at its output and at the input of the next stage.<br><br>
Two primary noise margins are used:
- **Noise Margin High ($$NM_H$$)** — the maximum DC noise voltage that can be superimposed on a valid logic HIGH at the output without causing a logic error at the next stage.
- **Noise Margin Low ($$NM_L$$)** — the maximum DC noise voltage that can be superimposed on a valid logic LOW at the output without causing a logic error at the next stage.

Formally (standard definitions):
- Let $$V_{IL}$$ and $$V_{IH}$$ be the input voltages that define the input-valid region boundaries.
- Let $$V_{OL}$$ and $$V_{OH}$$ be the output voltages corresponding to those boundaries:
  * $$V_{OL} = V_{out}$$ at $$V_{in} = V_{IH}$$ (output low level at the right slope point).
  * $$V_{OH} = V_{out}$$ at $$V_{in} = V_{IL}$$ (output high level at the left slope point).

Then the noise margins are:

$$
NM_H = V_{OH} - V_{IH} \hspace{2cm} \text{and} \hspace{2cm} NM_L = V_{IL} - V_{OL}
$$

<div align="center">
    <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/d945dc1b25bd990793ecf58d4fa3642c432b6a8a/Day%204/Images/NoiseMargin_Visualization.png" alt="" width="800"/>
    <p align="center"><em>Visualization of Noice Margin from VTC of a CMOS inverter</em></p>
</div>

A useful visual is a vertical stack showing these voltages, as shown above.<br>
Interpretation of harmfulness:
- Within safe HIGH ($$V_{IH} → V_{OH}$$): small positive or negative bumps are tolerated — the next stage still sees a valid HIGH.
- Between $$V_{IL} and V_{IH}$$ (indeterminate region): the next stage may flip unpredictably — this is the most harmful region.
- Below $$V_{IL}$$: safe LOW region.

<div align="center">
    <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/f9cf5e79e79f6c4b9624c7238f2d6b5551723d58/Day%204/Images/NoiseMargin_Regions.png" alt="" width="800"/>
</div>

A small table summarizing harmfullness:
| Region        | Voltage band              | Likelihood of logic error |
| :-----------: | :-----------------------: | :-----------------------: |
| Safe HIGH     | $$(V_{IH} \le V \le V_{OH})$$ | Low                       |
| Indeterminate | $$(V_{IL} < V < V_{IH})  $$   | High (danger)             |
| Safe LOW      | $$(V_{OL} \le V \le V_{IL})$$ | Low                       |

---

## 2. Lab: Noise Margin Extraction from VTC Simulations
### <ins>1. Objective</ins>
To extract and analyze the noise margins (NM<sub>H</sub> and NM<sub>L</sub>) of a CMOS inverter from its Voltage Transfer Characteristics (VTC) for various PMOS widths $$(W_p)$$, while keeping NMOS parameters constant.

### <ins>2. Simulation setup</ins>
- **Supply voltage:** V<sub>DD</sub> = 1.8V
- **NMOS width:** W<sub>n</sub> = 0.36µm
- **NMOS & PMOS lengths:** L<sub>n</sub> = L<sub>p</sub> = 0.15µm
- **PMOS width** (W<sub>p</sub>) varied across 5 cases:  0.42µm, 0.84µm, 1.26µm, 1.68µm, 2.10µm

A DC sweep simulation was performed for each case, and from the resulting VTCs the following voltage levels were extracted:
- **V<sub>OH</sub>:** Output high level
- **V<sub>OL</sub>:** Output low level
- **V<sub>IH</sub>:** Input high threshold (slope = –1 point on right)
- **V<sub>IL</sub>:** Input low threshold (slope = –1 point on left)
- **V<sub>M</sub>:** Switching threshold (midpoint of VTC)

The SPICE Netlists used for the simulation of the above 5 cases can be found [here](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/tree/3a217e62605f9e29df601df23a072ce9c33ecbbe/Day%204/SPICE%20Netlists).

### <ins>3. Output VTC curves</ins>
<div align="center">
 <img src="https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/7c358eddbf804fa2a72d19151870f9182e1330b2/Day%204/Images/CMOS_VTC_curves.png" alt="" width="1000"/>
 <p align="center"><em>VTC of the 5 different variations of CMOS, and determination of Noise Margins and Switching Voltage</em></p>
</div>

### <ins>4. Extracted Results</ins>
| Case | $$W_p$$ (µm) | $$V_{OH}$$ (V) | $$V_{OL}$$ (V) | $$V_{IH}$$ (V) | $$V_{IL}$$ (V) | $$V_M$$ (V) | $$NM_H = V_{OH}-V_{IH}$$ (V) | $$NM_L = V_{IL}-V_{OL}$$ (V) |
| :--: | :--------: | :----------: | :----------: | :----------: | :----------: | :-------: | :------------------------: | :------------------------: |
|   1  |    0.42    |     1.75     |     0.06     |     0.90     |     0.69     |    0.82   |          **0.85**          |          **0.63**          |
|   2  |    0.84    |     1.74     |     0.09     |     1.00     |     0.74     |    0.88   |          **0.74**          |          **0.65**          |
|   3  |    1.26    |     1.74     |     0.07     |     0.99     |     0.76     |    0.88   |          **0.75**          |          **0.69**          |
|   4  |    1.68    |     1.74     |     0.08     |     1.03     |     0.77     |    0.91   |          **0.71**          |          **0.69**          |
|   5  |    2.10    |     1.74     |     0.07     |     1.05     |     0.78     |    0.92   |          **0.69**          |          **0.71**          |

### <ins>5. Observations and Analysis</ins>
- As the PMOS width $$(W_p)$$ increases, the switching threshold $$(V_M)$$ gradually shifts to higher values (from 0.82 V to 0.92 V).  → This confirms that the inverter becomes more balanced between the pull-up and pull-down networks, moving the transition point toward mid-supply.
- The Noise Margin High $$(NM_H)$$ initially decreases from 0.85 V to around 0.69 V as $$(W_p)$$ increases.  → A stronger PMOS raises $$(V_{IH})$$, reducing the voltage difference $$(V_{OH} - V_{IH})$$.
- The Noise Margin Low $$(NM_L)$$ stays roughly constant or slightly increases, from 0.63 V to 0.71 V.  → The NMOS defines the low-state behavior; increasing PMOS width does not strongly affect $$(V_{OL})$$, but it raises $$(V_{IL})$$, leading to a mild increase in $$(NM_L)$$.
- Beyond a certain $$(W_p / W_n)$$ ratio (~4–5), both margins begin to saturate — further upsizing $$(W_p)$$ gives diminishing returns and may only increase parasitics.

### <ins>6. Conclusions</ins>
The extracted results illustrate the delicate balance of CMOS inverter sizing:
- Increasing $$(W_p)$$ strengthens the PMOS pull-up, making logic ‘1’ transitions faster and improving $$(NM_L)$$ marginally.
- However, it also shifts the threshold $$(V_M)$$ upward and reduces $$(NM_H)$$.
- Beyond moderate sizing, further increase yields no substantial improvement in noise immunity.

Hence, an optimum $$(W_p / W_n)$$ ratio must be chosen such that both $$(NM_H)$$ and $$(NM_L)$$ are close and maximized — typically when $$(V_M) \approx V_{DD}/2$$.















