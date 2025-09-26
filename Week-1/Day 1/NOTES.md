#  📘Day 1: Introduction to Verilog RTL Design & Synthesis

---

##  📑Table of Contents

1. 🔍[What is Simulation ?](#1-what-is-simulation-)
2. 📂[Cloning of sky130RTLDesignandSynthesisWorkshop git](#2-cloning-of-sky130rtldesignandsynthesisworkshop-git)
3. 💻[Lab: Simulating a 2-to-1 Multiplexer using iverilog and gtkwave](#3-lab-simulating-a-2-to-1-multiplexer-using-iverilog-and-gtkwave)
4. 📝[Verilog Code Analysis](#4-verilog-code-analysis)
5. ⚙️ [Introduction to Yosys & Logic Synthesis](#5-%EF%B8%8F-introduction-to-yosys--logic-synthesis)
6. 🔧[Lab: Synthesis of 2-to-1 Multiplexer with Yosys](#6-lab-synthesis-of-2-to-1-multiplexer-with-yosys)
7. 📌[Summary](#7-summary)


---

## 1. 🔍What is Simulation ?

###  Simulator

A **simulator** is used to check whether the RTL design is sticking to the given specification or not. We do this by simulating the design.
- Simulator is the tool used for simulating the design (iverilog).
  
###  Design

The **design** is the actual Verilog code which describes the intended logic functionality to meet the required specifications.

###  Testbench

A **testbench** is the setup to apply various inputs to your design and checks if the outputs are correct.

###  How Simulator Works?

- Simulator looks for the changes on the input signals.
- Upon the change of the input the output is calculated. <br>
  Note: If no changes are made to the input then there is no change in the output.
<div align="center">
  <img src="https://github.com/user-attachments/assets/2c9f24f8-f457-4554-972f-aefef2d1291f" alt="Design & Testbench Overview" width="70%">
</div>
<br>
The Design may have one or more primary inputs or primary output but testbench doesn't.

### Simulation Flow in Iverilog

<br>
<div align="center">
  <img width="70%" alt="simulation flow" src="https://github.com/user-attachments/assets/1998e460-ea53-4a92-8f42-a8005e758b5c" />
</div>
<br>

* For a simulator we provide both RTL design (written in HDL) and Testbench (stimulus). Then the simulator does the simulation.
* And then a vcd file is generated which has the information of the signals whose values are changed.
* The vcd file is given to a waveform viewer (gtkwave).
* A waveform is generated from the gtkwave by observing it we can check that our design is meeting our requirements or not.

--- 

## 2. 📂Cloning of sky130RTLDesignandSynthesisWorkshop git

### Step-1 Make a new folder(directory)

```shell
mkdir VLSI
cd VLSI
```

### Step-2 Cloning sky130RTLDesignandSynthesisWorkshop git repo

``` shell
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
```

<div align="center">
  <img width="70%"  alt="image" src="https://github.com/user-attachments/assets/3ae6caf5-b6ff-4140-91ba-d81007b86858" />
</div>

### Step-3 Check all the library files

``` shell
cd VLSI/sky130RTLDesignAndSynthesisWorkshop/
ls
```

<div align="center">
  <img width="70%" height="300%" alt="image" src="https://github.com/user-attachments/assets/dab7801d-6dd5-4f60-97c8-d2970c9be883" />
</div>

It contains
- my_lib folder which has a primitives and standard cell information.
- lib contains the library file for the standard cell.
- Verilog_files contains all the lab files (Design and testbench).

<div align="center">
  <img width="70%"  alt="image" src="https://github.com/user-attachments/assets/b1b53e69-aa75-43d0-bbf2-f3d845a365e4" />
</div>

---

## 3. 💻Lab: Simulating a 2-to-1 Multiplexer using iverilog and gtkwave

* Provide design and testbench file to the iverilog simulator and execute the a.out output file

```shell
iverilog good_mux.v tb_good_mux.v
./a.out
```

* Open the waveform viewer gtkwave and observe the waveform
  
```shell
gtkwave tb_good_mux.vcd
```

<div align="center">
  <img width="70%" alt="image" src="https://github.com/user-attachments/assets/0efb2b62-a657-4e09-a4ff-149632e2f995" />
</div>

* Drag the signals and observe the signal transitions

<div align="center">
  <img width="70%" alt="image" src="https://github.com/user-attachments/assets/daffa679-f759-4115-a2af-d5a3514d21c4" />
</div>

---

## 4. 📝Verilog Code Analysis

* The verilog code for the multiplexer.
  
  - module code
    
    ```verilog
    module good_mux (input i0 , input i1 , input sel , output reg y);
    always @ (*)
    begin
	    if(sel)
		    y <= i1;
	    else 
		    y <= i0;
    end
    endmodule
    ```
    
    Explanation:
      * Inputs
        - i0 → Data input 0
        - i1 → Data input 1
        - sel → Select line (decides which input goes to output)
      * Output
        y → Selected output
      * Function
        - If sel = 0, then y = i0
        - If sel = 1, then y = i1
          
  - Testbench code
    
    ```verilog
    `timescale 1ns / 1ps
    module tb_good_mux;
	    // Inputs
	    reg i0,i1,sel;
	    // Outputs
	    wire y;

        // Instantiate the Unit Under Test (UUT)
	    good_mux uut (
		    .sel(sel),
		    .i0(i0),
		    .i1(i1),
		    .y(y)
	    );

	    initial begin
	      $dumpfile("tb_good_mux.vcd");
	      $dumpvars(0,tb_good_mux);
	      // Initialize Inputs
	      sel = 0;
	      i0 = 0;
	      i1 = 0;
	      #300 $finish;
	    end

      always #75 sel = ~sel;
      always #10 i0 = ~i0;
      always #55 i1 = ~i1;
    endmodule
    ```
    
   Explanation:
     - It has 3 inputs (sel,i0,i1) and one output y.
     - Instantiation of the module program.
     - Initialization of the input signals.
     - Input signals are repeated for some timeunits and the simulation is finished at 300ns.
   
* Get into the verilog_files directory of the sky130RTLDesignAndSynthesisWorkshop

```shell
cd VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
gvim good_mux.v -o tb_good_mux.v
```

---

## 5. ⚙️ Introduction to Yosys & Logic Synthesis

#### What is Yosys?

A **Yosys** is a powerful synthesizer tool used for converting the RTL to netlist.

#### Features

- **Synthesis:** Converts HDL to a netlist (circuit).
- **Optimization:** Used to improve speed or area
- **Technology Mapping:** Matches RTL logic to actual hardware cells
- **Verification:** Checks correctness
 <br> 
<div align="center">
  <img width="70%" alt="Screenshot from 2025-09-26 14-54-46" src="https://github.com/user-attachments/assets/41cd343c-4671-434d-bfed-0e636d75a443" />
</div>
<br>

To the synthesizer tool we provide 
 - Design module file (by read_verilog command)
    Behavioral representation of the required specification.
 - Library liberty file (by read_liberty)
    * Collection of logical modules.
    * Includes different flavours of same gates
      
<br>
<div align="center">
  <img width="70%" height="383" alt="Screenshot from 2025-09-26 14-53-46" src="https://github.com/user-attachments/assets/996fe035-42ec-4a9c-9dc3-00fce6a123e5" />

</div>
<br>
Using these files synthesis tool provides netlist (by write_verilog command) mapped to standard cells in the library file.

#### Why do we need different flavours for gate?

- To make our design to work faster or to have a maximum operable frequency or to have required performance, we need to decrease the delay.The delay can be reduced by using fast cells in the combinational path.
 <br> 
<div align="center">
  <img width="70%" alt="Screenshot from 2025-09-26 12-42-17" src="https://github.com/user-attachments/assets/77174fc3-eef7-481b-b300-85604a8e867c" />
</div>
<br>

- To ensure that there are no **HOLD** issues at the capture side, we need cells that work slowly.

 <br> 
<div align="center">
  <img width="70%"  alt="image" src="https://github.com/user-attachments/assets/a4f33409-0e9a-496c-9f63-f89b88c0d904" />
</div>
<br>

The synthesizer tool selects the standard cells provided in the library file by using **Constraints**

#### How to verify the synthesis?

<br>
<div align="center">
  <img width="70%" height="547" alt="Screenshot from 2025-09-26 14-55-34" src="https://github.com/user-attachments/assets/b1f37eec-b7c3-43d2-8cde-0d3b6b34dcd5" />
</div>
<br>

We can verify the synthesis by using the netlist provided by synthesizer tool and the testbench (used in simulation) to iverilog and giving the output file to gtkwave.Both the waveforms that are generated at the simulation and the synthesis should be same.

---

## 6. 🔧Lab: Synthesis of 2-to-1 Multiplexer with Yosys

Let’s synthesize the `good_mux` design using Yosys!

###  Procedure of Yosys Flow

1. **Invoke Yosys**
    ```shell
    yosys
    ```
2. **Read the Verilog code**
    ```shell
    read_verilog good_mux.v

    ```    
3. **Read the liberty library**
    ```shell
    read_liberty -lib /home/mrincognito/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```
4. **Synthesize the design**
    ```shell
    synth -top good_mux
    ```

5. **Technology mapping**
    ```shell
    abc -liberty /home/mrincognito/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
    ```

6. **Visualize the gate-level netlist**
    ```shell
    show
    ```
7. **Close Yosys**
    ```shell
    exit
    ```
<div align="center">
  <img src="https://github.com/user-attachments/assets/d0cbcf01-7e66-44f6-9f57-8c55f299a345"  alt="Yosys Gate-level Schematic" width="70%">
</div>

---

## 7. 📌Summary

- Learned about RTL Design & Simulation flow using Icarus Verilog (iverilog) and GTKWave.
- Understood the role of Design, Testbench, and Simulator in verifying functionality.
- Practiced cloning the sky130RTLDesignAndSynthesisWorkshop repo and explored its structure (library files, standard cells, Verilog files).
- Simulated a 2:1 multiplexer using Verilog design and testbench, and verified results with GTKWave.
- Analyzed Verilog code (design + testbench) and confirmed expected behavior.
- Got introduced to Yosys for logic synthesis:
- Read Verilog design and Liberty files.
- Performed synthesis, optimization, and technology mapping.
- Generated gate-level netlist and visualized schematic.
- Understood why multiple flavors of gates (fast/slow cells) are used for performance and timing closure.
- Verified synthesis correctness by comparing gate-level simulation with RTL simulation outputs.

---
