# ⚡ **Week 3 – CMOS Inverter Design & Analysis (Sky130)**

---

## 🧠 **Introduction / Background**

This experiment focuses on designing and analyzing a **CMOS inverter** using the **Sky130 PDK**.
It demonstrates how transistor sizing and device characteristics influence **switching threshold**, **propagation delays**, and **noise margins** — key aspects in **Static Timing Analysis (STA)**.

You will:

* Derive the **VTC (Voltage Transfer Characteristic)** of a CMOS inverter 🧮
* Simulate **transient switching behavior** using a pulse input ⏱️
* Measure **rise/fall times**, **delays**, and observe inverter **robustness**

---

## ⚙️ **SPICE Netlists**

---

### 🧩 **1️⃣ VTC Simulation (DC Sweep)**

```spice
*******************************************************
* 📁 File: inverter_vtc.spice
* 📗 Purpose: CMOS Inverter DC Sweep (VTC)
* 📚 PDK: sky130_fd_pr (1.8 V devices)
*******************************************************

* === Model Description ===
.param temp = 27

* === Include Sky130 model files ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Netlist Description ===
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

* === Simulation Commands ===
.op
.dc Vin 0 1.8 0.01

.control
run
setplot dc1
display
.endc

.end
```

---

### 🧩 **2️⃣ Transient Simulation (Dynamic Switching Behavior)**

```spice
*******************************************************
* 📁 File: inverter_tran.spice
* 📗 Purpose: CMOS Inverter Transient Response
* 📚 PDK: sky130_fd_pr (1.8 V devices)
*******************************************************

* === Model Description ===
.param temp = 27

* === Include Sky130 model files ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Netlist Description ===
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

* === Simulation Commands ===
.tran 1n 10n

.control
run
.endc

.end
```

---

## 📊 **Plots & Figures**

### 1️⃣ **Voltage Transfer Characteristic (VTC)**

* Sweep `Vin` from 0 V → 1.8 V
* Plot `Vout` vs `Vin`
* Identify:

  * **Switching threshold (Vm)** → point where `Vout = Vin`
  * **VOH, VOL, VIH, VIL**
* From these values, compute:

  * **NML = VIL − VOL**
  * **NMH = VOH − VIH**

---

### 2️⃣ **Transient Waveforms**

* Apply a **pulse input** at Vin (0 V ↔ 1.8 V)
* Plot **Vin and Vout over time**
* Measure:

  * **Rise delay (tpLH)** – when output rises
  * **Fall delay (tpHL)** – when output falls
* Delays measured at 50% Vdd crossing points

### 📎 ngspice screenshots 
<img width="1920" height="1080" alt="day_3_inv_vtc_Wp084_Wn036" src="https://github.com/user-attachments/assets/37301314-b310-425d-be7e-bffabeb987ea" />
<img width="1754" height="1006" alt="day_3_inverter_vtc_output" src="https://github.com/user-attachments/assets/9709c07b-028e-473b-b98b-2d6bdb0045ce" />
<img width="1920" height="1080" alt="day_3_inv_tran_Wp084_Wn036" src="https://github.com/user-attachments/assets/cd2db59a-2ef1-4fa1-8dd9-a00b7d705b3a" />
<img width="1754" height="1006" alt="day_3_inv_tran_out" src="https://github.com/user-attachments/assets/f7a66b99-fe5b-4126-95dd-9ad7f6bb618c" />

---

## 📋 **Tabulated Results**

| Parameter | Description         | Measured Value  |
| --------- | ------------------- | --------------- |
| **Vm**    | Switching Threshold | [your value] V  |
| **tpLH**  | Low → High Delay    | [your value] ns |
| **tpHL**  | High → Low Delay    | [your value] ns |
| **NML**   | Noise Margin (Low)  | [your value] V  |
| **NMH**   | Noise Margin (High) | [your value] V  |

*(Replace with values from your simulation results.)*

---

## 🔍 **Observations / Analysis**

* **VTC:** Shows sharp transition near Vm (~ Vdd / 2), verifying proper inverter behavior.
* **Delays:** tpHL and tpLH differ due to **mobility difference** between NMOS & PMOS.
* **Noise margins** indicate inverter’s tolerance to input noise.
* **W/L ratio adjustment** shifts Vm and affects propagation delay.
* Variations in **Vdd or device size** directly influence **timing and robustness**.

---

## ⏱️ **Relevance to STA**

* The **switching threshold (Vm)** defines logical “1”/“0” regions used in STA.
* **Propagation delays** correspond to STA’s **cell delay models**.
* **Rise/fall asymmetry** impacts overall **timing skew** and **slack**.
* **Noise margins** reflect how process variation or supply noise can impact setup/hold timing.

---

## 🏁 **Conclusions**

* The CMOS inverter forms the **fundamental timing element** in digital circuits.
* Its **VTC and transient analysis** provide the data STA tools rely on for delay modeling.
* Understanding **transistor sizing, threshold shift, and delay variation** is key for robust digital design.

---

## 📚 **References**

* Sky130 PDK & model documentation
* “CMOS Circuit Design and SPICE Simulation using Sky130” (GitHub: [kunalg123/sky130CircuitDesignWorkshop](https://github.com/kunalg123/sky130CircuitDesignWorkshop))
* Basic MOSFET theory and timing analysis textbooks

---
