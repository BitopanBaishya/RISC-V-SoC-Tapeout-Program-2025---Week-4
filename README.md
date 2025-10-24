# Week 4: CMOS Circuit Design (sky130-style)
 
The focus of this week is to explore the device-level foundations of CMOS circuits using SPICE simulations. We begin by understanding MOSFET physics — their operation regions, I–V characteristics, and how scaling to nanometer dimensions introduces short-channel and velocity saturation effects. Then, we transition from single transistors to the CMOS inverter, analyzing its static and dynamic behavior, noise margins, and transient performance. Finally, we investigate how power supply scaling and device parameter variations influence gain, delay, and robustness. This week marks the bridge between semiconductor physics and digital logic — revealing how transistor-level nuances shape the soul of modern VLSI design.

---

## 📑 Objectives
- Day 1: Introduction to SPICE Simulation & MOSFET I–V Characteristics
- Day 2: Velocity Saturation and Basics of CMOS Inverter VTC
- Day 3: CMOS Switching Threshold and Dynamic Simulations
- Day 4: CMOS Noise Margin Robustness Evaluation
- Day 5: CMOS Power Supply and Device Variation Robustness Evaluation

---

## 📋 Prerequisites
- Basic understanding of Verilog codes.
- Basic understanding of Linux commands.

---

## 📈 Proceedings
- Proceed to [Day 1](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/main/Day%201/README.md)
- Proceed to [Day 2](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/main/Day%202/README.md)
- Proceed to [Day 3](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/main/Day%203/README.md)
- Proceed to [Day 4](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/main/Day%204/README.md)
- Proceed to [Day 5](https://github.com/BitopanBaishya/RISC-V-SoC-Tapeout-Program-2025---Week-4/blob/main/Day%205/README.md)

---

## 🏁 Final Remarks
Week 4 marked the transition from digital abstraction to transistor-level insight, using SPICE as the bridge between physics and logic. The objective was to understand how real devices behave — how currents flow, voltages shift, and imperfections ripple through a circuit’s performance.<br>
Key takeaways include:
- **MOSFET Device Characterization:**<br>
  Explored the I–V behavior of NMOS transistors across operation regions — cutoff, linear, and saturation — validating theoretical equations through SPICE simulations. This established a foundation for understanding current drive and threshold behavior.
- **Short-Channel and Velocity Saturation Effects:**<br>
  Compared long-channel and short-channel devices to visualize the onset of velocity saturation and drain-induced barrier lowering (DIBL). These experiments revealed how device scaling shapes modern transistor performance.
- **CMOS Inverter Analysis:**<br>
  Constructed and analyzed CMOS inverter Voltage Transfer Characteristics (VTC), studying how transistor sizing shifts the switching threshold (V<sub>M</sub>) and influences gain symmetry between pull-up and pull-down paths.
- **Transient and Delay Behavior:**<br>
  Simulated dynamic switching, extracting rise and fall delays. Observed how transistor sizing affects timing, confirming the trade-off between speed, symmetry, and power.
- **Noise Margins and Robustness:**<br>
  Calculated $$(NM_H)$$ and $$(NM_L)$$ from the VTC to quantify inverter stability under input fluctuations. Demonstrated that balanced sizing enhances logic reliability against noise and variations.
- **Power Supply Scaling & Device Variations:**<br>
  Investigated how lowering V<sub>DD</sub> reduces energy but degrades performance and margins. Simulated the impact of device parameter variations (W<sub>p</sub>/W<sub>n</sub> changes) to emulate real-world process corners, observing CMOS resilience under mismatch.

This week illuminated how transistor-level nuances define circuit-level behavior — from energy efficiency and delay to robustness and noise tolerance. By bridging SPICE simulation with physical understanding, Week 4 grounded digital design in its most fundamental truth: every logic gate is a story told in electrons.

>[!IMPORTANT]
> For easy navigation to all the weeks of the program, please visit the [Master Repository](https://github.com/BitopanBaishya/VSD-Tapeout-Program-2025.git).



