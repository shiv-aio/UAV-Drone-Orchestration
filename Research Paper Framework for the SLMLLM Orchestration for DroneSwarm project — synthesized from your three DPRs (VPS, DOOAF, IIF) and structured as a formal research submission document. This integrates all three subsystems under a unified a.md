# **Research Paper Framework** for the **SLM/LLM Orchestration for Drone/Swarm** project — synthesized from your three DPRs (VPS, DOOAF, IIF) and structured as a formal research submission document. This integrates all three subsystems under a unified agentic AI orchestration umbrella.

***

# SLM/LLM Orchestration for Tactical Drone & Swarm Systems
## A Research Framework by Aiotize Inc. in collaboration with Pace Defence & Indian Army

***

## 1. Abstract

**Research Project Title:** *SLM/LLM-Orchestrated Multi-Agent Architecture for Autonomous Tactical Drone and Swarm Operations in Contested Environments*

Modern battlefield operations demand drone systems that can reason, adapt, and collaborate in real time — capabilities far beyond classical rule-based autopilot architectures. This research proposes a unified **Small Language Model (SLM) and Large Language Model (LLM) orchestration layer** that binds three independently validated tactical systems — the Vision-Based Positioning System (VPS), Direction of Own Artifact (DOOAF), and Integrated Drone Command & Fleet Management (IIF) — into a single, context-aware, agentic decision-making platform.[1][2][3]

### Research Objectives
- Establish a formal orchestration architecture where SLMs execute edge-resident tactical reasoning and LLMs provide mission-level planning and situational understanding
- Design multi-agent harnesses enabling cooperative autonomy across heterogeneous drone platforms (fixed-wing, multirotor, VTOL)[1]
- Achieve GPS-denied autonomous navigation, real-time target direction computation, and fleet-level IFF identification via unified agent coordination[2][3]
- Validate system resilience under electronic warfare, communication jamming, and adverse terrain conditions[3]

### Implementation Goals
- Deploy edge-resident SLM agents on NVIDIA Jetson Orin (40 TOPS) within power/weight constraints of tactical UAVs[3]
- Integrate orchestrated agent outputs with ArduPilot/MAVLink autopilot stack via standardized interfaces[1]
- Achieve sub-500ms end-to-end latency from sensor perception to command execution[2]

### Expected Outcomes
- **TRL 6** prototype demonstrating SLM/LLM-orchestrated autonomous swarm operations in GPS-contested environments[3][1]
- Validated agentic IFF identification, cooperative navigation, and real-time targeting pipelines[2]
- Publishable research establishing benchmarks for edge-AI orchestration in military UAV contexts[1]

***

## 2. Project Implementation Plan

### Resource Architecture

| Resource Domain | Component | Specification | Purpose |
|---|---|---|---|
| **Edge Compute** | NVIDIA Jetson Orin Nano | 40 TOPS, 10W TDP | SLM inference on-drone [3] |
| **Ground Compute** | LLM Server (GPU cluster) | RTX 4090 / A100 | Mission planning, context reasoning [1] |
| **Vision Hardware** | Stereo RGB + IR cameras | Global shutter, 1280×720 | VPS & DOOAF perception [3][2] |
| **Autopilot** | Pixhawk / CubeOrange | ArduPilot, MAVLink v2 | Flight execution interface [1] |
| **Communication** | RFD900x + FHSS mesh | AES-256, frequency-hopped | Secure swarm comm link [1] |
| **IMU/Sensors** | 9-axis IMU + barometer | <10°/hr gyro bias | Sensor fusion layer [3] |
| **Data Infrastructure** | InfluxDB + PostgreSQL | Time-series + relational | Telemetry & mission state [1] |

### Development Objectives by Agent Type

**Tier 1 — Perception Agents (Edge SLM)**
- Visual SLAM agent: Runs ORB-SLAM3/OpenVINS on Jetson, outputs 6-DOF pose estimates to autopilot[3]
- Target detection agent: YOLOv8 custom model for enemy asset classification, feeds DOOAF geolocation engine[2]
- IFF beacon agent: Validates encrypted drone ID broadcasts, classifies aerial objects as friend/foe[1]

**Tier 2 — Coordination Agents (Lightweight LLM/SLM)**
- Fleet coordinator agent: Manages task allocation, deconfliction, and collision avoidance across the swarm[1]
- Sensor fusion agent: Runs Extended Kalman Filter (EKF) integrating visual, IMU, barometric, and GPS data[3]
- Direction computation agent: Calculates azimuth/elevation/range vectors from drone-detected targets to friendly gun positions[2]

