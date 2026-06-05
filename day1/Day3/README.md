Day 3 – Combinational and Sequential Logic Optimization

Objectives

* Understand combinational logic optimization
* Learn constant propagation techniques
* Analyze sequential logic optimization
* Observe removal of redundant hardware during synthesis

Files Used

* opt_check.v
* opt_check2.v
* opt_check3.v
* opt_check4.v
* dff_const1.v
* dff_const2.v
* dff_const3.v
* dff_const4.v
* dff_const5.v
* counter_opt.v

⸻



## Combinational Logic Optimization

The opt_check examples demonstrated how Yosys simplifies Boolean expressions and removes redundant logic through constant propagation and optimization techniques.

**opt_check.v** — Ternary mux simplifies to AND gate:
```verilog
module opt_check (input a, input b, output y);
   assign y = a ? b : 0;   // Optimised to: y = a & b
endmodule
```

**opt_check2.v** — Simplifies to OR gate

**opt_check3.v / opt_check4.v** — Further Boolean reductions

### Sequential Optimisation Examples

| File | Description | Result |
|------|-------------|--------|
| dff_const1.v | Reset to 0, else Q=1 | Flip-flop retained (follows clock) |
| dff_const2.v | Always Q=1 regardless of reset | Optimised to constant 1 (no FF) |
| dff_const3.v | Two FF chain | Both retained |
| dff_const4.v | Reset to 1, else Q1=1, Q=Q1 | Optimised — Q always 1 |
| dff_const5.v | Reset to 0, else Q1=1, Q=Q1 | Two FFs retained |

### Unused Output Optimisation — Counter

```verilog
module counter_opt (input clk, input reset, output q);
   reg [2:0] count;
   assign q = count[0];    // Only bit 0 used as output
   always @(posedge clk, posedge reset)
      if(reset) count <= 3'b000;
      else      count <= count + 1;
endmodule
// Synthesis result: Only 1 FF inferred (bits 1 and 2 optimised away)
```

---### Combinational Optimisation Command

```bash
opt_clean -purge   # Remove unused wires and cells
```

### Examples

**opt_check.v** — Ternary mux simplifies to AND gate:
```verilog
module opt_check (input a, input b, output y);
   assign y = a ? b : 0;   // Optimised to: y = a & b
endmodule
```

**opt_check2.v** — Simplifies to OR gate

**opt_check3.v / opt_check4.v** — Further Boolean reductions

### Sequential Optimisation Examples

| File | Description | Result |
|------|-------------|--------|
| dff_const1.v | Reset to 0, else Q=1 | Flip-flop retained (follows clock) |
| dff_const2.v | Always Q=1 regardless of reset | Optimised to constant 1 (no FF) |
| dff_const3.v | Two FF chain | Both retained |
| dff_const4.v | Reset to 1, else Q1=1, Q=Q1 | Optimised — Q always 1 |
| dff_const5.v | Reset to 0, else Q1=1, Q=Q1 | Two FFs retained |

### Unused Output Optimisation — Counter

```verilog
module counter_opt (input clk, input reset, output q);
   reg [2:0] count;
   assign q = count[0];    // Only bit 0 used as output
   always @(posedge clk, posedge reset)
      if(reset) count <= 3'b000;
      else      count <= count + 1;
endmodule
// Synthesis result: Only 1 FF inferred (bits 1 and 2 optimised away)
```

---

Results


1. constant propagation
---
dff const1

---
simulation
---
<img width="1360" height="768" alt="dff_const1_simulation png" src="https://github.com/user-attachments/assets/a0654ff7-f2d0-42e0-bf95-3ffefc1a61ff" />

---
code
---
<img width="1360" height="768" alt="dff_const1_code png" src="https://github.com/user-attachments/assets/d0a5d0a8-2b57-4732-8cf7-5061d7814df9" />

---
synthesis
---
<img width="1360" height="768" alt="dff_const1_synthesis png" src="https://github.com/user-attachments/assets/5d7cb2f4-8af1-403e-b6d9-2b00c722560d" />

---
gtkwave
---
<img width="1360" height="768" alt="gtkwave_dff_const1 png" src="https://github.com/user-attachments/assets/81a8aeba-5e27-4458-8527-40c6091d6b51" />

---
dff const2
---
simulation
---
<img width="1360" height="768" alt="dff_const2_simulation png" src="https://github.com/user-attachments/assets/860eba4e-0ec8-4745-8118-f3265568e253" />

---
synthesis
---

<img width="1360" height="768" alt="dff_const2_synthesis,png" src="https://github.com/user-attachments/assets/d5d92cdd-4d1b-475a-b6a1-83fbf9978038" />

---
gtkwave
---
<img width="1360" height="768" alt="gtkwave_dff_const2 png" src="https://github.com/user-attachments/assets/367bba88-4d1d-4522-9a51-2fdda7157a7b" />

---
2. Counter Optimization

---
simulation
---
<img width="1360" height="768" alt="counter_opt_simulation png" src="https://github.com/user-attachments/assets/7c35752d-36ae-48c5-84cb-4771aa426a87" />

---
code
---
<img width="1360" height="768" alt="counter_opt_code png" src="https://github.com/user-attachments/assets/7d60df21-6e29-4e3e-aed9-cb55bf7e56a8" />

---
synthesis
---
<img width="1360" height="768" alt="counter_opt_synthesis png" src="https://github.com/user-attachments/assets/0b4d6497-2669-4996-a20c-8e57c375b4e4" />


---


Key Learning

Learned how synthesis tools perform automatic logic reduction using constant propagation, Boolean simplification, and sequential optimization techniques, resulting in reduced area and improved efficiency.
