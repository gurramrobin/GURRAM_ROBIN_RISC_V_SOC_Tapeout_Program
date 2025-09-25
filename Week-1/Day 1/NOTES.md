#  Day 1: Introduction to Verilog RTL Design & Synthesis

##  Table of Contents

1. [What is a Simulator?(Iverilog)](#1-what-is-a-simulator?)
2. [Cloning of sky130RTLDesignandSynthesisWorkshop git](#2-Cloning-of-sky130RTLDesignandSynthesisWorkshop-git)
3. [Lab: Simulating a 2-to-1 Multiplexer](#3-lab-simulating-a-2-to-1-multiplexer)
4. [Verilog Code Analysis](#4-verilog-code-analysis)
5. [Introduction to Yosys & Gate Libraries](#5-introduction-to-yosys--gate-libraries)
6. [Synthesis Lab with Yosys](#6-synthesis-lab-with-yosys)
7. [Summary](#7-summary)

---

## 1. What is a Simulator(Iverilog)?

###  Simulator

A **simulator** is used to check whether the RTL design is sticking to the given specification.We do this by simulating the design.
- Simulator is the tool used for simulating the design(iverilog)
  
###  Design

The **design** is the actual Verilog code which describes the intended logic functionality to meet the required specifications

###  Testbench

A **testbench** is the setup to apply various inputs to your design and checks if the outputs are correct.

###  How Simulator Works?

- Simulator looks for the changes on the input signals.
- Upon the change of the input the output is calculated.
  Note: If no changes are made to the input then there is no change in the output.

<img width="744" height="354" alt="image" src="https://github.com/user-attachments/assets/2c9f24f8-f457-4554-972f-aefef2d1291f" />

The Design may have one or more primary inputs or primary output but testbench doesn't.

### Simulation Flow in Iverilog

<img width="807" height="334" alt="image" src="https://github.com/user-attachments/assets/1998e460-ea53-4a92-8f42-a8005e758b5c" />

* For a simulator we provide both RTL design(written in HDL) and Testbench (stimulus).Then the simulator does the simulation.
* And then a vcd file is generated which has the information of the signals whose values are changed.
* The vcd file is given to a waveform viewer(gtkwave).
* A waveform is generated from the gtkwave by observing it we can check that our design is meeting our requirements or not.

--- 

## 2. Cloning of the sky130RTLDesignandSynthesisWorkshop git

<img width="1274" height="391" alt="image" src="https://github.com/user-attachments/assets/3ae6caf5-b6ff-4140-91ba-d81007b86858" />

### Step 1 Make a nwe folder(directory)

```shell
mkdir VLSi
```
### Step 2 Cloning sky130RTLDesignandSynthesisWorkshop git repo

``` shell
mrincognito@mrincognitopc:~/VLSI$ git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop
```

### Step 3 Check all the library files








