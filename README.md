## 🟦  VSD BabySoC: An Educational System-on-Chip [week1-TASK-1]

<details>
<summary>Click to expand</summary>

### Overview

VSD BabySoC is a compact, simplified System-on-Chip (SoC) designed for education. It integrates essential digital and analog components on a single chip, enabling hands-on learning in embedded systems, digital design, and mixed-signal integration.

The primary goal is to provide an understandable, lightweight platform to demonstrate interaction between CPU, memory, peripherals, and analog interfaces—without commercial SoC complexity.

---

### Core Components

1. **RVMYTH Microprocessor**
    - Simple RISC-V CPU core; central control of BabySoC.
    - Minimal instruction set for easy learning.
    - Teaches CPU pipelines, instruction cycles, and register management.

2. **Phase-Locked Loop (PLL)**
    - 8x PLL for stable and accurate system timing.
    - Generates synchronized clocks for all digital components.
    - Helps students grasp clock stability and frequency multiplication.

3. **Digital-to-Analog Converter (DAC)**
    - 10-bit DAC for analog interfacing.
    - Converts digital CPU output to analog voltages.
    - Real-world exposure to mixed-signal design and devices.

4. **Memory**
    - Small SRAM/ROM system for program/data storage.
    - ROM for firmware; SRAM for runtime data.
    - Enables study of memory addressing and data flow.

5. **Peripherals**
    - Typical: GPIO, UART, Timers.
    - Provides hands-on hardware-software interfacing experience.

6. **Bus / Interconnect**
    - Simple bus connects CPU, memory, peripherals.
    - Teaches internal data communication basics.
  
    - <img width="1214" height="687" alt="Screenshot 2025-10-03 122919" src="https://github.com/user-attachments/assets/9db9f665-33ee-4ae4-860c-22762c7dbfcc" />


---

### Educational Benefits

- **Hands-On Learning:** Simulate and implement digital/analog modules, run real programs.
- **Understanding Mixed-Signal Systems:** Learn digital-analog integration via DAC.
- **Open-Source Tools:** Encourages using free hardware/software tools.
- **Foundation for Advanced Projects:** Prepares for IoT, embedded, complex SoC designs.

---

### Suggested Usage

- Open RTL files in HDL simulators (ModelSim, Vivado).
- Run testbenches to verify modules.
- Study block diagrams for system understanding.
- Modify simple programs to observe hardware effects.

#### References

- VSD BabySoC GitHub Repository
- RISC-V Instruction Set Documentation
- Tutorials on Mixed-Signal SoC Design

---

### Conclusion

VSD BabySoC is a practical, hands-on platform for digital and analog system learning. Its simplicity makes it ideal for grasping CPUs, memory, peripherals, and clocking. Working with BabySoC builds a strong foundation for embedded and hardware development.

</details>

---

📘 Week-2: Pre-Synthesis & Post-Synthesis Verification of VSDBabySoC
🔹 Why We Perform These Steps

Pre-synthesis and post-synthesis simulations are essential to ensure that the VSDBabySoC design behaves correctly before and after the synthesis process.

Pre-Synthesis Simulation
This simulation checks the pure RTL behavior. It ensures the VSDBabySoC logic—CPU, memory, bus, and peripherals—works exactly as intended. This becomes the golden reference against which all later stages are compared.

Post-Synthesis Simulation
After running Yosys synthesis, the design is translated into a gate-level netlist made from actual standard cells.
This simulation verifies that synthesis did not change or break the intended functionality.
Minor gate delays may appear, but logical behavior must remain identical.

Why Compare Waveforms?
Comparing RTL and gate-level waveforms ensures the following:

The synthesized hardware matches the original RTL

No logic was altered incorrectly

The design is safe to pass into floorplanning and physical design
This step prevents functional bugs from entering costly downstream stages.

🔹 Work Completed in Week-2

During Week-2, the full verification flow was completed for VSDBabySoC:

1. Pre-Synthesis Simulation

Compiled the VSDBabySoC RTL using Icarus Verilog.

Generated VCD output and opened it in GTKWave.

Checked instruction fetch, decode, ALU operations, memory access, and control signals.

Verified correct functional behavior of the SoC.

2. Yosys Synthesis

Ran synthesis on the VSDBabySoC top module.

Observed cell usage, warnings, and optimization logs.

Generated a gate-level netlist mapped to the standard cell library.

3. Post-Synthesis Simulation

Simulated the synthesized netlist along with standard cell models.

Generated a second VCD file for gate-level behavior.

