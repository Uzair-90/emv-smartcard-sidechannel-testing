# Open-Source EMV Smart-Card Side-Channel Testing Platforms

Several open platforms combine custom hardware and software for electromagnetic (EM) side-channel analysis of EMV smartcards (chip-and-PIN cards). These include general-purpose SCA testbeds (like ChipWhisperer) and specialized smart-card interfaces (like SAKURA-W or the LEIA board). Each solution’s hardware setup, software stack, supported ISO7816/EMV commands, and community use are summarized below. Relevant open datasets, scripts and documentation are noted where available.

---

## ChipWhisperer (with LEIA smartcard adapter)

![CW505 Probe](https://www.packetlabs.net/posts/chipwisperer-an-open-source-platform-for-side-channel-security-testing/)

The **ChipWhisperer** is an open-source hardware/software toolkit by NewAE for side-channel and fault-injection analysis  
([Packet Labs](https://www.packetlabs.net/posts/chipwisperer-an-open-source-platform-for-side-channel-security-testing/),  
[SSTIC’19 LEIA Paper](https://www.sstic.org/media/SSTIC2019/SSTIC-actes/LEIA_the_lab_embedded_iso7816_analyzer/SSTIC2019-Article-LEIA_the_lab_embedded_iso7816_analyzer-el-baze_renard_trebuchet_benadjila.pdf)).  

It includes capture devices (e.g. CW1173/CW1174 scopes up to 200 MS/s) and target boards (CW308, CW305, etc.) which can drive and measure microcontrollers or FPGA DUTs. EM coupling probes (e.g. the planar H-field CW505 probe and amplifier) are supported for non-invasive EM capture ([ChipWhisperer Docs](https://chipwhisperer.readthedocs.io/en/latest/getting-started.html)).

All ChipWhisperer schematics and firmware are open-source on GitHub, and the Python-based SDK provides APIs for collecting traces and running DPA/CPA analysis.

### Hardware
- CW308 USB capture/control board (ADC + trigger).
- CW117x oscilloscopes.
- Optional H-field probes (CW505 planar probe + CW502 LNA).
- OpenADC expansion for high-speed capture.
- MCU targets (STM32, XMEGA, etc.) and FPGAs.
- **LEIA adapter:** smartcard reader with STM32F439 MCU, ISO7816 socket, and power shunt.

### Software
- Python toolchain for capture and analysis.
- Full ISO7816 smartcard support via LEIA.
- LEIA firmware supports ATR parsing, T=0/T=1 protocols, extended APDUs.
- Arbitrary ISO7816 APDUs can be sent from host PC.

### EMV Commands
- Supports all ISO7816-4 commands (SELECT, READ RECORD, GPO, GENERATE AC, etc.).
- Tested with real cards (NXP, Gemalto, banking cards).
- Negotiates T=0 or T=1 protocols successfully.

### Documentation & Community
- Extensive docs and tutorials ([ChipWhisperer Docs](https://chipwhisperer.readthedocs.io/en/latest/getting-started.html)).
- LEIA design presented at [SSTIC 2019](https://www.sstic.org/media/SSTIC2019/SSTIC-actes/LEIA_the_lab_embedded_iso7816_analyzer/SSTIC2019-Article-LEIA_the_lab_embedded_iso7816_analyzer-el-baze_renard_trebuchet_benadjila.pdf).
- Widely used in academia and training.
- Kits range from ~$250 to $3800.

### Datasets & Scripts
- [ASCAD dataset](https://www.sstic.org/media/SSTIC2019/SSTIC-actes/LEIA_the_lab_embedded_iso7816_analyzer/SSTIC2019-Article-LEIA_the_lab_embedded_iso7816_analyzer-el-baze_renard_trebuchet_benadjila.pdf) (AES on smartcard).
- [ChipWhisperer Datasets Repo](https://github.com/newaetech/chipwhisperer-datasets) (AES traces, some EM measurements).
- Example notebooks and scripts available.

---

## SAKURA-G/W Platform

![Sakura-G Board](https://www.researchgate.net/figure/figure/SAKURA-G-board-with-an-OpenADC-mounted-and-the-Xilinx-USB-download-cable-b-ChipWhisperer_fig16_304459655/actions#embed)

The **SAKURA-G** platform (Troche/Meytang) is a dedicated SCA evaluation board with smart-card support.  
It contains two Spartan-6 FPGAs (crypto + controller), a programmable system clock (up to 48 MHz), and a high-bandwidth power amplifier (0–360 MHz, +20 dB) ([Fenix PDF](https://fenix.tecnico.ulisboa.pt/downloadFile/281870113703803/Resumo%20Alargado.pdf)).  

The companion **SAKURA-W** adapter provides an ISO7816 smartcard slot. Users typically attach an oscilloscope or OpenADC for acquisition. Board size is 140×120 mm; price ~€1.5k. Used widely in NIST DPA contests and academic SCA research.

### Hardware
- Crypto FPGA: user-programmable (crypto implementations).
- Controller FPGA: drives smartcard I/O.
- Onboard amplifier, clean power supply.
- SAKURA-W: ISO7816 smartcard adapter.

### Software/Commands
- No official open tools.
- Host PC issues APDUs via FPGA controller (USB).
- All EMV commands possible (SELECT, READ RECORD, GPO, etc.).
- Typically requires custom Verilog and/or PC/SC readers.

### Documentation & Community
- Proprietary hardware (Troche/Meytang).
- Schematics/code not public.
- Widely documented in academic papers (AES SCA, Trojan detection).
- Legacy platform, less used today compared to ChipWhisperer.

---

## FOBOS (Flexible Open-Source SCA Workbench)

The **FOBOS** framework (George Mason University) is a fully open-source bench for side-channel analysis and benchmarking ([CERG GMU](https://cryptography.gmu.edu/fobos/)).

It uses off-the-shelf FPGA boards and a custom **FOBOS Shield** providing ADC (100 MS/s) and current-sense monitors. Control is via PYNQ Zynq or similar, communicating with targets over GPIO/PCIe.

### Hardware
- Dev boards: Digilent Nexys, Xilinx PYNQ, CW305 Artix7, STM/TI launchpads.
- FOBOS Shield: 100 MS/s ADC, shunt sensors.
- Onboard clocking and trigger.

### Software & Analysis
- MATLAB/Octave + Python/Jupyter support.
- Automates trace capture and analysis.
- Supports SCA (SPA, DPA, CPA) and leakage tests (TVLA).
- GUI and notebooks for ease of use.

### EMV Commands
- No built-in smartcard reader.
- Focus on FPGA/MCU targets.
- Possible to add ISO7816 reader externally.

### Documentation & Use
- All design files, PCB, software are open.
- Used in NIST lightweight crypto evaluations.
- Publications: *FOBOS 3: An open-source platform for SCA and benchmarking* ([Springer](https://link.springer.com/article/10.1007/s13389-025-00368-6)).

---

## RASCv3 / SPERO (Dual-Channel EM+Power Platform)

The **RASCv3** project (Bai et al., UF) captures power and EM simultaneously.  
It uses a microcontroller board with ADC and a near-field EM antenna (PCB coil like a Langer probe). Demonstrated on AES, including masked implementations.  

The authors released the **SPERO dataset**: synchronized EM + power traces of AES (ASCAD format) ([Arxiv](https://arxiv.org/html/2405.06571v1)).

### Hardware
- Compact board (power supply + ADC + EM sensor).
- Printed PCB EM coil.
- Streams EM + power in real time.

### Software
- MCU firmware collects traces, streams to PC.
- Capture routines and example attacks (mutual-information CPA).
- Open PCB and firmware designs.

### EMV Commands
- No smartcard support.
- General embedded targets only.

### Community & Data
- Public release of SPERO dataset (AES EM+power).
- Encouraged for academic research.
- First public dataset with simultaneous EM and power traces.

---

## Other Resources and Testbeds
- **ChipWhisperer Extensions:** OpenADC, CW-Shuttle, external smartcard interposers.  
- **Educational Platforms:** SASEBO boards (Japan), not fully open.  
- **Datasets:** ASCAD, SPERO, TinyPower (AES, MCU traces). No public EMV dataset yet.  
- **Community:** Active GitHub repos, forums, SSTIC papers, CHES/ECB tutorials.  

---

## Sources
- [PacketLabs: ChipWhisperer Overview](https://www.packetlabs.net/posts/chipwisperer-an-open-source-platform-for-side-channel-security-testing/)  
- [SSTIC’19 LEIA Paper](https://www.sstic.org/media/SSTIC2019/SSTIC-actes/LEIA_the_lab_embedded_iso7816_analyzer/SSTIC2019-Article-LEIA_the_lab_embedded_iso7816_analyzer-el-baze_renard_trebuchet_benadjila.pdf)  
- [ChipWhisperer Docs](https://chipwhisperer.readthedocs.io/en/latest/getting-started.html)  
- [ChipWhisperer Datasets GitHub](https://github.com/newaetech/chipwhisperer-datasets)  
- [ResearchGate: Sakura-G Board](https://www.researchgate.net/figure/figure/SAKURA-G-board-with-an-OpenADC-mounted-and-the-Xilinx-USB-download-cable-b-ChipWhisperer_fig16_304459655/actions#embed)  
- [Fenix PDF](https://fenix.tecnico.ulisboa.pt/downloadFile/281870113703803/Resumo%20Alargado.pdf)  
- [NIST LWC Workshop Paper](https://csrc.nist.gov/CSRC/media/Events/lightweight-cryptography-workshop-2019/documents/papers/an-open-source-platform-for-evaluatiing-side-channel-lwc2019.pdf)  
- [Springer FOBOS 3](https://link.springer.com/article/10.1007/s13389-025-00368-6)  
- [FOBOS Project](https://cryptography.gmu.edu/fobos/)  
- [SPERO Dataset Arxiv](https://arxiv.org/html/2405.06571v1)  
