---
title: Product Requirements
---

# Product Requirements Document

**Project Name:** 2 DOF Smart Robot Arm Joint Showcase  
**Course:** RAS304 - Embedded Systems  
**Organization:** Arizona State University - Department of Robotics and Autonomous Systems  
**Team Members:** Elijah Koiki (Team Coordinator), Ayaan Ahmad (Project Planner), Connor Loos (Assignment Leader), Walker Knaggs (Project Monitor)  
**Date:** September 18, 2026  
**Version:** 1.0  

---

## 1. Introduction & Overview

Modern robotic platforms leverage modular, distributed architectures to isolate dynamic sensing and high-speed actuation into self-contained embedded nodes[cite: 1]. The 2 DOF Smart Robot Arm Joint Showcase is a modular robotic articulation subsystem designed to demonstrate precise 2-Degree-of-Freedom positional control (Pitch and Roll/Rotation), closed-loop strain gauge force feedback, active thermal safety monitoring, and inter-module bus coordination[cite: 1]. Built in strict accordance with RAS304 requirements, the project connects 4 individual, custom-designed Printed Circuit Boards (PCBs) via a standardized 8-wire ribbon cable bus[cite: 1].

---

## 2. Project Objectives & Stakeholders

### 2.1 Project Objectives

* **Modular Architectural Showcase:** Design, assemble, program, and verify a 4-node modular embedded robotic articulation system where each node functions autonomously with its own microcontroller and power management stage[cite: 1].
* **Standardized Inter-Module Communication:** Establish reliable real-time signal passing across a standardized 8-pin bus (5 Digital lines, 2 Analog lines, 1 Common Ground)[cite: 1].
* **Closed-Loop Actuation & Active Sensing:** Realize precision multi-axis joint movement integrated with active signal conditioning op-amp circuits for load/force detection and safety management[cite: 1].

### 2.2 Stakeholders

* **End-Users / Educational Demonstrators:** Robotics students and researchers evaluating distributed bus performance and multi-axis joint kinematics[cite: 1].
* **Development Team:** Elijah Koiki, Ayaan Ahmad, Connor Loos, and Walker Knaggs (responsible for individual PCB hardware design, firmware, integration, and testing)[cite: 1].
* **Course Teaching Team / Evaluators:** RAS304 instructors and TAs verifying adherence to component guidelines, electrical safety, and team deliverables[cite: 1].

---

## 3. Use Cases

### 3.1 Use Case 1: Automated Pick-and-Place Trajectory in an Educational Lab

* **Actor:** Student / Lab Demonstrator[cite: 1]
* **Setting:** University Robotics Laboratory[cite: 1]
* **Description:** The demonstrator selects a pre-programmed pick-and-place routine using controls on Node 1 (UI Node)[cite: 1]. Node 1 transmits control signals across the ribbon bus[cite: 1]. Node 2 (Pitch Axis) and Node 3 (Roll Axis) execute coordinated joint moves using feedback-controlled driver circuits[cite: 1]. As the arm picks up an object, Node 4 continuously amplifies and filters strain gauge load-cell signals to measure torque[cite: 1]. If payload limits or structural resistance exceed safety thresholds, a global digital fault signal is triggered across Bus Line 5 to halt all movement immediately[cite: 1].

### 3.2 Use Case 2: System Telemetry Monitoring & Emergency Fault Override

* **Actor:** Test Supervisor / Safety Inspector[cite: 1]
* **Setting:** Hardware-in-the-Loop Verification Bench[cite: 1]
* **Description:** An inspector connects a PC to Node 1 via UART/Serial interface to monitor real-time operational status, joint positions, power consumption, and thermal data[cite: 1]. If an operational anomaly (such as a motor stall or overheating condition) occurs on Node 2, the local MCU asserts a high-priority digital supervisor line[cite: 1]. Node 1 logs the fault and commands all motor channels to disengage within 20 milliseconds[cite: 1].

---

## 4. Subsystem Allocation & Modular Architecture

Each team member is responsible for designing, building, programming, and verifying one dedicated standalone PCB equipped with a Microchip PIC18F57Q43 Curiosity Nano Development Board, an independent 5V linear power regulator stage fed by a 9V barrel jack adapter, fuse protection, power status LEDs, and standard 2x4 IDC bus headers[cite: 1].

| Board / Node | Lead Owner | Hardware Functionality & Active Circuitry | Bus Communication Role |
| :--- | :--- | :--- | :--- |
| **Node 1: Bus Master & UI Supervisor** | Elijah Koiki | PIC18 MCU, 5V regulator, power fuse, status LEDs, user switches, and UART-to-PC debug interface. | Coordinates master state machine, monitors safety fault lines, and streams system telemetry. |
| **Node 2: Pitch Axis Actuation Node** | Ayaan Ahmad | PIC18 MCU, 5V regulator, MOSFET H-bridge motor driver with low-side current shunt and op-amp overload comparator. | Receives Pitch PWM/direction signals on Digital Pins 1 & 2; asserts fault signal on Digital Pin 5 if current exceeds limit. |
| **Node 3: Roll Axis Actuation Node** | Connor Loos | PIC18 MCU, 5V regulator, stepper/servo driver stage with active optical endstop/encoder debouncing conditioning. | Receives Roll step/dir timing on Digital Pins 3 & 4; outputs analog joint position telemetry on Analog Pin 6. |
| **Node 4: Joint Load & Thermal Node** | Walker Knaggs | PIC18 MCU, 5V regulator, load-cell strain gauge with custom 2-stage active op-amp instrumentation amplifier and NTC thermistor scaling circuit. | Transmits active load/force analog data across Analog Pin 7; triggers thermal fault override on Digital Pin 5. |

