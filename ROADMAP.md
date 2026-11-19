# RDK Edge Guardian — Roadmap

This roadmap is dated from the Stage 2 work start on November 19, 2026. Required camera-first work is scheduled before optional audio work.

| Date | Milestone | Work and evidence | Exit criterion |
|---|---|---|---|
| Nov 19 | Scope locked | Proposal, measurable targets, privacy/safety boundary | Camera-first MVP and stretch scope are clearly separated |
| Nov 20 | Architecture reviewed | Data flow, ROS 2 graph, module interfaces, compute allocation | Every required node has inputs, outputs, rates, and failure behavior |
| Nov 21 | Engineering plan reviewed | BOM, top five risks, repository plan, test scenarios | No new camera required; optional purchases have fallbacks |
| Nov 22–23 | ROS 2 skeleton | Packages/interfaces, YAML config, launch file, synthetic rule tests | Nodes launch and rule logic can be tested without hardware |
| Nov 24 | Board baseline | Camera enumeration, optional audio check, ROS 2 version, sustained capture test | Hardware choices and actual device paths are recorded |
| Nov 25 | Stage 2 publication | Push documents, update showcase PR, publish Discord summary/permalink | All six official deliverables have public links |
| Nov 26 | Buffer and Stage 2 close | Fix reviewer/community feedback and archive evidence | Submission remains reproducible from README |
| Nov 27–30 | Continuous BPU detector | USB camera to BPU detector node with timestamps/diagnostics | Stable live detections and raw latency log |
| Dec 1–3 | Tracking and zone rules | Concurrent CPU tracker, zone state machine, alert output | Scripted enter/dwell/exit scenario passes |
| Dec 4–5 | Reliability | Watchdog, bounded queues, camera reconnect, offline run | Recovery and stale-data behavior are demonstrated |
| Dec 6 | Scope gate | Decide whether optional audio is viable | Audio retained only if core system is stable |
| Dec 7–9 | Benchmark and test | Five-minute benchmark, 20 scenario trials, 30-minute soak test | FPS, p95 latency, success/false-alert and recovery metrics published |
| Dec 10–11 | Demo recording | Record 3–7 minute story with >=30 seconds continuous AI | Video clearly shows hardware, BPU, tracking, alert and metrics |
| Dec 12–13 | Documentation | Setup, architecture, safety/privacy, benchmark, troubleshooting | A new user can reproduce the demo from public instructions |
| Dec 14 | Final submission | Update Discord and showcase PR with Stage 3 links | All official Stage 3 deliverables linked |
| Dec 15 | Final buffer | Correct broken links or organizer-requested details | Submission complete before final close |

## Weekly decision gates

- **Nov 23:** If ROS 2 packaging is delayed, keep custom interfaces minimal and prioritize the detector-to-alert path.
- **Nov 30:** If 10 FPS is not reached, reduce input resolution/rate before adding features.
- **Dec 6:** Optional audio is cancelled unless detection, tracking, alerts, metrics, and recovery already work.
- **Dec 11:** No new features after recording begins; only evidence, documentation, and defect fixes.

## Public progress evidence

Each milestone will be represented by at least one reproducible artifact: a commit, configuration file, test result, benchmark log, screenshot, short clip, or documented command. Claims will be labelled as either **target** or **measured result**.

