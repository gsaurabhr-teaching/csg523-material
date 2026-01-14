# CS G523 – Capstone Project  
## p7-elevator-door – Elevator Door Safety Controller  
**(UART-Simulated Sensors and Commands)**

---

## 1. Problem Statement / Specification

Design and implement an **embedded safety controller for an elevator door subsystem**.  
The controller is responsible for ensuring **safe opening and closing of elevator doors** in the presence of user requests, sensor inputs, and fault conditions.

The system shall:
- Receive **simulated door-related inputs over UART**
- Control door motion using discrete commands (open / close / stop)
- Prevent unsafe door movement under all operating conditions
- Handle abnormal scenarios such as obstructions, sensor faults, and conflicting commands
- Behave deterministically and conservatively when input data is missing, delayed, or inconsistent

All safety-relevant decisions must be made **locally by the embedded controller**.

---

## 2. Motivation and Context

Elevator door systems are **safety-critical subsystems**.  
Failures can result in passenger injury, equipment damage, or service disruption.

Real-world elevator door controllers must:
- Respond correctly to concurrent requests
- Enforce strict safety interlocks
- Handle faults predictably
- Remain safe even when sensors or inputs fail

This project focuses on **embedded software design for safety, concurrency, and event-driven control**, not on mechanical design.

---

## 3. Brief Description of Elevator Door Operation

At a high level:
- Doors open when the elevator is stationary and aligned with a floor
- Doors close after a dwell time or on command
- Closing must be interrupted if an obstruction is detected
- Emergency or fault conditions require immediate safe action

Door behavior is typically governed by **explicit state machines** with well-defined transitions.

---

## 4. System Boundary

### In Scope
- Embedded software responsible for:
  - Receiving door-related commands and sensor inputs over UART
  - Managing door states (open, closed, opening, closing, stopped, fault)
  - Enforcing safety interlocks
  - Generating door actuator commands
  - Handling faults and recovery behavior

### Out of Scope
- Motor drive electronics
- Mechanical door design
- Elevator car motion control
- User-interface hardware
- Cloud-based monitoring or control

---

## 5. Input Specification (UART-Based Simulation)

### Rationale for UART Simulation

UART-based simulation allows:
- repeatable testing of normal and abnormal scenarios
- systematic fault injection
- decoupling of software logic from physical hardware

---

### Simulated Input Data

Inputs may include:
- Door open request
- Door close request
- Obstruction sensor (e.g., light curtain)
- Door fully open sensor
- Door fully closed sensor
- Emergency stop signal

#### Characteristics
- Inputs arrive as independent UART frames
- Frames may be delayed, missing, duplicated, or malformed
- Conflicting commands may occur

**Example frame (illustrative):**
```
$DOOR,
OPEN_REQ=1,
OBSTRUCTION=0,
TS=123456
*
```

---

## 6. Control Outputs

The controller generates **discrete door actuator commands**, such as:
- OPEN
- CLOSE
- STOP

The mapping to physical actuators is abstracted.

---

## 7. Safety Expectations (Intentionally High-Level)

At a high level, the controller is expected to:
- Never close doors when obstruction is detected
- Prevent motion in unsafe or undefined states
- Prioritize passenger safety over throughput
- Enter a safe state on sensor failure or ambiguity
- Avoid oscillatory or unstable door behavior

Exact timing parameters, dwell times, and recovery policies are **intentionally left unspecified** and must be justified by the team.

---

## 8. What Is Intentionally Not Provided

The following are deliberately not specified:
- Exact state machine definitions
- Timing constants and dwell times
- Fault classification and recovery strategies
- Scheduling or task models
- UART frame formats beyond examples

Teams are expected to:
- derive detailed requirements
- design an event-driven state machine
- justify safety and fault-handling decisions
- validate behavior using systematic test cases

---

## 9. Expected Outcome

By the end of the project, teams should be able to demonstrate:
- A working elevator door safety controller
- Correct handling of normal operation and faults
- Robust behavior under conflicting or missing inputs
- Clear safety reasoning and verification evidence

---

**Note:** This project emphasizes **state-machine-driven design and safety interlocks**, which are foundational patterns in embedded control systems.
