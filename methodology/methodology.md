#  Methodology for EMV Smartcard Side-Channel Project

We will follow a two-phase approach: **Phase 1 (POC with existing tools)** and **Phase 2 (Redesign & Miniaturization)**.  

---

## Phase 1: Proof of Concept (POC)

### 1. Platform Setup
- We will set up **ChipWhisperer (CW308 or CW1173) with the LEIA smartcard adapter** as our main testbed.  
- We will use the **CW505 H-field EM probe** for capturing electromagnetic signals.  
- As a backup, we will also test with a **generic USB PC/SC smartcard reader** while capturing EM signals using ChipWhisperer.  

### 2. Test Cards & Commands
- We will use real or test EMV smartcards.  
- We will focus on key EMV flows:  
  - `GENERATE AC` (INS=0xAE) – 3DES cryptogram generation.  
  - `READ RECORD` – Track 2 data parsing.  
  - `VERIFY` (INS=0x20) – RSA signature verification.  

### 3. Data Capture
- We will configure ChipWhisperer triggers to align on APDU command bytes.  
- We will capture **both power and EM traces** during each transaction.  
- We will store all datasets with metadata (APDU type, timestamps, trace length).  

### 4. Data Analysis
- We will analyze traces using Python/Jupyter notebooks.  
- We will perform:  
  - Trace alignment.  
  - Correlation analysis (CPA/DPA).  
  - Leakage modeling (Hamming weight/distance).  
- We will benchmark **signal-to-noise ratio (SNR)** and compare with known datasets like ASCAD and SPERO.  

### 5. Deliverables (Phase 1)
- EM trace datasets (3DES, Track 2, RSA).  
- Python scripts for alignment, correlation, leakage analysis.  
- A **POC report** confirming feasibility, leakage presence, and EMV compliance gaps.  

---

## Phase 2: Redesign & Miniaturization

### 1. Hardware Redesign
- We will design a **compact PCB board** with:  
  - FPGA or STM32 MCU integration.  
  - High-speed ADC (≥100 MS/s).  
  - Low-noise analog front-end.  
  - ISO7816 smartcard interface (CLK, I/O, VCC, GND, RST).  
  - Probe connector pads for modular EM probes.  
  - Options for SD card storage, USB, and Wi-Fi connectivity.  

### 2. Firmware & Software
- We will implement embedded firmware to:  
  - Handle smartcard APDUs (ATR, T=0/T=1 protocols).  
  - Trigger capture on APDU events.  
  - Log data to SD card or stream to host via USB/Wi-Fi.  
- We will build host-side Python tools for:  
  - Capture automation.  
  - EMV transaction scripting.  
  - Automated leakage analysis.  

### 3. Validation
- We will run the same EMV flows (`GENERATE AC`, `READ RECORD`, `VERIFY`) on the prototype.  
- We will compare trace quality and leakage detectability with our POC baseline.  
- We will confirm interoperability with EMV standards.  

### 4. Deliverables (Phase 2)
- Miniaturized PCB design files (schematic, layout, BOM).  
- Firmware and FPGA code for capture automation and EMV triggers.  
- Python SDK and Jupyter notebooks for analysis.  
- Prototype board for demonstration.  
- A **final validation report** comparing results against the baseline.  

---

##  Overall Flow
1. We will **start with open-source ChipWhisperer + LEIA** to quickly validate feasibility.  
2. We will **capture and analyze EMV transactions** to establish baseline datasets and scripts.  
3. We will **redesign custom hardware** inspired by ChipWhisperer, FOBOS, and RASCv3.  
4. We will **validate the new prototype** against the baseline for compliance and fidelity.  
5. We will **deliver a compact EMV security evaluation device** ready for lab or commercial use.  
