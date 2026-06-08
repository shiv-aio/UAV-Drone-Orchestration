# **SLM/LLM Orchestration for Tactical Drone & Swarm Systems**, structured as a formal R&D program document with full documentation plans per phase.

***

# Project Phase Documentation
## SLM/LLM Orchestration — Tactical Drone & Swarm Systems
### Aiotize Inc. | Pace Defence | Indian Army Collaboration
**Document Class:** Research & Development Program Plan | **Version:** 1.0 | **Date:** June 2026

***

## PHASE 0 — Initial Setup & Foundation
### Duration: Weeks 1–6 (1.5 Months)

This phase establishes the complete R&D infrastructure — no code is written, no drone flies — until every resource, environment, role, and baseline is locked and documented.

***

### 0.1 Team Formation & Role Assignment

- **Research Director / Project Lead** — Architecture ownership, stakeholder interface, milestone gating[1]
- **AI/ML Research Engineers (×3)** — SLM fine-tuning, SLAM algorithms, YOLOv8, LLM orchestration harness[2]
- **Embedded Systems Engineers (×2)** — NVIDIA Jetson Orin, ROS2, real-time C++ pipelines[2]
- **Drone Systems Engineers (×2)** — ArduPilot firmware, MAVLink, airframe integration[1]
- **RF/Comms Engineer (×1)** — AES-256, FHSS mesh networking, telemetry link validation[1]
- **Backend/DevOps Engineers (×2)** — Node.js GCS server, InfluxDB/PostgreSQL, Docker, CI/CD[1]
- **Test & Validation Engineers (×2)** — SITL/HITL harness, metrics logging, field trial protocols[3]
- **Technical Writer / IP Manager (×1)** — Documentation standards, patent tracking, IP registry[2]

***

### 0.2 Infrastructure Procurement & Lab Setup

**Compute & Hardware**
- 2× NVIDIA Jetson Orin Nano (40 TOPS each) — edge SLM inference[2]
- 2× Stereo camera pairs (global shutter, 1280×720 minimum RGB + IR)[2]
- 2× High-grade 9-axis IMUs (gyro bias stability <10°/hr)[2]
- 2× Pixhawk/CubeOrange flight controllers with ArduPilot firmware[1]
- 2× RFD900x telemetry radio pairs with FHSS capability[1]
- 1× EOIR dual-band camera payload for DOOAF integration[3]
- 2× Ruggedized ground terminals (Qt-based GUI, AndroidLinux)[3]
- 2× Development multirotor drone platforms (integration-ready)[1]
- 4× High-performance developer workstations (AI training, simulation)[1]
- 1× GPU workstation for model training (RTX 4090 class)[3]
- 128GB NVMe SSDs per Jetson module (terrain database storage)[2]

**Software Environment**
- Ubuntu 22.04 LTS on all development machines
- ROS2 Humble — installed, workspace configured, package scaffolds created[2]
- ArduPilot SITL environment + Gazebo Garden + Microsoft AirSim[2]
- Docker + GitHub Actions CI/CD pipeline initialized[1]
- InfluxDB (telemetry) + PostgreSQL (mission data) — deployed locally[1]
- Python 3.10, C++17 toolchain, CUDA toolkit on GPU workstation[2]
- Git repositories initialized: `aiotize/vps-core`, `aiotize/dooaf-core`, `aiotize/iif-core`, `aiotize/orchestration-layer`

***

### 0.3 Requirements Analysis & Architecture Lock

- Stakeholder workshops with Pace Defence and Indian Army representatives[1]
- Functional Requirements Document (FRD) drafted per sub-system (VPS, DOOAF, IIF)
- SLM/LLM orchestration architecture design — agent tier definitions (Tier 1 Perception, Tier 2 Coordination, Tier 3 Mission Planning)
- MAVLink message mapping document — which agent outputs which MAVLink message type[2][1]
- Interface Control Document (ICD) — defining API contracts between all agents and autopilot
- Threat model and security architecture documented (AES-256 key management, RBAC, PKI)[1]

***

### 0.4 Phase 0 Documentation Package

| Document | Owner | Format |
|---|---|---|
| **ENV-001** Lab & Infrastructure Setup Report | DevOps Engineer | Markdown + Inventory Sheet |
| **ORG-001** Team Charter & RACI Matrix | Research Director | PDF |
| **REQ-001** Functional Requirements Document (FRD) | All Leads | Structured Doc |
| **ARC-001** System Architecture Document v0.1 (SAD) | Research Director | Draw.io + PDF |
| **ICD-001** Interface Control Document — Agent ↔ MAVLink | Drone Eng + AI Eng | Table-based spec |
| **SEC-001** Security Architecture & Threat Model | RF/Comms Engineer | PDF |
| **IP-001** IP Registry Initialized — Joint Ownership Recorded | IP Manager | Confidential Registry |
| **PM-001** Project Management Plan (sprint calendar, gates) | Research Director | Jira/Notion |

**Phase 0 Gate Criteria:** All hardware received and bench-tested, all software environments running, FRD signed off by all stakeholders, Git repos initialized with CI/CD green.[1]

***

***

## PHASE 1 — Proof of Concept (POC)
### Duration: Months 2–4 (10 Weeks)

POC is broken into three sequential sub-phases that progressively build toward orchestrated autonomy: pure simulation, single-UAV orchestration, and multi-UAV orchestration.

***

