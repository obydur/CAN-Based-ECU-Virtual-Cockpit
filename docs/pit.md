# PIT (Periodic Interrupt Timer)

## 1. Introduction

The Periodic Interrupt Timer (PIT) is a hardware peripheral used in embedded systems to generate periodic timer events and interrupts.

In this CAN-Based ECU Virtual Cockpit project, the PIT peripheral is used to schedule periodic operations, control CAN message transmission, and manage indicator blinking.

The timer allows the ECU to execute time-dependent tasks without continuously checking elapsed time in the main application loop.

## 2. PIT Fundamentals

A Periodic Interrupt Timer generates interrupts at configured time intervals.

The main concepts include:

- Timer channel: An independent timer resource.
- Timer frequency: The configured number of timer events per second.
- Timer period: The time interval between consecutive events.
- Interrupt: A signal that requests the processor to execute a specific callback.
- Interrupt Service Routine (ISR): The routine associated with an interrupt event.

The relationship between frequency and period is:

Period = 1 / Frequency

For example:

| Frequency | Period |
|---:|---:|
| 1 Hz | 1000 ms |
| 10 Hz | 100 ms |
| 100 Hz | 10 ms |
| 1000 Hz | 1 ms |

## 3. Timer Configuration

PIT initialization and configuration are implemented in `pit.c`.

The project provides the following functions:

### PIT_Init()

Initializes the PIT peripheral using the low-level driver.

### PIT_ConfigureTimer()

Configures a selected timer channel.

Function prototype:

`void PIT_ConfigureTimer(uint8_t timerChannel, uint32_t timerFrequency);`

The implementation calculates:

`1000 / timerFrequency`

and passes the result to the low-level timer driver.

### PIT_StartTimer()

Starts the selected PIT timer channel.

### PIT_StopTimer()

Stops the selected PIT timer channel.

These functions provide an interface between the application and the underlying PIT driver.

## 4. Periodic Interrupt Generation

The application configures two PIT timer channels in `main.c`.

| Timer Channel | Configured Frequency | Application Purpose |
|---|---:|---|
| Channel 1 | 100 Hz | Periodic CAN message processing |
| Channel 2 | 1000 Hz | Indicator blinking state update |
| Channel 3 | Not configured in the shown application code | Callback reserved for future use |

The corresponding timer periods calculated by the configuration formula are 10 ms for Channel 1 and 1 ms for Channel 2.

The actual hardware timing should be confirmed against the low-level PIT driver configuration.

## 5. Interrupt Handling

The project defines three PIT callback functions in `main.c`.

### 5.1 PIT_Channel_1()

This callback handles periodic vehicle-related CAN message processing.

It calls the following functions:

- `can_send_engine_on()`
- `can_send_speed()`
- `can_send_rpm()`
- `can_send_lights()`

These functions prepare the corresponding vehicle signals for CAN transmission.

### 5.2 PIT_Channel_2()

This callback updates the indicator blinking state.

Implementation:

`blink = !blink;`

The Boolean variable `blink` changes between TRUE and FALSE on each callback.

This state is used by the light-processing function to control indicator output behavior.

### 5.3 PIT_Channel_3()

A callback is defined for Channel 3, but no application functionality is currently implemented in it.

## 6. ECU State and Timer Control

The ECU uses two operating states:

- IDLE
- ENGINE_ON

The main application checks the current ECU state and controls PIT Channel 1 accordingly.

| ECU State | Timer Channel 1 |
|---|---|
| IDLE | Stopped |
| ENGINE_ON | Started |

When a CAN command changes the ECU state to ENGINE_ON, Channel 1 is started.

When the ECU returns to IDLE, Channel 1 is stopped.

This approach ensures that periodic vehicle-related CAN message processing is controlled by the engine operating state.

## 7. PIT and CAN Communication

PIT provides the periodic timing mechanism for the CAN communication tasks.

The communication workflow is:

1. Initialize the ECU peripherals.
2. Configure PIT timer channels.
3. Receive an engine control command through CAN.
4. Update the ECU operating state.
5. Start PIT Channel 1 when the engine is ON.
6. Execute the Channel 1 callback periodically.
7. Prepare engine status, vehicle speed, RPM, and light status data.
8. Request CAN message transmission.

The second timer channel independently updates the blinking state used by the indicator-processing logic.

## 8. Software Implementation

The PIT functionality is separated into a driver interface and application-level callbacks.

| Source File | Responsibility |
|---|---|
| `pit.c` | PIT initialization, configuration, start and stop functions |
| `pit.h` | PIT function declarations and driver includes |
| `main.c` | Timer configuration, callback implementation and ECU state control |

### Important Functions

| Function | Description |
|---|---|
| `PIT_Init()` | Initializes the PIT peripheral |
| `PIT_ConfigureTimer()` | Configures a timer channel |
| `PIT_StartTimer()` | Starts a timer channel |
| `PIT_StopTimer()` | Stops a timer channel |
| `PIT_Channel_1()` | Processes periodic CAN messages |
| `PIT_Channel_2()` | Updates indicator blinking state |
| `PIT_Channel_3()` | Reserved callback |

## 9. Learning Outcomes

This project provides practical experience with:

- Periodic Interrupt Timer fundamentals.
- Hardware timer configuration.
- Timer channels and frequency settings.
- Periodic interrupt callbacks.
- Interrupt-driven application design.
- ECU operating-state management.
- Periodic CAN message processing.
- Time-dependent indicator control.
- Embedded C programming.

## 10. References

- CAN-Based ECU Virtual Cockpit project source code.
- `pit.c` and `pit.h`.
- `main.c`.
- TU Chemnitz – M.Sc. Automotive Software Engineering.
- ASE Tutorial – Unit 2.
- Microcontroller PIT peripheral documentation.

---

**Project:** CAN-Based ECU Virtual Cockpit  
**Institution:** Technische Universität Chemnitz  
**Programme:** M.Sc. Automotive Software Engineering
