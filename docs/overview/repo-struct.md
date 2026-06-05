# Repository Structure Overview

This repository is organized to separate system design, hardware design, firmware implementation, ECU integration, testing, and capstone documentation.

The goal is to keep the project modular so each part of the paddle shift system can be developed, tested, and understood independently.

The main sections are:

* docs -> system understanding, requirements, and design decisions
* hardware -> PCB and electrical design files
* firmware -> STM32 embedded software
* ecu -> vehicle ECU integration logic
* tests -> validation and verification of the full system
* capstone -> academic documentation and deliverables

---

# docs/

This folder contains all system-level documentation.

Explains what the system is, how it works, and why design decisions were made.

## overview/

High-level description of the system.

Includes:

* system-intent.md -> what the paddle shift system is designed to do
* how-it-works.md -> overall system behavior and operation flow
* user-operation-flow.md -> how a driver interacts with the system

## requirements/

Defines what the system must achieve.

Includes:

* functional-requirements.md -> required system behaviors and features
* constraints.md -> limitations such as timing, hardware, and safety constraints

## architecture/

How the system is structured internally.

Includes:

* high-level-block-diagrams.pdf -> system-level architecture diagrams
* system-architecture.md -> explanation of components and interactions
* communication-protocols.md -> how PCB1, PCB2, and ECU communicate
* timing-diagrams.pdf -> signal timing and shift behavior

## design-decisions/

Explains why engineering choices were made.

Includes:

* spi-vs-can.md -> communication method selection
* debounce-strategy.md -> handling noisy paddle inputs
* safety-rules.md -> fail-safe behavior and system safety logic

---

# hardware/

This folder contains all PCB and electrical design files.

It is split into individual boards so each PCB can be designed and manufactured independently.

## steering-wheel-pcb/

PCB for steering wheel. Includes STM32 as well as other power circutry. 

Includes:

* schematic/ -> circuit schematic files
* layout/ -> PCB routing and design files
* fabrication/ -> manufacturing outputs (Gerbers, BOM, pick-and-place files)
* README.md -> explanation of board purpose, inputs, and outputs

## solenoid-pcb/

PCB to control solenoid fixed to spot where shift lever would be.

Includes:

* schematic/
* layout/
* fabrication/
* README.md -> explanation of board role in system

# firmware/

This folder contains all STM32 embedded software.

It defines the real-time behavior of the paddle shift system.

## shared/

Code used by both PCB firmware projects.

Includes:

* communication/ -> message formats and protocols
* drivers/ -> reusable hardware drivers
* utilities/ -> helper functions (timers, filters, debounce logic)

## steering-wheel-firmware/


Includes:

* src/ -> main application code
* include/ -> header files
* startup/ -> initialization and boot code
* README.md -> explanation of firmware behavior on this board

## solenoid-firmware/

*Omitted in the case CAN architecture isn't implemented and PWM is used instead*


Includes:

* src/
* include/
* startup/
* README.md -> explanation of board role and logic

## stm32_core/

Low-level microcontroller configuration.

Includes:

* clock-config/ -> clock setup
* hal-config/ -> STM32 HAL configuration
* interrupt-handlers/ -> interrupt service routines

---

# ecu/

This folder contains all software related to vehicle ECU integration.

The ECU is responsible for final decision-making and vehicle-level control actions based on inputs received from the paddle shift system. It evaluates shift requests and determines whether they can be executed safely.

The ECU layer is separated from STM32 firmware because it operates under different safety requirements, timing constraints, and communication responsibilities.

## src/

Contains main ECU logic.

Includes:

* gear request processing
* shift approval / rejection logic
* vehicle state evaluation
* control command generation

## include/

Contains ECU data structures and interfaces.

Includes:

* gear state definitions
* message structures
* fault and error codes
* function declarations

## communication/

Handles communication between ECU and paddle shift system.

Includes:

* CAN/LIN message parsing
* signal mapping between systems
* request/response handling
* message validation

## safety/

Contains safety-critical logic.

Includes:

* invalid shift rejection logic
* timeout handling (missed signals)
* fail-safe states
* system fault handling and recovery

## calibration/

Contains tunable parameters.

Includes:

* shift timing thresholds
* RPM/speed limits
* debounce constants
* vehicle-specific tuning values

## README.md/

Explains:

* ECU role in system architecture
* interaction with STM32 boards
* communication pathways
* safety assumptions and constraints

---

# tests/

This folder contains all verification and validation for the system.

It ensures hardware and software work correctly together under real-world and edge conditions.

## unit/

Tests for individual logic components.

Includes:

* debounce logic tests
* state machine tests
* communication encoding/decoding tests

## integration/

Tests for system interactions.

Includes:

* PCB1 ↔ PCB2 communication tests
* STM32 ↔ ECU communication tests
* full paddle shift cycle tests
* latency and timing verification

## hardware_in_loop/

Tests using real or simulated hardware signals.

Includes:

* simulated paddle inputs
* STM32 response validation
* ECU response validation
* output signal verification

## simulation/

Software-only testing environment.

Includes:

* shift logic simulation
* timing models
* edge-case behavior testing

---

# capstone/

This folder contains all academic and submission-related documentation.

It is separated from engineering documentation to keep the system design clean.

## proposal/

Includes:

* project proposal
* initial concept description

## progress_reports/

Includes:

* milestone updates
* development logs
* interim submissions

## final_report/

Includes:

* final written report
* system evaluation
* results and discussion

## presentation/

Includes:

* slides.pdf
* poster.pdf
* demo materials

## reflection/

Includes:

* personal reflection
* lessons learned
* design improvements

---

# Summary

* docs/ -> system understanding and design reasoning
* hardware/ -> physical PCB design
* firmware/ -> STM32 embedded software
* ecu/ -> vehicle-level decision and control logic
* tests/ -> validation of system behavior
* capstone/ -> academic documentation and deliverables

Each section is separated to ensure the system remains modular, scalable, and easy to maintain as it evolves.
