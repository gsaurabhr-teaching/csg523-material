# CS G523 – Capstone Project  
## p1-nrf52-bootloader – Portable Bootloader for nRF52840 / nRF52820  
**(CDC + BLE DFU, Single-Button Entry)**

---

## 1. Problem Statement / Specification

Design and implement a **portable, robust embedded bootloader** for Nordic Semiconductor **nRF52840**, with explicit design choices that allow the same bootloader architecture to be **ported to nRF52820 with minimal changes**.

The bootloader shall:
- Support **firmware update (DFU)** over:
  - USB CDC (virtual serial port), and
  - Bluetooth Low Energy (BLE)
- Support **single-button operation** for bootloader entry:
  - A **double button press within a short time window** shall cause the system to enter bootloader mode
  - Any other button behavior shall result in a normal application start
- Reliably distinguish between:
  - normal power-on reset
  - user-requested bootloader entry
  - reset after firmware update
- Ensure that **invalid, incomplete, or corrupted firmware updates do not brick the device**

The system must behave **deterministically and conservatively**, prioritizing recoverability over convenience.

---

## 2. Motivation and Context

Bootloaders are a critical part of deployed embedded systems, especially for:
- IoT devices
- Wearables
- Remote or inaccessible systems

A failure in bootloader software can:
- permanently brick devices
- prevent recovery after partial updates
- leave devices in undefined or unsafe states

In real products, bootloaders must:
- work across hardware variants
- support multiple update transports
- provide a simple and reliable user entry mechanism
- remain small, robust, and easy to verify

This project focuses on **embedded software architecture, fault handling, and portability**, not on application-level features.

---

## 3. System Boundary

### In Scope
- Bootloader software running from reset
- Startup decision logic (bootloader vs application)
- Firmware update over USB CDC
- Firmware update over BLE
- Flash memory layout and image management
- Button-based bootloader entry logic
- Validation of firmware images before execution

### Out of Scope
- Application firmware functionality
- Mobile or PC UI design for DFU tools
- Cryptographic security beyond basic integrity checks
- Manufacturing-time programming workflows

---

## 4. Hardware and Platform Assumptions

### Target MCUs
- **Primary:** nRF52840
- **Secondary (portability target):** nRF52820

The bootloader must:
- avoid hard-coding device-specific details where possible
- isolate hardware-specific code
- clearly document what changes are required to support nRF52820

---

## 5. Bootloader Entry Mechanism (Single Button)

The system provides **only one physical button**.

### Required Behavior
- On reset or power-up, the bootloader shall:
  - monitor the button for a short, bounded time window
  - detect a **double press pattern**
- If a valid double press is detected:
  - enter bootloader (DFU) mode
- Otherwise:
  - transfer control to the application firmware

### Constraints
- False entry into bootloader should be rare
- Button bounce and timing uncertainty must be handled
- Behavior must be deterministic and well-defined

Precise timing values and detection logic are to be **designed and justified by the team**.

---

## 6. Firmware Update Interfaces

### USB CDC DFU
- Device enumerates as a virtual serial port
- Firmware image is transferred in packets
- The bootloader manages:
  - packet ordering
  - error detection
  - recovery from interrupted transfers

### BLE DFU
- Firmware image is transferred over BLE
- The bootloader manages:
  - connection establishment
  - packet reception
  - update progress and completion

The internal DFU logic should be **transport-agnostic**, with CDC and BLE acting as interchangeable front-ends.

---

## 7. Safety and Robustness Expectations (High-Level)

At a high level, the bootloader is expected to:
- Never jump to an invalid or partially written application
- Always remain reachable after a failed update
- Handle power loss during firmware update safely
- Avoid undefined behavior on reset or unexpected input
- Prefer staying in bootloader mode over running a potentially invalid application

Exact validation checks, memory layouts, and recovery strategies are **intentionally left unspecified** and must be justified by the team.

---

## 8. What Is Intentionally Not Provided

The following are deliberately not specified:
- Exact flash memory map
- Image validation mechanisms
- DFU packet formats
- Button timing thresholds
- Transport-specific protocol details

Teams are expected to:
- derive detailed requirements
- design a portable architecture
- justify trade-offs between simplicity, robustness, and size
- validate behavior through systematic testing

---

## 9. Expected Outcome

By the end of the project, teams should be able to demonstrate:
- A working bootloader on nRF52840
- A clear portability plan for nRF52820
- Reliable DFU over USB CDC
- Reliable DFU over BLE
- Correct single-button bootloader entry behavior
- Safe recovery from interrupted or invalid updates

---

**Note:** This project emphasizes **boot-time software correctness and recoverability**, which are often more critical than application-level features in deployed embedded systems.