## POC Sub-Phase 1A — Simulation
### Duration: Weeks 7–11 (Month 2 – Early Month 3)

**Objective:** Validate all three core algorithms (VPS SLAM, DOOAF targeting, IFF identification) in virtual environments before touching hardware. Establish the LLM/SLM orchestration harness in simulation.

***

### 1A.1 Simulation Environment Architecture

- **Gazebo Garden + ArduPilot SITL** — physics-accurate drone dynamics with simulated GPS denial[2]
- **Microsoft AirSim** — photorealistic visual feeds for VPS SLAM algorithm validation[2]
- **Multi-SITL setup** — multiple ArduPilot instances running simultaneously on one server for IIF testing[1]
- **RF channel emulator** — simulates FHSS, jamming, and packet loss for comms agent testing[1]
- **Synthetic terrain database** — pre-generated reference imagery loaded into VPS for SLAM matching[2]

### 1A.2 Simulation Test Cases

**TC-SIM-001: VPS SLAM Initialization**
- *Setup:* Single drone in AirSim, GPS enabled, VPS agent active in background
- *Action:* Drone flies 500m waypoint mission; VPS builds 3D map in parallel
- *Pass Criteria:* SLAM map constructed without gaps; VPS position estimate within 3m of GPS ground truth[2]

**TC-SIM-002: GPS Denial Handover**
- *Setup:* Drone mid-mission at 200m altitude; GPS signal cut at T+30s
- *Action:* VPS agent auto-switches to visual odometry; continues waypoint navigation
- *Pass Criteria:* Navigation continues without abort; position estimate maintained within 5m CEP; switchover latency <2s[2]

**TC-SIM-003: VPS Return-to-Home**
- *Setup:* GPS denied; drone at 300m from launch point
- *Action:* Return-to-home command issued via LLM mission agent
- *Pass Criteria:* Drone returns to within 5m of launch using visual landmarks only; ≥90% success across 20 simulation runs[2]

**TC-SIM-004: DOOAF Target Detection**
- *Setup:* AirSim scene with simulated ground vehicle as target; EOIR feed active
- *Action:* YOLOv8 target detection agent identifies target; geolocation engine computes 3D position
- *Pass Criteria:* Target detected with >85% confidence; 3D geolocation within 15m CEP; azimuth/elevation error <2°[3]

**TC-SIM-005: DOOAF Direction Computation Latency**
- *Setup:* Moving target in simulation; gun system position fixed
- *Action:* Full pipeline — detection → geolocation → vector computation → data relay
- *Pass Criteria:* End-to-end pipeline latency <500ms; packet delivery 100% in ideal channel[3]

**TC-SIM-006: IFF Beacon Validation**
- *Setup:* 3 simulated drones with encrypted IFF beacons; 1 simulated unknown/hostile object
- *Action:* IFF agent classifies all objects; fleet coordinator renders unified picture
- *Pass Criteria:* 100% correct classification of beaconed drones; unknown object correctly flagged within 1s[1]

**TC-SIM-007: SLM Orchestration Round-Trip**
- *Setup:* LLM ground agent issues mission instruction to edge SLM on simulated Jetson
- *Action:* SLM interprets task, commands SITL autopilot via MAVLink, reports status back
- *Pass Criteria:* Instruction-to-execution latency <300ms; correct MAVLink command generated; status reported accurately[1]

### 1A.3 Simulation Success Parameters

| Metric | Target | Minimum Acceptable |
|---|---|---|
| VPS Position Accuracy (GPS available) | ≤3m CEP | ≤5m CEP [2] |
| VPS Position Accuracy (GPS denied) | ≤5m CEP | ≤10m CEP [2] |
| GPS→VPS Switchover Time | <2s | <5s [2] |
| RTH Success Rate (simulation) | ≥90% (20 runs) | ≥80% |
| DOOAF Target Detection Confidence | >85% | >70% [3] |
| DOOAF End-to-End Latency | <500ms | <800ms [3] |
| IFF Classification Accuracy | 100% (beaconed) | ≥98% [1] |
| SLM Command Round-Trip | <300ms | <500ms |
| SLAM Map Continuity | 0 gaps on 500m run | ≤2 gap events |

### 1A.4 Phase 1A Documentation Package

| Document | Content |
|---|---|
| **SIM-001** Simulation Environment Setup Guide | Gazebo, AirSim, SITL install, config, launch scripts |
| **SIM-002** Test Case Specification (TC-SIM-001 to 007) | Input, steps, pass/fail criteria per test |
| **SIM-003** Simulation Test Execution Log | Raw logs per run, timestamp, metric values |
| **SIM-004** Algorithm Benchmark Report | SLAM accuracy, DOOAF latency, IFF accuracy plots |
| **SIM-005** Simulation Phase Review Report | Results vs. targets, gap analysis, go/no-go recommendation |

***

## POC Sub-Phase 1B — Single UAV Orchestration
### Duration: Weeks 12–16 (Month 3 – Mid Month 4)

**Objective:** Deploy the SLM/LLM orchestration stack on real hardware (Jetson Orin + Pixhawk) and validate that a single physical drone can be autonomously commanded through the full VPS + DOOAF pipeline.

***

### 1B.1 Hardware Integration Checklist (Pre-flight Gate)

