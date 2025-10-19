# 🌞 **Day 5 – CMOS Inverter: Power-Supply & Device Variation Analysis (Sky130)**

---

## 🧠 **Introduction / Background**

In this experiment, we analyze how **supply voltage (Vdd)** and **transistor sizing (W/L)** variations affect a **CMOS inverter**’s behavior using **Sky130 PDK**.

Goals:

* Study **VTC shift under supply voltage variation**
* Observe effects of **device sizing changes**
* Evaluate inverter **robustness** under variations
* Connect device-level variation to **STA margins and critical path timing**

---

## ⚙️ **SPICE Netlists**

### 🧩 **1️⃣ Device Variation Example**

```spice
*******************************************************
* 📁 File: inverter_device_variation.spice
* 📗 Purpose: CMOS inverter VTC with W/L variations
* 📚 PDK: sky130_fd_pr (1.8 V devices)
*******************************************************

* === Model Description ===
.param temp=27

* === Include Sky130 model files ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Netlist Description ===
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42 l=0.15

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

### 🧩 **2️⃣ Power-Supply Variation Sweep**

```spice
*******************************************************
* 📁 File: inverter_supply_variation.spice
* 📗 Purpose: CMOS inverter VTC under Vdd variation
* 📚 PDK: sky130_fd_pr (1.8 V devices)
*******************************************************

* === Model Description ===
.param temp=27

* === Include Sky130 model files ===
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* === Netlist Description ===
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 1.8V

* === Simulation Commands ===
.control
let powersupply = 1.8
alter Vdd = powersupply
let voltagesupplyvariation = 0
dowhile voltagesupplyvariation < 6
    dc Vin 0 1.8 0.01
    let powersupply = powersupply - 0.2
    alter Vdd = powersupply
    let voltagesupplyvariation = voltagesupplyvariation + 1
end

plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "Input Voltage (V)" ylabel "Output Voltage (V)" title "Inverter DC characteristics vs Supply Voltage"
.endc

.end
```

---

## 📊 **Plots & Figures**

* **VTC curves** under varying **Vdd** → observe how **Vm** shifts
* Compare **different W/L ratios** → see changes in **switching threshold** and noise margins
* Annotate:

  * Vm shift for each Vdd
  * Output HIGH/LOW changes
  * Impact on NML & NMH

### 📎 Annotated ngspice screenshots

<img width="1920" height="1080" alt="day5_inv_supplyvariation_Wp1_Wn036" src="https://github.com/user-attachments/assets/a4c3febc-00c7-4067-ba06-35fd2e0be01b" />
<img width="1754" height="1006" alt="day_5_inv_supply_variation" src="https://github.com/user-attachments/assets/4796c55c-5fa2-48dc-b8db-0c9ed8aa3d62" />
<img width="1920" height="1080" alt="day5_inv_devicevariation_wp7_wn042" src="https://github.com/user-attachments/assets/c849e53c-358a-4be5-8529-6539d759c7ca" />
<img width="1754" height="1006" alt="day_5_pfet_id_vgs_waveform" src="https://github.com/user-attachments/assets/c4d36305-8548-49f1-b44c-382e71cafa37" />

---

## 📋 **Tabulated Results (Example Values)**

| Parameter | Description         | Values (Vdd variation)                         |
| --------- | ------------------- | ---------------------------------------------- |
| **Vm**    | Switching threshold | 0.90 V, 0.85 V, 0.80 V, 0.75 V, 0.70 V, 0.65 V |
| **VOH**   | Output HIGH         | 1.75 V, 1.70 V, 1.65 V, 1.60 V, 1.55 V, 1.50 V |
| **VOL**   | Output LOW          | 0.05 V, 0.10 V, 0.15 V, 0.20 V, 0.25 V, 0.30 V |
| **NML**   | Noise margin low    | 0.85 V, 0.75 V, 0.65 V, 0.55 V, 0.45 V, 0.35 V |
| **NMH**   | Noise margin high   | 0.85 V, 0.80 V, 0.75 V, 0.70 V, 0.65 V, 0.60 V |

---

## 🔍 **Observations / Analysis**

* Decreasing **Vdd** shifts **Vm lower** → logic “1” reduced
* Noise margins **decrease** with lower supply → less robust inverter
* Larger PMOS/NMOS widths → improved drive, better noise tolerance
* Highlights **STA-critical paths sensitivity** to supply and device variation

---

## 🏁 **Conclusions**

* **Supply voltage variation** has a direct impact on **inverter switching point and robustness**.
* **Transistor sizing** can be tuned to compensate for variation and improve **noise margins**.
* These insights are essential for **robust timing analysis and STA verification** in modern digital circuits.

---

## 📚 **References**

* Sky130 PDK Documentation
* CMOS Circuit Design & SPICE textbooks
* GitHub: [Sky130CircuitDesignWorkshop](https://github.com/kunalg123/sky130CircuitDesignWorkshop)

---
