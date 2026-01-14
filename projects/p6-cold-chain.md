# CS G523 – Capstone Project  
## p6-cold-chain – Cold Chain Vaccine Monitoring and Telemetry System  
**(UART-Simulated Inputs, BLE Telemetry on Master Detection)**

---

## 1. Problem Statement / Specification

Design and implement an **embedded cold chain monitoring system** for vaccines that ensures temperature-sensitive payloads are handled within allowable limits throughout transport and storage.

The system shall:
- Monitor environmental conditions relevant to vaccine safety using **simulated sensor data over UART**
- Detect and record **temperature excursions** and other violations of safe storage conditions
- Maintain a **local, persistent log** of measurements and violations
- Advertise and transmit stored telemetry over **BLE when a master device is detected**
- Operate correctly and conservatively even in the presence of missing, delayed, or corrupted input data

All safety-relevant decisions and logging must be performed **locally on the embedded system**.

---

## 2. Motivation and Context

Many vaccines must be maintained within strict temperature ranges (typically 2–8 °C) from manufacture to administration.  
Breaks in the cold chain can render vaccines ineffective or unsafe.

Cold chain failures often occur:
- during transport,
- in remote locations,
- or when connectivity is intermittent.

As a result, cold chain systems must:
- operate autonomously,
- record violations reliably,
- and synchronize data opportunistically when connectivity becomes available.

This project focuses on the **embedded software responsible for monitoring, logging, and reporting**, not on sensor hardware or logistics optimization.

---

## 3. System Boundary

### In Scope
- Embedded software responsible for:
  - Receiving simulated environmental data over UART
  - Evaluating conditions against safe storage limits
  - Detecting and recording excursions
  - Maintaining persistent logs in non-volatile memory
  - Advertising and transmitting telemetry over BLE when a master is detected

### Out of Scope
- Physical temperature sensor hardware
- GPS or real-time location tracking
- Cloud-side data analytics
- User-interface-heavy applications
- Real-time control of refrigeration hardware

---

## 4. Input Specification (UART-Based Simulation)

### Rationale for UART Simulation

UART-based simulation allows:
- repeatable testing of edge cases,
- systematic fault injection,
- decoupling of monitoring logic from sensor hardware.

---

### Simulated Input Data

Simulated data may include:
- temperature readings
- optional humidity or other environmental indicators
- timestamps or sequence identifiers

#### Characteristics
- Data frames arrive at variable rates
- Frames may be delayed, missing, duplicated, or malformed
- Temperature profiles may include gradual drift or sudden excursions

**Example frame (illustrative):**
```
$COLD,
TEMP=6.3,
TS=987654
*
```

---

## 5. Logging Requirements (High-Level)

The system is expected to:
- Record periodic measurements locally
- Log all detected excursions with timestamps
- Preserve logs across resets or power loss
- Distinguish between normal operation and violation periods

Exact log formats, storage strategies, and retention policies are **intentionally left to the design team**.

---

## 6. Telemetry over BLE

### Operating Principle

The system operates autonomously without assuming continuous connectivity.

- When no master device is present:
  - data is logged locally
- When a BLE master (e.g., phone or gateway) is detected:
  - the system advertises its presence
  - logged telemetry is transmitted over BLE

Telemetry transmission must not interfere with local monitoring and logging.

---

## 7. Safety Expectations (Intentionally High-Level)

At a high level, the system is expected to:
- Detect and record all temperature excursions
- Avoid loss or corruption of critical logs
- Behave conservatively under uncertain input data
- Avoid reporting false compliance when data is missing or invalid

Precise thresholds, timing windows, and reporting policies are **intentionally not specified** and must be justified by the team.

---

## 8. What Is Intentionally Not Provided

The following are deliberately not specified:
- Exact temperature limits
- Logging data structures
- Non-volatile memory layout
- BLE service and characteristic definitions
- Synchronization or upload protocols

Teams are expected to:
- derive requirements from the problem context
- design a robust logging architecture
- justify telemetry and synchronization behavior
- validate correctness using simulated test scenarios

---

## 9. Expected Outcome

By the end of the project, teams should be able to demonstrate:
- A working cold chain monitoring system
- Reliable detection and logging of excursions
- Persistent storage of critical data
- Opportunistic BLE telemetry transmission
- Robust behavior under faulty or missing input data

---

**Note:** The emphasis of this project is on **embedded monitoring, logging, and opportunistic communication**, which are common requirements in real-world IoT systems.