---

## 5. Detailed Design Aspects

### 5.1 Hardware / Product Design

* **Microcontroller Standard:** Microchip PIC18F57Q43 Curiosity Nano Development Board on every node[cite: 1].
* **Power Architecture:** Onboard 5V linear voltage regulator powered by an individual 9V barrel jack adapter, with fast-acting fuse protection and LED power indication[cite: 1]. No logic power is shared across the ribbon cable except common ground[cite: 1].
* **Interconnect Standards:** 2x4 male IDC headers on each board connecting to an 8-wire ribbon cable bus[cite: 1].
* **Component Constraints:** Strict adherence to permitted through-hole and active components[cite: 1]. Peripheral daughterboards are strictly prohibited[cite: 1].

### 5.2 Software / Functionality

* **Firmware:** Embedded C/C++ running on each PIC18F57Q43 for sensor acquisition, PWM motor generation, pin interrupts, and data routing[cite: 1].
* **Ribbon Bus Pinout Mapping:**
  * **Pin 1 (Digital I/O):** Pitch Direction / Control
  * **Pin 2 (Digital I/O):** Pitch Step / PWM Enable
  * **Pin 3 (Digital I/O):** Roll Direction / Control
  * **Pin 4 (Digital I/O):** Roll Step / PWM Enable
  * **Pin 5 (Digital I/O):** Global Fault / E-Stop Supervisor Line (Active Low)
  * **Pin 6 (Analog I/O):** Roll Joint Position Feedback
  * **Pin 7 (Analog I/O):** Strain Gauge / Joint Load Force Signal
  * **Pin 8 (Ground):** Common System Ground

### 5.3 Interactivity & User Experience

* **Status Indication:** Onboard visual LEDs for 5V power state, active drive state, bus activity, and fault flags[cite: 1].
* **User Controls:** Tactile pushbuttons/switches on Node 1 for home positioning, routine selection, and manual emergency stop[cite: 1].
* **Telemetry Output:** Live serial terminal output to PC via USB-UART displaying real-time joint positions, force measurements, and system health status[cite: 1].

### 5.4 Customization & Scalability

* **Modular Flexibility:** Modular 8-wire bus architecture allows replacing or upgrading individual sensing or actuation nodes without redesigning the core master controller layout[cite: 1].
* **Software Parameters:** Configurable EEPROM parameters for force-stop thresholds, motion speed curves, and thermal warning limits[cite: 1].

### 5.5 Manufacturing & PCB Guidelines

* **Board Design:** Each teammate designs a custom 2-layer PCB adhering to standard manufacturing rules (minimum trace width/spacing, clear silkscreen labels)[cite: 1].
* **Mounting:** Standard M3 mounting holes placed on all corners to allow secure mechanical installation onto the arm showcase frame[cite: 1].

### 5.6 Safety & Compliance

* **Electrical Safety:** Dedicated inline fuses on each 9V power entry line prevent electrical overcurrent damage[cite: 1].
* **Emergency Stop:** Dedicated hardware interrupt line (Pin 5) capable of disengaging motor drivers instantly upon error detection[cite: 1].
* **Thermal Limits:** Continuous active thermal monitoring on motor drivers and joint structures to guard against thermal runaway[cite: 1].

---

## 6. Requirement Criteria Specifications

| Req ID | Description | Requirement Specification / SMART Target Metric | Verification Method |
| :--- | :--- | :--- | :--- |
| **REQ-HW-01** | Power Regulation | Each board converts 9V DC input to regulated 5V ±0.2V logic power with an onboard linear regulator, status LED, and inline fuse. | **Test:** Measure voltage rails with DMM under full load; verify fuse open-circuit under overcurrent condition. |
| **REQ-HW-02** | Standard Interconnect | All PCBs incorporate a 2x4 male IDC header matching the team pinout with Pin 8 tied to common ground. | **Inspection:** Audit PCB schematics and layout files against team block diagram. |
| **REQ-HW-03** | Component Compliance | All circuits use discrete active/passive through-hole or IC components without using unapproved commercial daughterboards. | **Inspection:** Physical inspection of assembled boards against forbidden component guidelines. |
| **REQ-SW-01** | Multi-Axis Joint Motion | Pitch (Node 2) and Roll (Node 3) axes achieve coordinated motion over a minimum range of 90° per axis. | **Demonstration:** Execute 10 continuous automated pick-and-place cycles without step loss. |
| **REQ-SW-02** | Fault Reaction Time | System triggers global motor shutdown within <20 ms when an overcurrent or overtemperature fault is asserted on Bus Pin 5. | **Test / Analysis:** Measure time offset from fault trigger to motor signal cutoff on an oscilloscope. |
| **REQ-SN-01** | Active Signal Conditioning | Strain gauge instrumentation op-amp conditions differential load signal to 0-5V range for loads between 0-2.0 kg. | **Test:** Measure analog output on Pin 7 against reference weights (250g, 500g, 1000g, 2000g). |

---

## 7. Open Questions & Risk Management

1. **Open Question:** Will switching noise from motor drivers on Pins 1-4 induce signal interference on analog lines (Pins 6 & 7) along the ribbon cable?[cite: 1]  
   * **Mitigation:** Implement passive RC low-pass filtering and active op-amp filtering on analog inputs prior to MCU ADC channels[cite: 1].

2. **Open Question:** Will thermal dissipation on linear 5V regulators become excessive during continuous load states?[cite: 1]  
   * **Mitigation:** Perform thermal power calculations ($P = (V_{in} - V_{out}) \cdot I$) and include adequate PCB copper heat-sink fills if thermal operating points exceed 60°C[cite: 1].
