# 1GSPS-ADC-Sampling-Board
1GSPS High-Speed ADC Sampling Board (HMCAD1511TR) | Hardware Design + FPGA Firmware Development + Performance Test Script | 2025 Undergraduate Innovation Practice Training Program Project, National Astronomical Observatories, Chinese Academy of Sciences

## Project Overview
This project is a **1GSps, 8-bit, 4-channel** high-speed ADC sampling system, consisting of a single 1GSPS ADC sampling board. It is designed specifically for high-speed RF & analog signal digital acquisition, and can be directly connected to the Alinx AXKU041 FPGA development board for data receiving and processing.

The system completes analog-to-digital conversion and high-speed transmission through a optimized signal link. The hardware design is completed based on JLC EDA, and the FPGA firmware is developed based on Vivado 2025.2. All hardware design files (schematic diagrams, PCBs, BOMs), FPGA firmware code, and performance test scripts are fully open-sourced for academic research and non-commercial educational purposes only.

## Physical Display
### Board Frontside Details
<img src="https://github.com/user-attachments/assets/60581344-ff36-46fd-b55c-9b065ce7c514" alt="1GSPS_ADC_Sampling_Board_Front" style="max-width:500px; height:auto;" />

### Board Backside Details
<img src="https://github.com/user-attachments/assets/203f4c6e-691e-4964-9652-ee5dd7d58e69" alt="1GSPS_ADC_Sampling_Board_Back" style="max-width:500px; height:auto;" />

## Project Background
This project is an approved project of the **2025 Undergraduate Innovation Practice Training Program of the National Astronomical Observatories, Chinese Academy of Sciences**, and is developed relying on the Pulsar and Gravitational Physics Research Group of the National Astronomical Observatories.

## Hardware Design
All hardware design files are stored in the `hardware/` directory, which contains only the 1GSPS ADC sampling board. All are drawn with **JLC EDA**, including complete schematic diagrams, PCB layouts, and Bill of Materials (BOM):

### 1. Core ADC Sampling Board (`hardware/1G_ADC_Sampling_Board/`)
#### Signal Acquisition Link (SMA Input → FMC Output)
`SMA signal source termination` → ESD protection (PESD5V0R1BSFYL) → 50Ω impedance matching  → Single-ended to differential conversion (TC1-1-13MX+) → Differential signal conditioning & common-mode biasing → ADC analog-to-digital conversion (HMCAD1511TR, 1GSps + 8bit + 4-channel) → FMC interface LVDS digital output (1.8V level)

#### Core Components
- RF ESD protection: PESD5V0R1BSFYL
- Single-ended to differential transformer: TC1-1-13MX+
- Analog-to-digital conversion chip: HMCAD1511TR (1GSPS, 8bit, 4-channel)
- Power management: LT1764AEQ-3.3, LT1963AES8-1.8

### 2. Hardware Connection Relationship
1GSPS ADC Sampling Board (FMC interface) ↔ AXKU041 FPGA Development Board (FMC interface), forming a complete high-speed signal acquisition link.

## Firmware Development
Firmware code and programming files are stored in the `firmware/` directory, which is the FPGA acquisition firmware adapted for AXKU041. It is developed entirely based on **Xilinx Vivado 2025.2**, including Vivado project source code, project configuration files, and directly programmable bitstream files.

The firmware supports ADC SPI configuration, LVDS DDR data receiving, 4-channel 8bit data reconstruction, clock synchronization and ILA debugging.

## Quick Start
### 1. Hardware Connection
Complete the hardware connection in the following order:
1. RF/analog signal source → Connect to the SMA input port of the 1GSPS ADC sampling board
2. External clock source → Connect to the clock SMA input port of the 1GSPS ADC sampling board
3. Plug the FMC port of the ADC sampling board directly into the FMC slot of the AXKU041 FPGA development board

### 2. Firmware Programming
Open Xilinx Vivado 2025.2 and program the bitstream file in the `firmware/` directory into the AXKU041 FPGA development board through the software.

### 3. Signal Acquisition and Data Capture
Power on all hardware. After the programming is completed, the AXKU041 FPGA will automatically start ADC acquisition and complete the digital conversion of high-speed analog signals. Then capture the acquired data through Vivado, the steps are as follows:
Open the ILA (Integrated Logic Analyzer) debugging interface of Vivado 2025.2, trigger the acquisition of the digital signal data after ADC sampling, and export the captured data to a **CSV format file**.

### 4. Circuit Board Performance Testing and Phenomenon Viewing
This project provides a Python automated analysis script (stored in the `test/` directory). The script can automatically parse the CSV data exported by ILA, visually present the test phenomena through **time-domain waveforms + frequency-domain spectrograms** to intuitively verify the validity of the sampled signal. The specific operation steps are as follows:
1. Put the CSV format sampling data file exported from the Vivado ILA interface into the `test/` directory of the repository;
2. Open the terminal/command line, enter the `test/` directory, and execute the Python script running command (the CSV file name needs to be passed as a parameter). The command format:
```bash
python 1G_test_plot_ila.py sampling_data.csv
```
3. After the script runs, it will automatically complete data parsing, **draw and pop up the time-domain waveform diagram and normalized frequency-domain spectrogram of each channel in real time**, and generate a PNG format visualization result file with the same name in the `test/` directory (CSV file name + .png). The integrity of the sampled signal can be intuitively analyzed through the waveform/spectrum to complete the circuit board performance verification.

## Repository Directory Structure
```
1GSPS High-Speed ADC Sampling Board/
├── images/          # Project images: hardware physical drawings, test-related drawings, etc.
├── hardware/        # Hardware design main directory
│   └── 1G_ADC_Sampling_Board/  # 1GSPS ADC sampling board: schematic diagrams, PCBs, BOMs
├── firmware/        # FPGA firmware: source code, Vivado project files, programmable bitstream files
├── test/            # Performance testing suite: core analysis script + categorized test data & generated plots
│   ├── 1_Single-Frequency_Test/   # Single-tone test (10MHz~1GHz @ -3dBm) for Channel 1-4
│   ├── 2_dB_Test/                 # Power level comparison test for multi-channel
│   ├── 3_Channel_Consistency_Test/ # Channel gain and phase consistency test
│   ├── 4_Frequency_Amplitude_Response/ # Wideband frequency sweep test
│   └── 1G_test_plot_ila.py        # Core analysis script: Parses CSV raw data → generates time/freq domain plots (PNG)
├── LICENSE          # Open-source license (MIT + academic use restrictions)
└── README.md        # Project description document (this document)
```

## Contact Information
If you have any questions, suggestions or want to communicate in depth about the project, please contact me via email:
siyuan.liu.research@outlook.com

## Acknowledgments
1. 2025 Undergraduate Innovation Practice Training Program, National Astronomical Observatories, Chinese Academy of Sciences
2. Pulsar and Gravitational Physics Research Group, National Astronomical Observatories, Chinese Academy of Sciences
