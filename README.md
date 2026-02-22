# 🚀 RTL to GDSII Implementation using OpenLANE  


---

<details>
<summary><strong>Phase 1 — OpenLANE Flow Familiarity (RTL → Synthesis literacy)</strong></summary>

--- 

<details>
  <summary>Theory</summary>

## 📌 Project Overview

This repository documents my hands-on implementation of the complete **RTL to GDSII ASIC design flow** using OpenLANE.

Goal:
- ✅ Clean GDSII
- ✅ Zero DRC
- ✅ Zero LVS mismatch
- ✅ Timing Clean Design

---

## 🏗️ Complete RTL to GDSII Flow


![flow](phase1/flow.jpeg)

---

## 1) RTL Synthesis

### Description
- Converts RTL (Verilog) into a gate-level netlist  
- Maps logic to Standard Cell Library (Sky130 SCL)  
- Performs logic optimization  

### Tool Used
- Yosys (via OpenLANE)

### Output
- Gate-level netlist



---

## 2) Floorplanning

### Description
- Defines die & core area  
- IO placement  
- Power planning  
- Macro placement (if any)

### Objective
- Efficient silicon usage  
- Proper power distribution  
- Low congestion  



---

## 3) Placement

### Steps
- Global Placement  
- Detailed Placement  
- Post-placement Optimization  

### Goals
- Minimize wirelength  
- Improve timing slack  
- Reduce congestion  

---

## 4) Clock Tree Synthesis (CTS)

### Description
- Builds clock distribution network  
- Inserts buffers  
- Minimizes clock skew  

### Objective
Stable clock delivery to all sequential elements.


---

## 5) Routing

### Steps
- Global Routing  
- Detailed Routing  

### Handles
- Congestion  
- Antenna violations  
- Wire optimization  

---


## 6) Signoff Verification

### Physical Verification
- DRC – Magic  
- LVS – Magic + Netgen  

### Timing Verification
- STA – OpenSTA  

### Parasitic Extraction
- RC Extraction – Magic  



---
##  Antenna Rule Handling

### Problem
Metal wires may accumulate charge and damage transistor gates.

### Solutions
- Antenna diode insertion  
- Layer hopping (bridging)  
- Magic antenna checks  


---


##  Logic Equivalence Check (LEC)

Ensures functionality remains same after:

- CTS  
- Optimization  
- Routing modifications  



---
</details>

--- 
<details> 
<summary>LAB</summary>
  
### Determine Flip-flop Ratio

The task is to find the flip-flop ratio ratio for the design picorv32a. This is the ratio of the number of flip flops to the total number of cells.

1. Run OpenLANE:
   $ make mount  -  Opens the Docker container for the OpenLane environment

   % ./flow.tcl -interactive  -  Starts the OpenLane flow in interactive mode to execute the RTL to GDSII flow step-by-step

   % package require openlane 0.9  -  Loads the OpenLane v0.9 package and its required dependencies

2. prep -design picorv32a

The command:

    prep -design picorv32a

is used to initialize the OpenLane flow for the `picorv32a` design.

It performs the following tasks:
- Loads the RTL (Verilog) files
- Reads the `config.tcl` file
- Links the Sky130 standard cell library
- Applies clock and design constraints
- Creates a new `runs/` directory to store logs, reports, and results

This step must be executed before running synthesis or any physical design stage.

In simple words, `prep -design` prepares the environment for the ASIC design flow.

![prep_design](phase1/1_prep_design.png)

3. Run Synthesis
   When we run:

    run_synthesis

in the VSD Cloud OpenLane environment:

- The RTL (Verilog) code is converted into a gate-level netlist
- The design is mapped to Sky130 standard cells
- Logic optimization is performed to reduce area and improve timing
- Static Timing Analysis (STA) is executed
- Reports for cell count, area, and timing are generated

All results are stored inside:

    designs/picorv32a/runs/RUN_2026.02.22_09.53.55

![path](phase1/3_path.png)
In simple words, run_synthesis converts RTL code into real standard cell logic and checks whether the design meets timing requirements.

### Synthesis Result : 


![run_synth](phase1/5-1_stat.png)
![run_synth](phase1/5-2_stat.png)
![run_synth](phase1/5-3_stat.png)

The flipflop ratio is (number of flip flops)/(total number of cells) is  (1613/15762) = 0.102334 = 10.2334%
  
</details>
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