- [ ] Jetson Orin Nano physically mounted and powered on drone airframe[2]
- [ ] Stereo cameras calibrated; ROS2 image topics publishing at 30fps[2]
- [ ] 9-axis IMU soldered, vibration-isolated, ROS2 topic verified[2]
- [ ] Jetson ↔ Pixhawk UART/Ethernet interface validated; MAVLink heartbeat confirmed[2]
- [ ] VPS SLAM agent running on Jetson; VISION_POSITION_ESTIMATE messages flowing to Pixhawk[2]
- [ ] YOLOv8 DOOAF agent loaded on Jetson; inference running at >10fps on live camera feed[3]
- [ ] RFD900x telemetry link tested; ground station receiving MAVLink telemetry[1]
- [ ] LLM ground server connected via encrypted TCP; SLM agent on Jetson responsive to task queue[1]
- [ ] Emergency RC override tested; pilot can assume manual control at any moment[2]

### 1B.2 Single UAV Test Cases

**TC-UAV1-001: Bench SLAM Validation**
- *Setup:* Drone stationary on bench; operator slowly moves in front of cameras
- *Action:* VPS agent builds visual map; SLAM pose estimate logged
- *Pass Criteria:* Stable map construction; no SLAM reset events; pose estimate changes proportionally to camera movement[2]

**TC-UAV1-002: Tethered Indoor Hover — GPS Active**
- *Setup:* Drone tethered, indoors; GPS module active; VPS running in parallel
- *Action:* Hover for 3 minutes; log VPS vs GPS position delta continuously
- *Pass Criteria:* VPS estimate within 3m of GPS; no SLAM failures; Jetson CPU/GPU within thermal limits[2]

**TC-UAV1-003: GPS Denial — Indoor Hover**
- *Setup:* GPS signal blocked (Faraday cage or GPS signal simulator off); VPS as sole navigation source
- *Action:* Autopilot switches to VPS; drone holds hover using VPS pose estimate
- *Pass Criteria:* Drone maintains position within 2m for 60 seconds; no crash; SLM reports mode change correctly[2]

**TC-UAV1-004: Autonomous Waypoint Mission — VPS Guided**
- *Setup:* Indoor space (warehouse/hangar); 4 waypoints set in a 20m × 20m square; GPS disabled
- *Action:* LLM ground agent issues mission plan; SLM on Jetson executes via MAVLink; VPS navigates
- *Pass Criteria:* All 4 waypoints reached within 3m; mission completes without operator intervention; <5% trajectory deviation[2]

**TC-UAV1-005: DOOAF Target Lock — Static Target**
- *Setup:* Drone hovering at 10m; stationary ground target (marked vehicle/board) below
- *Action:* DOOAF agent detects target; computes azimuth/elevation; transmits to ground terminal
- *Pass Criteria:* Target detected and locked; alignment data received on terminal within 500ms; angular error <2°[3]

**TC-UAV1-006: LLM Mission Replanning**
- *Setup:* Mid-mission; LLM agent receives simulated "obstacle detected" event
- *Action:* LLM replans route; pushes new waypoints to SLM; SLM re-commands autopilot
- *Pass Criteria:* New mission uploaded mid-flight; drone follows updated route; replanning latency <3s[1]

**TC-UAV1-007: Return-to-Home — GPS Denied**
- *Setup:* Drone at far waypoint; GPS denied; RTH command issued by LLM agent
- *Action:* SLM invokes VPS RTH module; drone navigates back using visual landmarks
- *Pass Criteria:* Landing within 5m of launch point; success rate ≥90% over 10 runs[2]

### 1B.3 Single UAV Success Parameters

| Metric | Target | Minimum Acceptable |
|---|---|---|
| VPS Hover Stability (GPS denied) | ≤2m position hold | ≤3m [2] |
| Waypoint Navigation Accuracy | ≤3m CEP | ≤5m [2] |
| GPS→VPS Handover (real hardware) | <3s | <5s [2] |
| DOOAF Lock Latency (real hardware) | <500ms | <800ms [3] |
| DOOAF Angular Error | <2° | <3° [3] |
| LLM→SLM→Autopilot Round-Trip | <300ms | <600ms |
| RTH Success Rate (10 runs, GPS denied) | ≥90% | ≥80% [2] |
| Jetson Thermal Headroom | >10°C below TJ | Must not throttle |
| System Uptime (60-min mission) | 100% | >95% |

### 1B.4 Phase 1B Documentation Package

| Document | Content |
|---|---|
| **HW-001** Hardware Integration Checklist & Sign-off Sheet | Pre-flight gate verification record |
| **HW-002** Sensor Calibration Records | Camera calibration matrices, IMU bias profiles |
| **TC-UAV1** Single UAV Test Case Log | Per-test: inputs, measured outputs, pass/fail, anomalies |
| **PERF-001** Single UAV Performance Report | VPS accuracy plots, latency histograms, thermal logs |
| **FAIL-001** Failure Mode Log | All SLAM resets, crashes, unexpected behaviors + root cause |
| **POC-1B-REVIEW** Go/No-Go Gate Review | Results vs. success parameters, sign-off by Research Director |

***

## POC Sub-Phase 1C — Multi-UAV Orchestration
### Duration: Weeks 17–20 (Mid Month 4)

**Objective:** Scale the orchestration layer to coordinate 2–3 physical drones simultaneously, validating IFF identification, fleet coordination, and inter-drone collision avoidance under the LLM orchestrator.

***

### 1C.1 Multi-UAV Orchestration Architecture

