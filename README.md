# UAV-USV-Landing
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated, research-oriented reading list for **unmanned aerial vehicle (UAV) landing on unmanned surface vehicle (USV)** in waves, wind, and open-water conditions — with a focus on **perception + estimation + control + learning**.

> Goal: help researchers quickly map the space, compare assumptions, and find practical baselines, code, and open problems.

---

## Contents
- [Scope](#scope)
- [How to Use This List](#how-to-use-this-list)
- [Core Papers](#core-papers)
- [Simulation, Benchmarks, and Evaluation](#simulation-benchmarks-and-evaluation)
- [Open Problems](#open-problems)
- [Contributing](#contributing)

---

## Scope
**Included**
- UAV landing / touchdown on **moving** USVs (waves, vessel tilt/roll/pitch, wind)
- Timing policies / “land at the right time & place”
- Relative pose estimation, onboard sensing, limited/no-comm settings
- Control: MPC, robust MPC, distributed MPC, safety filters, learning residuals / GP / RL (when relevant)

**Excluded (unless directly tied to USV landing)**
- Generic UAV landing on static pads
- UAV landing on UGVs without marine motion modeling

---

## How to Use This List
Each paper entry is written to be *comparable*, not just a link.

**Legend**
- **Setting**: open water? waves? wind? indoor emulation?
- **Sensing**: AprilTag / vision / VIO / RTK / IMU fusion / etc.
- **Method**: MPC / distributed MPC / GP / RL / safety filter / hybrid
- **Assumptions**: comms? shared states? known deck motion model?
- **Validation**: sim / hardware / sea trials
- **Notes**: 1–2 lines on what’s actually new + key limitations

---

## Core Papers

### Distributed / Decentralized MPC for UAV–USV Landing

#### Distributed Model Predictive Control for Cooperative Multirotor Landing on Uncrewed Surface Vessel in Waves
- **Link**: https://arxiv.org/abs/2402.10399  
- **Authors / Venue / Year**: Jess Stephenson, Nathan T. Duncan, Melissa Greeff — (ICUAS / arXiv) 2024
- **Setting**: UAV–USV cooperative landing in waves; choose safer location & timing
- **Sensing / State**: (fill)  
- **Method**: **Sequential distributed MPC** with **artificial intermediate goals** + coupling cost
- **Assumptions**: (fill: comms? shared state? delays?)  
- **Validation**: (fill: simulation only? platform? real experiments?)  
- **Key idea (1-liner)**: Distributed coordination by sharing only an *artificial goal* (not full states/controls) while accounting for wave-induced tilt.
- **Notes**:
  - Strength: (fill)
  - Limitation: (fill)
- **Tags**: `distributed-MPC`, `cooperative-landing`, `waves`, `timing`

#### A Time and Place to Land: Online Learning-Based Distributed MPC for Multirotor Landing on Surface Vessel in Waves
- **Link**: https://arxiv.org/abs/2410.21674  
- **Authors / Venue / Year**: Jess Stephenson, William S. Stewart, Melissa Greeff — 2024 (arXiv; check final venue)
- **Setting**: wave-like conditions; emphasizes timing/place to reduce tilt risk
- **Sensing / State**: (fill)  
- **Method**: Distributed MPC + **tilt-aware coupling cost learned as a Gaussian Process (GP)**
- **Assumptions**: (fill: comms? partial sharing? delayed?)  
- **Validation**: indoor experiments using a custom platform attached to a UGV to emulate USV tilt motion
- **Key idea (1-liner)**: Learn a tilt-related cost online (GP) so the distributed MPC becomes “tilt-smart”.
- **Notes**:
  - Strength: (fill)
  - Limitation: (fill)
- **Tags**: `distributed-MPC`, `GP`, `learning-cost`, `tilt-aware`, `experiments`

> (Optional project / lab page) Robora Lab landing project hub: https://robora-lab.github.io/uav-usv-landing/

---

### MPC for Harsh Winds & Open Waters

#### Landing a UAV in Harsh Winds and Turbulent Open Waters
- **Link**: https://arxiv.org/abs/2301.00255  
- **Authors / Venue / Year**: Parakh M. Gupta, Eric Pairet, Tiago Nascimento, Martin Saska — IEEE RA-L (check final year/issue)
- **Setting**: harsh winds + turbulent open waters; UAV landing on USV with severe roll/pitch risk
- **Sensing / State**: onboard **vision-based** observation & prediction of USV motion (no external comms)
- **Method**: MPC with a **novel objective** + online decomposition of oscillatory vessel motion (for near-zero-tilt touchdown timing)
- **Assumptions**: no communication with USV / ground station (confirm details)
- **Validation**: extensive simulations + real-world scenarios (fill specifics: sea trials? outdoor? wind conditions?)
- **Key idea (1-liner)**: Predict the deck motion from onboard vision and land during near-zero tilt windows.
- **Notes**:
  - Strength: (fill)
  - Limitation: (fill)
- **Tags**: `MPC`, `vision`, `no-comms`, `open-waters`, `wind`

---

## Simulation, Benchmarks, and Evaluation

### Suggested Metrics
- **Success rate** (touchdown without bounce / slip / crash)
- **Touchdown relative velocity** (vertical + lateral)
- **Tilt at touchdown** (roll/pitch, or deck normal angle)
- **Tracking error** (relative position/heading)
- **Time-to-land** / energy
- **Robustness sweeps**: wind levels, wave states, comms delay/dropout, perception failure rate

### Sim Tools / Building Blocks

#### Wave + Vessel Motion (USV / marine environment)
- [VRX (Virtual RobotX) Simulation](https://github.com/osrf/vrx) — Gazebo-based USV competition-grade marine environment (waves, tasks, sensors).  
- [Gazebo Sim](https://gazebosim.org/) — the core simulator stack used by many marine + aerial robotics projects.  
- [UUV Simulator](https://github.com/uuvsimulator/uuv_simulator) — Gazebo/ROS plugins for underwater/ocean environments (useful for ocean-current / fluid plugins even if you only need “water physics”).

#### UAV Dynamics + Wind / Disturbance Models
 [PX4 SITL + Gazebo (gz)](https://docs.px4.io/main/en/sim_gazebo_gz/) — common research baseline for multirotors/VTOL; supports wind and rich worlds.  
- [PX4 Gazebo Models & Worlds](https://github.com/PX4/PX4-gazebo-models) — curated vehicles/worlds used by PX4’s Gazebo Sim workflows.  
- [PX4-SITL Gazebo Classic Plugins](https://github.com/PX4/PX4-SITL_gazebo-classic) — classic plugin suite (still cited in older papers).  
- [RotorS Simulator](https://github.com/ethz-asl/rotors_simulator) — classic academic multirotor dynamics + Gazebo plugins (includes wind plugin; widely referenced in literature).  
- [JSBSim](https://github.com/JSBSim-Team/jsbsim) ([official site](https://jsbsim-team.github.io/jsbsim/index.html)) — flight dynamics model (FDM) used in flight simulation research.

#### Photorealistic / Vision-Centric Simulation (for perception-heavy landing papers)
- [AirSim](https://github.com/microsoft/AirSim) ([docs](https://microsoft.github.io/AirSim/)) — Unreal/Unity-based sim with PX4/ArduPilot SITL support; popular for vision-based UAV research.  
- [Flightmare](https://github.com/uzh-rpg/flightmare) ([project page](https://uzh-rpg.github.io/flightmare/)) — modular quadrotor sim (Unity renderer + physics engine); common in learning/perception pipelines.  
- [NVIDIA Isaac Sim](https://developer.nvidia.com/isaac/sim) ([docs](https://docs.omniverse.nvidia.com/isaacsim/index.html)) — high-fidelity robotics sim + synthetic data generation.

#### Autopilot + SITL Tooling (common in reproducible baselines)
- [ArduPilot SITL (official docs)](https://ardupilot.org/dev/docs/sitl-simulator-software-in-the-loop.html) — widely used SITL workflow (often paired with Mission Planner / MAVProxy).  
- [PX4 Simulation Overview](https://docs.px4.io/main/en/simulation/) — entry point for all PX4 sim backends.

#### ROS / MAVLink Interfaces (bridge research code ↔ autopilot)
- [ROS 2 Docs](https://docs.ros.org/) — standard middleware for research pipelines.  
- [MAVROS](https://github.com/mavlink/mavros) — MAVLink ↔ ROS bridge (ROS1/ROS2 branches).  
- [MAVSDK](https://github.com/mavlink/MAVSDK) ([guide](https://mavsdk.mavlink.io/)) — high-level MAVLink API (C++/Python/etc.) used for offboard control + integration tests.

#### Vision + AprilTag / Marker Toolchain (landing pad detection, relative pose)
- [AprilTag 3](https://github.com/AprilRobotics/apriltag) — core AprilTag detector library.  
- [apriltag_ros](https://github.com/AprilRobotics/apriltag_ros) — ROS wrapper for AprilTag 3.  
- [OpenCV ArUco module](https://docs.opencv.org/3.4/d9/d6d/tutorial_table_of_content_aruco.html) — alternative marker family + pose estimation tools (often used when AprilTag isn’t required).

#### Reproducible Baselines (add your own as you build!)
- (your baseline) `px4 + gazebo + deck-motion-world` — minimal example to reproduce your landing setup
- (your baseline) `mpc-controller + estimator + logs` — scripts for replay and benchmarking

---

## Open Problems
- **Perception failures**: marker loss, glare, motion blur, sea spray
- **Latency & comms**: delayed state sharing in distributed MPC; packet loss
- **Contact dynamics**: touchdown friction, hooks/magnets, deck compliance
- **Generalization**: from indoor tilt rigs → real sea states
- **Safety guarantees**: hard constraints with learning in the loop
- **Benchmarking**: lack of standardized wave/wind suites + public datasets

---

## Contributing
PRs are welcome! Please add entries using the template below.

### Paper Entry Template
~~~md
#### Paper Title
- **Link**: (arXiv / DOI / publisher)
- **Authors / Venue / Year**:
- **Setting**:
- **Sensing / State**:
- **Method**:
- **Assumptions**:
- **Validation**:
- **Key idea (1-liner)**:
- **Notes**:
  - Strength:
  - Limitation:
- **Tags**:
~~~

---