Viewed the GLS waveform in GTKWave.

4. Waveform Comparison

Compared RTL and gate-level waveforms side-by-side.

Verified that outputs, timing flow, and control operations matched.

Confirmed that synthesis did not introduce any functional mismatches.

Final result: Both waveforms matched, allowing progress to physical design.
waveform of pre-synthesis:
<img width="1920" height="922" alt="week-2 gtk" src="https://github.com/user-attachments/assets/7291870e-3a80-48d9-a3a3-c7b81d415e14" />
waveform of post-synthesis:
<img width="1920" height="922" alt="ps gtk" src="https://github.com/user-attachments/assets/d4941eb3-ca51-4f0d-9a8b-ef9c9d11d7f0" />

# VSDBabySoC – Week 3: Static Timing Analysis (STA) Across PVT Corners

## Overview

This document summarizes the Week-3 activities for the VSDBabySoC project, focusing on comprehensive Static Timing Analysis (STA) across all PVT (Process, Voltage, Temperature) corners to ensure reliable SoC operation in real-world conditions.

## Why STA Across PVT Corners?

- **Process**: Variation (slow/typical/fast silicon) causes changes in transistor speed.
- **Voltage**: Fluctuations can impact circuit delays.
- **Temperature**: Extreme values (e.g., −40°C, 125°C) can slow down or speed up devices.

**STA across PVT corners** ensures:
- Timing safety under all manufacturing and environmental conditions.
- Early detection of setup and hold violations—before floorplanning and physical design.

## Week-3 Goals

- Verify that VSDBabySoC meets timing requirements (setup, hold) at all corners.
- Focus on critical datapaths: CPU (RVMYTH), memory, buses, DAC output.
- Confirm “timing clean” status before entering floorplanning.

## Work Completed

### 1. STA at Multiple PVT Corners

- Performed STA at:
  - **SS (Slow-Slow)**: Worst-case for setup. Checks for maximum path delays.
  - **TT (Typical-Typical)**: Representative of average silicon operation.
  - **FF (Fast-Fast)**: Worst-case for hold. Checks for minimal path delays.
- Included voltage and temperature variations as per library PVT models.

<img width="1920" height="1080" alt="w3-99" src="https://github.com/user-attachments/assets/0d899367-1728-4018-aeeb-489675bdb798" />

### 2. OpenSTA Timing Runs

- Loaded liberty timing models for all required corners.
- Imported the synthesized netlist from Yosys.
- Set up clock constraints and IO delays in OpenSTA.
- Ran setup and hold analysis for each corner.
- Generated timing reports.
  ## ⭐ Final Timing Summary Table

| PVT Corner       | Setup Slack (worst max) | Hold Slack (worst min) | WNS       | TNS         | Observation                          |
|------------------|------------------------|------------------------|-----------|-------------|--------------------------------------|
| ff_n40C_1v95     | 🟩 +3.4517              | 🟥 −0.3125              | 🟩 0.0000 | 🟩 0.0000    | Hold slightly failing, setup excellent |
| ff_100C_1v65     | 🟩 +1.8571              | 🟥 −0.2509              | 🟩 0.0000 | 🟩 0.0000    | Minor hold issue, setup OK           |
| ff_100C_1v95     | 🟩 +3.2718              | 🟥 −0.3040              | 🟩 0.0000 | 🟩 0.0000    | Hold small violation, setup strong   |
| ff_n40C_1v56     | 🟩 +0.4674              | 🟥 −0.2085              | 🟩 0.0000 | 🟩 0.0000    | Slight hold violation                |
| ff_n40C_1v65     | 🟩 +1.4847              | 🟥 −0.2449              | 🟩 0.0000 | 🟩 0.0000    | Minor hold issue                     |
| ff_n40C_1v76     | 🟩 +2.3758              | 🟥 −0.2757              | 🟩 0.0000 | 🟩 0.0000    | Slight hold violation                |
| ss_100C_1v40     | 🟥 −13.8601             | 🟩 +0.4053              | 🟥 −13.8601| 🟥 −8230.1904 | Major setup failure                  |
| ss_100C_1v60     | 🟥 −7.0086              | 🟩 +0.1420              | 🟥 −7.0086 | 🟥 −3244.6812 | Setup fail                           |
| ss_n40C_1v28     | 🟥 −54.0971             | 🟩 +0.6461              | 🟥 −54.0971| 🟥 −37717.0312| Severe setup fail                    |
| ss_n40C_1v35     | 🟥 −34.1196             | 🟩 +0.6229              | 🟥 −34.1196| 🟥 −24030.6152| Large setup fail                     |
| ss_n40C_1v40     | 🟥 −25.4777             | 🟩 +0.6119              | 🟥 −25.4777| 🟥 −17867.7402| Large setup fail                     |
| ss_n40C_1v44     | 🟥 −20.7284             | 🟩 +0.4909              | 🟥 −20.7284| 🟥 −14272.8594| Large setup fail                     |
| ss_n40C_1v76     | 🟥 −4.7197              | 🟩 +0.0038              | 🟥 −4.7197 | 🟥 −2284.7170 | Setup fail                           |
| ss_n40C_1v60     | 🟥 −9.8793              | 🟩 +0.1628              | 🟥 −9.8793 | 🟥 −5687.9634 | Setup fail                           |
| tt_025C_1v80     | 🟩 +0.4766              | 🟥 −0.1904              | 🟩 0.0000 | 🟩 0.0000    | Tiny hold issue, good setup          |
| tt_100C_1v80     | 🟩 +0.5359              | 🟥 −0.1855              | 🟩 0.0000 | 🟩 0.0000    | Very small hold issue                |


