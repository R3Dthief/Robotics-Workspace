# RA1 v0 CAD Architecture Document

*Last updated: 2025-11-28*

## 1. Global Design Principles

  ### 1.1 Units & Coordinate System

    * Units: millimeters (mm)
    * Coordinate System:

      * Z+ = upward (vertical)
      * X+ = forward (arm extending outward)
      * Y+ = left from operator’s perspective
    * All assemblies shall use a consistent origin at the center of the base rotation axis.

  ### 1.2 Modeling Standards

    * All components shall be fully parametric.
    * Critical dimensions must be driven by parameters.
    * Joint clearances defined using offset parameters.
    * Parts designed for 3D printability with minimal supports.

  ### 1.3 File & Assembly Naming Convention

  Format: `RA1-<Subsystem>-<Component>-v<Revision>.<ext>`

  Examples:

  * `RA1-BASE-LowerHousing-v0.step`
  * `RA1-J1-ShoulderBracket-v1.step`
  * `RA1-J2-ElbowGearbox-v0.stl`
  * `RA1-WRIST-RollModule-v0.step`

  Folder alignment:

  ```
  RA1/
  └─ cad/
      ├─ assemblies/
      ├─ parts/
      ├─ exports/
      │   ├─ step/
      │   └─ stl/
      └─ architecture/
  ```

## 2. Mechanical Architecture Overview

RA1 v0 consists of:

  * Base Rotation (J0)
  * Shoulder Pitch (J1)
  * Elbow Pitch (J2)
  * Wrist Pitch (J3)
  * Wrist Roll (J4)
  * Gripper (End Effector)

All joints beyond J1 shall be modular.

## 3. Joint-by-Joint Architecture Specification

  ### 3.1 Base Assembly (J0)

    * High-torque servo (30–40 kg·cm)
    * Dual 608 or 6000-series bearings
    * Gear or timing pulley drive
    * Future slip ring integration planned
    * Minimum PETG thickness: 6 mm
    * Bearing pockets: -0.1 mm interference

  ### 3.2 Shoulder Joint (J1)

    * Servo: 25–30 kg·cm
    * Dual bearings + 8–10 mm steel shaft
    * Servo offset with linkage or gear drive
    * Shaft clearance: +0.1 to +0.15 mm

  ### 3.3 Elbow Joint (J2)

    * Servo: 15–20 kg·cm
    * Similar architecture to J1 but lighter
    * Wall thickness ≥ 3.5 mm
    * Reinforced ribs required

  ### 3.4 Wrist Pitch (J3)

    * Servo: 10–15 kg·cm
    * Embedded servo design
    * Low-mass structure preferred

  ### 3.5 Wrist Roll (J4)

    Options:

      1. Spur gear (module 1.0–1.5)
      2. GT2 belt drive (6 mm width)

      * Axle: 6–8 mm steel

  ### 3.6 End Effector (Gripper)

    * Servo: 5–10 kg·cm
    * Parallel-jaw design
    * Mounting: 20x20 mm, 4× M3 bolts

## 4. Hardware Standards

  ### 4.1 Fasteners

    * M3 for general assembly
    * M4 for high-load joints
    * PETG clearance: M3 (3.2–3.4 mm), M4 (4.2–4.4 mm)

  ### 4.2 Bearings

    * J0: 608 or 6000-series
    * J1/J2: 608 or 6001
    * J3/J4: 604/605 or bushings

  ### 4.3 Shafting

    * Steel: 8 mm (preferred), 10 mm for high stiffness

## 5. Material Rules (PETG)

  * Structural walls: 3.5–6 mm
  * Ribs: 2–3 mm
  * Servo mounts: ≥ 5 mm
  * Infill: 40–60%
  * Layer height: 0.2–0.28 mm

## 6. Modularity & Assembly Rules

  * All joints must be removable without total disassembly.
  * End effector uses standard mount pattern.
  * Servo mounts allow mixed-brand replacement.
  * All pivots use bearings + shafts, never servo shafts.
  * Cable routing required in every joint.

## 7. Revisioning & File Lifecycle

  * v0: Prototype
  * v1: Functional refinement
  * v2: Material upgrades
  * v3: Pre-production

Deprecated parts stored in `/cad/legacy`.