- **Fleet Coordinator Agent** (ground server) manages task graph across all drones[1]
- **Each drone** runs its own Jetson-resident SLM agent connected to the common LLM orchestrator via encrypted mesh[1]
- **MAVLink Router** instance on ground server demultiplexes telemetry from all drones to shared InfluxDB[1]
- **IFF Engine** validates beacon packets from each drone; renders unified operational picture on React.js dashboard[1]
- **Collision Deconfliction Module** enforces minimum separation distance (≥10m) via pre-programmed priority rules[1]

### 1C.2 Multi-UAV Test Cases

**TC-MUAV-001: Dual Drone Simultaneous Telemetry**
- *Setup:* 2 drones hovering; both transmitting MAVLink telemetry via RFD900x
- *Action:* GCS dashboard displays both drones; InfluxDB logs both streams with correct SYSID tagging
- *Pass Criteria:* No telemetry cross-contamination; both drones visible on map; telemetry latency <200ms[1]

**TC-MUAV-002: IFF Classification — Mixed Fleet**
- *Setup:* 2 friendly drones (beaconed) + 1 simulated unknown aerial object (beacon-less RF target)
- *Action:* IFF agent classifies all three; fleet coordinator renders IFF overlay on dashboard
- *Pass Criteria:* Both friendly drones correctly identified green; unknown object flagged red within 1s; zero false classifications[1]

**TC-MUAV-003: Coordinated Waypoint Mission**
- *Setup:* 2 drones assigned complementary ISR waypoint routes by LLM mission planner
- *Action:* LLM generates task graph; SLM agents on each Jetson execute respective missions concurrently
- *Pass Criteria:* Both missions complete without collision; routes do not overlap; LLM task graph executed correctly[1]

**TC-MUAV-004: Collision Avoidance — Converging Paths**
- *Setup:* 2 drones on converging waypoints set to cross same point at same time
- *Action:* Fleet coordinator deconfliction module detects conflict; re-sequences one drone's waypoint timing
- *Pass Criteria:* Minimum separation ≥5m maintained; no collision; replanning occurs >3s before projected intersection[1]

**TC-MUAV-005: GPS Denial — Fleet-Wide VPS Handover**
- *Setup:* 2 drones in flight; GPS denied fleet-wide simultaneously
- *Action:* Both VPS agents activate independently; LLM orchestrator receives mode-change reports from both SLMs
- *Pass Criteria:* Both drones maintain navigation autonomously; LLM correctly logs state change for both; no mission abort[2][1]

**TC-MUAV-006: Lost-Link Recovery**
- *Setup:* 3 drones in coordinated mission; telemetry link to Drone 3 deliberately cut for 30s
- *Action:* Fleet coordinator detects lost link; LLM orchestrator issues RTH command to last known SLM state; Drones 1 & 2 continue
- *Pass Criteria:* Drone 3 executes autonomous RTH within 5s of link loss; remaining 2 drones unaffected; lost link alert fires on dashboard[1]

**TC-MUAV-007: DOOAF + IIF Concurrent Operation**
- *Setup:* Drone A performs IFF scan; Drone B executes DOOAF targeting of simulated ground target
- *Action:* LLM orchestrator manages both sub-system agents concurrently; no resource conflict on ground server
- *Pass Criteria:* IFF results and DOOAF alignment data arrive simultaneously within spec latencies; no data corruption[3][1]

### 1C.3 Multi-UAV Success Parameters

| Metric | Target | Minimum Acceptable |
|---|---|---|
| Concurrent Drone Telemetry | 3 drones, 0 cross-contamination | 2 drones clean [1] |
| IFF Classification Accuracy | 100% (beaconed fleet) | ≥98% [1] |
| Minimum Inter-Drone Separation | ≥5m at all times | ≥3m |
| Lost-Link RTH Response Time | <5s from link loss | <10s [1] |
| LLM Task Graph Execution Accuracy | 100% correct role assignment | ≥95% |
| Fleet-Wide GPS→VPS Handover | All drones within 3s | Within 5s [2] |
| GCS Dashboard Refresh Rate | <200ms latency | <500ms [1] |
| Concurrent Agent CPU Load (GCS) | <70% total | <90% |

### 1C.4 Phase 1C Documentation Package

| Document | Content |
|---|---|
| **MUAV-001** Multi-UAV Network Architecture Diagram | MAVLink router config, IP map, SYSID assignment |
| **TC-MUAV** Multi-UAV Test Case Log | All TC-MUAV-001 to 007 execution records |
| **FLEET-001** Fleet Coordinator Agent Spec | Task graph format, conflict resolution logic, API |
| **IFF-001** IFF Engine Technical Document | Beacon protocol, classification algorithm, threshold tuning |
| **PERF-002** Multi-UAV Performance Report | Separation logs, IFF accuracy stats, latency CDFs |
| **POC-FINAL** POC Completion Review & Sign-off | All three sub-phases aggregated; TRL milestone record |

***

***

## PHASE 2 — Lab Scale Implementation
### Duration: Months 5–7 (10 Weeks)

Lab scale takes the validated POC and scales it to the full intended operational scope: 5-drone coordinated fleet, GPS-jammed indoor environment, full DOOAF + IIF integration, edge SLM autonomous decision loops, and stress-tested communication infrastructure.[1]

***

### 2.1 Lab Scale Scope Expansion

