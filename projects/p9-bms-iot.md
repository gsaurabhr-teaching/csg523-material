# CS G523 – Capstone Project  
## IoT-Enabled Battery Management System (BMS) Safety Supervisor  
**(UART-Simulated Inputs + MQTT Telemetry)**

---

## 1. Problem Statement / Specification

Design and implement an **embedded software system that supervises the safe operation of a battery pack**.

The system shall:
- Receive simulated battery sensor data over a UART interface
- Decide locally whether battery operation is safe
- Enable or disable charging and discharging accordingly
- Publish battery metrics and safety-related status to a remote server using MQTT

The system must operate **safely and deterministically**, even in the presence of faulty, missing, or inconsistent input data.

---

## 2. Motivation and Context

Battery systems are widely used in:
- Electric vehicles
- Energy storage systems
- Medical and industrial equipment

Failures in battery management software can lead to:
- Thermal runaway
- Permanent battery damage
- Unsafe system shutdowns

In industrial practice, safety-critical battery software is validated using **software-in-the-loop (SIL)** or **hardware-in-the-loop (HIL)** setups with simulated sensor data.  
This project mirrors that workflow and emphasizes **software architecture, safety reasoning, and verification**, rather than analog electronics.

---

## 3. System Boundary

### In Scope
- Embedded software responsible for:
  - Safety supervision
  - Fault detection and handling
  - Operational state management
- UART-based reception of simulated battery data
- Publishing telemetry data to a cloud service using MQTT

### Out of Scope
- Analog sensor interfacing (ADCs)
- Battery chemistry or electrochemical modeling
- State-of-charge or state-of-health estimation
- Cloud-based or remote control of safety decisions

All safety decisions **must be made locally** on the embedded system.

---

## 4. Safety Expectations (Intentionally High-Level)

The system is expected to behave conservatively in the presence of abnormal conditions.

At a high level, the software should:
- Prevent operation outside safe electrical or thermal limits
- Transition to safe states on detection of abnormal or uncertain inputs
- Avoid unsafe behavior on reset, startup, or data loss
- Prefer disabling operation over risking unsafe operation

**Precise safety requirements, thresholds, fault classifications, and recovery policies are intentionally left to the design teams** and must be justified and verified as part of the project.

---

## 5. UART Input Specification

Battery data is provided by a **PC-based UART emulator script**.  
This allows controlled and repeatable testing, including fault injection.

### General Characteristics
- UART frames arrive at variable rates
- Not all sensor values are present in every frame
- Different sensors may be sampled at different frequencies
- Frames may be delayed, malformed, or missing

### Example Frame Format
```
$BMS,
V1=4.12,
V3=4.15,
PACK_I=-2.3,
T2=38.1
*
```

Another frame at a later time may contain a different subset:
```
$BMS,
V2=4.10,
V4=4.08,
T1=36.5,
CHG=1
*
```

### Available Fields (Example)
- Cell voltages (V1, V2, …)
- Pack current
- One or more temperature sensors
- Charger connection status

### Expectations
The software must:
- Parse frames robustly
- Handle missing, stale, or inconsistent data
- Detect malformed frames and timeouts
- Decide safe behavior under partial information

---

## 6. What Is Intentionally Not Provided

To emphasize design thinking and engineering judgment, **the following are not provided**:

- Detailed functional requirements
- Complete fault lists
- Threshold values
- State machine definitions
- Architecture diagrams
- Task or thread structure

Teams are expected to:
- Derive requirements from the problem context
- Propose and justify a software architecture
- Define safety policies and fault handling strategies
- Demonstrate correctness through testing and verification

---

## 7. Expected Outcome

At the end of the project, teams should be able to demonstrate:
- A working embedded safety supervisor
- Robust handling of normal and abnormal input scenarios
- Clear design rationale for safety decisions
- Evidence-based verification using UART-driven test cases
- Cloud-based visibility into system state and behavior

---

**Note:** The use of UART-based simulation is deliberate and reflects industry validation practices.  
Projects will be evaluated on **software correctness, safety discipline, architectural clarity, and verification rigor**.
