# Day 4: CMOS Noise Margin Robustness Evaluation

The focus of today is to

---

## 📜 Table of Contents
[1. What is Noise Margin and why it matters?](#1-what-is-noise-margin-and-why-it-matters)<br>

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




