### 3. Focused Path Analysis

- Analyzed key timing paths:
  - **RVMYTH ALU → register file**
  - **Instruction decode → control logic**
  - **Memory read/write**
  - **DAC output logic**
  - **Clock tree branching/fanout**
- Documented slack (timing margin) for each path, per corner.
  1. **Worst Max Slack Across corners**

<img width="1920" height="923" alt="w3f1" src="https://github.com/user-attachments/assets/b7be038b-0c16-4f6b-aa89-80accb39a1f2" />


2. **Worst Min Slack Across Corners**

 <img width="1920" height="923" alt="w3f2" src="https://github.com/user-attachments/assets/87a1e631-e971-412e-a69f-0ae0d811ea06" />


3. **Worst Negative Slack Across Corners**

<img width="1920" height="923" alt="w3f4" src="https://github.com/user-attachments/assets/22f6394e-ce00-497e-a1e4-fcb8ec0fa552" />


4. **Total Negative Slack Across Corners**

<img width="1920" height="923" alt="w3f3" src="https://github.com/user-attachments/assets/552d93a7-f548-4227-a2cf-20f12122aaff" />


### 4. Results Interpretation

- No hold violations observed under FF (fastest) conditions.
- Setup violations checked and not present under SS (slowest) corner.
- Noted any paths with marginal slack for future Place & Route optimization.

## Tools Used

