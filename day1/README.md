Day 1 – Introduction to RTL Design and Synthesis

Objectives

* Understand RTL Design Flow
* Simulate Verilog Designs using iVerilog
* Visualize Waveforms using GTKWave
* Perform Logic Synthesis using Yosys

Files Used

* good_mux.v
* tb_good_mux.v

### iVerilog Flow

```
RTL Design (.v) ──┐
                  ├──► iVerilog ──► a.out ──► .vcd file ──► GTKWave
Test Bench (.v) ──┘
```

### Lab Setup Commands

```bash
mkdir vlsi
cd vlsi
mkdir vsdflow
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop
ls -ltr
cd my_lib/lib        # Sky130 Standard Cell library
cd ../verilog_model  # Verilog models
cd ../verilog_files  # RTL and Testbench files
```

### Simulation of good_mux

```bash
iverilog good_mux.v tb_good_mux.v   # Step 1: Compile
./a.out                              # Step 2: Generate .vcd
gtkwave tb_good_mux.vcd             # Step 3: View waveform
```

### Yosys Synthesis of good_mux

```bash
cd verilog_files
yosys                                                          # Invoke Yosys
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib  # Load library
read_verilog good_mux.v                                        # Read design
synth -top good_mux                                            # Synthesize
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib  # Map to cells
show                                                           # View netlist graph
write_verilog -noattr good_mux_netlist.v                      # Write netlist
```

### good_mux Netlist (Generated)

```verilog
### Generated Netlist

The synthesized gate-level netlist was generated using Yosys and mapped to SKY130 standard cells.

File:
- good_mux_netlist.v
```

### Why Different Flavours of Standard Cells?

- **Setup Time** → Needs cells with **least propagation delay** → Wide transistors → Higher power/area
- **Hold Time** → Needs cells with **more propagation delay** → Narrow transistors → Lower power/area
- This is why the library contains multiple versions of the same logic gate.

---

Results

RTL Code

---
<img width="1360" height="768" alt="rtl_code png" src="https://github.com/user-attachments/assets/6d49ecfa-f8f9-43e5-b57b-a02c5e08800c" />


---

GTKWave Output

---
<img width="1360" height="768" alt="gtkwave_output png" src="https://github.com/user-attachments/assets/2e16e557-0d45-4e13-ac1b-b206064fa055" />

---

Yosys Synthesis

---
<img width="1360" height="768" alt="yosys_synthsis_step1 png" src="https://github.com/user-attachments/assets/77c1bcce-f6ab-45c1-a0fe-4bc1b49980bf" />

---
<img width="1360" height="768" alt="yosys_synthsis_step2 png" src="https://github.com/user-attachments/assets/adb92dc6-5a4e-4595-a1d8-9be67299c1aa" />

---

Synthesized Netlist

---
<img width="1360" height="768" alt="good_mux_netlist png" src="https://github.com/user-attachments/assets/dc3d3505-0802-41f0-9067-5dc07adfea7c" />



----

Key Learning

Successfully simulated and synthesized a 2:1 Multiplexer using Verilog, GTKWave, Yosys, and the SKY130 standard cell library.



