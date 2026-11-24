# 4-Bit Odd Counter on FPGA (Artix-7)

A Verilog implementation of a synchronous 4-bit odd counter designed and implemented in **Xilinx Vivado 2024.2** targeting the **Artix-7 (`xc7a35tftg256-1`)** FPGA.

## Overview
The module counts exclusively through 4-bit odd numbers (`1, 3, 5, 7, 9, 11, 13, 15`) and wraps back around to `1` (`4'b0001`). A parameterized clock divider slows down the high-frequency board clock so the binary count is clearly visible on four onboard LEDs.

* **Counting Sequence:** `0001` → `0011` → `0101` → `0111` → `1001` → `1011` → `1101` → `1111` → `0001`
* **Reset Behavior:** Synchronous active-high reset initializes the counter to `4'b0001` (1).
* **Clock Divider:** Parameterized (`CLK_FREQ_HZ = 50_000_000`) to generate a visible tick rate from the onboard clock.

> **Note on Clock Frequency:** The `.xdc` constraint file defines a `100 MHz` (`10 ns` period) system clock on pin `P10`, while the Verilog parameter `CLK_FREQ_HZ` is set to `50,000,000`—resulting in a `0.5 second` step interval per count.

---

## Pinout & Hardware Mapping

| Signal | Direction | FPGA Pin | I/O Standard | Board Component |
| :--- | :--- | :--- | :--- | :--- |
| `clk` | Input | `P10` | `LVCMOS33` | 100 MHz System Clock |
| `reset` | Input | `F4` | `LVCMOS33` | Slide Switch `SW0` |
| `led[0]` | Output | `F5` | `LVCMOS33` | LED 0 (LSB) |
| `led[1]` | Output | `G4` | `LVCMOS33` | LED 1 |
| `led[2]` | Output | `H3` | `LVCMOS33` | LED 2 |
| `led[3]` | Output | `J5` | `LVCMOS33` | LED 3 (MSB) |

---

## Vivado Synthesis & Implementation Results

* **Target Device:** `xc7a35tftg256-1` (Artix-7)
* **Synthesized/Implemented Cells:** 55 Cells, 6 I/O Ports, 85 Nets
* **Total On-Chip Power:** `0.188 W` (Junction Temp: `25.9°C`)

### RTL Elaborated Schematic
![RTL Schematic](images/rtl_schematic.jpg)

### Implemented Design Schematic
![Implemented Schematic](images/implemented_schematic.jpg)

### FPGA Device Floorplan
![Device Layout](images/device_layout.jpg)

---

## How to Run in Vivado
1. Clone this repository:
   ```bash
   git clone [https://github.com/](https://github.com/)<your-username>/4-bit-odd-counter-fpga.git
   ```
2. Open **Vivado** and create a new RTL project targeting **`xc7a35tftg256-1`**.
3. Add `src/odd_counter4bit.v` as a Design Source.
4. Add `constraints/odd_counter_constraints.xdc` as a Constraints File.
5. Run **Synthesis**, **Implementation**, and **Generate Bitstream**.
6. Open **Hardware Manager**, connect your FPGA board, and program the device with the generated `.bit` file.