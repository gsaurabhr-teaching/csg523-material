# CS G523 – Capstone Project  
## p5-emg-brake – Emergency Brake Intent Detection using EMG (+ Optional EDA)  
**(UART-Simulated Inputs, Live BLE Input from OpenBCI Cyton)**

---

## 1. Problem Statement / Specification

Design and implement an **embedded safety subsystem that detects emergency braking intent** using physiological signals and triggers an emergency brake request when unsafe driving conditions are inferred.

The system shall:
- Detect **emergency braking intent** using calf muscle EMG and EDA activity
- Operate as a **redundant, safety-oriented intent detection channel**, not as a replacement for the brake pedal
- Use **UART-simulated EMG (and optional EDA) data** for development, testing, and verification
- Support **live EMG data acquisition over BLE from an OpenBCI Cyton device** as an integration extension
- Trigger a discrete **emergency brake request signal** when intent is detected
- Behave conservatively and deterministically under noisy, missing, or inconsistent input data

All safety-relevant decisions must be made **locally on the embedded system**.

---

## 2. Motivation and Context

Emergency braking situations often involve:
- sudden hazards,
- high stress,
- delayed mechanical actuation,
- or reduced driver responsiveness.

Physiological signals such as **EMG (electromyography)** can reveal **driver intent earlier than mechanical pedal movement**, since muscle activation precedes physical motion.

This project explores the use of EMG as a **supplementary safety signal** that:
- provides early intent detection,
- acts independently of conventional controls,
- and can enhance overall system safety when designed conservatively.

The emphasis is on **embedded safety logic and verification**, not on human physiology or clinical interpretation.

---

## 3. Brief Working Principle (High-Level)

At a conceptual level, the system operates as follows:
- EMG signals from calf muscles are monitored continuously
- Elevated or sustained muscle activation patterns indicate braking intent
- When such patterns persist beyond a defined confidence window:
  - the system raises an **emergency brake request**
- If signal quality is poor or ambiguous:
  - the system defaults to **non-assertion or conservative behavior**

The system does **not** control braking force; it only signals intent.

---

## 4. System Boundary

### In Scope
- Embedded software responsible for:
  - Receiving EMG (and optional EDA) data
  - Windowing and preprocessing physiological signals
  - Computing simple EMG-derived metrics
  - Detecting emergency braking intent
  - Managing intent states and time consistency
  - Generating a discrete emergency brake request output

### Out of Scope
- Brake actuation hardware
- Continuous braking control
- Vehicle dynamics modeling
- Medical diagnosis
- Cloud-based decision making

---

## 5. Input Specification

### 5.1 UART-Based Simulation (Primary Development Path)

For initial development and verification, physiological data is provided via **UART-based simulation**.

This allows:
- clean and repeatable testing,
- systematic fault injection,
- controlled variation of signal characteristics.

#### UART Data Characteristics
- Frames may contain:
  - EMG samples
  - optional EDA samples
- Sampling rates may vary
- Frames may be delayed, missing, or malformed
- Noise and artifacts are intentionally injected

**Example frame (illustrative):**
```
$EMG,
EMG=0.72,
EDA=0.31,
TS=123456
*
```

---

### 5.2 Live BLE Input (Integration Extension)

As an extension, teams may acquire **live EMG data from an OpenBCI Cyton device over BLE**.

The system architecture must:
- allow BLE input to be substituted for UART input
- keep intent detection and safety logic unchanged

---

## 6. EMG Metrics to Be Computed (Guidance Provided)

To constrain scope while preserving design freedom, teams are expected to compute **at least one** of the following EMG-derived metrics over fixed-duration windows:

- **EMG signal envelope or RMS value**
- **Sustained activation duration above a baseline**
- **Rate of change of EMG activity**

If EDA is included:
- it may be used as a **contextual or corroborating signal**, not as a primary trigger

Exact thresholds and fusion logic are **intentionally left unspecified** and must be justified by the team.

---

## 7. Safety Expectations (Intentionally High-Level)

At a high level, the system is expected to:
- Avoid false emergency triggers due to noise or transient muscle activity
- Prefer missing a marginal event over triggering unsafe braking
- Detect prolonged signal loss or sensor faults
- Avoid repeated or oscillatory emergency requests
- Fail safely under uncertainty

Precise definitions of:
- intent thresholds,
- confidence windows,
- recovery behavior,
are **left to the design team**.

---

## 8. What Is Intentionally Not Provided

The following are deliberately not specified:
- Exact EMG thresholds or calibration methods
- Classification algorithms
- State machine definitions
- Timing constants or hysteresis values
- BLE protocol details

Teams are expected to:
- derive requirements from the problem context
- design an event-driven architecture
- justify conservative safety policies
- validate behavior using simulated and live data

---

## 9. Expected Outcome

By the end of the project, teams should be able to demonstrate:
- A working emergency brake intent detection system
- Robust behavior under noisy and missing data
- Clear separation between data acquisition and safety logic
- Justified safety decisions supported by test evidence
- Successful substitution of UART-simulated input with live BLE data (if attempted)

---

**Note:** UART-based simulation reflects industry validation workflows for safety-critical systems.  
Projects will be evaluated on **software robustness, safety reasoning, architectural clarity, and verification rigor**, not on physiological accuracy.
