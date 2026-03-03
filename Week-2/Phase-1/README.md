# Task 1.2 – RTL-to-GDS Flow using ORFS (Sky130)

## Objective
To run the complete RTL-to-GDS flow for the RISC-V design (`riscv32i`) using the OpenROAD Flow Scripts (ORFS) in GitHub Codespaces and verify all major implementation stages.

------------------------------------------------------------

## Step 1: Navigate to Flow Directory

cd orfs/flow

Modified Makefile:
- Commented default config
- Enabled: sky130hd/riscv32i/config.mk

------------------------------------------------------------

# 1. Synthesis Stage

Command:
make synth

Tool Used:
Yosys (0.58+94)

What Happened:
- RTL files (adder.v, alu.v, datapath.v, regfile.v, etc.) were read
- Liberty file loaded: sky130_fd_sc_hd_tt_025C_1v80.lib
- Gate-level netlist generated
- Design linked hierarchically
- SDC constraints applied

Evidence from Log:
- Liberty frontend executed
- Verilog-2005 frontend executed for all RISC-V modules
- write_db 1_synth.odb
- write_sdc 1_synth.sdc

Generated Files:
results/sky130hd/riscv32i/base/
- 1_1_yosys.v
- 1_synth.odb
- 1_synth.sdc

------------------------------------------------------------

# 2. Floorplan Stage

Command:
make floorplan

What Happened:
- Core size calculated
- IO placement done
- Tap cells inserted
- Power distribution planning initialized

From Log:
- Floorplan utilization ≈ 46%
- Design area ≈ 61496 µm²
- 1768 tapcells inserted
- Row creation completed

Files Generated:
- 2_1_floorplan.odb
- 2_1_floorplan.sdc

GUI Verification:
make gui_floorplan

Observed:
- Core boundary visible
- Standard cell rows arranged
- Power rails present

------------------------------------------------------------

# 3. Placement Stage

Command:
make place

What Happened:
- Global placement performed
- Detailed placement optimized
- Overlap removal done

From Log:
- Total instances: 6536
- Movable instances: 4768
- Fixed instances: 1768
- Utilization ≈ 49%
- No placement violations

Detailed Placement Report:
- Design area: 69408 µm²
- Utilization: 52%

Files Generated:
- 3_place.odb
- 3_place.sdc

GUI Verification:
make gui_place

Observed:
- Standard cells placed properly
- No overlaps
- Congestion manageable

------------------------------------------------------------

# 4. Clock Tree Synthesis (CTS)

Command:
make cts

What Happened:
- Clock net buffered
- Clock tree built using TritonCTS
- Clock sinks clustered

From Log:
- Clock: clk
- Total sinks: 1056
- Buffers inserted: 115
- Characterization buffer used: sky130_fd_sc_hd__clkbuf_16
- No hold violations found

CTS Final Report:
- Design area ≈ 75857 µm²
- Utilization ≈ 57%

Files Generated:
- 4_cts.odb
- 4_cts.sdc

GUI Verification:
make gui_cts

Observed:
- Clock tree branches visible
- Buffers inserted
- Balanced clock network

------------------------------------------------------------

# 5. Routing Stage

Command:
make route

What Happened:
- Global routing executed
- Detailed routing completed
- Via insertion done
- DRC cleanup applied

From Log:
- Number of layers: 13
- Routing grid patterns: 12
- No routing crashes
- Design successfully routed

Visual Confirmation:
- Metal1 to Metal5 used
- Vias inserted
- Fully connected routing

------------------------------------------------------------

# 6. Timing Analysis (Post-Route)

Timing Reports (from GUI):

### Hold Slack
- Majority endpoints have positive hold slack
- No hold violations reported

### Setup Slack
- Some setup violations present
- Example worst slack ≈ -0.66 ns

Clock Period Used:
6.0 ns

Clock Network:
- Fanout: 1056
- Logic depth up to 17 levels

------------------------------------------------------------

# 7. Final Observations

✔ Synthesis completed successfully  
✔ Floorplan generated  
✔ Placement done without violations  
✔ CTS built correctly  
✔ Routing completed  
✔ Timing analyzed  
✔ No hold violations  
⚠ Setup violations exist (optimization required for closure)

------------------------------------------------------------

## Learning Outcomes

- Understood complete RTL-to-GDS flow
- Observed how tools interact in ORFS
- Verified every stage using GUI
- Analyzed setup and hold timing
- Learned clock tree behavior and buffering

------------------------------------------------------------

## Conclusion

The RISC-V design was successfully taken from RTL to routed layout using the Sky130 PDK inside GitHub Codespaces. All major physical design stages were executed and verified.

This task demonstrates understanding of:
- Tool orchestration via Makefile
- OpenROAD physical design flow
- Timing analysis
- Layout visualization
- Design implementation in Sky130 technology

------------------------------------------------------------
