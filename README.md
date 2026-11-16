# CAN-Based ECU for Virtual Cockpit Control

## Project Overview

This project focuses on developing a CAN-based Electronic Control Unit (ECU) using Embedded C. The ECU receives CAN messages, processes input signals, and transmits vehicle-related data for controlling a Virtual Cockpit interface.

The implementation demonstrates automotive communication, peripheral register control, timer-based operations, and basic ECU state management.

## Project Objectives

- Implement CAN message transmission and reception.
- Handle CAN message filtering and communication.
- Read analog input from a potentiometer (ADC).
- Process vehicle-related signals such as speed and RPM.
- Control indicator lights using GPIO inputs and outputs.
- Manage ECU operating states using a state machine.
- Support interaction with a Virtual Cockpit interface.

## Key Features

### 1. CAN Communication
- CAN message transmission and reception.
- CAN message handling using message buffers.
- CAN receive callback implementation.
- Engine ON/OFF state control based on received CAN identifiers.

### 2. Vehicle Speed Processing
- Read potentiometer input.
- Convert ADC input into a speed value.
- Map the input range (0–4095) to a speed range (0–240 km/h).

### 3. RPM Processing
- Generate changing RPM values.
- Transmit RPM data using CAN message buffers.
- Split RPM data into low and high bytes.

### 4. Indicator and Light Control
- Read switch inputs through GPIO.
- Process left and right indicator signals.
- Control indicator LEDs.
- Transmit light status through CAN.

### 5. Timer-Based Operations
- Use Periodic Interrupt Timer (PIT) channels.
- Trigger periodic CAN transmission.
- Manage indicator blinking through timer callbacks.

## Software Architecture

The application is organized into the following functional areas:

| Module | Responsibility |
|---|---|
| `main.c` | Main application, ECU state machine, CAN data processing |
| `can.c` / `can.h` | CAN communication functions |
| `adc.c` / `adc.h` | Analog input handling |
| `pit.c` / `pit.h` | Periodic timer configuration and callbacks |
| `siu.c` / `siu.h` | System Integration Unit and GPIO operations |
| `init.c` / `init.h` | Peripheral initialization |

## CAN Communication Logic

### Received Messages

The current implementation checks the following CAN Standard Identifiers (SID):

| CAN ID | Function |
|---|---|
| `0x11` | Switch ECU state from IDLE to ENGINE_ON |
| `0x10` | Switch ECU state from ENGINE_ON to IDLE |

### Transmitted Data

The application prepares CAN messages for:

- Engine status
- Vehicle speed
- Engine RPM
- Indicator light status

The transmission functions use CAN message buffers in the ECU.

## Technologies and Concepts

- Embedded C
- CAN Bus Communication
- CAN Message Buffers
- CAN Message Filtering
- ADC (Analog-to-Digital Conversion)
- GPIO (General-Purpose Input/Output)
- PIT (Periodic Interrupt Timer)
- ECU State Machine
- Register-Level Peripheral Control
- Virtual Cockpit Interface

## Repository Structure

```text
.
├── README.md
├── main.c
├── can.c
├── can.h
├── adc.c
├── adc.h
├── pit.c
├── pit.h
├── siu.c
├── siu.h
├── init.c
├── init.h
│
├── can_buffer.pdf
├── can_filtering.pdf
├── pin_muxing.pdf
└── unit2-question .pdf
```

## Technical Documentation

Detailed technical documentation of the ECU modules:

- [CAN Communication](docs/can-communication.md)
- [ADC – Analog-to-Digital Converter](docs/adc.md)
- [GPIO – General-Purpose Input/Output](docs/gpio.md)
- [PIT – Periodic Interrupt Timer](docs/pit.md)

### System Architecture

![CAN-Based ECU Virtual Cockpit Architecture](docs/CAN-Based%20ECU%20Virtual%20Cockpit%20Architecture.png)

## Learning Outcomes

Through this project, I worked with:

- Automotive CAN communication concepts.
- Embedded C programming and peripheral registers.
- ECU state management.
- ADC-based sensor input processing.
- GPIO-based input and output control.
- Interrupt-driven and timer-based operations.
- Vehicle signal processing for Virtual Cockpit applications.

## Academic Context

**Program:** M.Sc. Automotive Software Engineering  
**Institution:** Technische Universität Chemnitz  
**Project:** ASE Tutorial – Unit 2

## Project Status

This repository contains the source code and supporting tutorial documents. Build, hardware, and Virtual Cockpit verification details should be added when the corresponding results are available.

## Author

**Mohammad Obydur Rahman**

- GitHub: [@obydur](https://github.com/obydur)
- LinkedIn: [LinkedIn Profile](https://www.linkedin.com/in/portfolio-obydur/)

---

*This project is part of my learning journey in Automotive Software Engineering, Embedded Systems, and Vehicle Communication.*
