# RISC-V SoC Tapeout Program - Week 2

## Overview
Welcome to Week 2 of the RISC-V SoC Tapeout Program! This week focuses on building a foundational understanding of System-on-Chip (SoC) design and hands-on functional modelling of the BabySoC using simulation tools. The program is divided into two parts: **Part 1 - Theory (Conceptual Understanding)** and **Part 2 - Labs (Hands-on Functional Modelling)**. By the end of Week 2, you should be able to explain the theory behind SoC design and demonstrate BabySoC functional modelling with simulation waveforms.

---

## Part 1 - Theory (Conceptual Understanding)

### Objective
To build a solid understanding of SoC fundamentals through conceptual learning.

### Theory Reference
- [Fundamentals of SoC Design Notes](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/11.%20Fundamentals%20of%20SoC%20Design)

### Focus Areas
- **What is a System-on-Chip (SoC)?**
  - An SoC is an integrated circuit that combines multiple components (e.g., CPU, memory, peripherals) into a single chip, enabling compact and efficient system design.
- **Components of a Typical SoC**
  - CPU (e.g., RISC-V core), memory (e.g., SRAM, DRAM), peripherals (e.g., UART, DAC), and interconnect (e.g., bus architecture).
- **Why BabySoC is a Simplified Model**
  - BabySoC is a minimalistic RISC-V-based SoC designed for educational purposes, simplifying complex SoC concepts while retaining core functionality (e.g., reset, clocking, dataflow).
- **Role of Functional Modelling**
  - Functional modelling (pre-RTL and physical design) validates the high-level behavior of the SoC, ensuring correctness before detailed implementation.

### Deliverable
- **Write-up**: A 1–2 page document summarizing your understanding of SoC design fundamentals and how BabySoC fits into this learning journey.
  - **File**: [soc_fundamentals_writeup.md](docs/part1.md)
  - **Supporting Images**: 
    - [479033_1_En_2_Fig10_HTML.png](docs/479033_1_En_2_Fig10_HTML.png)
    - [System-on-Chip-SoC-Wiki.jpg](docs/System-on-Chip-SoC-Wiki.jpg)
  - **Content**: Include key points from the focus areas, personal insights, and a brief explanation of BabySoC's role as a learning tool (e.g., its simplified architecture aids in mastering SoC design stages).

---

## Part 2 - Labs (Hands-on Functional Modelling)

### Objective
To practice functional modelling of the BabySoC using simulation tools (Icarus Verilog & GTKWave).

### Lab Reference
- [VSDBabySoC Project Labs](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/12.%20VSDBabySoC%20Project)

### Install and Use
- **Icarus Verilog (iverilog)**: For compiling Verilog code.
  - Installation (Ubuntu): `sudo apt-get install iverilog`
- **GTKWave**: For viewing simulation waveforms.
  - Installation (Ubuntu): `sudo apt-get install gtkwave`

### Steps
1. **Clone the BabySoC Project Repo**
   - Command: `git clone https://github.com/manili/VSDBabySoC.git`
   - Navigate: `cd VSDBabySoC`

2. **Compile the BabySoC Verilog Modules Using Iverilog**
   - Command: `make pre_synth_sim` (or adjust based on Makefile, e.g., `iverilog -o sim src/module/testbench.v src/module/vsdbabysoc.v src/module/rvmyth.v src/module/avsdpll.v src/module/avsddac.v src/module/clk_gate.v output/compiled_tlv/rvmyth_gen.v`)

3. **Simulate and Generate .vcd Waveform Files**
   - Command: Runs automatically with `make pre_synth_sim`, generating `output/pre_synth_sim/pre_synth_sim.vcd`.

4. **Open .vcd Files in GTKWave and Analyze**
   - Command: `gtkwave output/pre_synth_sim/pre_synth_sim.vcd`
   - **Analysis Areas**:
     - **Reset Operation**: Verify `reset` toggling (t=20–120 ns) holds `CPU_pc_a0` at 0x00000000 and initializes registers (e.g., `CPU_Xreg_value_a3[17]`).
     - **Clocking**: Check `CLK` stability (~12.5 ns period) from `REF` and `VCO_IN`, with `OUT` updates.
     - **Dataflow Between Modules**: Trace `CPU_pc_a0`, `CPU_instr_a1`, `CPU_result_a3`, `CPU_Xreg_value_a5[17]`, `RV_TO_DAC`, and `OUT` for instruction execution and data propagation.

