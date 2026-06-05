Day 2 – Timing Libraries and Synthesis Flow

Objectives

* Explore SKY130 standard cell libraries
* Understand hierarchical and flat synthesis
* Analyze different flip-flop coding styles

Files Used

* multiple_modules.v
* dff_asyncres.v
* dff_async_set.v
* dff_syncres.v

### SKY130 Library

- **Technology:** SKY130 — a mature 180nm-130nm hybrid process by SkyWater Technology / Google
- **Library file:** `sky130_fd_sc_hd__tt_025C_1v80.lib` (tt = typical, 025C = temperature, 1v80 = voltage)

```bash
gvim sky130_fd_sc_hd__tt_025C_1v80.lib
:syn off   # Turn off syntax highlighting
:se nu     # Show line numbers
:/cell     # Search for cells
:g//       # List all matches
```

### Hierarchical Synthesis (Multiple Modules)

```verilog
module sub_module1 (input a, input b, output y);
   assign y = a & b;
endmodule

module sub_module2 (input a, input b, output y);
   assign y = a | b;
endmodule

module multiple_modules (input a, input b, input c, output y);
   wire net1;
   sub_module1 u1(.a(a), .b(b), .y(net1));
   sub_module2 u2(.a(net1), .b(c), .y(y));
endmodule
```

- In **hierarchical synthesis**, module hierarchy is preserved — sub_module1 and sub_module2 appear as instances inside multiple_modules.
- In **flat synthesis** (`flatten` command), all hierarchy is dissolved into a single netlist.

### Sub-Module Level Synthesis

Useful when:
- Same module is instantiated multiple times (synthesize once, replicate)
- Divide-and-conquer for large designs

```bash
synth -top sub_module1
```

### Flop Coding Styles

```bash
dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

| Style | Behaviour |
|-------|-----------|
| Async Reset | Q goes to 0 immediately on reset, independent of clock |
| Async Set | Q goes to 1 immediately on set, independent of clock |
| Sync Reset | Q goes to 0 only on clock edge when reset is high |
| Sync Set | Q goes to 1 only on clock edge when set is high |

### Interesting Optimisation — mul2

```verilog
module mul2(input [2:0] a, output [3:0] y);
   assign y = a * 2;  // No cells needed! Just wire shift
endmodule
// Synthesized as: assign y = {a, 1'b0};
```

No standard cells are inferred — multiplication by 2 is just a **1-bit left shift** (wire connection).

---
Results

1. SKY130 Library Analysis
---
<img width="1360" height="768" alt="async_simulation png" src="https://github.com/user-attachments/assets/6112ca38-5d1b-44f5-a09f-10f70d4ded55" />

---
<img width="1360" height="768" alt="sync_simulation png" src="https://github.com/user-attachments/assets/301af46d-de62-4c7d-ab16-495132c63c4a" />


---

<img width="1360" height="768" alt="multiple_module_simulation png" src="https://github.com/user-attachments/assets/ccf3219a-5fa5-409c-8702-ecab6d9f80ef" />


2. Hierarchical Synthesis
---
<img width="1360" height="768" alt="yosys_synthesis_4 png" src="https://github.com/user-attachments/assets/c9bd8e9b-3d48-4479-b24a-db7aba0926f7" />

---

flat synthesis
---
<img width="1360" height="768" alt="multiple_modules_flat_code png" src="https://github.com/user-attachments/assets/bf2520d8-5907-430d-b0b1-9c556fd04b40" />

---
sub-module level synthesis
---
<img width="1360" height="768" alt="multiple_modules_hier_code png" src="https://github.com/user-attachments/assets/49cec3f8-86ca-472d-baab-7d4f2827c9a0" />

---
3. Special optimization Example -mul2
---
<img width="1360" height="768" alt="mult2_code png" src="https://github.com/user-attachments/assets/1dae2a6d-7f4a-4544-89fb-b12958e41575" />

---

4. Flip-flop synthesis results

Sync rest DFF
---
<img width="1360" height="768" alt="gtkwave_output3 png" src="https://github.com/user-attachments/assets/8f6c9e28-e89b-4d99-8d4d-52774940af0a" />

---
Async set DFF
---
<img width="1360" height="768" alt="gtkwave_output2 png" src="https://github.com/user-attachments/assets/d43ea259-f8b5-4adb-a695-9f47adce6a6f" />

---
Async reset DFF
---

<img width="1360" height="768" alt="gtkwave_output1 png (2)" src="https://github.com/user-attachments/assets/238174d4-9713-4ec1-af7e-6867de5ef33f" />

---




Key Learning

Learned how synthesis maps RTL designs into standard cells and how different coding styles affect the generated hardware.