**Tier 3 — Mission Orchestration (LLM — Ground)**
- Mission planner: Interprets high-level commander intent, generates waypoint sequences and swarm task graphs[1]
- Threat response agent: Re-plans routes and swarm formations in response to EW events or asset losses[3]
- Post-mission analyst: Generates structured after-action reports from logged telemetry[1]

### Codebase & Dependency Stack

- **Languages:** C++ (real-time ROS2 nodes, SLAM), Python (AI training, LLM orchestration, MAVLink parsing)[3]
- **AI Frameworks:** TensorFlow Lite, PyTorch Mobile, ONNX Runtime (edge), HuggingFace Transformers (ground LLM)[3]
- **Middleware:** ROS2 (sensor integration, agent communication bus)[3]
- **Simulation:** Gazebo + ArduPilot SITL, Microsoft AirSim (photorealistic)[3]
- **Orchestration Harness:** LangChain / custom multi-agent framework over ROS2 action servers
- **DevOps:** Docker containerization, CI/CD pipelines, Git version control[1]
- **Backend Services:** Node.js (real-time GCS server), React.js + Leaflet (map dashboard), WebSocket (telemetry streaming)[1]

***

## 3. Workflow, Simulation & Success Parameters

### Operational Workflow (Mission Execution Loop)

1. **Pre-Mission** — LLM ground agent ingests commander intent, generates mission plan, loads terrain database into VPS, assigns drone roles in swarm[1][3]
2. **Launch & Ingress** — Drones launch under GPS; VPS agent monitors GPS quality and prepares visual SLAM handover[3]
3. **GPS Denial Event** — VPS agent auto-switches to visual odometry; SLAM map construction begins; pose estimates routed to autopilot via MAVLink[3]
4. **Target Acquisition** — DOOAF agent detects target via EOIR payload; geolocates in 3D using GPS/IMU fusion; computes azimuth/elevation to friendly gun system[2]
5. **IFF Validation** — IFF agent cross-validates all airborne objects against encrypted beacon registry; flags unknowns for operator review[1]
6. **Swarm Coordination** — Fleet coordinator agent deconflicts airspace, assigns sub-tasks (ISR, loitering, relay), maintains collision margins[1]
7. **Engagement / Data Relay** — DOOAF transmits gun alignment packet (AES-256 encrypted) to ground terminal within 500ms latency target[2]
8. **Return & Landing** — VPS return-to-home logic navigates using stored visual landmarks; safe landing zone detection via terrain AI[3]
9. **Post-Mission** — LLM analyst generates structured debrief from InfluxDB telemetry logs; flags anomalies for research records[1]

### Simulation Strategy

| Phase | Environment | Method | Purpose |
|---|---|---|---|
| Algorithm validation | Gazebo / AirSim | SITL (Software-in-the-Loop) | Baseline SLAM & agent testing [3] |
| Hardware integration | Lab bench | HITL (Hardware-in-the-Loop) | Real autopilot + simulated world [1] |
| Swarm coordination | Multi-agent AirSim | Multi-SITL | Fleet coordination testing [1] |
| GPS denial scenarios | Controlled RF chamber | Live EW jamming | VPS failover validation [3] |
| LLM orchestration | Virtual mission environment | Agent sandbox | Full pipeline end-to-end test |

### Success Parameters

- **VPS Navigation Accuracy:** ≤2m CEP (Circular Error Probable) in GPS-denied mode[3]
- **Return-to-Home Success Rate:** ≥95% under complete GPS jamming[3]
- **DOOAF Target Geolocation:** ≤10m CEP; ≤1° angular direction error[2]
- **End-to-End Targeting Latency:** ≤500ms from target detection to gun terminal display[2]
- **IFF Classification Accuracy:** ≥98% correct friend/foe identification in multi-drone scenarios[1]
- **Fleet Scalability:** Real-time coordination of ≥5 drones (prototype), architecture designed for ≥50[1]
- **VPS Processing Latency:** ≤100ms frame-to-pose-estimate pipeline[3]
- **Communication Reliability:** ≥99.9% packet delivery in mesh network[2]
- **Agent Inference Latency (Edge SLM):** ≤200ms per decision cycle on Jetson Orin
- **TRL Target:** TRL 6 for all three sub-systems upon project completion[1][3]

***

## 4. Project Management, Documentation & Records Plan

### Phase-Gated Development Structure

**Phase 1 — Foundation & Research Setup (Months 1–2)**
- Requirements analysis workshops with Pace Defence / Army stakeholders[1]
- SLM/LLM orchestration architecture design and agent harness specification
- Simulation environment setup (Gazebo, AirSim, multi-SITL)[3]
- *Deliverables:* Feasibility report, architecture document, agent spec sheets, hardware bill-of-materials

