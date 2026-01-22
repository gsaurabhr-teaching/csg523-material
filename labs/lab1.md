**Student Name:**  

# CS G523 – Software for Embedded Systems
## Lab 1: Requirements Engineering

### Objective
This lab introduces requirements engineering as practiced in embedded and safety-critical systems.  
The goal is to learn how to:
- identify system boundaries and assumptions,
- write clear, testable requirements,
- distinguish requirements from design and implementation decisions.

---

## Background

In embedded systems, requirements:
- define system obligations,
- constrain unsafe behavior,
- drive architecture and verification.

Poorly written requirements lead to ambiguous behavior and unverifiable systems.

---

## Submission guidelines

1. Individual submission as described below under `tasks` should be submitted on Google Classroom.
   > Add your name on top of this sheet.

   > Task 0 should be completed by entering your priorities in this same file in the infusion pump section below.

   > Tasks 1 - 4 should be completed by adding your responses to the template below.

   > After completing this document, paste it into `https://apitemplate.io/pdf-tools/convert-markdown-to-pdf/`, generate the PDF and submit it on Google Classroom.

2. **Group submission:**  
   > 1. As a group, pool together your requirements, discuss and come up with a concise list (priority 1 and 2 only).  
   > 2. Add these to the `docs/sections/requirements.md` in your group repository, commit and push to GitHub.
---

# Tasks

## Task 0: Analyze example requirements
You will find an example requirements sheet below for the infusion pump. Read through it.  
For each requirement, think about how important it is to include the requirement, and mark the priority level (1/H-absolutely essential, 2/M-good to have, 3/L-superficial/not important).  

### Worked Example: Infusion Pump – Requirements with Testability Intent and Priority

This document provides a **worked-out, detailed example** of requirements engineering
for a medical infusion pump.  
It augments each requirement with:
- explicit **testability intent** (how violation would be detected), and
- a **priority classification column** (High / Medium / Low) for students to fill in.

---

### Problem Description

An infusion pump delivers medication to a patient at a prescribed rate.
The system must:
- deliver medication accurately,
- prevent unsafe delivery,
- respond to faults in a predictable and conservative manner,
- alert caregivers when intervention is required.

---

### System Boundary

#### Inside the System
- Embedded control software
- Motor actuation commands
- Monitoring of sensors (flow rate, occlusion, door status)
- Alarm generation logic

#### Outside the System
- Medication reservoir and tubing
- Patient physiology
- Caregiver interaction
- External power source

#### Assumptions
- Sensors provide periodic updates
- Power loss and resets are possible
- Caregivers respond to alarms

---

### Functional Requirements

| ID | Requirement | Testability Intent | Priority |
|----|-------------|--------------------|----------------------|
| R-F1 | The system shall deliver medication at the configured infusion rate during normal operation. | Violation is detected if the measured delivery rate deviates beyond allowable tolerance from the configured rate during normal operation. |  |
| R-F2 | The system shall support starting and stopping infusion through explicit user commands. | Violation is detected if infusion starts or stops without a corresponding explicit user command. |  |
| R-F3 | The system shall prevent infusion unless all required configuration parameters are valid. | Violation is detected if infusion begins while one or more required configuration parameters are missing or invalid. |  |

---

### Safety Requirements

| ID | Requirement | Testability Intent | Priority |
|----|-------------|--------------------|----------------------|
| R-S1 | The system shall stop medication delivery within a bounded time after detecting an occlusion. | Violation is detected if medication delivery continues beyond the specified response time after occlusion detection. |  |
| R-S2 | The system shall not permit infusion when the pump door is open. | Violation is detected if infusion is active while the door-open condition is present. |  |
| R-S3 | The system shall prevent delivery of medication beyond the configured dose limit. | Violation is detected if cumulative delivered dose exceeds the configured limit. |  |

---

### Fault Detection and Fail-Safe Behavior

