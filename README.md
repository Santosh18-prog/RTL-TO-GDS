# 🚀 RTL to GDSII Implementation using OpenLANE  


---

<details>
<summary><strong>Phase 1 — OpenLANE Flow Familiarity </strong></summary>

--- 

<details>
  <summary>Theory</summary>

## SoC Design Using OpenLane

This repository documents my hands-on implementation of the complete **RTL to GDSII ASIC design flow** using OpenLANE.

---

## 1. Digital ASIC Design

Digital ASIC design is the process of converting a hardware description (RTL) into a fabricated silicon chip (ASIC).

Traditionally, ASIC design flow required:

- EDA Tools
- PDK Data
- Standard Cell Libraries

These together enable the transformation of RTL → ASIC.

---

## 2. Open-Source Digital ASIC Design

In open-source ASIC design:

- EDA tools are open-source
- RTL design is written in HDL (Verilog/VHDL)
- Standard cell libraries are publicly available
- Open PDKs are used

OpenLane integrates:

- Open-source EDA tools
- Open PDK
- Automated flow

And produces:

RTL → GDSII (layout ready for fabrication)

---

## 3. What is a PDK?

PDK = Process Design Kit

A PDK is a collection of files used to model the fabrication process for EDA tools while designing an IC.

It allows designers to create layouts that match actual manufacturing rules.

---

### Contents of a PDK

A PDK typically contains:

- Process Design Rules (DRC rules)
- LVS rules
- PEX rules
- Device Models
- Digital Standard Cell Libraries
- I/O Libraries

These ensure that the design matches the real manufacturing process.

---

## 4. Evolution of IC Design and PDK Usage

In earlier days:

- Chip design was tightly integrated with fabrication.
- Each company (e.g., Intel) controlled manufacturing.
- Designers and fabrication teams worked under the same organization.

These companies controlled:

- Process technology
- Design methodology
- Manufacturing flow

Later:

- Fabless design companies emerged.
- Foundries handled fabrication.
- Designers needed a standardized interface to fabrication.

To separate design from fabrication:

Process Design Kits (PDKs) were introduced.

PDKs standardized the interaction between:

Design team ↔ Manufacturing process

---

## 5. Open PDK

Open PDK used:

- License: Apache 2.0
- Collaboration between Google and SkyWater Technology

FOSS 130nm Production PDK:

Available at:
github.com/google/skywater-pdk

---

### Is 130nm Fast?

Yes.

Example reference:

Intel Pentium 4 Extreme Edition
3.46 GHz (Quad-core era reference)

Even 130nm technology was capable of high-performance designs.


---

## Openlane ASIC Flow


Goal:
- ✅ Clean GDSII
- ✅ Zero DRC
- ✅ Zero LVS mismatch
- ✅ Timing Clean Design

![flow](phase1/flow.jpeg)

---

## 1. RTL Synthesis

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

## 3. Placement

### Steps
- Global Placement  
- Detailed Placement  
- Post-placement Optimization  

### Goals
- Minimize wirelength  
- Improve timing slack  
- Reduce congestion  

---

## 4. Clock Tree Synthesis (CTS)

### Description
- Builds clock distribution network  
- Inserts buffers  
- Minimizes clock skew  

### Objective
Stable clock delivery to all sequential elements.


---

## 5. Routing

### Steps
- Global Routing  
- Detailed Routing  

### Handles
- Congestion  
- Antenna violations  
- Wire optimization  

---


## 6. Signoff Verification

### Physical Verification
- DRC – Magic  
- LVS – Magic + Netgen  

### Timing Verification
- STA – OpenSTA  

### Parasitic Extraction
- RC Extraction – Magic  



---
###  OpenLane Regression Testing

The design exploration utility is used for regression testing.

Regression testing means:

- Running automation on multiple designs
- Comparing results with previously known correct results
- Ensuring no functionality breaks

This ensures flow stability.

---

###  Design For Test (DFT)

DFT techniques improve testability of a chip.

Includes:

- Scan Insertion
- Automatic Test Pattern Generation (ATPG)
- Test Pattern Compression
- Fault Coverage
- Fault Simulation

These techniques ensure high manufacturing test quality.

---

###  Physical Implementation (Automated Place & Route)

Also called:

Automated PnR (Place and Route)

Steps include:

1. Floorplanning (includes power planning)
2. End Decoupling Capacitors & Tap Cell Insertion
3. Placement (Global & Detailed)
4. Post-Placement Optimization
5. Clock Tree Synthesis (CTS)
6. Routing (Global & Detailed)

This converts logical netlist into physical layout.

---

###  Logic Equivalence Check (LEC)

Every time the netlist is modified, verification must be performed.

Why?

Because:

- CTS modifies the netlist.
- Post-placement optimizations modify the netlist.
- Routing optimizations may affect structure.

LEC is used to formally confirm that:

The functionality has NOT changed after netlist modifications.

It ensures:

Original RTL ≡ Final netlist

---

###  Dealing with Antenna Rule Violations

When a metal wire segment is fabricated, it can act as an antenna.

During fabrication:

- Reactive ion etching causes charge accumulation on the wire.
- Accumulated charge can damage transistor gates.
- Gate oxide may break.

This is called an **Antenna Effect**.

---

### Two Solutions for Antenna Violations

1) Bridge the wire to a higher metal layer (by adding jump/bridge)

- Requires router awareness.

2) Add Antenna Diodes

- These leak away accumulated charge.
- Antenna diodes are provided by the Standard Cell Library (SCL).

---

###  Preventive Antenna Approach

Preventive method:

- Add a fake antenna diode near cell input after placement.
- Run antenna checks (Magic tool) on routed layout.
- If violation is detected on a cell input pin:
    Replace fake diode with real antenna diode.

This ensures safe fabrication.

---

###  Static Timing Analysis (STA)

After routing:

RC extraction is performed using:

- Def2Spef

Timing analysis is performed using:

- OpenSTA (via OpenROAD)

This ensures setup and hold timing constraints are satisfied.

---

###  Physical Verification (DRC & LVS)

Physical verification ensures manufacturability.

Includes:

1) DRC – Design Rule Check
2) LVS – Layout Versus Schematic


---
</details>

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

### 3. Run Synthesis
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

<details>
<summary><strong>Phase 2 — Floorplan Fundamentals</strong></summary>

---

<details>
  <summary>Theory</summary>
  
## Floor Planning, Pre-Placed Cells & Power Planning

---

##  Chip Floor Planning Considerations

### 1. Define Width and Height of Core & Die

In Physical Design (PD) flow, the first step is to determine:

- Core dimensions  
- Die dimensions  

The chip dimensions depend on the total area of logic gates inside the netlist.

---

### Example Netlist

Consider a simple netlist containing:

- Flip-flops (FF)
- AND gates (A1Y)
- OR gates (O1Y)

These are standard cells.

The chip area depends on the number and dimensions of these logic gates.

---

### Assume Standard Cell Dimensions

Let:

- Each standard cell = 1 unit width × 1 unit height  
- Area of each cell = 1 square unit  

If we place 4 cells together:

Total Area = 4 units  
Possible layout → 2 units × 2 units core  

---

### What is Core & Die?

- Core: Area where logic blocks are placed  
- Die: Entire silicon area including core and IO region  

A chip contains multiple dies on a silicon wafer.

---

### 1.1 Utilization Factor

Even if core area is 2 × 2 = 4 units,

Not all area is usable for cell placement due to:

- Routing space
- Buffers
- Spare cells
- Congestion
- Power planning

### Formula:

Utilization Factor = Area occupied by Netlist / Total Core Area

Example:

UF = 4 / (2 × 2) = 1

But 100% utilization is not practical.

### Practical Utilization:
- Typically 50% – 60%
- Rarely exceeds 70%

Because routing and power network also require space.

---

### 1.2 Aspect Ratio

### Definition:

Aspect Ratio = Height / Width

### Example 1:
Core = 2 units × 2 units

Aspect Ratio = 2 / 2 = 1

![utilization_factor](phase2/utilization_factor.png)

### Example 2:
Core Area = 8 units  
Width = 4 units  
Height = 2 units  

Aspect Ratio = 2 / 4 = 0.5

### Notes:
- Ideal aspect ratio is usually close to 1
- Most chips are rectangular
- Practical aspect ratio range: 0.5 to 2

---

## 2. Define Locations of Pre-Placed Cells

---

### 2.1 What Are Pre-Placed Cells?

Pre-placed cells are:

- Large IP blocks
- Fixed-location modules
- Placed before standard cell placement
- Cannot be moved during placement & routing

---

### Examples of Pre-Placed Cells (IPs)

- Memory
- Clock Gating Cells
- Comparator
- Multiplexer (MUX)

These IPs:

- Are designed separately
- May appear multiple times
- Have fixed locations in the chip
- Are treated as black boxes

---

### 2.2 Black Boxing

When logic blocks are not expanded internally:

- Internal logic is hidden
- Treated as a single module
- Called black boxing

![Black_box](phase2/blk_box.png)

Purpose:

If a block repeats multiple times:
- Instead of showing all internal logic
- Represent as a single block

This simplifies:

- Timing analysis
- Placement
- Floorplanning

---

### 2.3 Floor Planning

Floorplanning involves:

- Arranging IP blocks
- Fixing pre-placed cells
- Assigning physical regions
- Leaving routing channels
- Defining core boundary

Automated placement & routing tools:
- Place standard cells around pre-placed macros

---

## 3. Surrounding Pre-Placed Cells with Decoupling Capacitors

### Pre-Placed Cells

Pre-placed cells (IP blocks/macros) are:

- Large blocks like Memory, Comparator, Clock gating cells, MUX etc.
- Placed before standard cell placement.
- Fixed in location.
- Not moved during placement and routing.
- Treated as black boxes.

  ![pre_placed](phase2/pre_placed.png)

These blocks:

- May consume large switching current.
- May switch simultaneously.
- Can cause supply instability.

---


### IR Drop and Noise Margin Impact

![circuit](phase2/ckt.png)

Example:

If VDD = 1V  
Due to IR drop, internal node may receive only 0.7V.

