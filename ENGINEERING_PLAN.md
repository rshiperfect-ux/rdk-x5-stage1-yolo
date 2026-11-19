# RDK Edge Guardian — Engineering Plan

## 1. Bill of materials

Prices are planning estimates only. Existing equipment is reused before buying new hardware.

| Item | Qty | Supplier / SKU | Voltage and interface notes | Status | Estimated incremental cost |
|---|---:|---|---|---|---:|
| D-Robotics RDK X5 | 1 | Existing challenge board | 5 V power; on-board BPU/CPU | Owned and verified | USD 0 |
| 64 GB microSD card | 1 | Existing card | UHS-I; RDK OS 3.2.3 | Owned and verified | USD 0 |
| Sunplus Full HD UVC webcam | 1 | USB ID `1bcf:2283` | USB 2.0; MJPEG 1920x1080 at 30 FPS | Owned and verified | USD 0 |
| Stable RDK X5 power supply | 1 | Existing approved adapter | Must meet the RDK X5 input specification | Owned | USD 0 |
| Local Wi-Fi/router | 1 | Existing network | SSH/development only; runtime can be offline | Owned | USD 0 |
| USB speaker or powered speaker | 1 | Any Linux-compatible USB audio device | USB; local alert output | Optional | USD 8-15 |
| USB microphone | 1 | Any Linux UAC-compatible device | USB audio; optional second sensor | Optional; first check webcam audio | USD 6-15 |
| Camera stand/tripod | 1 | Generic | No electrical interface | Optional but recommended | USD 5-10 |

**Purchase decision:** do not buy a new camera. The current UVC webcam already captures 1080p MJPEG and produced successful BPU evidence. First run `arecord -l` to determine whether it also exposes a microphone. Optional audio hardware is purchased only if the core camera pipeline is stable and time remains.

## 2. Top five engineering risks

| # | Risk | Likelihood / impact | Early trigger | Mitigation | Fallback |
|---|---|---|---|---|---|
| 1 | ROS 2 camera stream or image conversion cannot sustain target rate | Medium / High | Processed FPS below 10 or rising frame age | Use MJPEG capture, bounded queues, lower capture resolution, profile conversions | Demonstrate stable 640x480 or 1280x720 stream and document trade-off |
| 2 | BPU model and runtime version mismatch affects reliability | Medium / High | DNN warning, wrong detections, or inference exceptions | Use the already verified OS model first; capture runtime/model versions; perform repeat tests | Use a model compiled with matching OpenExplorer package or retain verified stock model with limitation documented |
| 3 | Tracker/zone logic creates unstable or repeated alerts | Medium / Medium | ID switches, alert chatter, false-alert rate above target | Add confidence threshold, dwell time, hysteresis, cooldown, and scripted tests | Reduce scope to person/object zone occupancy without long-lived identity |
| 4 | USB camera disconnect, device renumbering, or power instability breaks demo | Medium / High | `/dev/video0` changes, frame timeout, USB resets | Resolve device by attributes, add retry/backoff, validate power, rehearse reconnect | Restart launch through one documented command and keep a recorded recovery run |
| 5 | Optional audio integration consumes time or hardware is unavailable | Medium / Medium | No device in `arecord -l` or noisy/late events by Dec 4 | Time-box audio, keep interface isolated, test timestamps before adding a model | Ship camera + detection + tracking + zone alert + metrics; describe audio as future work |

## 3. Planned repository structure

```text
rdk-x5-edge-guardian/
├── README.md
├── LICENSE
├── PROPOSAL.md
├── ARCHITECTURE.md
├── ENGINEERING_PLAN.md
├── ROADMAP.md
├── config/
│   ├── guardian.yaml
│   └── zones.yaml
├── launch/
│   └── edge_guardian.launch.py
├── models/
│   └── README.md
├── src/
│   ├── edge_guardian_camera/
│   ├── edge_guardian_detector/
│   ├── edge_guardian_tracker/
│   ├── edge_guardian_rules/
│   ├── edge_guardian_alerts/
│   ├── edge_guardian_metrics/
│   └── edge_guardian_interfaces/
├── scripts/
│   ├── benchmark.py
│   └── collect_diagnostics.sh
├── tests/
│   ├── test_tracker.py
│   ├── test_zone_rules.py
│   └── scenarios.md
├── docs/
│   ├── setup.md
│   ├── benchmark.md
│   ├── safety-and-privacy.md
│   └── demo-script.md
└── evidence/
    ├── stage1/
    ├── stage2/
    └── stage3/
```

The existing Stage 1 repository is the starting point. Files will be reorganized only when it improves reproducibility; working evidence will remain reachable through permanent links.

## 4. Verification strategy

### Unit and configuration checks

- Validate YAML schema and reject invalid zones or thresholds at startup.
- Test point-in-polygon, dwell timer, cooldown, stale-message rejection, and tracker expiry.
- Test alert state transitions using synthetic detections without requiring a camera.

### Hardware-in-the-loop scenarios

1. Empty configured zone for ten minutes.
2. Target object enters and leaves before dwell threshold: no alert.
3. Target remains beyond dwell threshold: one alert.
4. Multiple objects appear while detection and tracking run concurrently.
5. Camera is unplugged and reconnected.
6. Network is disconnected while the local pipeline continues.
7. Optional audio event occurs inside and outside the fusion window.

### Benchmark evidence

The benchmark report will include board/OS/runtime/model versions, input format and resolution, run length, processed frames, average/median/p95 inference and end-to-end latency, FPS, dropped-frame count, CPU/memory observations, event success count, false-alert count, and recovery time. Raw CSV/JSON and the command used to collect it will be public.

## 5. Definition of done

Stage 2 is done when the proposal, architecture, BOM, roadmap, five risks, planned repository structure, public community post, and updated showcase PR are all publicly linked. Stage 3 is done only after the camera-first system runs on the real RDK X5, BPU execution is evidenced, two workloads run concurrently, measurements replace targets, and the 3–7 minute public video includes at least 30 seconds of continuous AI operation.

