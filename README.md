# Experiment 4: Timing Validation of Flip-Flop Input using Random Data Generator for Setup and Hold Constraints
## Giri R
## 21223060068
---

## Aim  
To validate the **timing of a Flip-Flop input** using **random data generation** in **SystemVerilog** and check **setup and hold constraints** using **ModelSim 2020.1**.

---

## Apparatus Required  
- Computer with **Windows OS**  
- **ModelSim 2020.1** (or later)  
- SystemVerilog source code editor  

---

## Description  
Timing validation ensures that a Flip-Flop operates correctly under all input conditions.  
- **Setup time:** Minimum time before the clock edge that the data input must be stable.  
- **Hold time:** Minimum time after the clock edge that the data input must remain stable.  

In this experiment:  
- A **random data generator** is used to drive the Flip-Flop input.  
- **SystemVerilog constructs** like `randomize()` and `class` or `logic` vectors can be used to generate test data.  
- Simulation is used to observe **violations or adherence** to setup and hold constraints.  

---

## Features  
- Flip-Flop timing validation using **random inputs**  
- Checks for **setup and hold constraints**  
- Includes a **testbench** for verification  
- Compatible with **ModelSim 2020.1**  

---

## Procedure  

1. **Open ModelSim 2020.1**  
   - Launch the IDE.  

2. **Create a New Project**  
   - `File → New → Project`  
   - Name it `FlipFlop_Timing_Project`.  

3. **Add SystemVerilog Files**  
   - Create `flipflop.sv` → Flip-Flop design  
   - Create `flipflop_tb.sv` → Testbench with random input generator  

4. **Compile the Design and Testbench**  
   - Select both files, right-click → **Compile Selected**  
   - Resolve any syntax errors  

5. **Start Simulation**  
   - `Simulate → Start Simulation`  
   - Select testbench module (`flipflop_tb`)  

6. **Add Signals to Waveform**  
   - Select clock, data, and output signals  
   - Right-click → **Add to → Wave → Selected Signals**  

7. **Run Simulation**  
   - Type in console:  
     ```
     run 200ns
     ```  
   - Or use **Run button**  

8. **Analyze Waveforms**  
   - Check setup and hold times at the Flip-Flop input  
   - Verify Flip-Flop output stability for all random input patterns  

9. **Save Results**  
   - Save waveform (`.wlf`) for documentation  

---

## SystemVerilog Code Skeleton  

### Flip-Flop Design (`flipflop.sv`)
```systemverilog
module flipflop (
    input  logic D,       
    input  logic CLK,     
    output logic Q        
);

    always_ff @(posedge CLK) begin
        Q <= D;
    end

endmodule
```
### Testbench (`flipflop_tb.sv`)
```systemverilog
module tb;
    logic D;
    logic CLK;
    logic Q;

    parameter setup_time = 2;
    parameter hold_time  = 1;

    flipflop uut (
        .D(D),
        .CLK(CLK),
        .Q(Q)
    );

    initial begin
        CLK = 0;
        forever #5 CLK = ~CLK;
    end

    initial begin
        D = 0;
        repeat (20) begin
            int delay;
            delay = $urandom_range(1, 9); 
            #(delay) D = $urandom_range(0, 15);  

            if (($time % 10) > (10 - setup_time) && ($time % 10) < 10) begin
                $display("**Setup violation at %0t", $time);
            end

            #(hold_time)
            if (($time % 10) < hold_time) begin
                $display("**Hold violation at %0t", $time);
            end
        end
        #50 $finish;
    end

    initial begin
        $dumpfile("wave.vcd");
        $dumpvars(0, tb);
    end
endmodule

```
---
### Simulation Output

Simulation is carried out using ModelSim 2020.1.
<img width="1920" height="1080" alt="Screenshot 2025-09-29 232424" src="https://github.com/user-attachments/assets/71720e3c-c876-46ff-893d-b21aac51aee7" />

---

### Result

The timing validation of Flip-Flop input using random data generation was successfully carried out in SystemVerilog HDL with ModelSim 2020.1.
The Flip-Flop maintained correct output behavior, and setup and hold times were verified for all random input cases.