**Phase 2 — Core Agent Development (Months 2–5)**
- VPS SLAM agent development and simulation validation[3]
- DOOAF target detection and direction computation agent development[2]
- IFF engine and fleet coordination agent coding and unit testing[1]
- LLM orchestration harness integration over ROS2
- *Deliverables:* Functional agents in simulation, unit test reports, SITL benchmark results

**Phase 3 — Hardware Integration (Months 5–7)**
- Jetson Orin module integration with cameras, IMU, and autopilot[3]
- MAVLink interface development for agent-to-autopilot command bridge[1]
- Ground LLM server setup and API gateway to GCS dashboard[1]
- *Deliverables:* Integrated hardware prototype, HITL test reports, MAVLink interface documentation

**Phase 4 — System Testing & Swarm Validation (Months 7–8)**
- Indoor GPS-denied flight tests with VPS agent active[3]
- Multi-drone HITL swarm coordination tests[1]
- DOOAF bench and HIL testing with simulated artillery targets[2]
- Full LLM orchestration pipeline end-to-end testing
- *Deliverables:* Test reports, tuned models, failure mode analysis, anomaly logs

**Phase 5 — Field Validation & Research Closure (Month 9)**
- Outdoor GPS jamming trials; terrain variation testing[3]
- Army evaluation exercises with IIF + DOOAF integration[1]
- Research paper drafting, IP filings, technology transfer package
- *Deliverables:* Field trial reports, final validated prototype, research publications, training materials[3][1]

### Team Structure (Cohort-Based R&D Model)

| Role | Count | Domains |
|---|---|---|
| Project Lead / Research Director | 1 | Architecture, stakeholder management [1] |
| AI/ML Research Engineers | 3 | SLM fine-tuning, SLAM, YOLOv8, orchestration |
| Embedded Systems Engineers | 2 | Jetson, ROS2, real-time C++ [3] |
| Drone Systems Engineers | 2 | ArduPilot, MAVLink, flight testing [1] |
| Communication / RF Engineers | 1 | AES-256, FHSS, mesh networking [1] |
| Backend / DevOps Engineers | 2 | Node.js, databases, CI/CD, Docker [1] |
| Test & Validation Engineers | 2 | HITL, field trials, metrics [2] |
| Technical Writer / IP Manager | 1 | Documentation, patent filings [3] |

### Documentation & Records Framework

- **Version Control:** Git repository (per sub-project: VPS, DOOAF, IIF, Orchestration Layer) with mandatory PR reviews[1]
- **Research Records:** Structured lab notebooks (digital) per sprint; experimental logs linked to simulation artifacts
- **Technical Documents:** System Architecture Document (SAD), Algorithm Technical Specs, API docs, Integration Manuals[3][1]
- **Test Records:** SITL/HITL test logs in InfluxDB; field trial reports with GPS-truth comparison data[3]
- **IP Registry:** Joint IP record maintained with Pace Defence and Indian Army; patent applications filed post-Phase 2[1][3]
- **Progress Reviews:** Bi-weekly sprint reviews + monthly milestone gate reviews with Pace Defence[1]
- **Research Publications:** Target 2–3 conference/journal papers covering VPS algorithms, DOOAF pipeline, and SLM/LLM orchestration architecture[3]
- **Security Compliance:** All documentation classified per defence data handling protocols; no external network connectivity on secure systems[1]

