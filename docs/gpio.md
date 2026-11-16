# GPIO (General-Purpose Input/Output)

## 1. Introduction

General-Purpose Input/Output (GPIO) is a fundamental peripheral in embedded systems. It allows a microcontroller to communicate with external digital devices through configurable input and output pins.

In this CAN-Based ECU Virtual Cockpit project, digital input signals are used to detect switch conditions and support vehicle indicator control.

The ECU processes these digital signals and prepares the corresponding status information for CAN communication.

## 2. GPIO Fundamentals

GPIO pins can generally operate in two modes:

### Digital Input

An input pin reads a digital signal from an external device.

Typical logic states are:

- LOW (Logic 0)
- HIGH (Logic 1)

Examples include switches, push buttons, and digital sensors.

### Digital Output

An output pin generates a digital signal to control an external device.

Examples include:

- LEDs
- Indicator lamps
- Relays
- Other digital control interfaces

The actual pin configuration depends on the microcontroller and hardware design.

## 3. GPIO Configuration

GPIO configuration typically involves:

1. Selecting the appropriate microcontroller pin.
2. Configuring the pin as input or output.
3. Selecting the required pin multiplexing function.
4. Configuring the electrical characteristics.
5. Initializing the relevant peripheral registers.
6. Reading or writing the digital signal.

Correct pin configuration is essential for reliable communication between the ECU and external hardware.

## 4. Digital Input Handling

Digital input handling allows the ECU to detect external switch conditions.

The general process is:

1. Configure the selected pin as a digital input.
2. Read the current pin state.
3. Identify whether the signal is HIGH or LOW.
4. Process the input condition.
5. Update the corresponding application state.

For this project, switch inputs are used as part of the indicator control functionality.

## 5. Digital Output Handling

A GPIO output pin can be used to control an external digital device.

The general output process includes:

1. Configure the selected pin as an output.
2. Determine the required output state.
3. Write HIGH or LOW to the pin.
4. Update the connected device.

The actual use of physical GPIO outputs must be confirmed against the project hardware configuration.

## 6. Switch and Indicator Control

The project uses digital switch inputs to represent vehicle indicator conditions.

The indicator information is encoded into a CAN data byte.

| Indicator Condition | CAN Data Value |
|---|---|
| No indicator selected | 0x00 |
| Left indicator | 0x01 |
| Right indicator | 0x02 |
| Both indicators | 0x03 |

These values represent the indicator status encoding documented for the project.

The resulting status information is prepared for transmission to the Virtual Cockpit through CAN communication.

## 7. GPIO and ECU Integration

GPIO is integrated into the ECU application as an input-processing interface.

The general workflow is:

1. Initialize the required GPIO pins.
2. Read the digital switch states.
3. Process the input conditions.
4. Determine the indicator status.
5. Encode the status into a CAN data value.
6. Prepare the corresponding CAN message.
7. Transmit the information to the Virtual Cockpit.

This integration connects physical digital inputs with the vehicle information displayed by the Virtual Cockpit.

## 8. Software Implementation

GPIO-related functionality should be documented according to the actual source code and microcontroller configuration.

The project source structure includes peripheral initialization and application-level processing.

| Module | Responsibility |
|---|---|
| `main.c` | Application-level switch and indicator processing |
| `init.c` / `init.h` | Peripheral initialization |
| `siu.c` / `siu.h` | System Integration Unit and pin-related configuration, where implemented |

Note: Exact function names, pin numbers, register settings, and electrical configurations should be verified against the implementation before being documented.

## 9. Learning Outcomes

This project provides practical experience with:

- GPIO fundamentals.
- Digital input and output configuration.
- Reading digital switch signals.
- Processing external input conditions.
- Vehicle indicator status encoding.
- Peripheral initialization.
- Integration of GPIO signals with CAN communication.
- Embedded C programming.

## 10. References

- CAN-Based ECU Virtual Cockpit project source code.
- TU Chemnitz – M.Sc. Automotive Software Engineering.
- ASE Tutorial – Unit 2.
- Microcontroller reference manual and GPIO documentation.

---

**Project:** CAN-Based ECU Virtual Cockpit  
**Institution:** Technische Universität Chemnitz  
**Programme:** M.Sc. Automotive Software Engineering
