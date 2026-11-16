# ADC (Analog-to-Digital Converter)

## 1. Introduction

An Analog-to-Digital Converter (ADC) is an important peripheral in embedded systems. It converts continuous analog signals into digital values that can be processed by a microcontroller.

In this CAN-Based ECU Virtual Cockpit project, an analog input from a potentiometer is used to generate a digital value representing vehicle speed.

The converted ADC value is processed by the ECU and used to calculate vehicle speed, which can then be transmitted through CAN communication.

## 2. ADC Fundamentals

An ADC converts an analog voltage into a corresponding digital number.

The main ADC parameters include:

- Input voltage range
- ADC resolution
- Digital output range
- Conversion accuracy
- Conversion time

For a 12-bit ADC, the digital output range is:

- Minimum value: 0
- Maximum value: 4095
- Total possible values: 4096

The digital output depends on the analog input voltage and the ADC reference voltage.

## 3. ADC Configuration

ADC configuration typically involves:

1. Selecting the analog input channel.
2. Configuring the corresponding microcontroller pin.
3. Setting the ADC resolution.
4. Configuring the reference voltage.
5. Initializing the ADC peripheral.
6. Starting the conversion process.

The exact configuration depends on the microcontroller and the project hardware setup.

## 4. Analog Input Acquisition

A potentiometer can be used as an analog input source.

When the potentiometer position changes, its output voltage changes accordingly.

The ADC converts this voltage into a digital value.

The general conversion relationship is:

ADC Value = (Input Voltage / Reference Voltage) × (2^N - 1)

Where:

- Input Voltage is the measured analog voltage.
- Reference Voltage is the ADC reference voltage.
- N is the ADC resolution in bits.

For a 12-bit ADC, the digital range is 0–4095.

## 5. ADC Value Processing

After conversion, the ECU receives a digital ADC value.

The value can be processed using mathematical operations to represent a vehicle parameter.

In this project, the ADC value is mapped to a vehicle speed range.

The configured mapping is:

| ADC Value | Vehicle Speed |
|---:|---:|
| 0 | 0 km/h |
| 1024 | Approximately 60 km/h |
| 2048 | Approximately 120 km/h |
| 3072 | Approximately 180 km/h |
| 4095 | 240 km/h |

These values represent the linear mapping between the ADC input range and the vehicle speed range.

## 6. Vehicle Speed Calculation

The vehicle speed is calculated using the following formula:

Speed (km/h) = (ADC Value × 240) / 4095

Example:

If the ADC value is 2048:

Speed = (2048 × 240) / 4095

Speed ≈ 120 km/h

This calculation converts the digital potentiometer input into a vehicle speed value.

## 7. ADC and ECU Integration

The ADC is integrated into the ECU application to process analog input signals.

The general workflow is:

1. Initialize the ADC peripheral.
2. Read the analog input.
3. Obtain the digital ADC value.
4. Process the ADC value.
5. Calculate the vehicle speed.
6. Prepare the speed data for CAN transmission.
7. Send the processed vehicle speed information to the Virtual Cockpit.

## 8. Software Implementation

The ADC-related implementation is part of the embedded software application.

Relevant project modules should be reviewed to understand the actual ADC configuration and data acquisition process.

| Module | Responsibility |
|---|---|
| `adc.c` | ADC-related implementation |
| `adc.h` | ADC declarations and definitions |
| `main.c` | ADC value processing and vehicle speed calculation |

The exact function names and hardware configuration should be documented according to the project source code.

## 9. Learning Outcomes

This project demonstrates practical concepts related to:

- Analog-to-digital conversion.
- ADC resolution and digital value ranges.
- Analog input acquisition.
- Potentiometer-based input processing.
- Mathematical signal scaling.
- Vehicle speed calculation.
- Integration of ADC data with CAN communication.
- Embedded C programming.

## 10. References

- CAN-Based ECU Virtual Cockpit project source code.
- TU Chemnitz – M.Sc. Automotive Software Engineering.
- ASE Tutorial – Unit 2.
- Microcontroller and ADC peripheral documentation.

---

**Project:** CAN-Based ECU Virtual Cockpit  
**Institution:** Technische Universität Chemnitz  
**Programme:** M.Sc. Automotive Software Engineering
