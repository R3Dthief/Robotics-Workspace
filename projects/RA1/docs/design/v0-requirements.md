# RA1 v0 System Requirements

_Last updated: 2025-11-28_

## 1. System Overview

RA1 v0 is a prototype 5-DOF robotic arm designed for general manipulation tasks, experimentation, and skill development in mechatronics, CAD, and control systems. The system emphasizes modularity, 3D-printed structural components, low-cost actuators, and rapid iteration. RA1 v0 is not intended for production or continuous-duty workloads but must demonstrate reliable joint motion, positional repeatability, and safe power handling. Future versions will refine structure, materials, and actuator selection as design constraints become clearer.

## 2. Functional Requirements

- RA1-FR-001 — The system shall provide at least 5 controllable degrees of freedom (Base Yaw, Shoulder Pitch, Elbow Pitch, Wrist Pitch, Wrist Roll).
- RA1-FR-002 — The system shall include an end-effector capable of basic grasping and releasing objects
- RA1-FR-003 — The system shall allow manual or programmatic control of every joint via a microcontroller.
- RA1-FR-004 _ The system shall provide power isolation, including a main power switch and an emergency stop.
- RA1-FR-005 _ The system shall allow modular replacement of actuators, arm segments, and the end-effector without redesigning the full arm.
- RA1-FR-006 _ The system shall support joint calibration or homing to ensure repeatable movement.
- RA1-FR-007 _ The system shall log or expose actuator status (position, state) where available.

## 3. Non-Functional Requirements

- RA1-NFR-001 — The system shall be designed for iterability, enabling reprinting or replacing components with minimal downtime.
- RA1-NFR-002 — The structural components (PETG) shall support at least 5 load cycles per minute during testing without deformation or failure.
- RA1-NFR-003 _ The system shall be designed for ease of assembly, requiring only common hand tools.
- RA1-NFR-004 _ All wiring shall be arranged with safe cable routing to prevent entanglement, abrasion, or interference with joints.

## 4. Performance Targets

- RA1-PT-001 — The arm shall achieve a horizontal reach of 350–450 mm measured from base center to wrist center.
- RA1-PT-002 — The arm shall be capable of lifting and holding a payload of at least 500 g at half extension.
- RA1-PT-003 _ The base and shoulder joints shall withstand a sustained torque load equal to the rated torque of their servos without mechanical failure
- RA1-PT-004 _ Positional repeatability for joints shall be within ±5° for v0.
- RA1-PT-005 _ The system shall power all servos simultaneously with a stable 5–7.4 V supply capable of delivering at least 15–20 A peak.

## 5. Interfaces

- RA1-IF-001 —The system shall expose joint control interfaces via PWM or serial bus, depending on the servo class used.
- RA1-IF-002 _The end-effector shall mount to the wrist via a standardized bolt pattern allowing future tool swaps.
- RA1-IF-003 _The system shall connect to the microcontroller via USB for programming and debugging.
- RA1-IF-004 _Power input shall connect via a dedicated high-current DC input connector rated above system load.


## 6. Constraints and Assumptions

- RA1-C-001 — Structural components for v0 shall be printed primarily in PETG; stronger materials may be used in future versions.
- RA1-C-002 _ Actuators shall be hobby-class high-torque servos, not industrial-grade motors.
- RA1-C-003 _ The arm shall operate only on a stable fixed base and is not designed for mobile platforms in v0.
- RA1-C-004 _ The system shall maintain safe operation within indoor environments under standard temperature and humidity.
- RA1-A-001 — v0 assumes the use of affordable, readily available Amazon/Prime parts and 3D-printed components.
- RA1-A-002 _ v0 assumes the use of a bench-top test area, not a hazardous or industrial environment.
- RA1-A-003 _ It is assumed the user has access to basic CAD tools, a 3D printer, and Git/GitHub for version control.
- RA1-A-004 _ v0 does not require precise accuracy; it is assumed improvements will come in v1–v3 as designs mature.
