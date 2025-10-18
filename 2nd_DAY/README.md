# 🌞 **Day 2 – Sky130 NMOS Characterization: Id–Vds & Id–Vgs Analysis**

---

## 🧠 **Introduction / Background**

This experiment extends your understanding of **MOSFET behavior** by analyzing how the **drain current (Id)** varies with both **drain-source voltage (Vds)** and **gate-source voltage (Vgs)** using **Sky130 PDK** models.

The goal is to:

* Explore **linear and saturation regions** in the Id–Vds curve.
* Extract the **threshold voltage (Vth)** from the Id–Vgs curve.
* Link these device-level behaviors to **timing and delay concepts** in STA.

---

## ⚙️ **SPICE Netlists**

### 🧩 **1️⃣ Id–Vds Characteristics Simulation**

```spice
*******************************************************
* 📁 File: id_vds_nfet.spice
* 📗 Purpose: NMOS Id–Vds characteristics simulation
* 📚 PDK: sky130_fd_pr (1.8 V device)
*******************************************************

* === Model Description ===
.param temp = 27

* === Include Sky130 model file ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Netlist Description ===
XM1 vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

* === Simulation Commands ===
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
```

---

### 🧩 **2️⃣ Id–Vgs Characteristics / Vth Extraction**

```spice
*******************************************************
* 📁 File: id_vgs_nfet.spice
* 📗 Purpose: NMOS Id–Vgs characteristics & Vt extraction
* 📚 PDK: sky130_fd_pr (1.8 V device)
*******************************************************

* === Model Description ===
.param temp = 27

* === Include Sky130 model file ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Netlist Description ===
XM1 vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=0.39 l=0.15
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

* === Simulation Commands ===
.op
.dc Vin 0 1.8 0.1

.control
run
display
setplot dc1
.endc

.end
```

---

## 📊 **Plots & Figures**

* **Id vs Vds** for multiple Vgs values → shows **linear → saturation transition**.
* **Id vs Vgs** → used to extract **Vth** via linear extrapolation.
* Annotate:

  * **Linear region:** Id ∝ Vds
  * **Saturation region:** Id ≈ constant beyond Vds ≈ Vgs − Vth
  * **Threshold point:** intersection from extrapolated line

📎 *Attach ngspice screenshots here.*
<img width="1920" height="1080" alt="day2_nfet_idvds_L015_W039" src="https://github.com/user-attachments/assets/12ad8d96-2828-48a5-a978-cb90f0cc03b4" />
<img width="1754" height="1006" alt="day_2_Id_vds_waveform" src="https://github.com/user-attachments/assets/7781eca9-4453-4658-9148-a15c91baf2ca" />
<img width="1920" height="1080" alt="day2_nfet_idvgs_L015_W039" src="https://github.com/user-attachments/assets/127d3d7b-1fc7-4e9e-83cd-d7425f2afced" />
<img width="1754" height="1006" alt="day_2_Id_vgs_waveform" src="https://github.com/user-attachments/assets/add25a80-cdd7-4215-a25f-b4469414a334" />

---

## 📋 **Tabulated Results**

| Parameter    | Description                         | Extracted Value |
| ------------ | ----------------------------------- | --------------- |
| **Vth**      | Threshold voltage (from Id–Vgs)     | [your value] V  |
| **Id_sat**   | Saturation current (at Vgs = 1.8 V) | [your value] µA |
| **Vds(sat)** | Saturation onset ≈ Vgs − Vth        | [your value] V  |

---

## 🔍 **Observations / Analysis**

* At low Vds → transistor operates in **linear region**; Id increases linearly.
* As Vds > (Vgs − Vth) → **saturation region** begins; Id saturates.
* Higher Vgs ⇒ stronger channel ⇒ higher Id.
* Extracted Vth helps determine **switching threshold** for CMOS logic.
* These characteristics directly impact **propagation delay** and **noise margins** in digital designs.

---

## 🧩 **Relevance to STA**

* **Vth variation** → affects timing uncertainty and slack.
* **Id variation** → changes drive current → affects delay and transition times.
* Device sizing and supply changes → alter Vth, Id_sat → impact critical path timing.

---

## 🏁 **Conclusions**

* The **Id–Vds** and **Id–Vgs** simulations form the foundation for **CMOS circuit understanding**.
* **Accurate Vth extraction** = better timing and noise analysis.
* These transistor-level insights connect directly to **post-synthesis STA and timing closure**.

---

## 📚 **References**

* Sky130 PDK Documentation
* *MOSFET Theory & SPICE Simulation Textbooks*
* GitHub Repos:

  * [CMOS-Circuit-Design-and-SPICE-Simulation-using-SKY130-Technology](https://github.com/kunalg123/sky130CircuitDesignWorkshop)
  * [Inverter-Design-and-Analysis-using-Sky130PDK](https://github.com/kunalg123/sky130CircuitDesignWorkshop)

---