Sources
[1] DPR-3_-Integrated-Drone-Command-Fleet-Management-IIF.pdf https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_0a41058c-f5d5-4d34-a75a-5e2193005f65/f7b6276c-93b3-4b39-a7cd-910248d80c3b/DPR-3_-Integrated-Drone-Command-Fleet-Management-IIF.pdf?AWSAccessKeyId=ASIA2F3EMEYEVXH2SGMQ&Signature=DdG5olbPkNqL9p%2BkLEnuF21xhhE%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJIMEYCIQCUzJ121PlsWjUjuvHiZIw%2BI6iKGyLBvxllcdGIQKE60AIhAJLXDg2fv91oy4eTFnCJjO5gI8ngByVdGlDZhBXVsnB%2BKvwECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQARoMNjk5NzUzMzA5NzA1Igz9qWTq20rBZeF0vRYq0AQU4we2CKDtQAhuAq8Q1xEsS2OWWrqKtZYM7uFy0ogTfdN5WJR8b9INHj3G0yhhqy2xVEET%2FkQCgMPqMkSV9MbueOPJso37S5MkbS4IAM1Xl6QZ96GVFwL32N4GUW6XkXd9v1AKGmwv3DW1BgVK%2BKdsbB5CaUdqCht3pEKhizd%2ByTVm1KbX1s6LQuwZX5GdFWgN8ZAdZEpCJfe9boXAbzDJibXtFimerEPXrHhj2gpjVOx7nFtlPCp1K0JlN%2BLFZw3x7A0UpIf1tFevzCm1eDfTal1oXxKY6NX3ZC908ysDYauS0UyF3RTliLsOkSkHTWm5GMAvi2N0%2FZRe%2F9rqDKsv7DVnMLzT7gQohwXoe%2FTkUhTuyoVFr53s1W758KI87R1M5S99A8wjZmS1gNqEIoOKz5C%2B2FqIKDjDW2kL3WFu0VobG0%2FYiRI7tfi42aPu1EkMJ27MvYVAQigSeN19%2FnQzy8IHkhzLhMd3cX3QMM2KcHvoxMQufxl5mc4uvwJuYDQFaB0Ag9C11GbNzCrHyJnPo%2BEZDBMi09uqZqBjZV3em4ZW7r7ypDsNiIJp2JzS5AQ8LhLsJEH6xbuCt%2BTbF8yYPuXQTCGLX0u%2BWUBJ60gyQBBYwlr6QaC%2BWTSVpdWMFTMI3deWd64uZPjrwBTBO3Vj0%2Fb5Ue4xq9Tc0YpKrdx1GLDmBaYyM8zLFV%2FZinTM6TwI7PI4D0o9RXy4oJo8IujkUml3Kk6BRg8RTglfWRM%2BbCXmCENLIGtF2sUVV9pvMM%2BNJxJU4k3GrH8gcp5s3xPkMIf9mtEGOpcBpaXEo83FnhWcl8OWzRgDPMxQkj35KLZO5juk6TS1dw3Kobyf%2B2nzPvpRhu7Ly75Guyagi7%2Bqe59rZ3aEg%2BuXwLREFenEDMwYVWO0jzq8kfI9mErJE4CH4dIqCQluzlH8YHZXo3apjenWF0LSz2%2FY8zwDWDZukLj8egpH0OivrE37m5kgFH6x%2FFDgQxDYDQz%2BsQPJU06rZA%3D%3D&Expires=1780927578
[2] DPR-2_-Direction-of-Own-Artifact-DOOAF.pdf https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_0a41058c-f5d5-4d34-a75a-5e2193005f65/44763cbf-382b-4b61-b2a2-0c0c074c6461/DPR-2_-Direction-of-Own-Artifact-DOOAF.pdf?AWSAccessKeyId=ASIA2F3EMEYEVXH2SGMQ&Signature=BYamFUHRyyy%2BLbLbMBNt75rsXrs%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJIMEYCIQCUzJ121PlsWjUjuvHiZIw%2BI6iKGyLBvxllcdGIQKE60AIhAJLXDg2fv91oy4eTFnCJjO5gI8ngByVdGlDZhBXVsnB%2BKvwECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQARoMNjk5NzUzMzA5NzA1Igz9qWTq20rBZeF0vRYq0AQU4we2CKDtQAhuAq8Q1xEsS2OWWrqKtZYM7uFy0ogTfdN5WJR8b9INHj3G0yhhqy2xVEET%2FkQCgMPqMkSV9MbueOPJso37S5MkbS4IAM1Xl6QZ96GVFwL32N4GUW6XkXd9v1AKGmwv3DW1BgVK%2BKdsbB5CaUdqCht3pEKhizd%2ByTVm1KbX1s6LQuwZX5GdFWgN8ZAdZEpCJfe9boXAbzDJibXtFimerEPXrHhj2gpjVOx7nFtlPCp1K0JlN%2BLFZw3x7A0UpIf1tFevzCm1eDfTal1oXxKY6NX3ZC908ysDYauS0UyF3RTliLsOkSkHTWm5GMAvi2N0%2FZRe%2F9rqDKsv7DVnMLzT7gQohwXoe%2FTkUhTuyoVFr53s1W758KI87R1M5S99A8wjZmS1gNqEIoOKz5C%2B2FqIKDjDW2kL3WFu0VobG0%2FYiRI7tfi42aPu1EkMJ27MvYVAQigSeN19%2FnQzy8IHkhzLhMd3cX3QMM2KcHvoxMQufxl5mc4uvwJuYDQFaB0Ag9C11GbNzCrHyJnPo%2BEZDBMi09uqZqBjZV3em4ZW7r7ypDsNiIJp2JzS5AQ8LhLsJEH6xbuCt%2BTbF8yYPuXQTCGLX0u%2BWUBJ60gyQBBYwlr6QaC%2BWTSVpdWMFTMI3deWd64uZPjrwBTBO3Vj0%2Fb5Ue4xq9Tc0YpKrdx1GLDmBaYyM8zLFV%2FZinTM6TwI7PI4D0o9RXy4oJo8IujkUml3Kk6BRg8RTglfWRM%2BbCXmCENLIGtF2sUVV9pvMM%2BNJxJU4k3GrH8gcp5s3xPkMIf9mtEGOpcBpaXEo83FnhWcl8OWzRgDPMxQkj35KLZO5juk6TS1dw3Kobyf%2B2nzPvpRhu7Ly75Guyagi7%2Bqe59rZ3aEg%2BuXwLREFenEDMwYVWO0jzq8kfI9mErJE4CH4dIqCQluzlH8YHZXo3apjenWF0LSz2%2FY8zwDWDZukLj8egpH0OivrE37m5kgFH6x%2FFDgQxDYDQz%2BsQPJU06rZA%3D%3D&Expires=1780927578
[3] DPR-1_-Vision-Based-Positioning-System-VPS.pdf https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/collection_0a41058c-f5d5-4d34-a75a-5e2193005f65/d9bd10f6-02e1-4763-a4d5-47e0b5eac2d4/DPR-1_-Vision-Based-Positioning-System-VPS.pdf?AWSAccessKeyId=ASIA2F3EMEYEVXH2SGMQ&Signature=kHWEnwvMXcyfaHjHecK6S%2BMaYmk%3D&x-amz-security-token=IQoJb3JpZ2luX2VjEO7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJIMEYCIQCUzJ121PlsWjUjuvHiZIw%2BI6iKGyLBvxllcdGIQKE60AIhAJLXDg2fv91oy4eTFnCJjO5gI8ngByVdGlDZhBXVsnB%2BKvwECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQARoMNjk5NzUzMzA5NzA1Igz9qWTq20rBZeF0vRYq0AQU4we2CKDtQAhuAq8Q1xEsS2OWWrqKtZYM7uFy0ogTfdN5WJR8b9INHj3G0yhhqy2xVEET%2FkQCgMPqMkSV9MbueOPJso37S5MkbS4IAM1Xl6QZ96GVFwL32N4GUW6XkXd9v1AKGmwv3DW1BgVK%2BKdsbB5CaUdqCht3pEKhizd%2ByTVm1KbX1s6LQuwZX5GdFWgN8ZAdZEpCJfe9boXAbzDJibXtFimerEPXrHhj2gpjVOx7nFtlPCp1K0JlN%2BLFZw3x7A0UpIf1tFevzCm1eDfTal1oXxKY6NX3ZC908ysDYauS0UyF3RTliLsOkSkHTWm5GMAvi2N0%2FZRe%2F9rqDKsv7DVnMLzT7gQohwXoe%2FTkUhTuyoVFr53s1W758KI87R1M5S99A8wjZmS1gNqEIoOKz5C%2B2FqIKDjDW2kL3WFu0VobG0%2FYiRI7tfi42aPu1EkMJ27MvYVAQigSeN19%2FnQzy8IHkhzLhMd3cX3QMM2KcHvoxMQufxl5mc4uvwJuYDQFaB0Ag9C11GbNzCrHyJnPo%2BEZDBMi09uqZqBjZV3em4ZW7r7ypDsNiIJp2JzS5AQ8LhLsJEH6xbuCt%2BTbF8yYPuXQTCGLX0u%2BWUBJ60gyQBBYwlr6QaC%2BWTSVpdWMFTMI3deWd64uZPjrwBTBO3Vj0%2Fb5Ue4xq9Tc0YpKrdx1GLDmBaYyM8zLFV%2FZinTM6TwI7PI4D0o9RXy4oJo8IujkUml3Kk6BRg8RTglfWRM%2BbCXmCENLIGtF2sUVV9pvMM%2BNJxJU4k3GrH8gcp5s3xPkMIf9mtEGOpcBpaXEo83FnhWcl8OWzRgDPMxQkj35KLZO5juk6TS1dw3Kobyf%2B2nzPvpRhu7Ly75Guyagi7%2Bqe59rZ3aEg%2BuXwLREFenEDMwYVWO0jzq8kfI9mErJE4CH4dIqCQluzlH8YHZXo3apjenWF0LSz2%2FY8zwDWDZukLj8egpH0OivrE37m5kgFH6x%2FFDgQxDYDQz%2BsQPJU06rZA%3D%3D&Expires=1780927578

