Day 4 – Gate Level Simulation (GLS) and Synthesis-Simulation Mismatch

Objectives

* Understand Gate Level Simulation (GLS)
* Compare RTL Simulation with Synthesized Netlist Simulation
* Study synthesis-simulation mismatches
* Learn the effects of blocking and non-blocking assignments
* Analyze coding mistakes that lead to incorrect hardware behavior

Files Used

* ternary_operator_mux.v
* bad_mux.v
* blocking_caveat.v
* blocking_caveat_netlist.v

⸻

### Gate Level Simulation (GLS)

```
Netlist (.v) ──┐
               ├──► iVerilog ──► Simulation Output
Test Bench ────┘
Gate Models ───┘
```

- GLS verifies **logical correctness** of the synthesized netlist
- Netlist is logically equivalent to RTL but uses actual standard cells
- The **same testbench** used for RTL can be reused for GLS

### Synthesis-Simulation Mismatch

**Cause 1: Missing Sensitivity List**

```verilog
// BAD MUX — only sensitive to sel
module bad_mux (input i0, i1, sel, output reg y);
   always @ (sel)          // ← WRONG: missing i0, i1
      if(sel) y <= i1; else y <= i0;
endmodule

// GOOD MUX — sensitive to all inputs
module good_mux (input i0, i1, sel, output reg y);
   always @ (*)            // ← CORRECT
      if(sel) y <= i1; else y <= i0;
endmodule
```

**Cause 2: Blocking vs Non-Blocking Statements**

```verilog
// BLOCKING CAVEAT — wrong ordering causes simulation mismatch
module blocking_caveat (input a, b, c, output reg d);
   reg x;
   always @ (*) begin
      d = x & c;   // Uses OLD value of x (evaluated first)
      x = a | b;   // x updated AFTER d is computed
   end
endmodule
```

| Statement | Behaviour |
|-----------|-----------|
| `=` (Blocking) | Executes sequentially; previous statement completes before next begins |
| `<=` (Non-Blocking) | All RHS evaluated first, then all LHS updated simultaneously |

> **Rule:** Always use `<=` (non-blocking) inside `always @(posedge clk)` sequential blocks.

---

Results

Bad Mux Example
----
rtl code
---
<img width="1360" height="768" alt="bad_mux_code png" src="https://github.com/user-attachments/assets/251d3143-e605-47c7-82e5-d1949e05cdfa" />

---
synthesis result
---
<img width="1360" height="768" alt="bad_mux_synthesis png" src="https://github.com/user-attachments/assets/0d358447-3b6e-4216-8e1d-571044336dbb" />

---
gtkwave output
---
<img width="1360" height="768" alt="gtkwave_bad_mux png" src="https://github.com/user-attachments/assets/fb17cf00-a047-4cf2-9b2a-29b6c6db2309" />

---
Blocking Assignment caveat
---
code
---
<img width="1360" height="768" alt="blocking_caveat_code png" src="https://github.com/user-attachments/assets/d54fab05-5341-4a6f-b837-1653be03e3c5" />

---
synthesis 
---

<img width="1360" height="768" alt="blocking_caveat_synthesis png" src="https://github.com/user-attachments/assets/ccd134b3-8a60-4d1b-8605-57f017c89fc9" />

---
gtkwave
---
<img width="1360" height="768" alt="gtkwave_blocking_caveat png" src="https://github.com/user-attachments/assets/7f5a8862-313b-42c8-8d69-76d49efff1a0" />


Ternary Operator
---
synthesis setup
---
<img width="1360" height="768" alt="ternary_synthesis_setup png" src="https://github.com/user-attachments/assets/2d0fcea3-1d90-48d4-be54-1d392823d8d5" />

---
Synthesis
---
<img width="1360" height="768" alt="trenary_opt_mux png" src="https://github.com/user-attachments/assets/9ee1f922-adf5-4328-bfb5-84fbf38336d4" />

---
gtkwave output
---

<img width="1360" height="768" alt="gtkwave_ternary_opt png" src="https://github.com/user-attachments/assets/1f5d3412-0b33-45a2-b5a0-869788ed0a11" />


Key Learning

Learned the importance of Gate Level Simulation in validating synthesized hardware and understood how incomplete sensitivity lists and improper use of blocking assignments can create synthesis-simulation mismatches.
