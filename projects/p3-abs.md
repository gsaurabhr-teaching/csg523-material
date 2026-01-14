# CS G523 – Capstone Project  
## p3-abs – Antilock Braking System (ABS) Controller  
**(UART-Simulated Wheel Speed Inputs, Binary Brake Modulation)**

---

## 1. Problem Statement / Specification

Design and implement an **embedded Antilock Braking System (ABS) controller** that prevents wheel lock during braking by modulating brake pressure based on wheel speed measurements.

The system shall:
- Continuously monitor **individual wheel speeds from all four wheels**
- Detect conditions that indicate **impending wheel lock**
- Modulate braking action by controlling **binary brake solenoids**:
  - brake applied
  - brake released
- Operate correctly under varying vehicle and road conditions
- Behave deterministically and conservatively in the presence of noisy, delayed, or missing sensor data

All safety-critical decisions must be made **locally and in real time**.

---

## 2. Motivation and Context

During hard braking, wheels may lock, causing:
- loss of steering control
- increased stopping distance
- vehicle instability

An ABS prevents wheel lock by **rapidly releasing and reapplying brake pressure**, allowing the wheels to maintain traction with the road surface.

Modern ABS implementations are:
- fully software-controlled
- event-driven
- safety-critical
- tightly coupled to sensor reliability and timing

This project focuses on the **embedded software logic of ABS**, not on hydraulic systems or detailed vehicle dynamics.

---

## 3. Brief Description of ABS Operation

At a high level, ABS operates as follows:
- Wheel speeds are continuously monitored
- A rapidly decreasing wheel speed relative to vehicle speed indicates imminent lock
- When lock is detected or predicted:
  - brake pressure on the affected wheel is released
- Once wheel speed recovers:
  - brake pressure is reapplied

This cycle repeats rapidly during braking to maintain traction and stability.

In this project:
- Brake actuation is simplified to **binary control** (apply / release)
- The focus is on **decision logic and safety behavior**, not fine-grained pressure control

---

## 4. System Boundary

### In Scope
- Embedded software responsible for:
  - Receiving wheel speed data over UART
  - Detecting wheel lock or near-lock conditions
  - Independently controlling brake solenoids for each wheel
  - Managing timing and modulation logic
  - Handling abnormal or uncertain sensor inputs

### Out of Scope
- Hydraulic brake hardware
- Continuous pressure modulation models
- Vehicle longitudinal dynamics modeling
- Driver interface design
- Cloud-based or remote control logic

---

## 5. Input Specification (UART-Based Simulation)

### Rationale for UART Simulation

Wheel speed data is provided via **UART-based simulation** to:
- enable repeatable testing
- allow systematic fault injection
- decouple ABS logic from sensor hardware

---

### Wheel Speed Inputs

- Four independent wheel speed values:
  - Front-left (FL)
  - Front-right (FR)
  - Rear-left (RL)
  - Rear-right (RR)

### UART Data Characteristics
- Wheel speed frames arrive at regular or variable intervals
- Frames may contain:
  - all wheel speeds, or
  - a subset of wheel speeds
- Data may be noisy, delayed, missing, or malformed

---

### Example Frame Format (Illustrative)

```
$ABS,
FL=42.1,
FR=41.8,
RL=40.9,
RR=41.0,
BRAKE=1
*
```

Another frame may omit one or more wheels or arrive later in time.

---

## 6. Control Outputs

For each wheel, the controller generates a **binary brake command**:
- APPLY brake
- RELEASE brake

The mapping between controller output and physical solenoid behavior is abstracted.

---

## 7. Safety Expectations (Intentionally High-Level)

At a high level, the ABS controller is expected to:
- Prevent sustained wheel lock during braking
- Act independently on each wheel where appropriate
- Remain stable and non-oscillatory
- Handle sensor noise and disagreement conservatively
- Prefer releasing brakes over risking wheel lock under uncertainty

Exact definitions of:
- lock detection thresholds
- timing windows
- modulation strategies
are **intentionally left unspecified** and must be derived and justified by the team.

---

## 8. What Is Intentionally Not Provided

The following are deliberately not specified:
- Vehicle speed estimation method
- Slip ratio equations
- Threshold values or timing constants
- State machine definitions
- Scheduling or task models

Teams are expected to:
- derive requirements from the problem context
- design an event-driven control architecture
- justify safety-oriented decisions
- validate behavior using simulated test scenarios

---

## 9. Expected Outcome

By the end of the project, teams should be able to demonstrate:
- A working ABS controller operating on simulated data
- Correct detection of lock and near-lock conditions
- Appropriate brake modulation for individual wheels
- Robust behavior under noisy or missing sensor inputs
- Clear safety reasoning and verification evidence

---

**Note:** This project emphasizes **real-time decision making, event-driven control, and safety reasoning**, which are central to embedded automotive systems.