- Scale fleet from 3 (POC) → **5 drones** (2 fixed-wing SITL + 3 multirotor physical)[1]
- Introduce **controlled RF jamming** using signal generator to create genuine GPS-denied conditions[2]
- Activate **full LLM mission planning** loop — commander-level natural language intent → SLM execution[1]
- Deploy **DOOAF full pipeline** on moving target scenarios (RC vehicle as surrogate)[3]
- Conduct **security penetration testing** on encrypted comms layer[1]
- Run **24-hour endurance tests** on Jetson modules under continuous inference load[2]

***

### 2.2 Lab Scale Test Cases

**TC-LAB-001: 5-Drone Fleet Coordination**
- *Setup:* 5 drones (mixed multirotor + SITL fixed-wing); LLM assigns ISR, relay, and targeting roles
- *Action:* 45-minute coordinated mission executed without operator intervention
- *Pass Criteria:* All 5 drones complete assigned roles; 0 collisions; ≥99.9% packet delivery across fleet[3][1]

**TC-LAB-002: Active GPS Jamming — Full Fleet**
- *Setup:* RF jammer active in lab; all 5 drones switch to VPS; fixed-wing drones use SITL VPS equivalent
- *Action:* Mission continues under jamming; LLM orchestrator adapts route to jamming-safe profiles
- *Pass Criteria:* ≥3 of 3 physical drones maintain VPS navigation; ≤5m drift over 10-minute jammed flight[2]

**TC-LAB-003: DOOAF Moving Target Acquisition**
- *Setup:* RC ground vehicle moving at 15 km/h in lab environment; Drone B assigned DOOAF role
- *Action:* YOLOv8 detects and tracks moving target; direction computation updates continuously
- *Pass Criteria:* Target tracking maintained ≥90% of target-visible time; alignment data updated every <500ms; geolocation CEP <10m[3]

**TC-LAB-004: Full LLM Natural Language Mission**
- *Setup:* Operator types mission intent in plain English: *"Drone A and B: ISR sweep north sector. Drone C: standby DOOAF targeting at grid reference X. Report hostile contacts."*
- *Action:* LLM parses intent; generates MAVLink mission plan; distributes to SLM agents; executes
- *Pass Criteria:* Mission correctly interpreted and executed; role assignments match intent; no manual waypoint entry required[1]

**TC-LAB-005: Communication Security Penetration Test**
- *Setup:* External pentester attempts MAVLink injection, replay attacks, and beacon spoofing
- *Action:* AES-256 encryption, PKI command authentication, and intrusion detection active[1]
- *Pass Criteria:* 0 successful command injections; 0 spoofed beacons accepted; all intrusion attempts logged[1]

**TC-LAB-006: 24-Hour Endurance Run**
- *Setup:* All Jetson modules running full inference stack (SLAM + DOOAF + IFF) continuously
- *Action:* Monitor CPU/GPU temperature, memory, SLAM accuracy drift over 24 hours[2]
- *Pass Criteria:* No thermal throttling; SLAM accuracy maintained within spec; no memory leak or crash[2]

**TC-LAB-007: Post-Mission LLM Debrief**
- *Setup:* Complete mission logged to InfluxDB; LLM analyst agent queried for after-action report
- *Action:* LLM generates structured report: timeline, anomalies, target contacts, IFF events, performance metrics
- *Pass Criteria:* Report generated within 60s of mission end; all key events correctly identified; report matches ground truth log[1]

***

### 2.3 Lab Scale Success Parameters

| Metric | Target | Source |
|---|---|---|
| Fleet Size Under Coordination | 5 drones | [1] |
| VPS Navigation Accuracy (jammed) | ≤5m CEP | [2] |
| Drift Over 10-min GPS-denied flight | ≤5m | [2] |
| DOOAF Moving Target Tracking | ≥90% time-on-target | [3] |
| DOOAF Geolocation CEP | ≤10m | [3] |
| Packet Delivery (5-drone mesh) | ≥99.9% | [3] |
| Security: Injection Attempts Blocked | 100% | [1] |
| Endurance: Uptime / 24hr run | 100%, no throttle | [2] |
| LLM After-Action Report Accuracy | All key events captured | [1] |
| TRL Target at Phase 2 Exit | TRL 5 | [1] |

***

### 2.4 Phase 2 Documentation Package

| Document | Content |
|---|---|
| **LAB-001** Lab Scale Test Plan | Full TC-LAB-001 to 007 specifications |
| **LAB-002** Lab Test Execution Reports | Logs, metrics, anomaly records per test |
| **SEC-002** Penetration Test Report | Findings, vulnerabilities, mitigations applied [1] |
| **PERF-003** Endurance & Thermal Analysis Report | 24-hour Jetson performance profiles [2] |
| **LLM-001** Orchestration Agent Technical Spec | Prompt formats, task graph schema, agent API contracts |
| **GCS-001** GCS Dashboard User Manual v1.0 | Operator interface guide for commanders and pilots [1] |
| **DB-001** Telemetry Database Schema | InfluxDB measurement definitions, retention policies [1] |
| **LAB-REVIEW** Lab Scale Phase Gate Review | Full metrics vs. targets, TRL advancement record |

***

***

## PHASE 3 — Implementation Guidelines & Plans
### Duration: Months 8–9 (Final Phase)

This phase transitions the validated lab prototype toward field readiness, structured handover to Pace Defence and Indian Army, and formal research closure.[2][1]

***

### 3.1 Field Readiness Checklist

