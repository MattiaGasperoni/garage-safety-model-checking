# Garage Door Safety Model Checking (UL 325)

Formal verification of a garage door controller safety protocol using symbolic model checking (**NuSMV** / **nuXmv**), modeled against **UL 325** safety requirements.

## Repository Structure

| Path | Description |
| :--- | :--- |
| `garage_door_ul325.smv` | NuSMV state-machine model of the garage door controller, sensors, actuators, and safety specifications (CTL/LTL). |
| `UL 325 Garage Door Safety Protocol.pdf` | Reference specification document detailing UL 325 safety behaviors, reversal/obstruction logic, and edge cases. |
| `test_results.txt` | Recorded verification output and model-checking results from running the specification suite. |

## Overview

Safety-critical electromechanical systems—such as motorized garage doors—require exhaustive state-space verification to ensure hazardous conditions cannot be reached under valid or adversarial sensor/command interleavings.

This project models the controller's state transitions, obstruction/safety sensor signals, motor commands, and timing/safety invariants in **SMV**, enabling automated verification of safety and liveness properties rather than relying solely on ad-hoc simulation or unit tests.

## Prerequisites

* **NuSMV** (2.6.0+) or **nuXmv** installed and available on your `PATH`.

## Usage

### Run Model Checking

Execute the SMV model directly from the repository root:

```bash
NuSMV garage_door_ul325.smv
