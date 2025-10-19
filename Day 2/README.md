# Day 2: Velocity Saturation and Basics of CMOS Inverter VTC
 
The focus of today is

---

## 📜 Table of Contents
[1. Introduction: The Era of Short Channels](#1-introduction-the-era-of-short-channels)<br>
[2. Lab: Comparison between Short-channel and Long-channel MOSFET Behaviour](#2-lab-comparison-between-short-channel-and-long-channel-mosfet-behaviour)<br>

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
The SPICE Netlist for the short-channel MOSFET used in this simulation is not present in the `design` directory by default. You may find it [here](day2_nfet_idvds_L025_W0625.spice) to download it and add it in the `design` directory before SPICE simulations.




















