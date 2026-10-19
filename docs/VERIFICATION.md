# Verification — 19 October 2026

[Deutsch](VERIFICATION.de.md) · [Project](../README.md)

These are local simulation results. The test code, complete logs and machine-readable run exports are retained privately. This public summary is a report of the work performed, not a publicly reproducible source release or an independent certification.

## Automated checks

**47 tests passed, none failed or skipped**, on Node 25.9.0 and Node 24.19.0. TypeScript checking and the production build passed.

| Area | What was checked |
| --- | --- |
| Robot geometry | Nominal forward kinematics against an independent UR5e DH calculation; target positions and orientations; joint limits; unreachable targets rejected |
| Movement | Joint speed and acceleration limits in the model; straight Cartesian tool paths; endpoint accuracy; slower motion changes duration without changing the target |
| Process state | Pause freezes time, feed and motion; one-command stepping; an operator hold retains the interrupted command |
| Part identity | One placement per identifier; matching inspection results; incoming-part events stay associated with the arriving part |
| Recovery | Camera timeout retains the part; full-bin interlock prevents release; acknowledgment leaves the system paused until explicitly resumed |
| Geometry checks | Belt/part height alignment; pickup and release positions; open-gripper clearance; distinct bin slots; visible camera face and five sight lines |
| Negative controls | A path through the conveyor is rejected; a downward-facing part fails the camera-pose check; a mismatched inspection response blocks sorting |
| Reproducibility | Fixed simulation steps give the same results across frame cadences and playback rates; seeded scores are repeatable |

A 24-part run exercised all intended routes and their sampled fixture checks. Four 12-part scenarios also completed: normal, slow inspection, camera timeout and full bin. The two fault scenarios required the expected repair before completion. There were no unexpected faults in these runs.

## Timing experiment

Both runs used 12 parts, an attempted feed every 4 seconds, a three-part buffer, 100% motion rate and seed 42. The fixed simulation step was 20 ms. Only camera delay and its corresponding timeout differed.

| Measure | Normal | Slow inspection |
| --- | ---: | ---: |
| Camera delay / timeout | 1.1 / 2.5 s | 6.0 / 8.0 s |
| Batch time, including final return | 498.74 s | 557.58 s |
| Mean pickup-to-placement interval | 33.58 s | 38.48 s |
| Feed blocked | 294.46 s | 333.70 s |
| Throughput: placed parts / elapsed time | 86.62 parts/h | 77.48 parts/h |
| Result | 8 pass, 1 review, 3 reject | 8 pass, 1 review, 3 reject |
| Checked motion commands | 144 | 144 |

The mean pickup-to-placement interval excludes the empty return and waiting for the next part. It is not the same as the full cycle interval. Throughput is a batch-derived model value, not a steady-state equipment rating.

![Completed browser runs with matching model results](../assets/comparison.jpg)

## Browser review

The normal and slow batches completed in the browser with the same displayed times and counts as the engine runs. Camera-timeout and full-bin recovery were exercised through the controls. Pause, command stepping, the three scene views, part filtering, JSON export and a 45-second video capture were checked. Widths of 390, 768 and 1440 pixels were reviewed; the page had no horizontal overflow, while the navigation and data tables can scroll within their own regions.

## Limits

The official UR5e description supplies nominal geometry, not calibration for a specific physical robot. Scope Cell uses its own interpolator and inverse-kinematics solver. URScript-like text in a screenshot is not proof of controller compatibility.

Collision checks sample conservative robot, gripper and held-part envelopes against modeled conveyor, camera, cabinet and bin geometry. They do not cover continuous collisions, robot self-collision, every decorative component, deformable objects, real contact dynamics or safety-rated stopping. Camera visibility checks cover orientation and five lines of sight, not optical calibration or defect recognition. Quality outcomes are synthetic.

No physical robot, URSim, production line or safety system was tested. The screenshots and video are evidence of the local application, not evidence of industrial commissioning.
