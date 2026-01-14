# CS G523 – Capstone Project  
## p10-alarm-mgmt – Industrial Alarm Management System  
**(UART-Simulated Sensor Streams, Deterministic and Safety-Oriented)**

---

## 1. Problem Statement / Specification

Design and implement an **embedded industrial alarm management system** that monitors multiple process sensors, detects abnormal operating conditions, and manages alarms in a **deterministic, safety-critical manner**.

The system shall:
- Receive **simulated sensor data over UART** from multiple independent sources
- Classify sensor conditions into **safe, caution, or critical**
- Raise, prioritize, and manage alarms based on predefined safety rules
- Handle alarm dependencies involving multiple sensors
- Prevent alarm flooding while ensuring critical alarms are never missed
- Behave conservatively under noisy, delayed, missing, or malformed input data

All alarm evaluation and state management must be performed **locally on the embedded system**.

---

## 2. Motivation and Context

Industrial facilities (chemical plants, power systems, manufacturing lines) rely on alarms to prevent unsafe operation.  
Poor alarm design can result in:
- missed critical conditions,
- alarm floods,
- operator confusion,
- unsafe delayed response.

Modern alarm management standards emphasize:
- clear prioritization,
- limited active alarms,
- deterministic alarm lifecycle behavior,
- conservative handling of uncertainty.

This project focuses on **embedded alarm logic and safety discipline**, not UI design or enterprise monitoring systems.

---

## 3. Sensor Set and Operating Context

The system monitors a simplified **industrial process unit** with the following sensors:

### Sensors (Simulated over UART)

| Sensor ID | Description | Units |
|---------|------------|-------|
| S1 | Reactor Temperature | °C |
| S2 | Reactor Pressure | bar |
| S3 | Feed Flow Rate | kg/s |
| S4 | Cooling Water Flow | kg/s |
| S5 | Power Supply Voltage | V |
| S6 | Vibration Level | mm/s |

Each sensor produces periodic measurements via UART frames.

---

## 4. Safety Thresholds and Alarm Levels

Each sensor has **three operating regions**:

### Individual Sensor Thresholds (Illustrative)

| Sensor | Safe | Caution | Critical |
|------|------|---------|----------|
| S1 (Temp) | ≤ 180 | 180–200 | > 200 |
| S2 (Pressure) | ≤ 12 | 12–15 | > 15 |
| S3 (Feed Flow) | 8–12 | 6–8 or 12–14 | < 6 or > 14 |
| S4 (Cooling Flow) | ≥ 10 | 7–10 | < 7 |
| S5 (Voltage) | 22–26 | 20–22 or 26–28 | < 20 or > 28 |
| S6 (Vibration) | ≤ 4 | 4–6 | > 6 |

Exact numeric values are provided **only to define scope and complexity**; teams may justify refinements.

---

## 5. Alarm Dependency Rules

Some unsafe conditions arise **only when multiple sensor conditions occur together**.

### Example Dependency Alarms

- **A1: Thermal Runaway Risk (Critical)**  
  Triggered if:  
  - S1 > 180 (Temperature in caution or critical) **AND**  
  - S4 < 10 (Cooling flow reduced)

- **A2: Mechanical Failure Risk (Critical)**  
  Triggered if:  
  - S6 > 4 (Vibration elevated) **AND**  
  - S3 outside safe range

- **A3: Power Instability (Caution)**  
  Triggered if:  
  - S5 outside safe range **AND**  
  - Any other sensor is in caution

Individually, these sensor conditions may be acceptable; combined, they represent elevated risk.

---

## 6. Alarm Priorities

Alarms are classified into priorities:

- **Priority 1 (Critical)**  
  Immediate operator action required  
  Must never be suppressed or masked

- **Priority 2 (Caution)**  
  Operator attention required  
  May be suppressed if a related critical alarm is active

- **Priority 3 (Advisory)**  
  Informational only  
  May be rate-limited or grouped

Exact prioritization and suppression rules must be justified by the team.

---

## 7. Alarm Lifecycle

Each alarm shall follow a well-defined lifecycle:

- Inactive
- Active (unacknowledged)
- Active (acknowledged)
- Cleared

The system must:
- avoid oscillation between states,
- preserve alarm history,
- ensure deterministic transitions.

---

## 8. Input Specification (UART-Based Simulation)

### Input Characteristics
- Sensor frames arrive independently
- Arrival rates vary per sensor
- Frames may be delayed, duplicated, missing, or malformed
- Bursts of abnormal values may occur simultaneously

**Example frame**
```
$SENSOR,
ID=S1,
VALUE=192.5,
TS=123456
*
```

---

## 9. Safety Expectations (High-Level)

The system is expected to:
- Never lose or mask critical alarms
- Default to conservative alarm states on uncertainty
- Handle alarm floods gracefully
- Maintain consistent alarm state under reset or input loss
- Prefer escalation over silence

---

## 10. What Is Intentionally Not Provided

The following are deliberately not specified:
- Exact scheduling or task models
- Alarm suppression timing policies
- Data structures for alarm storage
- Operator acknowledgment mechanisms
- UART framing details beyond examples

Teams are expected to:
- derive a clear alarm architecture
- justify prioritization and dependency handling
- validate behavior under bursty and faulty input scenarios

---

## 11. Expected Outcome

By the end of the project, teams should demonstrate:
- A working alarm management system
- Correct handling of individual and dependent alarms
- Robust prioritization and lifecycle management
- Deterministic behavior under stress conditions
- Clear safety rationale and verification evidence

---

**Note:** This project emphasizes **event-driven design, prioritization under load, and safety-oriented decision making**, which are essential skills in industrial embedded software.