- [ ] All lab-scale success parameters met and documented
- [ ] Security penetration test completed, 0 critical vulnerabilities open[1]
- [ ] DGCA / military airspace clearance secured for outdoor test corridor[2]
- [ ] Outdoor GPS jamming test clearance obtained from competent authority[2]
- [ ] Training materials completed and reviewed by Pace Defence[1]
- [ ] Technology transfer package finalized[2]
- [ ] Patent applications filed for novel algorithms[2][1]

***

### 3.2 Outdoor Field Trial Plan

**Trial 1 — GPS Jamming Corridor Test (Month 8)**
- Location: Designated military test range
- Setup: 2 physical multirotor drones; active GPS jammer at 100m range
- VPS agent navigates 500m corridor without GPS; RTH tested[2]
- Success: ≤2m CEP navigation; ≥95% RTH success over 10 runs[2]

**Trial 2 — DOOAF Artillery Alignment Trial (Month 8)**
- Location: Artillery range (coordinated with Army unit)
- Setup: Drone with DOOAF payload; static and moving surrogate targets; gun system terminal active
- DOOAF end-to-end pipeline validated against ground truth bearing measurements[3]
- Success: ≤1° angular error; ≤500ms latency; ≥99.9% packet delivery[3]

**Trial 3 — Fleet IFF Outdoor Evaluation (Month 9)**
- Location: Open test field, ≥500m × 500m
- Setup: 3 friendly drones + 1 unknown commercial drone flown by evaluator
- IFF engine classifies all assets in real outdoor RF environment[1]
- Success: 100% friendly classification; unknown flagged in <1s; no false positives[1]

**Trial 4 — Full System Integration Trial (Month 9)**
- All three sub-systems (VPS + DOOAF + IIF) running on the same mission under LLM orchestration
- Army evaluation team observes and scores against pre-defined operational scenarios[1]
- Success: TRL 6 milestones met across all sub-systems[2][1]

***

### 3.3 Implementation Guidelines — Deployment Standards

**Onboarding a New Drone Platform**
1. Verify ArduPilot/MAVLink compatibility (CubeOrange or Pixhawk series)[1]
2. Mount Jetson Orin module with vibration isolation; verify power draw budget (≤20W additional)[2]
3. Calibrate stereo cameras using provided calibration target; export matrices to `/etc/vps/calibration/`[2]
4. Flash signed VPS + DOOAF + IFF firmware via secure OTA update script[2]
5. Run TC-UAV1-001 (bench SLAM test) and TC-UAV1-002 (tethered hover) before any free flight
6. Register drone SYSID and IFF beacon key in central PKI registry[1]

**Scaling the Fleet**
- Each new drone requires unique MAVLink SYSID; registered in MAVLink router config[1]
- Fleet coordinator agent auto-discovers new drones via heartbeat; no manual GCS config required[1]
- InfluxDB horizontal scaling via additional nodes — supports up to 50 concurrent drones before infrastructure upgrade needed[1]

**LLM Ground Server Sizing**
- Minimum: 1× RTX 4090, 64GB RAM for ≤10 drone fleet with real-time mission planning
- Recommended: Dual GPU node for ≤50 drones; Redis message queue for SLM task distribution
- All LLM inference air-gapped from public internet; local model weights only[1]

***

### 3.4 Project Management Standards (Ongoing)

- **Sprint Cadence:** 2-week sprints with backlog grooming at Sprint Start, retrospective at Sprint End
- **Milestone Gates:** Monthly formal reviews with Pace Defence; gate sign-off required before phase advance[1]
- **Version Control:** All code merged via Pull Request with mandatory peer review + CI/CD pass[1]
- **Bug Tracking:** Jira tickets for all failures logged in TC execution; P1 bugs resolved before phase gate[1]
- **Change Control:** Any architectural change requires SAD amendment + Research Director sign-off
- **IP Logging:** Every novel algorithm/method logged in IP Registry within 5 days of development[2]

***

### 3.5 Phase 3 Final Documentation Package

| Document | Content |
|---|---|
| **FTR-001** Field Trial Reports (Trials 1–4) | Execution logs, metrics, Army evaluator sign-off [1][2] |
| **TRL-001** TRL Advancement Record | Evidence matrix for TRL 6 claim [2] |
| **TT-001** Technology Transfer Package | Source code, build instructions, calibration procedures [2] |
| **OPS-001** Operator Manual — Field Edition | Pre-mission setup, flight ops, emergency procedures [1] |
| **MAINT-001** Maintenance & Troubleshooting Guide | Hardware replacement, SLAM reset, firmware update procedures [2] |
| **TRAIN-001** Training Curriculum | Army UAV operator workshop + simulation modules [1] |
| **IP-002** Patent Filing Records | Application numbers, claims, joint ownership split [2][1] |
| **PUB-001** Research Paper Drafts (2–3 papers) | VPS algorithms, DOOAF pipeline, SLM/LLM orchestration architecture [2] |
| **PCR-001** Project Closure Report | Final milestone summary, lessons learned, recommendations [1] |

***

## Phase Timeline Summary

