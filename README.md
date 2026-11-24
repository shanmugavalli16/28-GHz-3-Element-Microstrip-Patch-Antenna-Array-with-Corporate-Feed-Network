# 28 GHz 3-Element Microstrip Patch Antenna Array with Corporate Feed Network

Design and simulation of a **3-element linear microstrip patch antenna array** for **5G mmWave (28 GHz)** using **Ansys HFSS**. The array is fed from a single 50 Ω port through a **corporate (parallel) feed network** with impedance-stepped microstrip lines that splits the power equally between the three patches. Before choosing the feed, the effect of **uniform, binomial and Chebyshev amplitude distributions** on the radiation pattern was studied to decide what the feed network must deliver.

<p align="center">
  <img src="images/design/dimensioned_layout.png" width="70%"><br>
  <em>Final HFSS model – 3 inset-fed patches connected through the corporate feed network (top view)</em>
</p>

---

## Table of Contents

1. [Key Results](#1-key-results)
2. [Design Specifications](#2-design-specifications)
3. [Theoretical Design](#3-theoretical-design)
4. [Final Design Parameters](#4-final-design-parameters)
5. [Feed Network Description](#5-feed-network-description)
6. [Design Workflow](#6-design-workflow)
7. [Amplitude Distribution Study](#7-amplitude-distribution-study-uniform-vs-binomial-vs-chebyshev)
8. [Simulation Results of the Final Array](#8-simulation-results-of-the-final-array)
9. [Extension: 2×2 Planar Array](#9-extension-22-planar-array-exploratory)
10. [How to Reproduce](#10-how-to-reproduce)
11. [Notes on the Reported Values](#11-notes-on-the-reported-values)
12. [Future Work](#12-future-work)

---

## 1. Key Results

| Parameter (at 28 GHz) | Value |
|---|---|
| Resonance (minimum S11) | ≈ 28.02 GHz |
| Minimum S11 | **−27.0 dB** |
| −10 dB impedance bandwidth | **≈ 1.1 GHz** (27.42 – 28.54 GHz, ≈ 4 %) |
| VSWR | **1.10** (1.0988) |
| Input impedance \|Z11\| | **≈ 52.9 Ω** |
| Peak gain | **≈ 12.0 dB** (broadside, θ = 0°) |
| Peak directivity | **≈ 11.8 dB** (broadside, θ = 0°) |
| Element spacing (centre-to-centre) | 8.1 mm ≈ 0.76 λ₀ |
| Substrate | Rogers RT/duroid 5880 (εr = 2.2), h = 0.4 mm |
| Overall substrate size | ≈ 25.73 mm × 14.55 mm |

---


## 2. Design Specifications

| Parameter | Value |
|---|---|
| Operating frequency | 28 GHz (5G mmWave) |
| Free-space wavelength λ₀ | 10.71 mm |
| Substrate | Rogers RT/duroid 5880, εr = 2.2 |
| Substrate height `h` | 0.4 mm |
| Conductor | Copper, thickness `mt` = 0.035 mm (35 µm) |
| Patch feeding technique | Inset feed |
| Input impedance | 50 Ω (single port) |
| Number of elements | 3 (linear array along X) |
| Feed network | Corporate (parallel) feed – 100 Ω branch lines, ≈ 70 Ω section, 50 Ω main feed |
| Simulator | Ansys HFSS 2025 R2 (Student) |
| Frequency sweep | 26 – 30 GHz |

---

## 3. Theoretical Design

The initial patch dimensions were calculated with the standard transmission-line model and then optimised in HFSS.

| Quantity | Equation | Value |
|---|---|---|
| Wavelength | $\lambda_0 = c / f_0$ | 10.71 mm |
| Patch width | $W_p = \dfrac{c}{2f_0}\sqrt{\dfrac{2}{\varepsilon_r+1}}$ | 4.23 mm |
| Effective permittivity | $\varepsilon_{eff} = \dfrac{\varepsilon_r+1}{2} + \dfrac{\varepsilon_r-1}{2}\left[1+\dfrac{12h}{W_p}\right]^{-1/2}$ | ≈ 2.0 |
| Length extension | $\Delta L = 0.412\,h\,\dfrac{(\varepsilon_{eff}+0.3)\left(\frac{W_p}{h}+0.264\right)}{(\varepsilon_{eff}-0.258)\left(\frac{W_p}{h}+0.8\right)}$ | ≈ 0.20 mm |
| Effective length | $L_{eff} = \dfrac{c}{2f_0\sqrt{\varepsilon_{eff}}}$ | ≈ 3.8 mm |
| Patch length | $L_p = L_{eff} - 2\Delta L$ | ≈ 3.39 mm |
| Substrate/ground width | $W_g = W_p + 6h$ | 6.63 mm (single patch) |
| Substrate/ground length | $L_g = L_p + 6h$ | 5.79 mm |
| Edge conductance | $G_1 = \dfrac{1}{90}\left(\dfrac{W_p}{\lambda_0}\right)^2$ | ≈ 1.74 mS |
| Edge input resistance | $R_{in}(0) = \dfrac{1}{2G_1}$ | ≈ 288 Ω |
| Inset depth for 50 Ω | $R_{in}(y_0) = R_{in}(0)\cos^2\!\left(\dfrac{\pi y_0}{L_p}\right)$ | $y_0 \approx 1.2$ mm |
| 50 Ω feed width | $W = \dfrac{7.48\,h}{e^{\,Z_0\sqrt{\varepsilon_r+1.41}/87}} - 1.25\,t$ | 0.96 mm |

**Analytical vs. final (HFSS-optimised) values**

| Parameter | Analytical | Final (array) |
|---|---|---|
| Patch width `Wp` | 4.23 mm | 4.2 mm |
| Patch length `Lp` | 3.39 mm | 3.3 mm |
| Inset depth `Yo` | ≈ 1.2 mm | 1.2 mm |
| Feed width `wf` | 0.96 mm | 0.9 mm |

The difference between analytical and final values is expected: the analytical model ignores mutual coupling between the patches and the discontinuities of the feed network, so the dimensions were tuned in HFSS.

---

## 4. Final Design Parameters

All dimensions are the HFSS design variables of the final array model.

<p align="center">
  <img src="images/design/dimensioned_layout.png" width="95%"><br>
  <em>Dimensioned top view of the final array (all lengths in mm) with the inset-feed detail</em>
</p>

<p align="center">
  <img src="images/design/hfss_design_variables.png" width="75%"><br>
  <em>HFSS local variables of the final array (design <code>patch_28Ghz_3corporate_sidelobe</code>)</em>
</p>

| Variable | Value | Description |
|---|---|---|
| `Wg` | 17.23 mm | Substrate / ground width parameter (optimised for the array) |
| `Lg` | 5.79 mm | Substrate / ground length parameter |
| `h` | 0.4 mm | Substrate (Rogers RT/duroid 5880) height |
| `mt` | 0.035 mm | Copper thickness |
| `Wp` | 4.2 mm | Patch width |
| `Lp` | 3.3 mm | Patch length |
| `wf` | 0.9 mm | Width of the main (50 Ω) feed line |
| `Wi` | 0.02602 mm | Inset gap width |
| `Yo` | 1.2 mm | Inset depth |
| `W100` | 0.358 mm | Width of the 100 Ω branch line feeding each patch |
| `L100` | 3.2 mm | Length of the 100 Ω branch line |
| `W70` | 1 mm | Width of the ≈ 70 Ω line connecting the three branches |
| `L70` | 3.2 mm | Length of the ≈ 70 Ω line |
| `d` | 8.1 mm | Centre-to-centre spacing of the patches (≈ 0.76 λ₀) |
| `Ws` | 25.73 mm | Overall substrate width (derived: `Wg` + 8.5 mm) – marked in the dimensioned layout |
| `Ls` | 14.548 mm | Overall substrate length (from the substrate-box expression) – marked in the dimensioned layout |


---

## 5. Feed Network Description

Three line impedances are used in the network:

| Section | Impedance | Dimensions | Role |
|---|---|---|---|
| Branch line to each patch | 100 Ω | `W100` × `L100` = 0.358 mm × 3.2 mm | Connects each inset patch to the common junction |
| Intermediate section | ≈ 70 Ω | `W70` × `L70` = 1 mm × 3.2 mm | Joins the three 100 Ω branches and steps the impedance towards the input |
| Main feed | 50 Ω | `wf` = 0.9 mm | Input port line |

- Each patch is **inset-fed** (`Yo`, `Wi`), so the patch itself is matched to its feed line.
- The three 100 Ω branches meet at a T-junction and are connected through the ≈ 70 Ω section to the 50 Ω input line.
- The impedance levels step between the high-impedance branch lines and the 50 Ω input; the exact widths and lengths were tuned with HFSS parametric optimisation to obtain the final match (|Z11| ≈ 52.9 Ω, S11 ≈ −27 dB at resonance).
- The outer branches are routed over the element spacing `d = 8.1 mm`, which makes the three patches be fed in phase.

---

## 6. Design Workflow

| Stage | What was done | Output |
|---|---|---|
| 1. Single patch | Analytical design (Section 3), inset feed, HFSS simulation | [`images/single_element/`](images/single_element) |
| 2. Two-element array | Two patches, one port each – S11 / S22 at 28 GHz | [`images/two_element/`](images/two_element) |
| 3. Three-element array, independent ports | Three patches, one port each – S11 / S22 / S33 | [`images/three_element_ports/`](images/three_element_ports) |
| 4. Amplitude distribution study | Uniform, binomial and Chebyshev weights applied through *HFSS → Fields → Edit Sources* | [`images/amplitude_distribution/`](images/amplitude_distribution) |
| 5. Corporate feed network | Single 50 Ω port, 100 Ω branches, ≈ 70 Ω section, uniform in-phase excitation | [`images/design/`](images/design), [`images/results/`](images/results) |
| 6. Extension | Exploratory 2×2 planar array | [`images/planar_2x2/`](images/planar_2x2) |

### Single patch (unit cell)

<p align="center">
  <img src="images/single_element/model_3d.png" width="40%">
  <img src="images/single_element/radiation_pattern_rE.png" width="45%"><br>
  <em>Single inset-fed patch at 28 GHz and its radiation pattern</em>
</p>

### Two-element array

<p align="center">
  <img src="images/two_element/model_3d.png" width="38%">
  <img src="images/two_element/s_parameters.png" width="55%"><br>
  <em>Two-element array with independent ports – S11 and S22 show the resonance near 28 GHz (≈ −27 to −29 dB)</em>
</p>

### Three-element array with independent ports

<p align="center">
  <img src="images/three_element_ports/model_3d.png" width="45%"><br>
  <img src="images/three_element_ports/s_parameters.png" width="70%"><br>
  <em>Three-element array with one port per patch – S11, S22 and S33 all resonate near 28.1 GHz (≈ −27 to −31 dB)</em>
</p>

---

## 7. Amplitude Distribution Study: Uniform vs Binomial vs Chebyshev

With one port per patch, different amplitude weights were applied in *HFSS → Fields → Edit Sources* (all phases 0°) to see how the excitation changes the radiation pattern. HFSS source magnitudes are entered as power in W.

| Distribution | Port power (W) | Power share (%) | Amplitude ratio | Peak directivity (3D) | Broadside gain (θ = 0°, φ = 0° cut) |
|---|---|---|---|---|---|
| **Uniform** | 1 : 1 : 1 | 33 / 33 / 33 | 1 : 1 : 1 | **13.57 dB** | **13.69 dB** |
| **Chebyshev** | 0.58 : 1 : 0.58 | 27 / 46 / 27 | 0.76 : 1 : 0.76 | 12.94 dB | 13.05 dB |
| **Binomial** | 0.25 : 1 : 0.25 | 17 / 67 / 17 | 1 : 2 : 1 | 11.31 dB | 11.39 dB |

> The gain legends of the 3D gain plots (11.37 / 11.16 / 10.57 dB) read lower than the polar-cut values above; see [Section 11](#11-notes-on-the-reported-values).

### 7.1 Uniform distribution (1 : 1 : 1)

<p align="center">
  <img src="images/amplitude_distribution/uniform/edit_sources.png" width="30%">
  <img src="images/amplitude_distribution/uniform/gain_3d.png" width="30%">
  <img src="images/amplitude_distribution/uniform/directivity_3d.png" width="30%"><br>
  <img src="images/amplitude_distribution/uniform/radiation_pattern_rE.png" width="40%">
  <img src="images/amplitude_distribution/uniform/gain_polar.png" width="45%"><br>
  <em>Edit-sources table · 3D gain · 3D directivity · radiation pattern (rE) · gain polar plot</em>
</p>

### 7.2 Binomial distribution (1 : 2 : 1 amplitude)

<p align="center">
  <img src="images/amplitude_distribution/binomial/edit_sources.png" width="30%">
  <img src="images/amplitude_distribution/binomial/gain_3d.png" width="30%">
  <img src="images/amplitude_distribution/binomial/directivity_3d.png" width="30%"><br>
  <img src="images/amplitude_distribution/binomial/radiation_pattern_rE.png" width="40%">
  <img src="images/amplitude_distribution/binomial/gain_polar.png" width="45%"><br>
  <em>Edit-sources table · 3D gain · 3D directivity · radiation pattern (rE) · gain polar plot</em>
</p>

### 7.3 Chebyshev distribution (0.76 : 1 : 0.76 amplitude)

<p align="center">
  <img src="images/amplitude_distribution/chebyshev/edit_sources.png" width="30%">
  <img src="images/amplitude_distribution/chebyshev/gain_3d.png" width="30%">
  <img src="images/amplitude_distribution/chebyshev/directivity_3d.png" width="30%"><br>
  <img src="images/amplitude_distribution/chebyshev/radiation_pattern_rE.png" width="40%">
  <img src="images/amplitude_distribution/chebyshev/gain_polar.png" width="45%"><br>
  <em>Edit-sources table · 3D gain · 3D directivity · radiation pattern (rE) · gain polar plot</em>
</p>

### 7.4 Conclusions and impact on the feed design

| Distribution | Behaviour | What the feed network would need |
|---|---|---|
| **Uniform** | Narrowest main beam, highest directivity and gain (13.6 dB). Side-lobe suppression is the weakest of the three (no taper). | Equal power split, equal phase |
| **Chebyshev** | Compromise: controlled side lobes with only ≈ 0.6 dB directivity loss relative to uniform. | Unequal split (≈ 27 % / 46 % / 27 %) |
| **Binomial** | Widest main beam and lowest directivity (11.3 dB) – the price paid for the strongest side-lobe suppression. | Strongly unequal split (≈ 17 % / 67 % / 17 %) |

**Decision:** the objective of this design is maximum gain at 28 GHz, so the **uniform distribution** was selected. It is realised by a **corporate feed with an equal three-way split and equal phase**, which needs no extra tapering networks. The Chebyshev and binomial cases show what an unequal-split (tapered) corporate feed could achieve if lower side lobes were required – this is listed under [Future Work](#12-future-work).

---

## 8. Simulation Results of the Final Array

All results are for the final array of Section 4  with a single 50 Ω input port (HFSS Setup1, sweep 26 – 30 GHz).

### 8.1 S-parameter (S11)

<p align="center">
  <img src="images/results/s11.png" width="80%"><br>
  <em>S11 – minimum −27.02 dB near 28 GHz; markers m1 = 27.42 GHz and m2 = 28.54 GHz (≈ −10 dB) give a bandwidth of ≈ 1.1 GHz</em>
</p>

### 8.2 VSWR

<p align="center">
  <img src="images/results/vswr.png" width="80%"><br>
  <em>VSWR = 1.0988 at 28 GHz</em>
</p>

### 8.3 Input impedance (Z-parameter)

<p align="center">
  <img src="images/results/z_parameter.png" width="80%"><br>
  <em>|Z11| = 52.92 Ω at 28 GHz – close to the 50 Ω reference</em>
</p>

### 8.4 Gain

<p align="center">
  <img src="images/results/gain_phi0_phi90.png" width="80%"><br>
  <em>Total gain vs θ at 28 GHz: φ = 0° (red, along the array axis – narrow beam) and φ = 90° (green, broad single-element beam). Peak gain ≈ 12.0 dB at broadside.</em>
</p>

### 8.5 Directivity

<p align="center">
  <img src="images/results/directivity_phi0_phi90.png" width="80%"><br>
  <em>Total directivity vs θ at 28 GHz. Peak directivity ≈ 11.8 dB at broadside.</em>
</p>

**Observations**
- The array concentrates the beam in the plane containing the array axis (φ = 0°), while the φ = 90° cut keeps the broad pattern of a single patch – as expected for a linear array.
- The −10 dB bandwidth of ≈ 1.1 GHz (≈ 4 %) is typical for a single-layer microstrip patch array on a thin substrate.
- The main beam points at broadside (θ = 0°), confirming that the corporate feed excites the three patches in phase.

---

## 9. Extension: 2×2 Planar Array (Exploratory)

A 2×2 planar array (4 ports) was also modelled to explore beam shaping in two dimensions. This is an exploratory extension and not part of the 3-element design above.

<p align="center">
  <img src="images/planar_2x2/model_3d.png" width="40%">
  <img src="images/planar_2x2/s_parameters.png" width="55%"><br>
  <img src="images/planar_2x2/directivity_3d.png" width="35%">
  <img src="images/planar_2x2/radiation_pattern_rE.png" width="35%"><br>
  <em>2×2 planar array model · S11–S44 (≈ −30 to −33 dB near 28.08 GHz) · 3D directivity · radiation pattern</em>
</p>

---

## 10. How to Reproduce

1. Open **Ansys HFSS** (tested with 2025 R2 Student) and insert a new *Driven Modal* design.
2. Define the design variables of [Section 4](#4-final-design-parameters).
3. Create the **RT/duroid 5880** substrate box (`h` = 0.4 mm) with the dimensions of the substrate-box definition, a copper ground plane and copper patches / feed lines (thickness `mt`).
4. Draw the three inset-fed patches at spacing `d`, the 100 Ω branches (`W100` × `L100`), the ≈ 70 Ω section (`W70` × `L70`) and the 50 Ω main feed (`wf`).
5. Assign **one excitation port** at the end of the 50 Ω feed line and an air box with a radiation boundary.
6. Add a solution setup and a **frequency sweep from 26 to 30 GHz**.
7. Create an **infinite-sphere far-field setup** and plot gain / directivity at 28 GHz for φ = 0° and φ = 90°.
8. For the amplitude study, use the independent-port model and set the weights in *HFSS → Fields → Edit Sources*.

Python packages in `requirements.txt` are optional and only needed to re-plot exported HFSS data:

```bash
pip install -r requirements.txt
```

---

## 11. Notes on the Reported Values

- **Gain vs directivity:** HFSS reports a peak gain of 12.02 dB and a peak directivity of 11.81 dB for the final array. Physically, gain cannot exceed directivity; the ≈ 0.2 dB difference is treated as solver / post-processing tolerance and is a candidate for re-verification (radiation efficiency and source-power normalisation settings).
- **3D gain legends vs polar plots:** in the amplitude-distribution study the legends of the 3D gain plots (uniform 11.37 dB, binomial 10.57 dB, Chebyshev 11.16 dB) are lower than the broadside gain of the polar cuts (13.69 / 11.39 / 13.05 dB), which agree with the directivity. The comparison in Section 7 is therefore based on directivity and on the polar-cut gain.
- **Bandwidth:** the markers m1 and m2 sit at about −9.9 dB, so the −10 dB bandwidth is slightly below the 1.12 GHz marker spacing; it is quoted as ≈ 1.1 GHz.
- **Layout values:** `Wg` is a parametric substrate-size variable; the physical board is ≈ 25.73 mm × 14.55 mm.

---

## 12. Future Work

- Document the **progressive phase shift (beam steering)** of the 3-element array – scan angle vs. phase step.
- Implement a **tapered (Chebyshev / binomial) corporate feed** with unequal power split to reduce side lobes.
- Add **isolation resistors** between branches (Wilkinson-based network) and compare isolation and bandwidth.
- Extend to a 4–8 element linear array for higher gain.
- Record the port excitations of the 2×2 planar array and add the corresponding pattern table.
- Fabrication-tolerance and substrate-thickness sensitivity analysis.

---

