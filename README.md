## CORDIC

This project presents the design and implementation of a 16-stage pipelined CORDIC processor using Verilog HDL for efficient trigonometric computation. The processor is based on the rotation-mode CORDIC algorithm, which computes sine and cosine values using only shift-and-add operations, making it suitable for hardware-efficient digital signal processing applications.

The architecture is fully pipelined, with each stage performing one iteration of the CORDIC algorithm. Pipeline registers are inserted between stages to enable continuous data flow and high throughput. This design significantly improves performance by allowing multiple input vectors to be processed concurrently while maintaining deterministic latency.

The processor operates on signed fixed-point arithmetic, ensuring a balance between numerical accuracy and hardware resource utilization. Angle preprocessing and sign correction logic are included to handle different input ranges and ensure correct output generation. Control signals and angle values are propagated across pipeline stages to maintain synchronization throughout the computation.

Each pipeline stage updates the X and Y coordinate values based on the direction of rotation, determined by the current angle residue. Shift operations replace multipliers, reducing hardware complexity and power consumption. After completing all 16 stages, the processor produces rotated vector outputs corresponding to sine and cosine values of the input angle.

The design is fully synthesizable and suitable for FPGA or ASIC implementation. Its modular and scalable structure allows the number of stages to be modified for different precision and performance requirements. This project demonstrates practical concepts of pipelined architecture, fixed-point arithmetic, and hardware-oriented algorithm optimization, making it relevant for high-performance digital signal processing and VLSI system design

## Main Module (`main`)

The `main` module is the top-level module of the **16-stage pipelined CORDIC processor**. It accepts signed 16-bit inputs `Xin`, `Yin`, and `Zin` along with clock and reset signals, and produces signed 16-bit outputs `Xout`, `Yout`, and `Zout`. This module integrates angle preprocessing, pipelined computation stages, and final output correction logic.

---

## Angle Transformation

Before entering the pipeline, the input angle `Zin` is preprocessed using an **angle transformation block**. The sign information is extracted using an XOR operation on the two MSBs of `Zin`, generating control signal `p0`. This ensures correct quadrant handling for CORDIC rotation.

---

## Pipelined Architecture

The design uses a **17-stage pipeline** (16 computation stages + 1 output stage). Each stage performs one CORDIC iteration, improving throughput by allowing one computation per clock cycle after pipeline fill.

---

## Pipeline Registers

Each stage contains `register` modules for X, Y, and Z values (`RXi`, `RYi`, `RZi`). These registers store intermediate results and ensure synchronous data transfer between stages on every clock edge.

---

## D Flip-Flops for Control Signal

A chain of **D flip-flops (`dff`)** propagates the control signal (`p0` → `p17`) alongside data through the pipeline. This maintains alignment between data and quadrant correction logic.

---

## Compute Modules

Each stage instantiates three `compute` blocks:

* **X update** using shifted Y values
* **Y update** using shifted X values
* **Z update** using precomputed arctangent constants

Shift operations (`>>> stage_number`) implement efficient shift-add CORDIC arithmetic without multipliers.

---

## CORDIC Iterations

Each stage uses a different arctangent constant (e.g., `16'h2000`, `16'h12E4`, …, `16'h0000`) corresponding to successive CORDIC iterations, achieving higher precision with each pipeline stage.

---

## Final Output Selection

In the final stage, a **MUX-based correction** is applied using control signal `p17`. This ensures correct sign adjustment of `Xout` and `Yout` based on the original input angle quadrant.

---

## Key Features

* 16-bit signed fixed-point design
* 16-stage pipelined CORDIC processor
* Shift-add based arithmetic (hardware efficient)
* Fully synchronous design using clock and reset
* Suitable for FPGA and DSP applications


