# Scope Cell

**English** · [Deutsch](README.de.md)

### A robot cell you can follow, one command at a time.

A Universal Robots UR5e takes a part from a conveyor, turns its inspection face toward a camera and places it in the assigned bin. Alongside the movement, Scope Cell shows the active command, all six joint angles, the tool position and the signals the next step is waiting for.

I built this local simulation with Codex to connect my experience in robotics and camera-based inspection with an interactive software project. The cell and its parts are fictional; no employer designs or operating data are included.

![A complete pickup, inspection and sorting cycle, with commands and joint telemetry](assets/cycle-preview.gif)

*Recorded from the working application. This preview plays at 3×; the [45-second video](assets/scope-cell-ur5e-demo.mp4) runs at 1×. The small inset is the simulated camera's actual view of the 3D scene.*

## What the demo does

- **Movement follows commands.** Joint moves and straight tool paths drive the model, telemetry and command progress from the same state.
- **The pickup is part of the process.** The part travels to the stop on the conveyor. The gripper descends, closes, waits for confirmation and lifts vertically before transferring it.
- **The camera gets a clear view.** The robot turns the part toward the side camera. The model checks the face orientation and five sight lines before starting inspection.
- **Faults interrupt the sequence.** A missing camera result holds the part. A full bin blocks placement. After fixing either cause, the operator must explicitly resume.
- **Each part has a history.** Its identifier connects inspection, route selection and placement. The full run can be exported locally.

The robot uses the original **UR5e model and nominal joint geometry from Universal Robots**. The gripper, conveyor, camera and bins are simplified demo components. Scope Cell has its own motion runtime; it does not run URSim or a Universal Robots controller. The command display follows URScript conventions, with separate `cell.*` commands for the simulated equipment.

## See the inspection pose

![The UR5e presents a part to the side camera, with the camera view and current command visible](assets/inspection-frame.png)

The inspection result is a seeded synthetic value. **There is no image-recognition model in this project.** The camera view helps inspect the pose; it does not generate the classification.

## Verification

**47 automated checks passed** on Node 24 and 25. They cover motion, part tracking, recovery and geometry, including a deliberately invalid path through the conveyor. The completed browser runs also matched the local engine results.

| Same 12-part batch | Normal inspection | Slow inspection |
| --- | ---: | ---: |
| Camera delay in the model | 1.1 s | 6.0 s |
| Total simulated time | 498.74 s | 557.58 s |
| Throughput in the model | 86.6 parts/h | 77.5 parts/h |
| Sorting result | 8 pass / 1 review / 3 reject | 8 pass / 1 review / 3 reject |

These numbers describe this fictional cell and its chosen timing. They are not UR5e performance specifications or measurements from a physical system. [Checks, settings and limitations](docs/VERIFICATION.md).

[Design and implementation decisions](docs/CASE_STUDY.md) explain how the movement, interlocks and visible commands fit together.

## Publication and rights

This repository contains the **presentation only**. Application source, executable builds, local model copies, configurations and detailed test exports stay private. There is no hosted public application.

Original portfolio material is not offered under an open-source or Creative Commons license. See [rights](RIGHTS.md). The UR5e geometry belongs to its upstream authors and is available separately under BSD-3-Clause; its existing license is unchanged. See [model source and third-party notice](THIRD_PARTY_NOTICES.md).

Independent project by **LeonScope**, developed with Codex. No Universal Robots partnership or endorsement is implied.
