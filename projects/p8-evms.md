# CS G523 – Capstone Project  
## p8-evm – Secure and Tamper-Resilient Electronic Voting Machine (EVM) Controller  
**(UART-Simulated Inputs, Safety- and Security-Critical Embedded System)**

---

## 1. Problem Statement / Specification

Design and implement an **embedded controller for an Electronic Voting Machine (EVM)** that ensures **correct, tamper-resilient, and fail-safe vote recording and reporting**.

The system shall:
- Accept **vote inputs** from a simulated ballot unit
- Maintain **correct and immutable recording of votes**
- Detect and respond to **physical and logical tamper events**
- Ensure **safe behavior under power loss, reset, or abnormal conditions**
- Provide a **verifiable end-of-election report**
- Operate deterministically and conservatively under all conditions

All voting, tamper handling, and reporting decisions must be made **locally on the embedded system**.

---

## 2. Motivation and Context

Electronic voting systems are among the most **security- and safety-critical embedded systems** deployed in the real world.

Failures can lead to:
- loss or corruption of votes
- undetectable tampering
- loss of public trust
- irreversible system misuse

As a result, EVMs are designed to emphasize:
- simplicity over performance
- fail-safe defaults
- strong separation of modes
- verifiability over convenience

This project focuses on the **embedded software architecture and safety/security reasoning** behind a voting machine, not on cryptographic protocols or election policy.

---

## 3. System Boundary

### In Scope
- Embedded software responsible for:
  - Accepting vote inputs
  - Managing election states (pre-election, voting, closed)
  - Recording votes in non-volatile memory
  - Detecting and handling tamper events
  - Managing power-loss and reset behavior
  - Producing an end-of-election summary

### Out of Scope
- Cryptographic voting protocols
- Networked or internet-based voting
- Graphical user interfaces
- Election administration procedures
- Voter authentication mechanisms

---

## 4. Input Specification (UART-Based Simulation)

### Rationale for UART Simulation

UART-based simulation allows:
- repeatable testing of rare but critical events
- systematic fault and tamper injection
- decoupling of logic from physical hardware

---

### Simulated Inputs

Inputs are provided as **independent UART frames**, mimicking separate hardware sources and interrupts.

#### Voting Inputs
- Vote button press (candidate ID)
- Vote confirmation signal

#### Tamper Detection Inputs
- Enclosure open sensor
- Voltage anomaly detector
- Clock anomaly detector

#### Control Inputs
- Election start
- Election end
- Administrative reset (authorized only in specific states)

**Example frame (illustrative):**
```
$VOTE,
CANDIDATE=2,
TS=123456
*
```

Another example:
```
$TAMPER,
CASE_OPEN=1,
TS=123457
*
```

Frames may be delayed, duplicated, missing, or malformed.

---

## 5. Vote Recording Requirements (High-Level)

The system is expected to:
- Record each vote **exactly once**
- Store votes in **non-volatile memory**
- Avoid overwriting or erasing recorded votes
- Ensure votes are not lost on power failure
- Maintain internal consistency after reset

Exact storage formats and memory layouts are **intentionally left unspecified**.

---

## 6. Tamper Detection and Response (High-Level)

At a high level, the system must:
- Detect tamper conditions promptly
- Transition to a **tamper-detected safe state**
- Disable further voting after critical tamper detection
- Preserve all previously recorded votes
- Record tamper events for audit

The precise classification of tamper events and response policies are **left to the design team**, but must be conservative and well-justified.

---

## 7. Operating Modes (Conceptual)

The system operates in distinct modes such as:
- Initialization
- Pre-election
- Voting active
- Voting closed
- Tamper-detected
- Audit / report mode

Transitions between modes must be:
- explicit
- well-defined
- irreversible where appropriate

---

## 8. Safety and Security Expectations (Intentionally High-Level)

At a high level, the system is expected to:
- Never lose or alter recorded votes
- Never accept votes outside authorized election states
- Default to safe, non-voting states on uncertainty
- Make tamper events visible and irreversible
- Behave deterministically under reset or fault conditions

Exact policies for:
- tamper thresholds,
- recovery behavior,
- administrative actions
are **intentionally not specified**.

---

## 9. What Is Intentionally Not Provided

The following are deliberately not specified:
- Cryptographic mechanisms
- Non-volatile memory data structures
- Tamper sensor thresholds
- State machine definitions
- Reporting formats

Teams are expected to:
- derive detailed requirements
- design a minimal and verifiable architecture
- justify safety and security decisions
- validate behavior through systematic testing

---

## 10. Expected Outcome

By the end of the project, teams should be able to demonstrate:
- A working embedded EVM controller
- Correct and fail-safe vote recording
- Robust tamper detection and response
- Deterministic behavior under power loss and reset
- Clear auditability and verification evidence

---

**Note:** This project emphasizes **trustworthy embedded system design, fail-safe storage, and conservative security reasoning**, which are essential skills for safety- and security-critical software.