## 8. Future Architecture (v1+ Planning)

  * Slip ring for continuous rotation
  * Aluminum reinforcement
  * Smart servos
  * Carbon fiber arms
  * IK model for advanced control

## 9. System Block Diagram

  flowchart TD

  PC[Host Computer] -->|USB/Serial| MCU[ESP32 Microcontroller]

  MCU -->|PWM / Serial Bus| J0[Base Servo]
  MCU -->|PWM| J1[Shoulder Servo]
  MCU -->|PWM| J2[Elbow Servo]
  MCU -->|PWM| J3[Wrist Pitch Servo]
  MCU -->|PWM| J4[Wrist Roll Servo]
  MCU -->|PWM| GRIP[Gripper Servo]

  PSU[5–7.4V Servo Power Supply] --> J0
  PSU --> J1
  PSU --> J2
  PSU --> J3
  PSU --> J4
  PSU --> GRIP

ESTOP[E-Stop Switch] -->|Cuts Servo Power| PSU

## 10. Mechanical Subsystem Overview

  RA1 v0 uses 3D-printed PETG components reinforced with ribs and metal shafting.
  All load-bearing rotational joints (J0–J2) use steel shafts supported by radial bearings, ensuring that servo output shafts never carry bending loads.
  Modularity allows removal and replacement of joints without full disassembly.

## 11. Electrical Subsytem Overview

  The RA1 v0 electrical subsystem provides clean power delivery, actuator control, and safe isolation of the system during testing.

# Power

  Servo rail: 5–7.4V, supplied by a dedicated high-current DC supply (15–20A peak).
  Logic rail: 5V USB via the ESP32 or external regulator.
  An E-stop physically cuts servo power while leaving MCU power active.

# Control

  ESP32 supplies PWM signals to standard hobby servos or serial commands to smart servos.
  All servo grounds are tied to MCU GND to maintain reference consistency.

# Wiring

  Power wires: 16–20 AWG
  Signal wires: 22–26 AWG
  Connectors: JST-VH or equivalent locking connectors
  Cable routing channels integrated into each joint

# Sensors (Optional v0)

  Limit switches for joint homing
  Current sensing on servo power rail
  Slip ring (v1+)

## 12. Firmware Responsibilities

  Firmware must provide real-time control, homing logic, and operational safety for the arm.

  # Core Responsibilities

  Generate PWM (or serial) signals to control all joints
  Read any available servo feedback (position, error flags)
  Manage system state: enabled, disabled, homing, calibration
  Debounce and process E-stop input (fail-safe behavior)
  Provide serial command interface to host computer
  Implement basic motion functions:
    - move_joint(joint_id, angle)
    - set_speed(joint_id, speed)
    - home()

  # Safety Behaviors

    Immediately disable servo signals when E-stop is pressed
    Reject commands outside safe joint limits
    Provide debugging logs over serial

## 13. Software Responsibilities

  Software (host-side or simple terminal tools) is responsible for sending commands, calibrating joints, and managing high-level behavior.
  # Responsibilities
    Provide a simple UI (CLI or minimal GUI) for:
      - Moving joints
      - Running calibration
      - Saving/recalling joint positions
      - Starting automated test sequences
    Send structured commands to the MCU (via USB serial)
    Display real-time feedback (joint angle, status)
    Log calibration data
    Handle configuration files (joint limits, default home angles)

  # v0 UI Requirement

    A simple serial terminal interface is sufficient.
    Advanced UI can be added in v1+.

## 14. Data Flow Between Components
  # Data Flow Table
    | Source | Destination     | Data Type             | Description                                     |
    | ------ | --------------- | --------------------- | ----------------------------------------------- |
    | PC     | MCU             | Serial Commands       | Move, calibrate, status request                 |
    | MCU    | Joints (Servos) | PWM or Serial Signals | Position/velocity commands                      |
    | Servos | MCU             | Feedback (optional)   | Position, torque, flags (only for smart servos) |
    | MCU    | PC              | Serial Logs           | Telemetry, errors, angles                       |
    | PSU    | Servos          | Power                 | High-current 5–7.4V rail                        |
    | E-stop | PSU             | Hardware Cutoff       | Removes servo power instantaneously             |

  # Data Flow Diagram
    [PC] <--> [MCU] --> [Servo Controller Layer] --> [Joints]
                    ^  
                    |  
                 Feedback (optional)