This may:

- Reduce noise margin.
- Push logic level toward undefined region.
- Cause functional failure.

---

![noise_margin](phase2/noise_margin.png)

### Noise Margin and Undefined Region (Unsafe Zone)

For digital circuits, valid voltage levels are defined as:

- VOL  → Maximum voltage recognized as Logic 0
- VOH  → Minimum voltage recognized as Logic 1
- VIL  → Input voltage threshold for Logic 0
- VIH  → Input voltage threshold for Logic 1

Noise margins are defined as:

NMH = VOH − VIH  
NML = VIL − VOL  

For proper logic operation, both NMH and NML must be positive and sufficiently large.

### Undefined Region

The voltage region between VIL and VIH is called the **Undefined Region**.

This region is an **unsafe zone** because:

- The logic level is not guaranteed to be 0 or 1.
- Circuit behavior becomes unpredictable.
- Output may oscillate or produce incorrect logic.
- Functional failures may occur.

If due to IR drop or ground bounce:

- Logic High drops below VIH
- Logic Low rises above VIL

The signal may enter the undefined region.

This unsafe voltage zone must always be avoided in reliable chip design.

---

## Solution: Decoupling Capacitors (DECAP)

![Decoupling_capacitor](phase2/decoupling_cap.png)

To solve supply instability:

We place decoupling capacitors near macros.

### What is a Decoupling Capacitor?

- A local capacitor placed between VDD and VSS.
- Stores charge.
- Supplies current locally during switching.
- Reduces sudden current demand from global supply.

---

## Surrounding the Pre-Placed Cells

Decoupling capacitors are placed around large IP blocks:

![Decoupling_capacitor_pre_placed](phase2/decoupling_cap_ip.png)

These DECAP cells are inserted:

- Around the boundary of the macro.
- Between large IP blocks.
- Near power sensitive regions.

---

## Why Surround the Pre-Placed Cells?

Because:

- Macros draw high instantaneous current.
- Long power routing introduces resistance.
- Large switching activity causes voltage fluctuations.

Decoupling capacitors:

- Provide a local charge reservoir.
- Reduce IR drop.
- Reduce ground bounce.
- Improve power integrity.
- Maintain stable VDD and VSS.
- Keep signals away from the undefined (unsafe) region.

---
## 4. Power Planning 

![power_supply](phase2/power_supply.png)

### Scenario:

If a macro is repeated many times:

- All blocks may switch simultaneously
- Large current drawn at same time
- Causes IR drop
- Causes Ground bounce

---

### Two Major Issues:

### 1) When Logic switches 1 → 0

- All capacitors discharge
- Sudden current to ground
- Causes ground bounce

![ground_bounce](phase2/ground_bounce.png)

### 2) When Logic switches 0 → 1

- All capacitors charge
- Heavy current from VDD
- Causes voltage drop at VDD

 ![Voltage_drop](phase2/voltage_drop.png)

---

### Solution: Multiple Power Tap Points

Instead of:

Single VDD/VSS source

We provide:

- Multiple VDD rails
- Multiple VSS rails
- Power grid network

![power_planning](phase2/power_planning.png)

![power_planning](phase2/power_plan.png)

This ensures:

- Current distributed evenly
- Reduced IR drop
- Reduced ground bounce
- Stable supply


---


## 5) Pin Placement

### Complete Design

The complete design connects:

![complete_design](phase2/complete_design.png)

The connectivity information (how gates are connected) is coded using a hardware description language.

This connectivity description is written using **VHDL language**  
and is called the **Netlist**.

---

After floorplanning, pins are placed on the boundary of the die.

Inputs (DIN1–DIN4) and clocks (CLK1, CLK2) are placed on one side.

Outputs (DOUT1–DOUT4, CLKOUT) are placed on another side.

Pin placement is done carefully to:

- Reduce routing congestion
- Reduce wirelength
- Improve timing

![pin_placement](phase2/pin_placement.png)

### Important Note on Clock Pins

As observed in the pin placement:

- Clock pins (CLK1, CLK2) are bigger in size.

Reason:

Clock signals drive multiple flip-flops continuously.

They require:

- Lower resistance path
- Better current handling
- Stronger metal routing

Similarly:

- CLKOUT should also have lower resistance.

Conclusion:

Bigger the pin size → Lower the resistance.

This improves clock distribution and reduces delay/skew.

---

### Placement Tool Behavior

Now once size and pin placement are defined,

Automatic routing and placement tools try to place standard cells in the available core area.

However,

Certain regions must not have standard cells placed.

---

## Logical Cell Placement Blockage

To prevent tools from placing cells in restricted areas,

We create a **Logical Cell Placement Blockage**.

![cell_placement_blockage](phase2/cell_placement_blockage.png)

Blockage means:

- A defined region in the core
- Where placement tool is restricted
- No cell can be placed in that area

This blockage ensures:

- Space is reserved for routing
- Space is reserved for future modifications
- Macros and critical routing areas are protected
- Congestion is reduced

The placement and routing tool will not place any cell inside the blockage area.

---
</details>
  
</details>
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
