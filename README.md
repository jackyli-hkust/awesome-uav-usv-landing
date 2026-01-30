# AWESOME UAV–USV Landing
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated, research-oriented reading list for **unmanned aerial vehicle (UAV) landing on unmanned surface vehicle (USV)** in waves, wind, and open-water conditions — with a focus on **perception + estimation + control + learning**.

> Goal: help researchers quickly map the space, compare assumptions, and find practical baselines, code, and open problems.

<!-- Option A (recommended): local image in your repo -->
<p align="center">
  <img src="assets/cover.png" alt="UAV–USV Landing Cover" width="900" />
</p>

---

## Contents
- [Scope](#scope)
- [How to Use This List](#how-to-use-this-list)
- [Core Papers](#core-papers)
  - [Distributed / Decentralized MPC](#distributed--decentralized-mpc-for-uavusv-landing)
  - [MPC for Harsh Winds & Open Waters](#mpc-for-harsh-winds--open-waters)
  - [Reinforcement Learning (RL) for UAV–USV / Moving-Deck Landing](#reinforcement-learning-rl-for-uavusv--moving-deck-landing)
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
- **Authors / Venue / Year**: Jess Stephenson, Nathan T. Duncan, Melissa Greeff — **ICUAS 2024** (also on arXiv), pp. **645–651**, DOI: **10.1109/ICUAS60882.2024.10557042**
- **Setting**: cooperative UAV–USV landing in waves; choose safer location & timing
- **Sensing / State**: assumes **state measurements available** for both UAV and USV (focus is coordination/control, not perception)
- **Method**: **Sequential distributed MPC** with **artificial intermediate goals** + coupling cost; uses a **spatio-temporal wave model** to reduce severe tilt at touchdown
- **Assumptions**: **bilateral communication**; each agent shares only the *artificial goal* (not full state/control), but measurements exist (and comms may be delayed)
- **Validation**: simulation study (no hardware in this paper)
- **Key idea (1-liner)**: Coordinate landing **time + place** using distributed MPC while communicating only a lightweight “goal” variable.
- **Notes**:
  - Strength: clean “minimal-sharing” coordination mechanism that’s easy to compare against centralized MPC
  - Limitation: depends on reliable state availability and a modeled wave/tilt representation
- **Tags**: `distributed-MPC`, `cooperative-landing`, `waves`, `timing`

#### A Time and Place to Land: Online Learning-Based Distributed MPC for Multirotor Landing on Surface Vessel in Waves
- **Link**: https://arxiv.org/abs/2410.21674  
- **Authors / Venue / Year**: Jess Stephenson, William S. Stewart, Melissa Greeff — **ICUAS 2025**, pp. **193–199**, DOI: **10.1109/ICUAS65942.2025.11007817** (also on arXiv)
- **Setting**: wave-like conditions; emphasizes timing/place to reduce tilt risk
- **Sensing / State**: indoor experiments with **external motion capture (Vicon)** for state; tilt motion emulated by a custom platform attached to a UGV
- **Method**: distributed MPC + **tilt-related coupling cost learned online as a Gaussian Process (GP)**
- **Assumptions**: agents optimize locally and **share only artificial goals**; communication limitations motivate reduced sharing
- **Validation**: indoor experimental validation with a tilting rig (UGV-mounted platform), plus comparisons to non-tilt-aware baselines
- **Key idea (1-liner)**: Learn a **tilt penalty** online (GP) so distributed MPC becomes “tilt-smart” in real time.
- **Notes**:
  - Strength: bridges “distributed MPC theory” to a real experimental setup with measurable success-rate gains
  - Limitation: still not full open-water sea trials; perception is not the main contribution
- **Tags**: `distributed-MPC`, `GP`, `learning-cost`, `tilt-aware`, `experiments`

---

### MPC for Harsh Winds & Open Waters

#### Landing a UAV in Harsh Winds and Turbulent Open Waters
- **Link**: https://arxiv.org/abs/2301.00255  
- **Authors / Venue / Year**: Parakh M. Gupta, Èric Pairet, Tiago P. Nascimento, Martin Saska — **IEEE Robotics and Automation Letters (RA-L)**, **Vol. 8(2)**, **pp. 744–751**, **2023**, DOI: **10.1109/LRA.2022.3231831**
- **Setting**: harsh open-water-like conditions (waves + strong wind); goal is touchdown during **near-zero tilt windows**
- **Sensing / State**: **onboard monocular vision** with **AprilTag** on landing platform; **no communication** with the USV; estimator predicts vessel motion online
- **Method**: MPC with (i) a **novel objective** and (ii) **online decomposition of oscillatory vessel motion** (FFT + prediction) to time the landing
- **Assumptions**: no UAV–USV comms; relies on fiducial detection availability and a prediction horizon that is short enough for real-time control
- **Validation**: simulations + “realistic” tests described in the paper (vision-rate variability, estimator behavior, touchdown tilt statistics)
- **Key idea (1-liner)**: Predict deck motion from onboard vision and commit to landing when the deck tilt is predicted to be small.
- **Notes**:
  - Strength: very clear “timing policy” story implemented inside an MPC loop with realistic sensing constraints
  - Limitation: vision dropouts / motion blur remain a major practical risk for open-water operations
- **Tags**: `MPC`, `vision`, `AprilTag`, `no-comms`, `open-waters`, `wind`

---

### Reinforcement Learning (RL) for UAV–USV / Moving-Deck Landing

> This section includes **direct ship/USV landing RL** *and* a few **high-quality moving-platform RL baselines** that are commonly reused for deck-landing research.

#### Robust Reinforcement Learning Algorithm for Vision-based Ship Landing of UAVs
- **Link**: https://arxiv.org/abs/2209.08381  
- **Authors / Venue / Year**: Vishnu Saj, Bochan Lee, Dileep Kalathil, Moble Benedict — **arXiv preprint**, 2022 (DOI: **10.48550/arXiv.2209.08381**)
- **Setting**: VTOL ship landing with **6-DoF deck motion** + wind gusts (sub-scale test platform)
- **Sensing / State**: **monocular camera only**; tracks a **horizon reference bar** on the landing platform (pilot-inspired visual cue)
- **Method**: robust deep RL controller (compared against a nonlinear PID baseline) + a dedicated vision pipeline for relative localization
- **Assumptions**: relies on consistent visibility of the horizon reference bar; learning targets robustness to gusts
- **Validation**: **Gazebo simulation** + **real-world tests** using a **Parrot ANAFI** quadrotor and a **sub-scale ship platform**
- **Key idea (1-liner)**: A “pilot-like” visual feature + robust RL yields stronger ship-landing performance than PID under gusts.
- **Notes**:
  - Strength: rare combo of **vision-only** + **real hardware** in an RL ship-landing setup
  - Limitation: custom visual reference (horizon bar) may not transfer directly to arbitrary USV decks
- **Tags**: `RL`, `vision-only`, `ship-landing`, `gust-robustness`, `real-world`

#### Deep Reinforcement Learning for sim-to-real policy transfer of VTOL-UAVs offshore docking operations
- **Link**: https://www.sciencedirect.com/science/article/pii/S1568494624006173  
- **Authors / Venue / Year**: Ali M. Ali, Aryaman Gupta, Hashim A. Hashim — **Applied Soft Computing**, **Vol. 162**, article **111843**, **2024**, DOI: **10.1016/j.asoc.2024.111843**
- **Setting**: VTOL-UAV landing/docking on an **offshore** platform with randomized wave-like disturbances
- **Sensing / State**: simulation state (paper focus is RL training + sim2real-style robustness, not a new perception stack)
- **Method**: split landing into **approach phase (model-based control)** + **landing phase (DRL)**; tests DQN and PPO; uses **JONSWAP** wave spectra for episode randomization
- **Assumptions**: sim disturbance model captures enough “wave reality” for transfer; approach controller is available
- **Validation**: numerical simulation experiments (sim-to-real is addressed via disturbance randomization methodology)
- **Key idea (1-liner)**: Use wave-spectrum randomization + phased architecture to make docking policies more transferable.
- **Notes**:
  - Strength: very explicit sim2real mindset (randomization via maritime wave spectrum)
  - Limitation: doesn’t provide a full perception pipeline for onboard-only landing
- **Tags**: `RL`, `sim2real`, `offshore-docking`, `PPO`, `JONSWAP`

#### Reinforcement learning based autonomous multi-rotor landing on moving platforms
- **Link**: https://doi.org/10.1007/s10514-024-10162-8  
- **Authors / Venue / Year**: Pascal Goldschmid, Aamir Ahmad — **Autonomous Robots (Springer)**, **Vol. 48(4)**, article **13**, **2024**, DOI: **10.1007/s10514-024-10162-8  
- **Code**: https://github.com/robot-perception-group/rl_multi_rotor_landing
- **Setting**: moving platform landing (not specifically marine), with realistic disturbances and a curriculum-style training pipeline
- **Sensing / State**: ROS/Gazebo training setup; real-world deployment supported (see repo + released datasets)
- **Method**: RL training with a **sequential curriculum**; end-to-end system integration in ROS (sim + deployment tooling)
- **Assumptions**: platform motion is observable in the training setup; primary focus is reproducible training + evaluation
- **Validation**: simulation + real flight experiments (logs/rosbags/videos provided by authors)
- **Key idea (1-liner)**: A practical, reproducible RL landing stack that many “deck landing” projects can reuse as a baseline.
- **Notes**:
  - Strength: arguably one of the most **engineer-friendly** RL landing repos (ROS/Gazebo + deployment workflow)
  - Limitation: not marine by default — you still need to add wave/tilt and maritime sensing failures
- **Tags**: `RL`, `moving-platform`, `baseline`, `ROS`, `Gazebo`, `reproducible`

#### A deep reinforcement learning strategy for UAV autonomous landing on a moving platform
- **Link**: https://doi.org/10.1007/s10846-018-0891-8  
- **Authors / Venue / Year**: Alejandro Rodriguez-Ramos, Carlos Sampedro, Hriday Bavle, Paloma de la Puente, Pascual Campoy — **Journal of Intelligent & Robotic Systems**, **Vol. 93(1–2)**, **pp. 351–366**, **2019**, DOI: **10.1007/s10846-018-0891-8**
- **Setting**: moving platform landing (general case; often cited as a canonical deep-RL landing reference)
- **Sensing / State**: platform-relative state (paper focus is deep RL control for landing)
- **Method**: deep RL approach for autonomous landing on a moving platform
- **Assumptions**: platform motion stays within trained regimes; perception is not the core novelty
- **Validation**: experiments as reported in the paper
- **Key idea (1-liner)**: One of the earlier widely-cited deep-RL formulations of “land on a moving target”.
- **Notes**:
  - Strength: good “classic reference” for framing RL landing as a control problem
  - Limitation: not tailored to wave-induced tilt / open-water disturbances
- **Tags**: `RL`, `moving-platform`, `classic-reference`

#### Reinforcement Learning based Optimal Guidance for Landing the Variable Skew Quad Plane on a Ship
- **Link**: https://www.imavs.org/papers/2025/7.pdf  
- **Authors / Venue / Year**: Cansu Yıkılmaz, Christophe De Wagter — **IMAV 2025 (16th International Micro Air Vehicle Conference and Competition)**, paper **IMAV2025-7**
- **Setting**: landing a **hybrid UAV (Variable Skew Quad Plane)** on a moving ship
- **Sensing / State**: uses simulated dynamics + **real ship motion data** for validation (paper emphasizes guidance robustness)
- **Method**: **PPO** outputs **optimal acceleration inputs** that feed an inner **INDI-based** controller; reward shaped around touchdown velocity, deviation, and duration
- **Assumptions**: inner controller handles attitude/low-level stabilization; RL focuses on guidance/acceleration commands
- **Validation**: simulations with randomized sinusoidal ship motion + validation using real ship data (no sea-trial flight experiments in this paper)
- **Key idea (1-liner)**: Treat RL as an outer-loop **guidance policy** that “waits for the moment” while an inner controller keeps the vehicle stable.
- **Notes**:
  - Strength: clean separation of concerns (RL guidance + strong inner-loop control)
  - Limitation: real ship data ≠ real flight on a ship; perception stack not addressed
- **Tags**: `RL`, `PPO`, `guidance`, `ship-landing`, `hybrid-UAV`

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
- [PX4 SITL + Gazebo (gz)](https://docs.px4.io/main/en/sim_gazebo_gz/) — common research baseline for multirotors/VTOL; supports wind and rich worlds.  
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