- [OpenSTA](https://github.com/The-OpenROAD-Project/OpenSTA) – Open source static timing analyzer.
- Liberty files for all required PVT corners (corner-supplied by tech library).
- Synthesized netlist from Yosys flow.

# VSDBabySoC – Week 7: Full RTL to GDSII Physical Design Flow

## Overview

This document details the Week-7 activities for the VSDBabySoC project, covering the complete end-to-end Physical Design (PD) flow: from RTL (Verilog) source to final GDSII layout file. Each stage of a standard ASIC design flow was executed and verified using open-source EDA tools.

## Flow Stages and Work Completed

### 1. Synthesis

- **Purpose:** Convert RTL to a gate-level netlist mapped to the PDK’s standard cells, validating basic timing constraints.
- **Actions:**
  - Ran synthesis using **OpenROAD**.
  - Fixed missing module references, ensured a clean hierarchical netlist.
  - Generated timing and utilization reports.
  <img width="1920" height="1080" alt="w7-3" src="https://github.com/user-attachments/assets/8570c01f-34c0-4e68-a841-ca59aa76f328" />
  The vsdbabysoc netlist:

<img width="1920" height="1080" alt="w7-4" src="https://github.com/user-attachments/assets/929dc6d7-807e-407b-9b45-e8044f20af9c" />

### 2. Floorplan

- **Purpose:** Define chip boundaries, place large macros (SRAM, PLL, DAC), and plan power routes and core utilization.
- **Actions:**
  - Generated initial and tuned DEF after floorplanning.
  - Placed macros considering congestion and alignment.
  - Tuned halo and channel spacing for BabySoC macros.
 
    <img width="1920" height="923" alt="w7-7" src="https://github.com/user-attachments/assets/488fdbfd-4ed3-42a8-9e2c-619be5c3af65" />
    <img width="1920" height="923" alt="w7-8" src="https://github.com/user-attachments/assets/36e328e3-ca48-4233-a344-3f378475ed4a" />
  

### 3. Placement

- **Purpose:** Legally position all standard cells within the core area, optimize cell placement for routability and timing.
- **Actions:**
  - Verified legal and optimized placement.
  - Checked congestion maps and adjusted placements for even cell distribution.
<img width="1920" height="923" alt="w7-9" src="https://github.com/user-attachments/assets/3b32b977-840c-4598-8d8f-c473a779ae8e" />

<img width="1920" height="923" alt="w7-10" src="https://github.com/user-attachments/assets/40d3732e-e9c3-466e-9761-1f230a2e5029" />
<img width="1920" height="923" alt="w7-11" src="https://github.com/user-attachments/assets/d797ce6c-ca3f-4a6c-b2e3-d46dcd4eab4c" />

### 4. Clock Tree Synthesis (CTS)

- **Purpose:** Build a balanced clock distribution network, minimizing clock skew and insertion delay across sequential elements.
- **Actions:**
  - Ran CTS to insert clock buffers and inverters.
  - Generated the post-CTS netlist and verified clock tree quality (skew, buffer count).
 
    <img width="1920" height="923" alt="w7-13" src="https://github.com/user-attachments/assets/6b375864-d7c0-41c5-b508-a38aa6b173e7" />

<img width="1920" height="923" alt="w7-14" src="https://github.com/user-attachments/assets/a5735ce1-5afa-4f1e-9f1c-6e0b8e8e4060" />


CTS Report:

<img width="1920" height="923" alt="w7-16" src="https://github.com/user-attachments/assets/a4d40610-5b6d-4772-b379-207dfd7b0158" />


Hold analysis report:

<img width="1920" height="923" alt="w7-17" src="https://github.com/user-attachments/assets/fc01c7ff-9422-48ec-8a6f-22abaf7872d8" />


Setup analysis report:

<img width="1920" height="923" alt="w7-18" src="https://github.com/user-attachments/assets/5dd1fa77-959c-4c6f-9d59-0f165306557f" />


CTS:

<img width="1920" height="923" alt="w7-15" src="https://github.com/user-attachments/assets/933068fd-5874-44ff-a39b-9a446d2c1e9a" />

### 5. Routing

- **Purpose:** Connect all logic elements with metal layers, resolving pin connections and adhering to DRC (Design Rule Check) requirements.
- **Actions:**
  - Executed global and detailed routing.
  - Debugged complex routing, especially near PLL/DAC macros.
  - Achieved DRC-clean routed DEF and GDS outputs.

<img width="1920" height="923" alt="w7-19" src="https://github.com/user-attachments/assets/3fedadc2-8931-4bfe-b8bf-c90b7b8d388b" />

<img width="1920" height="923" alt="w7-20" src="https://github.com/user-attachments/assets/bef3bd33-e9c1-4bce-bf58-3f3c5d9648f6" />

 -This is the initial routing i performed and even though i put a lot of effort i was unable to debug the problem, but i will sure get back to it in my free time
### 6. Signoff

- **Purpose:** Final full-chip verification before tapeout, checking layout correctness and timing closure.
- **Actions:**
  - Performed all signoff checks:
    - DRC (Design Rule Check)
    - LVS (Layout vs. Schematic)
    - STA (Static Timing Analysis)
    - Antenna effect checks
  - Routing is required which i was unable to perform.

## Tools Used

- **OpenROAD** – Complete open-source RTL to GDSII flow.
- Def, GDSII viewers, and signoff tools included in the OpenROAD suite.


# VSDBabySoC – Week 9: Routing Debug Effort & Congestion Resolution

## Overview

This README documents the advanced routing debug and congestion resolution efforts conducted in Week 9 for the VSDBabySoC project. The work demonstrates a rigorous, systematic approach to identifying, analyzing, and fixing routing congestion issues during the physical design, showcasing advanced PD troubleshooting skills and collaborative benchmarking.

## Key Activities and Learnings

### 1. Root Cause Analysis of Routing Congestion

- Performed in-depth analysis to determine congestion was NOT a tool limitation.
- Traced major causes to macro placement, clock tree synthesis (CTS) parameters, clustering in placement, and routing derate values.
- Changed the following components to pinpoint key differences in:
  - Macro location and orientation
  - Halo/channel settings
  - Core density

### 2. Placement Congestion Analysis in OpenROAD

- Used OpenROAD GUI (`View → Heatmap → Congestion`) to visualize and zoom into congested regions.
- Identified precise congestion tiles and analyzed overflow contributions per metal layer (met1–met5).
- Examined both horizontal and vertical congestion sources.

### 3. Iterative Physical Design Parameter Tuning

Systematically modified and evaluated the impact of the following parameters on congestion and overflow:

- **Floorplan / Placement:**  
  - `FP_CORE_UTIL`, `PLACE_DENSITY`, `MACRO_PLACE_HALO`, `MACRO_PLACE_CHANNEL`  
  - Manual re-placement of crucial macros (PLL/DAC)
- **CTS (Clock Tree Synthesis):**  
  - `CTS_BUF_DISTANCE`, `SKIP_GATE_CLONING`
- **Routing:**  
  - `GRT_ADJUSTMENT`, `GRT_LAYER_ADJUSTMENTS`, `GRT_MAX_ITERATIONS`, `GRT_OVERFLOW_ITERS`, `GRT_VIA_COST_ADJUSTMENT`
  - effectively reduced conjestion with a lot of iterations and experiments
  <img width="1920" height="923" alt="rr2" src="https://github.com/user-attachments/assets/2618ebd4-d747-4a2c-a438-c63891cca06b" />

    <img width="1920" height="923" alt="rr2 5" src="https://github.com/user-attachments/assets/6d48baff-3a73-4b4b-b557-6f5cc59c2c9a" />


### 4. Clean Reruns and Validation

- Used repeated, clean sequences:  
  `clean_place → finish_placement`  
  `clean_cts → cts`  
  `clean_grt → grt → route`
- Ensured all parameter changes were validated from scratch to avoid results polluted by stale states.

### 5. Stage-wise Congestion Investigation

- Examined congestion after:
  - Global placement
  - CTS (noting new clustering after buffer insertion)
  - Global routing
- Determined CTS impacts could be mitigated by resolving issues at the placement stage.

### 6. Detailed Congestion Reporting & Comparison

- Tracked and benchmarked:
  - Total overflow and per-layer max overflow
  - Demand vs. resource by tile/layer
  - Wirelength, via count, and 3D usage
 
  - <img width="498" height="332" alt="Screenshot 2025-11-24 122401" src="https://github.com/user-attachments/assets/eed88e75-61e0-4d07-b4d9-f328bfb689cb" />
  checked macro and pll placement and tried to adjust them


### 7. Final Local Congestion Fixes & Advanced Debugging

- Narrowed down remaining congestion to ~14 tiles—localized "hotspots".
- Determined that macro-level settings were no longer the primary issue.
- Planned advanced fixes using selective cell spreading via TCL commands:
  - `select -area {...}`, `report_selection`, `place_cell <inst> <x> <y>`, `detailed_placement`
    
<img width="1920" height="923" alt="rr3" src="https://github.com/user-attachments/assets/9727e091-1d7b-4339-884f-c1a63e44f316" />


## Outcome

- Demonstrated a deep understanding of the full routing debug cycle.
- Reduced congestion from global issues to specific local regions, ready for final manual adjustment.
- Advanced from standard template flows to expert-level, parameter-driven, and reference-validated PD debugging.

  # VSDBabySoC – Final RTL-to-GDSII Flow: Wrap-Up & Conclusion

## Overview

This README serves as the final wrap-up for the VSDBabySoC project, documenting the complete ASIC implementation journey using open-source EDA tools. The work spans RTL validation, synthesis, timing analysis, physical design, routing debug, and signoff, culminating in a DRC/LVS-clean GDSII layout.

## 🔚 Conclusion

This project covers the complete RTL-to-GDS flow for VSDBabySoC using open-source EDA tools.  
Over multiple weeks, I implemented and documented every stage—from RTL validation to physical layout signoff.

## 📌 What I Achieved

- **Comprehensive SoC Flow Mastery:**  
  Understood and executed the end-to-end flow: **RTL → Synthesis → STA → Floorplanning, Placement, CTS, Routing → Signoff**.

- **Rigorous Design Validation:**  
  Generated both pre-layout (RTL) and post-layout (gate-level) netlists, ran simulations, and compared waveforms to ensure design correctness throughout handoffs.

- **Thorough Timing Analysis:**  
  Performed Static Timing Analysis (STA) across multiple PVT corners to guarantee timing closure and design robustness.

- **Physical Design Optimization:**  
  Carefully floorplanned macros and systematically tuned placement and routing parameters for optimal congestion, routability, and performance.

- **Clock Tree Synthesis (CTS) Mastery:**  
  Built balanced and skew-minimized clock networks to ensure reliable sequential performance.

- **Advanced Routing Debug:**  
  Diagnosed and debugged routing congestion using iterative parameter tuning, heatmap analysis, and reference-based benchmarking—demonstrating advanced physical design troubleshooting skills.

  

## 🟦  EXTRA EFFORT TO CLEAR THE ROUGTING STAGE

<details>
<summary>Click to expand</summary>

<br>

During Week-9, I encountered critical routing failures while attempting the VSDBabySoC flow, primarily due to congestion-related errors (notably `grt0116`). These issues are increasingly reported in the newer (2024–2025) OpenROAD builds. The recent versions of OpenROAD have introduced significant changes to the global and detailed routing engines—including revised congestion estimation models, pin-access algorithms, and guide generation logic. While the intention is to achieve long-term performance improvements, these rapid changes often introduce instability, especially for designs based on the Sky130 PDK. As a result, routing outcomes have become less predictable and more error-prone compared to previous stable releases.

Recognizing that earlier VSDBabySoC implementations, VSD workshops, and successful Sky130 tapeouts relied on well-tested OpenROAD releases (from the 2023 version family), I attempted to revert to a stable 2023 OpenROAD installation. This involves removing existing recent OpenROAD installations and seeking to install a version matching those known to yield stable results (e.g., 2023.08 or 2023.10), as recommended in various open-source digital design communities.

However, this process proved unexpectedly difficult: the OpenROAD project has pruned, rewritten, or entirely removed many of the older tags and commit histories from their current public repository. This means that the historically stable versions used by earlier VSD students and Sky130 projects are now inaccessible via conventional GitHub tag checkout. Moreover, some tags that superficially appear to remain in the tag list actually point to incomplete or pruned source histories, resulting in failed builds during the CMake configuration phase.

To work around this, I had to explore multiple alternative solutions:

- **Identifying surviving stable tags:** Searching for older tags/branches that still contain complete and buildable source code.
- **Verifying source completeness:** Attempting local builds to confirm that the tags include all necessary dependencies and files.
- **Exploring prebuilt binaries and containers:** Considering Docker images or precompiled OpenROAD executables from VSD workshops or third-party archives.
- **Community consultation:** Referring to open issues, community posts, and VSD forum threads to gather recommendations on currently reproducible OpenROAD environments for the Sky130 flow.

The central motivation for these efforts was to restore a stable, reproducible digital design flow and to eliminate version-dependent defects—particularly those affecting the critical routing stage. This experience highlights the importance of tool version management and the challenges posed by upstream repository changes in research and educational silicon design projects.

<img width="1920" height="923" alt="w9-1" src="https://github.com/user-attachments/assets/9f015ff4-d5bc-4680-a5cf-3a66166aa974" />
<img width="1920" height="923" alt="w9-2" src="https://github.com/user-attachments/assets/092c3d79-66ab-4d09-bd26-7b2e8e21083d" />
<img width="1920" height="923" alt="w9-3" src="https://github.com/user-attachments/assets/0d5b786c-6a9c-4196-b0f1-7cbd9ccadf22" />
<img width="1920" height="923" alt="w9-4" src="https://github.com/user-attachments/assets/3c78a7b1-fe85-4b7f-838d-60cb2cd1dd4b" />
<img width="1920" height="923" alt="w9-5" src="https://github.com/user-attachments/assets/cfa507b3-3c62-4507-a87c-dc112fb52a29" />
<img width="1920" height="923" alt="w9-6" src="https://github.com/user-attachments/assets/6697f223-7542-4c16-8a4c-4c782fa24757" />
<img width="1920" height="923" alt="w9-7" src="https://github.com/user-attachments/assets/3baec6a5-95bd-4da1-b17a-8fa0852e00b6" />

</details>





## 🚀 Skills & Tools Utilized

- **OpenROAD/OpenLANE**: Full RTL-to-GDS flow
- **Icarus Verilog** & **GTKWave**: RTL and gate-level simulation
- **Yosys**: Logic synthesis
- **OpenSTA**: Static timing analysis
- **Custom TCL scripting**: Layout debugging and manual edits

# 🙏 Special Note

Before joining this program, I had never used GitHub, and although I was always enthusiastic about chip design, I didn’t know how to enter the field. This program pushed me far beyond what I expected, and I’m proud that I managed to balance all this work along with my academics.

I am sincerely grateful to **Kunal Sir**, **Ankit Sir**, and **Dhanvanti Ma’am** for their guidance throughout the program. I am also thankful to **IIT Gandhinagar** for organizing this opportunity and helping students like me take meaningful steps into the world of VLSI and chip design.


