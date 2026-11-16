# CAN Communication

## 1. Introduction

Controller Area Network (CAN) is a serial communication protocol widely used in automotive embedded systems. It enables multiple Electronic Control Units (ECUs) to exchange information through a shared communication bus.

In this project, CAN communication is implemented using Embedded C to exchange vehicle-related signals between the ECU and a Virtual Cockpit interface.

The implementation demonstrates CAN message transmission, reception, message identification, message buffers, and ECU state management.

## 2. CAN Communication Architecture

The ECU acts as a communication node that processes input signals and prepares CAN messages for transmission.

The main communication functions include:

- Receiving CAN messages from the communication bus.
- Identifying incoming messages using CAN identifiers.
- Processing engine state commands.
- Transmitting vehicle speed, RPM, engine status, and lighting information.
- Supporting communication with the Virtual Cockpit.

## 3. CAN Message Types

CAN supports two main frame formats:

- Standard Frame: Uses an 11-bit identifier.
- Extended Frame: Uses a 29-bit identifier.

This project uses both standard and extended CAN identifiers.

## 4. CAN Initialization

CAN initialization is implemented in `can.c` through the `CAN_Init()` function.

The initialization process includes:

1. Starting the CAN driver.
2. Configuring the CAN receive mask.
3. Setting up message buffers.
4. Configuring CAN identifiers.
5. Defining frame formats and data lengths.

The implementation uses the CAN controller message buffers for transmitting vehicle-related information.

## 5. CAN Message Configuration

The following CAN message identifiers are configured in the current source code.

| Message | CAN ID | Frame Type | Data Length |
|---|---|---|---|
| Engine Status | 0xDA0 | Extended | 1 byte |
| Vehicle Speed | 0x5A0 | Standard | 1 byte |
| Engine RPM | 0x280 | Standard | 2 bytes |
| Light Status | 0x470 | Standard | 1 byte |

Note: These identifiers describe the configured transmit message buffers.

## 6. CAN Message Transmission

CAN transmission is implemented using dedicated functions in `main.c`.

### 6.1 Engine Status

Function: `can_send_engine_on()`

- Prepares an engine status value.
- Stores the value in message buffer 8.
- Requests transmission through the CAN controller.

The transmitted data value is `0x01`.

### 6.2 Vehicle Speed

Function: `can_send_speed()`

The vehicle speed is calculated from the potentiometer ADC value.

The conversion formula is:

Speed (km/h) = (ADC Value × 240) / 4095

The calculated speed is stored in message buffer 9 and transmitted using CAN ID `0x5A0`.

### 6.3 Engine RPM

Function: `can_send_rpm()`

The project generates changing RPM values between 0 and 8000.

The RPM value is divided into two bytes:

- Byte 0: Lower 8 bits.
- Byte 1: Higher 8 bits.

The data is transmitted using message buffer 10 and CAN ID `0x280`.

### 6.4 Light Status

Function: `can_send_lights()`

The ECU reads two switch inputs through GPIO.

The light status is encoded as follows:

| Light Condition | Data Value |
|---|---|
| No indicator selected | 0x00 |
| Left indicator | 0x01 |
| Right indicator | 0x02 |
| Both indicators | 0x03 |

The light information is transmitted using message buffer 11 and CAN ID `0x470`.

## 7. CAN Message Reception

The receive callback is implemented in `can.c`:

`cfg0_Fifo_RX()`

The callback checks:

- Whether the received message belongs to the configured CAN driver.
- Whether the received frame uses a standard CAN identifier.
- Whether the identifier passes the hardware filtering condition.

Accepted messages are forwarded to `can_receive()` in `main.c`.

## 8. ECU State Management Through CAN

The ECU uses two operating states:

- IDLE
- ENGINE_ON

The received standard CAN identifiers control the state transition.

| Received CAN ID | Current State | New State |
|---|---|---|
| 0x11 | IDLE | ENGINE_ON |
| 0x10 | ENGINE_ON | IDLE |

When the engine is switched ON, the ECU enables the periodic transmission timer.

When the engine is switched OFF, the periodic transmission timer is stopped.

## 9. CAN Message Buffer Management

The implementation uses dedicated CAN message buffers for different vehicle signals.

| Buffer | Purpose |
|---|---|
| 8 | Engine status |
| 9 | Vehicle speed |
| 10 | Engine RPM |
| 11 | Light status |

Each buffer is configured with its corresponding CAN identifier, frame format, and data length.

## 10. Software Modules

| Source File | Responsibility |
|---|---|
| `can.c` | CAN initialization and receive callback |
| `can.h` | CAN definitions and function declarations |
| `main.c` | CAN message processing and transmission functions |
| `init.c` / `init.h` | Peripheral initialization |

## 11. Communication Workflow

1. The ECU initializes the CAN controller.
2. The CAN receive callback monitors incoming messages.
3. Received engine commands update the ECU operating state.
4. When the ECU enters ENGINE_ON, periodic CAN transmission starts.
5. Vehicle speed, RPM, engine status, and light status are prepared.
6. The configured CAN message buffers request transmission.
7. The Virtual Cockpit can use the transmitted vehicle signals for display.

## 12. Learning Outcomes

This implementation provides practical experience with:

- Automotive CAN communication.
- Standard and extended CAN identifiers.
- CAN message buffers.
- CAN message transmission and reception.
- Hardware message filtering.
- ECU state management.
- Vehicle signal processing.
- Embedded C programming.

## 13. References

- Project source code: `can.c`, `can.h`, and `main.c`.
- TU Chemnitz – M.Sc. Automotive Software Engineering.
- ASE Tutorial – Unit 2.
- Supporting project documents: `can_buffer.pdf` and `can_filtering.pdf`.

---

**Project:** CAN-Based ECU Virtual Cockpit  
**Institution:** Technische Universität Chemnitz  
**Programme:** M.Sc. Automotive Software Engineering
