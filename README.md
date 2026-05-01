# PSU_USB Power Board
**Squadra Corse PoliTo — Formula Student (Season SC25)**

[![Hardware](https://img.shields.io/badge/Hardware-PCB_Design-blue)](#)
[![Power](https://img.shields.io/badge/Power-5V_USB--C-orange)](#)
[![Status](https://img.shields.io/badge/Status-Manufactured_&_Tested-brightgreen)](#)

## 🏎️ System Overview

This repository contains the hardware design for the PSU_USB board. The primary purpose of this board is to provide a highly stable 5V power supply to electronics boards across the vehicle system. 

Utilizing a standard USB-C connector, the board is designed for maximum utility and ease of use, allowing critical racing electronics to be powered up directly from any standard power supply or PC during bench testing and debugging. 

## ✨ Key Features & Hardware Specifications

* **Connector:** USB-C Female Connector (Molex / Wurth Elektronik).
* **Power Output:** Configured to deliver a stable 5V at up to 3A (15W).
* **PD Negotiation:** To optimize board space and reduce system complexity, a dedicated Power Delivery (PD) integrated circuit was intentionally omitted. Instead, two 5.1 kOhm pull-down resistors are tied to the CC1 and CC2 pins to strictly hardware-limit the connection to standard 5V operation.
* **Operating Temperature:** Designed to withstand standard automotive/racing environments from -40°C to 85°C.
* **Form Factor:** Extremely compact footprint measuring 27.94mm x 30.10mm.

## 🧠 Subsystem Engineering & Design Highlights

### 1. Multi-Stage Protection Circuits
To protect downstream electronics from bench-power anomalies, the board features a robust 3-stage protection strategy:
* **Reverse Current Protection:** An STPS2L40AF Zener diode with a low voltage drop is placed inline to prevent back-feeding into the source.
* **ESD Protection:** An SMAJ30CA-TR TVS diode provides fast, bidirectional repelling of voltage spikes and electrostatic discharge.
* **Over-Voltage & Over-Current:** Handled via a parallel 5.6V Zener diode (D3) paired with a fast-blow inline fuse (C1Q_1).

### 2. Dual-Stage Power Filtering
Stable power is critical for sensors and MCUs. The board utilizes two filter stages to condition the USB input:
* **LPF Filter:** A Low-Pass Filter (10uF capacitor and 10kOhm resistor) debounces the physical power switch (S1) and increases output stability, achieving a 1.6Hz cutoff frequency.
* **PI Filter:** A classic PI filter network (using a BLM31KN102SH1L ferrite bead) cleans high-frequency noise from the DC line. *Design Note: While standard USB filtering calls for a bulk electrolytic capacitor, packaging constraints forced a pivot to a Multi-Layer Ceramic Capacitor (MLCC). While slightly less performant, this trade-off was necessary to meet the strict spatial requirements of the board.*

### 3. Integrated Testability
For rapid debugging, the top layer includes dedicated, labeled test points: `GND`, `5V PRE` (to verify raw USB-C output), and `5V POST` (to verify the output after the filters and protection circuits).

## 📁 Repository Structure

```text
├── Gerbers/                  # Production files for manufacturing
├── BOM/                      # Bill of Materials
├── Schematics/               # PDF exports of the circuit design
├── Step/                     # 3D Model of the board for mechanical packaging
├── PSU_USB_v1.0_Report.pdf   # System and Subsystem Specification Document
└── README.md