| Phase | Duration | TRL Entry | TRL Exit | Key Gate |
|---|---|---|---|---|
| **Phase 0** Initial Setup | Weeks 1–6 | — | TRL 3 | Infrastructure ready; FRD signed [2] |
| **Phase 1A** Simulation POC | Weeks 7–11 | TRL 3 | TRL 3+ | All TC-SIM pass at minimum acceptable [2] |
| **Phase 1B** Single UAV POC | Weeks 12–16 | TRL 3+ | TRL 4 | RTH ≥90%; VPS holds GPS-denied hover [2] |
| **Phase 1C** Multi-UAV POC | Weeks 17–20 | TRL 4 | TRL 4+ | IFF 100%; 3-drone fleet deconflicted [1] |
| **Phase 2** Lab Scale | Months 5–7 | TRL 4+ | TRL 5 | 5-drone jammed mission; pentest clean [1] |
| **Phase 3** Field & Handover | Months 8–9 | TRL 5 | TRL 6 | Army evaluation trial success [1][2] |

Sources
[1] DPR-3_-Integrated-Drone-Command-Fleet-Management-IIF.pdf https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_0a41058c-f5d5-4d34-a75a-5e2193005f65/f7b6276c-93b3-4b39-a7cd-910248d80c3b/DPR-3_-Integrated-Drone-Command-Fleet-Management-IIF.pdf?AWSAccessKeyId=ASIA2F3EMEYE4NRWDHBD&Signature=OMqH%2BF4Yh8LuMv%2BL4K9dorzhqRI%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIDL21cJJgd%2B0c9QXGWFfuS%2FTbzgKYPmQwEwdqI%2BrJQ9jAiEAxfmq4pXm%2BN0jDVfae16KJpDfbHzwGDTBQdTYCOWvwtcq%2FAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARABGgw2OTk3NTMzMDk3MDUiDKRnoMuqI18XN8WVBirQBDhp8fg2LuP4vnMRaYyJOH5hhVYCYYl54sH13lI9GhIDuLPVXyU8zxO1f7iCIlXJfkV2RYkTThFcAy2tNTgQUY7myPT4VtTtAtm%2F2rl72Z%2FBct69Z7XMYFDcSPcm4lBiUyPnujhiq%2FIro%2BhuL2%2BBV9jublWhGvJq%2FFYznx0pdSxrq6ZyfAOrL2VPhaUmE9RgI6UBF6TL8lZX%2BxlPboejP43feiKXVprcmbqG0wkmCgh39PVrnI6hfr8U55jQ2kTwfiirvHezJstF7%2B3A7YVSNVAc%2FHRbh3yvEBSaq011fNBhVarYeWabfFmPSzNB8zNVmwIijL7jlIvjN8G%2BAF8g%2BkU5mrUtkfS8Wx%2Bjy5VOQ9GFMK6gObupvOz1VcYMpGzJc54JpqOB%2FNWwJrgiEa2Gee%2Fmfiy%2Frh34x74IHBKeKwr5ona%2ByBCRvSlFNJR1eONhlBZvYHJnwFdhDVo72pWPV22o7s86pa9kaWxLp3oANkxxcszoHf4U1AjYiCWTO5Z7DvCEDhhn6X%2FNK5S1YHBXKeMP6gNVlXBIjXDStkcfjxuOia1eh7XxG%2Fg8MhKcKgOYuaBD2v5%2BPqBSs2hy%2FzDWLwUhlrfowt%2Bdn58bHQwrJTMuP68H1ZX1CdPrbTcm101VH9FDto3ycl63BbmbN6JxjKM3Kc9Svw6NF5Xgj0N8KvuHVNCHi%2BhSOnyklVmz1boAvYl9MH8K74B1m%2Fbhj65je7DAvv5WEzUGA7yZMskUks7VHiAT79pqTJU2m6yoRQDb1xw2UNUk10qWJ9j8i1bKSDkw9%2Fea0QY6mAHYJHqsX0F1Gde46T9gfKkaPd0rsK%2Bdf5r04E6FBVUK%2FjywayeYnsFAG9ZtQe69Ua87qrlIrFPP%2BVZD4SQijB%2FMagJWpPSz8ApPhTbS%2Fx3SsV9MenH7SvSKGr8w6GdeYgnWM1xEqZ%2FSADA2bhYUNue06CykYMdZg3ZGY2sTX28sF6f%2BTzJkVrm0X0lM7CHI6P0zK6bdDF19xw%3D%3D&Expires=1780926922
[2] DPR-1_-Vision-Based-Positioning-System-VPS.pdf https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_0a41058c-f5d5-4d34-a75a-5e2193005f65/d9bd10f6-02e1-4763-a4d5-47e0b5eac2d4/DPR-1_-Vision-Based-Positioning-System-VPS.pdf?AWSAccessKeyId=ASIA2F3EMEYE4NRWDHBD&Signature=n%2FuDDCsz6AEgKaxO1W7VOYxLLCI%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIDL21cJJgd%2B0c9QXGWFfuS%2FTbzgKYPmQwEwdqI%2BrJQ9jAiEAxfmq4pXm%2BN0jDVfae16KJpDfbHzwGDTBQdTYCOWvwtcq%2FAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARABGgw2OTk3NTMzMDk3MDUiDKRnoMuqI18XN8WVBirQBDhp8fg2LuP4vnMRaYyJOH5hhVYCYYl54sH13lI9GhIDuLPVXyU8zxO1f7iCIlXJfkV2RYkTThFcAy2tNTgQUY7myPT4VtTtAtm%2F2rl72Z%2FBct69Z7XMYFDcSPcm4lBiUyPnujhiq%2FIro%2BhuL2%2BBV9jublWhGvJq%2FFYznx0pdSxrq6ZyfAOrL2VPhaUmE9RgI6UBF6TL8lZX%2BxlPboejP43feiKXVprcmbqG0wkmCgh39PVrnI6hfr8U55jQ2kTwfiirvHezJstF7%2B3A7YVSNVAc%2FHRbh3yvEBSaq011fNBhVarYeWabfFmPSzNB8zNVmwIijL7jlIvjN8G%2BAF8g%2BkU5mrUtkfS8Wx%2Bjy5VOQ9GFMK6gObupvOz1VcYMpGzJc54JpqOB%2FNWwJrgiEa2Gee%2Fmfiy%2Frh34x74IHBKeKwr5ona%2ByBCRvSlFNJR1eONhlBZvYHJnwFdhDVo72pWPV22o7s86pa9kaWxLp3oANkxxcszoHf4U1AjYiCWTO5Z7DvCEDhhn6X%2FNK5S1YHBXKeMP6gNVlXBIjXDStkcfjxuOia1eh7XxG%2Fg8MhKcKgOYuaBD2v5%2BPqBSs2hy%2FzDWLwUhlrfowt%2Bdn58bHQwrJTMuP68H1ZX1CdPrbTcm101VH9FDto3ycl63BbmbN6JxjKM3Kc9Svw6NF5Xgj0N8KvuHVNCHi%2BhSOnyklVmz1boAvYl9MH8K74B1m%2Fbhj65je7DAvv5WEzUGA7yZMskUks7VHiAT79pqTJU2m6yoRQDb1xw2UNUk10qWJ9j8i1bKSDkw9%2Fea0QY6mAHYJHqsX0F1Gde46T9gfKkaPd0rsK%2Bdf5r04E6FBVUK%2FjywayeYnsFAG9ZtQe69Ua87qrlIrFPP%2BVZD4SQijB%2FMagJWpPSz8ApPhTbS%2Fx3SsV9MenH7SvSKGr8w6GdeYgnWM1xEqZ%2FSADA2bhYUNue06CykYMdZg3ZGY2sTX28sF6f%2BTzJkVrm0X0lM7CHI6P0zK6bdDF19xw%3D%3D&Expires=1780926922
[3] DPR-2_-Direction-of-Own-Artifact-DOOAF.pdf https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_0a41058c-f5d5-4d34-a75a-5e2193005f65/44763cbf-382b-4b61-b2a2-0c0c074c6461/DPR-2_-Direction-of-Own-Artifact-DOOAF.pdf?AWSAccessKeyId=ASIA2F3EMEYE4NRWDHBD&Signature=bnEqIej1IafWdQsn5bAjSVsdYU0%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIDL21cJJgd%2B0c9QXGWFfuS%2FTbzgKYPmQwEwdqI%2BrJQ9jAiEAxfmq4pXm%2BN0jDVfae16KJpDfbHzwGDTBQdTYCOWvwtcq%2FAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARABGgw2OTk3NTMzMDk3MDUiDKRnoMuqI18XN8WVBirQBDhp8fg2LuP4vnMRaYyJOH5hhVYCYYl54sH13lI9GhIDuLPVXyU8zxO1f7iCIlXJfkV2RYkTThFcAy2tNTgQUY7myPT4VtTtAtm%2F2rl72Z%2FBct69Z7XMYFDcSPcm4lBiUyPnujhiq%2FIro%2BhuL2%2BBV9jublWhGvJq%2FFYznx0pdSxrq6ZyfAOrL2VPhaUmE9RgI6UBF6TL8lZX%2BxlPboejP43feiKXVprcmbqG0wkmCgh39PVrnI6hfr8U55jQ2kTwfiirvHezJstF7%2B3A7YVSNVAc%2FHRbh3yvEBSaq011fNBhVarYeWabfFmPSzNB8zNVmwIijL7jlIvjN8G%2BAF8g%2BkU5mrUtkfS8Wx%2Bjy5VOQ9GFMK6gObupvOz1VcYMpGzJc54JpqOB%2FNWwJrgiEa2Gee%2Fmfiy%2Frh34x74IHBKeKwr5ona%2ByBCRvSlFNJR1eONhlBZvYHJnwFdhDVo72pWPV22o7s86pa9kaWxLp3oANkxxcszoHf4U1AjYiCWTO5Z7DvCEDhhn6X%2FNK5S1YHBXKeMP6gNVlXBIjXDStkcfjxuOia1eh7XxG%2Fg8MhKcKgOYuaBD2v5%2BPqBSs2hy%2FzDWLwUhlrfowt%2Bdn58bHQwrJTMuP68H1ZX1CdPrbTcm101VH9FDto3ycl63BbmbN6JxjKM3Kc9Svw6NF5Xgj0N8KvuHVNCHi%2BhSOnyklVmz1boAvYl9MH8K74B1m%2Fbhj65je7DAvv5WEzUGA7yZMskUks7VHiAT79pqTJU2m6yoRQDb1xw2UNUk10qWJ9j8i1bKSDkw9%2Fea0QY6mAHYJHqsX0F1Gde46T9gfKkaPd0rsK%2Bdf5r04E6FBVUK%2FjywayeYnsFAG9ZtQe69Ua87qrlIrFPP%2BVZD4SQijB%2FMagJWpPSz8ApPhTbS%2Fx3SsV9MenH7SvSKGr8w6GdeYgnWM1xEqZ%2FSADA2bhYUNue06CykYMdZg3ZGY2sTX28sF6f%2BTzJkVrm0X0lM7CHI6P0zK6bdDF19xw%3D%3D&Expires=1780926922
