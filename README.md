# Garage Safety Model Checking: UL 325 Formal Verification

Formal modeling and automated verification of a safety-critical residential garage door controller compliant with the **UL 325** safety standard. Implemented in **SMV** and verified using **NuSMV** against temporal logic specifications (LTL/CTL).

---

## Table of Contents
- [Overview](#overview)
- [System Architecture & Safety Model](#system-architecture--safety-model)
- [Formal Properties Verified](#formal-properties-verified)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Verification Results](#verification-results)
- [Documentation](#documentation)

---

## Overview

Automated garage door systems represent cyber-physical systems where software or hardware faults can cause severe physical injury or property damage. This project translates the **UL 325 (Standard for Door, Drapery, Gate, Louver, and Window Operators and Systems)** safety requirements into a finite-state transition system (`garage_door_ul325.smv`).

Using symbolic model checking in NuSMV, the model exhaustively explores all reachable states to prove that safety invariants, obstruction reversal behaviors, and fail-safe degradation modes hold under every possible sequence of environmental inputs and sensor faults.

---

## System Architecture & Safety Model

The finite-state machine models the interaction between the physical door assembly, user controls, and mandatory entrapment protection devices:

* **Door Kinematics & Actuation**: Discrete states tracking door position (`OPEN`, `CLOSED`, `OPENING`, `CLOSING`, `STOPPED`) and motor direction commands.
* **Primary & Secondary Entrapment Protection**:
  * **Photoelectric Sensor (IR Beam)**: Non-contact optical sensor detecting obstructions across the door threshold.
  * **Contact / Inherent Force Reversal**: Edge sensor / current-sensing mechanism detecting physical contact during downward travel.
* **Control & Fault Handling**:
  * Wall console / remote RF push-button inputs (`OPEN`/`CLOSE`/`STOP` cycling).
  * Sensor fault injection and fail-safe lockout mechanisms preventing unattended closing when entrapment sensors are disconnected or malfunctioning.

---

## Formal Properties Verified

The model is verified against a comprehensive suite of temporal logic specifications covering:

1. **Safety Invariants (AG / G)**:
   * The motor never drives the door downward while the photoelectric beam is obstructed or a sensor fault is active.
   * Conflicting motor commands (`OPEN` and `CLOSE` simultaneously) are unreachable.
2. **Obstruction Reversal**:
   * Any obstruction detected during a closing cycle immediately halts downward motion and reverses the door to the fully open position within bounded steps.
3. **Fail-Safe Degradation**:
   * Upon entrapment sensor failure, automatic closing is inhibited unless constant-pressure manual override is maintained, in accordance with UL 325 mandates.
4. **Liveness & Progress (AF / F)**:
   * Under nominal conditions without persistent obstructions, valid open/close requests eventually reach their target end-states without deadlock or livelock.

---

## Project Structure

```text
garage-safety-model-checking/
├── garage_door_ul325.smv                    # NuSMV finite-state model and LTL/CTL specs
├── test_results.txt                         # Verification log and property check outputs
├── UL 325 Garage Door Safety Protocol.pdf   # Technical specification and formal verification report
└── README.md                                # Project documentation
```

---

## Getting Started

### Prerequisites
- [NuSMV](https://nusmv.fbk.eu/) (v2.6+) installed and available in your `PATH`.

### Running the Model Checker

1. Clone the repository:
   ```bash
   git clone https://github.com/mattiagasperoni/garage-safety-model-checking.git
   cd garage-safety-model-checking
   ```

2. Execute batch verification on the SMV model:
   ```bash
   NuSMV garage_door_ul325.smv
   ```

3. Optionally run in interactive mode to inspect counterexamples or simulate traces:
   ```bash
   NuSMV -int garage_door_ul325.smv
   NuSMV > go
   NuSMV > check_ctlspec
   NuSMV > check_ltlspec
   ```

---

## Verification Results

All safety, fail-safe, and liveness specifications defined in `garage_door_ul325.smv` have been exhaustively checked across the full reachable state space. Detailed execution logs and verification outcomes are recorded in [`test_results.txt`](test_results.txt).

---

## Documentation

For the complete state-transition diagrams, formalization of the UL 325 clauses, and detailed discussion of the verified temporal formulas, refer to [`UL 325 Garage Door Safety Protocol.pdf`](UL%20325%20Garage%20Door%20Safety%20Protocol.pdf).
