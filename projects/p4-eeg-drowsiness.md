# CS G523 – Capstone Project  
## p1 – Driver Drowsiness Detection using Wearable EEG  
**(UART-Simulated EEG Input, BLE Optional Extension)**

---

## 1. Problem Statement / Specification

Design and implement an **embedded software system that detects driver drowsiness using EEG data** and raises appropriate alerts when unsafe cognitive states are inferred.

The system shall:
- Receive EEG data as **simulated streams over UART** (provided by a PC-based emulator)
- Support live EEG data acquisition over BLE as an extension
- Process EEG data in real time to infer drowsiness-related conditions
- Detect transitions from safe to unsafe cognitive states
- Trigger alerts or safety signals when drowsiness persists
- Behave deterministically and conservatively in the presence of noisy, missing, or inconsistent EEG data

All safety-relevant decisions must be made **locally on the embedded system** and must not depend on cloud connectivity or external computation.

---

## 2. Motivation and Context

Driver drowsiness is a major contributor to road accidents, especially in long-duration driving, night driving, and semi-autonomous vehicle operation.

EEG-based approaches are studied because they:
- detect cognitive fatigue earlier than behavioral cues
- are robust to lighting conditions and visual occlusion
- provide a direct physiological measure of alertness

In real deployments, EEG-based systems must cope with:
- noisy and non-stationary signals
- user-specific variability
- sensor dropouts and artifacts

This project focuses on **robust embedded software design and safety reasoning**, not on clinical EEG analysis.

---

## 3. System Boundary

### In Scope
- Embedded software responsible for:
  - EEG data reception over UART
  - Windowing and basic preprocessing
  - Computation of simple EEG-derived metrics
  - Drowsiness inference and state management
  - Alert generation

### Out of Scope
- EEG hardware and electrode design
- Medical diagnosis
- Advanced signal processing or deep learning
- UI-heavy visualization systems
- Cloud-based decision making

---

## 4. EEG Input Specification (UART-Based Simulation)

### Rationale for UART Simulation

EEG simulation over UART is **intentional** and allows:
- clean and repeatable testing
- systematic fault injection
- isolation of safety logic from communication complexity

The BLE implementation (if attempted) must be swappable without modifying the core drowsiness logic.

---

### EEG Channel Configuration

The simulated EEG data represents **6 channels** corresponding to broad scalp regions:

- Frontal (F1, F2)
- Temporal (T1, T2)
- Occipital (O1, O2)

Each channel provides sampled EEG amplitude values.

---

### UART Data Characteristics

- EEG samples arrive as frames over UART
- Frames may contain:
  - samples from all channels, or
  - samples from a subset of channels
- Sampling rate may vary
- Frames may be delayed, missing, or malformed
- Noise and artifacts are intentionally injected

---

### Example Frame Format (Illustrative)

```
$EEG,
CH=F1:12.3,F2:10.9,T1:8.1,T2:7.6,O1:6.4,O2:6.7,
TS=123456
*
```

Another frame may omit channels or arrive later in time.

---

## 5. EEG Metrics to Be Computed (Guidance Provided)

To reduce ambiguity while still encouraging design choices, teams are expected to compute **at least the following metrics** over fixed-duration windows (e.g., 1–5 seconds):

### Required Metrics
1. **Alpha / Beta Power Ratio**
   - Alpha band (≈8–13 Hz)
   - Beta band (≈13–30 Hz)
   - Elevated alpha-to-beta ratios are commonly associated with drowsiness

2. **Signal Variance or Energy**
   - Used as a coarse indicator of signal activity and quality
   - Can help detect dropouts or flatlining signals

Teams may compute these metrics:
- per channel, or
- aggregated across regions (e.g., frontal vs occipital)

Exact implementation details are left to the team but must be justified.

---

## 6. Safety Expectations (Intentionally High-Level)

At a high level, the system is expected to:
- Avoid declaring “safe” states under uncertain or poor-quality EEG data
- Prefer conservative alerts over missed drowsiness
- Detect and handle prolonged signal loss
- Avoid rapid oscillations between alert states

Precise definitions of:
- drowsiness thresholds
- timing windows
- state transitions
are **intentionally left unspecified** and must be derived and justified by each team.

---

## 7. What Is Intentionally Not Provided

The following are deliberately not specified:
- Exact thresholds or decision logic
- Complete state machine definitions
- Task or scheduling models
- Communication protocol details beyond UART framing

Teams are expected to:
- derive requirements from the problem statement
- design and justify an architecture
- define conservative safety policies
- validate behavior using both clean and noisy simulated EEG data

---

## 8. Expected Outcome

By the end of the project, teams should be able to demonstrate:
- A working embedded drowsiness detection system
- Robust handling of noisy, partial, and missing EEG data
- Clear architectural separation between data acquisition and safety logic
- Justified safety decisions backed by test evidence

---

**Note:** UART-based EEG simulation reflects industry validation practices used before deploying physiological sensing systems.  
Projects will be evaluated on **software robustness, safety reasoning, architectural clarity, and verification rigor**, not on clinical accuracy.
