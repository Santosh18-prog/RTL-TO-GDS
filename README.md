# 🚀 RTL to GDSII Implementation using OpenLANE  


---

<details>
<summary>Phase 1 — OpenLANE Flow Familiarity (RTL → Synthesis literacy)</summary>

---

## 📌 Project Overview

This repository documents my hands-on implementation of the complete **RTL to GDSII ASIC design flow** using OpenLANE.

Goal:
- ✅ Clean GDSII
- ✅ Zero DRC
- ✅ Zero LVS mismatch
- ✅ Timing Clean Design

---

## 🏗️ Complete RTL to GDSII Flow

```
RTL → Synthesis → Floorplanning → Placement → CTS → Routing → Signoff → GDSII
```

---

<details>
<summary><b>🔹 1️⃣ RTL Synthesis</b></summary>

### Description
- Converts RTL (Verilog) into a gate-level netlist  
- Maps logic to Standard Cell Library (Sky130 SCL)  
- Performs logic optimization  

### Tool Used
- Yosys (via OpenLANE)

### Output
- Gate-level netlist

</details>

---

<details>
<summary><b>🔹 2️⃣ Floorplanning</b></summary>

### Description
- Defines die & core area  
- IO placement  
- Power planning  
- Macro placement (if any)

### Objective
- Efficient silicon usage  
- Proper power distribution  
- Low congestion  

</details>

---

<details>
<summary><b>🔹 3️⃣ Placement</b></summary>

### Steps
- Global Placement  
- Detailed Placement  
- Post-placement Optimization  

### Goals
- Minimize wirelength  
- Improve timing slack  
- Reduce congestion  

</details>

---

<details>
<summary><b>🔹 4️⃣ Clock Tree Synthesis (CTS)</b></summary>

### Description
- Builds clock distribution network  
- Inserts buffers  
- Minimizes clock skew  

### Objective
Stable clock delivery to all sequential elements.

</details>

---

<details>
<summary><b>🔹 5️⃣ Routing</b></summary>

### Steps
- Global Routing  
- Detailed Routing  

### Handles
- Congestion  
- Antenna violations  
- Wire optimization  

</details>

---

<details>
<summary><b>🔹 6️⃣ Antenna Rule Handling</b></summary>

### Problem
Metal wires may accumulate charge and damage transistor gates.

### Solutions
- Antenna diode insertion  
- Layer hopping (bridging)  
- Magic antenna checks  

</details>

---

<details>
<summary><b>🔹 7️⃣ Signoff Verification</b></summary>

### Physical Verification
- DRC – Magic  
- LVS – Magic + Netgen  

### Timing Verification
- STA – OpenSTA  

### Parasitic Extraction
- RC Extraction – Magic  

</details>

---

<details>
<summary><b>🔹 8️⃣ Logic Equivalence Check (LEC)</b></summary>

Ensures functionality remains same after:

- CTS  
- Optimization  
- Routing modifications  

</details>

---

</details>

---

## 🛠 Tools Used

| Stage | Tool |
|-------|------|
| Synthesis | Yosys |
| Floorplan & Placement | OpenROAD |
| Routing | OpenROAD |
| DRC | Magic |
| LVS | Netgen |
| STA | OpenSTA |
| Flow Automation | OpenLANE |
| PDK | Sky130 |

---

## 📂 Repository Structure

```
.
├── design/
├── images/
└── README.md
```

---

## 🎯 Learning Outcomes

- Complete ASIC Backend Flow understanding  
- Timing closure concepts  
- Power planning basics  
- DRC & LVS debugging  
- Antenna rule fixing techniques  

---

## 👨‍💻 Author

**Sandy**  
ECE Undergraduate  
Aspiring VLSI / ASIC Physical Design Engineer  