| ID | Requirement | Testability Intent | Priority |
|----|-------------|--------------------|----------------------|
| R-FD1 | The system shall detect invalid or missing sensor data. | Violation is detected if invalid or missing sensor data does not result in a fault indication. |  |
| R-FD2 | Upon detection of a sensor fault, the system shall transition to a safe, non-infusing state. | Violation is detected if infusion continues after a sensor fault has been detected. |  |
| R-FD3 | The system shall use a watchdog mechanism to detect internal software failures. | Violation is detected if internal software failure occurs without watchdog intervention. |  |

---

### Timing and Performance Requirements

| ID | Requirement | Testability Intent | Priority |
|----|-------------|--------------------|----------------------|
| R-T1 | The system shall monitor flow rate at a minimum specified frequency during active infusion. | Violation is detected if flow rate updates occur less frequently than specified during infusion. |  |
| R-T2 | The system shall activate alarms within a bounded time after detecting safety-critical faults. | Violation is detected if alarm activation exceeds the specified response time following fault detection. |  |

---

### Power and Reset Behavior

| ID | Requirement | Testability Intent | Priority |
|----|-------------|--------------------|----------------------|
| R-P1 | The system shall stop infusion immediately upon loss of power. | Violation is detected if medication delivery continues after power loss. |  |
| R-P2 | The system shall not automatically resume infusion after a power cycle or reset. | Violation is detected if infusion resumes without explicit user action after power restoration. |  |
| R-P3 | The system shall remember the configuration and status just prior to power loss. | Violation is detected if configuration or infusion status is lost or corrupted after power restoration. |  |

---

### Alarm and Notification Requirements

| ID | Requirement | Testability Intent | Priority |
|----|-------------|--------------------|----------------------|
| R-A1 | The system shall generate alarms for all safety-critical conditions. | Violation is detected if a safety-critical condition occurs without a corresponding alarm. |  |
| R-A2 | Safety-critical alarms shall have higher priority than advisory notifications. | Violation is detected if advisory notifications delay, suppress, or override safety-critical alarms. |  |

---

### Data Integrity and Logging

| ID | Requirement | Testability Intent | Priority |
|----|-------------|--------------------|----------------------|
| R-D1 | The system shall log infusion start, stop, and fault events. | Violation is detected if a required event occurs without a corresponding log entry. |  |
| R-D2 | Logged safety events shall persist across power cycles. | Violation is detected if logged safety events are missing after a power cycle. |  |

---

## Key Takeaway

Good requirements:
- define *what* the system must do,
- constrain unsafe behavior,
- are **testable**, and
- can be **prioritized** to guide design trade-offs.
 ---
 ---

## Task 1: Identify System Boundaries
Answer briefly:

### 1.1 Functionality Inside the System
(Describe what the system is responsible for)

- 
- 
- 

### 1.2 Entities Outside the System
(Describe external actors, hardware, users, environment)

- 
- 
- 

### 1.3 Environmental Assumptions
(State assumptions about sensors, users, power, timing, environment)

- 
- 
- 

---
---

## Task 2–4: Requirements, Classification, Rationale, and Testability

Write **at least three requirements**.  
Add more rows if needed.

| Requirement ID | Requirement (single declarative sentence) | Classification (Functional / Safety / Non-Functional) | Rationale (Why important? What could go wrong if violated?) | Test Intent / Testability (How violation would be detected) |
|---------------|-------------------------------------------|--------------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------|
| R-1 |  |  |  |  |
| R-2 |  |  |  |  |
| R-3 |  |  |  |  |
| R-4 (optional) |  |  |  |  |
| R-5 (optional) |  |  |  |  |

---

## Key Reminder

> If a requirement cannot be tested, it is not a requirement.  

## Submission Checklist

Before submitting, ensure that:

- [ ] At least **one functional** requirement is included  
- [ ] At least **one safety or non-functional** requirement is included  
- [ ] All requirements are **testable**  
- [ ] No implementation details (language, OS, protocol) are mentioned  

---

