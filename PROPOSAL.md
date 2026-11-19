# RDK Edge Guardian — Stage 2 Project Proposal

## 1. One-sentence concept

RDK Edge Guardian is a privacy-first, on-device visual assistant that watches a user-defined indoor safety zone, detects and tracks relevant objects in real time on the RDK X5, and produces clear local alerts without sending camera frames to the cloud.

## 2. Problem and operating scenario

Indoor workspaces and homes contain temporary hazards: an object left in a walking path, a person entering a restricted zone, or a frequently used item that has been misplaced. A normal camera records video but does not turn the scene into an immediate, useful action. Cloud-based monitoring adds network dependency and privacy concerns.

The first demonstration will cover a desk or small room with one fixed USB camera. The user defines one or more polygonal zones in a configuration file. Edge Guardian continuously detects COCO objects, assigns short-lived track IDs, evaluates simple zone rules, and shows an annotated preview. When a configured condition persists, it publishes an alert and optionally plays a local sound.

### Constraints

- Compute must run on one RDK X5 using its BPU and CPU.
- The available camera is a Sunplus Full HD USB UVC webcam.
- The system must remain useful when internet access is unavailable.
- Lighting, occlusion, camera motion, and the stock model's class set limit accuracy.
- Alerts are advisory only; the project is not a certified safety or medical device.
- People in view must be informed, and raw frames are not uploaded by default.

## 3. Intended users and interaction

Primary users are makers, home users, and small-workspace operators who want a low-cost local safety assistant. A user:

1. Connects the camera and starts one ROS 2 launch file.
2. Views the annotated local stream and system status.
3. Selects target classes and zones in YAML configuration.
4. Receives a visual and optional audio alert when a rule remains true for the configured dwell time.
5. Can acknowledge or temporarily mute alerts without stopping detection.

No face recognition, identity database, or cloud video storage is planned.

## 4. Core capabilities

| Layer | Capability | Stage 3 implementation |
|---|---|---|
| Perception | Continuous object detection | YOLOv5s 672x672 NV12 on the RDK X5 BPU |
| Perception | Temporal tracking | Lightweight centroid/IoU tracker on CPU |
| Decision | Zone and dwell-time rules | Configurable class, polygon, confidence, and duration thresholds |
| Actuation | Local notification | Annotated preview, ROS 2 alert message, and optional USB audio alert |
| Reliability | Health monitoring | Frame age, inference latency, FPS, dropped frames, and node heartbeat |
| Privacy | Edge-only processing | No frame leaves the RDK X5 in the default configuration |

An optional stretch feature adds a USB microphone for acoustic-event confirmation. It will only be enabled if the existing webcam exposes a usable audio device or a low-cost USB microphone is available. The camera-first system remains the deliverable if audio is unavailable.

## 5. Innovation beyond the stock demo

The Stage 1 stock-style example performs inference on one still image. Edge Guardian turns it into an operational system by adding:

- continuous ROS 2 streaming instead of single-image inference;
- BPU detection plus a concurrent CPU tracking workload;
- stateful zone and dwell-time reasoning rather than drawing boxes only;
- configuration-driven behavior without modifying source code;
- observable performance, node health, and recovery from a camera interruption;
- privacy-by-design defaults and an evidence-based benchmark report;
- an optional second sensor whose timestamps are aligned with camera events.

## 6. Measurable success targets

These are Stage 2 targets. Stage 3 will report measured average, median, and p95 values rather than presenting targets as achieved results.

| Metric | Minimum success target | Stretch target | Measurement method |
|---|---:|---:|---|
| Continuous processed frame rate | 10 FPS | 15 FPS | ROS 2 metrics over a 5-minute run |
| End-to-end frame-to-alert latency | p95 <= 250 ms | p95 <= 180 ms | Timestamp at capture and alert publication |
| BPU inference | Confirmed for every benchmark run | Stable for 30 minutes | DNN runtime log plus latency samples |
| Zone-event success | >= 8/10 scripted trials | >= 18/20 trials | Repeatable scenario checklist |
| False alert rate in empty zone | <= 1 per 10 minutes | 0 per 10 minutes | Timed empty-scene test |
| Camera interruption recovery | <= 15 seconds | <= 10 seconds | Unplug/reconnect test |
| Continuous demonstration | >= 5 minutes | 30 minutes | Video/log timestamps and health metrics |

## 7. Demonstration story

The public demo will show the live camera, detections with stable track IDs, a visible zone, a person or object entering the zone, the dwell-time countdown, and the resulting local alert. A metrics panel or terminal view will prove BPU use, FPS, latency, and concurrent tracking. The camera will then be briefly disconnected and reconnected to demonstrate fault reporting and recovery.

## 8. Scope control

The required release is camera + BPU detection + CPU tracking + zone alerts + metrics. Audio fusion, a web dashboard, and additional models are stretch items. This ordering protects the final demo from optional hardware or integration delays.

