# ⚡ Day 1 – Sky130 MOSFET Characterization

## 🔬 NFET Id–Vds Analysis

---

### 🧠 **Introduction / Background**

This experiment focuses on **characterizing an NFET MOSFET** using the **Sky130 PDK**.
The objective is to study **Id vs Vds** characteristics for different **Vgs** values. These curves help identify MOSFET operation regions — **Linear (Ohmic)** and **Saturation**, and enable extraction of key parameters like:

* ⚙️ **Threshold Voltage (Vth)**
* ⚡ **Saturation Current (Id_sat)**

Understanding these parameters is fundamental for **timing**, **switching**, and **STA (Static Timing Analysis)** applications.

---

### 🧾 **SPICE Netlist**

```spice
*****************************************************
* 🧩 Model Description
.param temp=27

* 📚 Include Sky130 PDK library
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

* ⚡ Netlist
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

* 🧮 Simulation commands
.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

.control
run
display
setplot dc1
.endc

.end
*****************************************************
```

---

### 📊 **Plots & Figures**

**Output:** Id vs Vds curves for multiple Vgs values.

📈 **Annotated Regions:**

|         Region        | Description                                    |
| :-------------------: | :--------------------------------------------- |
| 🟢 **Linear (Ohmic)** | Id increases **linearly** with Vds             |
|   🔵 **Saturation**   | Id becomes **constant** beyond Vds ≈ Vgs – Vth |

📍 Threshold voltage (**Vth**) and **saturation onset** are marked on the plot.

🖼️ **Figures:**

<img width="1920" height="1080" alt="day_1_nfet_idvds_L2_W5" src="https://github.com/user-attachments/assets/48b3fe7c-edce-4784-af92-5a393593820b" />  
<img width="1754" height="1006" alt="day_1_nfet output" src="https://github.com/user-attachments/assets/42a45906-0dd3-4f1d-b20d-224b34fa23e3" />

---

### 📋 **Tabulated Results**

| Parameter                            | Value  |
| :----------------------------------- | :----- |
| ⚙️ Extracted Vth (Threshold Voltage) | 0.47 V |
| ⚡ Id_sat (Typical)                   | 120 µA |
| 📍 Vds at Saturation Onset           | 0.45 V |

---

### 🔍 **Observations / Analysis**

* 📈 At **low Vds**, the device operates in the **linear region** (Id ∝ Vds).
* ⚡ As **Vds increases**, the MOSFET enters the **saturation region**, where Id stabilizes.
* 🔺 Increasing **Vgs** strengthens the channel → **higher Id**.
* 📏 The **saturation onset** occurs when **Vds ≈ Vgs – Vth**.
* 🧠 In the context of **STA**, these variations in **Vth** and **Id** directly affect:

  * ⏱️ Switching speed
  * ⚙️ Output drive strength
  * 🧮 Gate delay

---

### 🏁 **Conclusions**

✅ **NFET Id–Vds characteristics** are vital for understanding transistor operation.
✅ Accurate **Vth extraction** is essential for **timing analysis** and **noise margin** optimization.
✅ These parameters form the foundation for **post-synthesis STA**, **delay modeling**, and **design optimization**.

---

### 📚 **References**

* 📘 *Sky130 PDK documentation and device models*
* 📗 *MOSFET Theory and SPICE Simulation* textbooks
* 💻 Example GitHub Repositories:

  * [CMOS-Circuit-Design-and-SPICE-Simulation-using-SKY130-Technology](#)
  * [Inverter-Design-and-Analysis-using-sky130pdk](#)
  * [sky130CircuitDesignWorkshop](#)

---