5. **Document Your Observations with Screenshots of Waveforms**
   - Use GTKWave to capture and save images (e.g., via **File > Write Image**).

### Deliverable
- **Simulation Logs**
  - **Compilation and Initial Error Log**: [compilation_log.txt](Week2/part2/simulation/compilation_log.txt)
    - Content includes the `git clone` and initial `iverilog` error due to missing `baby_soc.v`.
  - **Pre-Synthesis Simulation Log**: [simulation_log.txt](Week2/part2/simulation/simulation_log.txt)
    - Content from `make pre_synth_sim`, showing SandPiper execution and `.vcd` file generation.
    - **Supporting Image**: [simulation_log.png](Week2/part2/simulation_log.png)
  - **VCD File**: [pre_synth_sim.vcd](Week2/part2/output/pre_synth_sim/pre_synth_sim.vcd)
    - Verify file presence and non-zero size.

- **GTKWave Screenshots Highlighting Correct BabySoC Behavior**
  - **Reset Operation Screenshot (t = 0–200 ns)**: [reset_behav.png](Week2/part2/reset_behav.png)
    - Show `reset`, `CPU_reset_a0`, `CPU_pc_a0`, and `CPU_Xreg_value_a3[17]` with PC held at 0 during reset.
  - **Clocking Screenshot (t = 0–600 ns)**: [clk_behav.png](Week2/part2/clk_behav.png)
    - Display `CLK`, `REF`, `VCO_IN`, `ENb_VCO`, and `OUT` showing stable clock generation.
  - **Dataflow Between Modules Screenshot (t = 100–600 ns)**: [data_flow.png](Week2/part2/data_flow.png)
    - Include `CPU_pc_a0`, `CPU_instr_a1`, `CPU_result_a3`, `CPU_Xreg_value_a5[17]`, `RV_TO_DAC`, and `OUT` for dataflow.
  - **Supporting Image**: [sim_output.png](Week2/part2/sim_output.png)

- **Short Explanation (per Screenshot) of What the Waveform Represents**
  - **Reset Operation (t = 0–200 ns)**: This waveform represents the reset phase, where `reset` is high (t=20–120 ns), holding `CPU_pc_a0` at 0x00000000 and initializing registers (e.g., `CPU_Xreg_value_a3[17]` to 17). Post-reset (t>120 ns), PC increments, confirming synchronous reset and clean execution start.
  - **Clocking (t = 0–600 ns)**: This waveform represents clock generation, showing `CLK` stabilizing (~12.5 ns period) from `REF` and `VCO_IN` toggles, with `OUT` updating on CLK edges post-reset, validating PLL-driven synchronization.
  - **Dataflow Between Modules (t = 100–600 ns)**: This waveform represents pipeline dataflow, with `CPU_pc_a0` incrementing, `CPU_instr_a1` fetching instructions (e.g., ADDI/BNE), `CPU_result_a3` computing ALU results, and data propagating to `RV_TO_DAC`/`OUT`, confirming core-to-DAC integration.

---

## Project Structure
- `docs/`: Theoretical write-up and images (e.g., `part1.md`, `479033_1_En_2_Fig10_HTML.png`, `System-on-Chip-SoC-Wiki.jpg`).
- `Week2/part2/src/module/`: Verilog source files (e.g., `testbench.v`, `vsdbabysoc.v`).
- `Week2/part2/output/compiled_tlv/`: Generated files (e.g., `rvmyth.v`, `rvmyth_gen.v`).
- `Week2/part2/simulation/`: Logs and `.vcd` files.
- `Week2/part2/`: GTKWave screenshots (e.g., `reset_behav.png`, `clk_behav.png`, `data_flow.png`, `sim_output.png`).

## References
- [Fundamentals of SoC Design](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/11.%20Fundamentals%20of%20SoC%20Design)
- [VSDBabySoC Project](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/12.%20VSDBabySoC%20Project)

## Additional Notes
- Ensure the simulation completes (look for `$finish` in logs) and `.vcd` file is valid.
- For issues, check file paths or rerun `make pre_synth_sim`.
- The `readme.markdown` files in `part1` and `part2` can be merged into this `README.md` or used as supplementary notes.


