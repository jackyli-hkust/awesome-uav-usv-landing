# UAV-USV-Landing
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated, research-oriented reading list for **unmanned aerial vehicle (UAV) landing on unmanned surface vehicle (USV)** in waves, wind, and open-water conditions — with a focus on **perception + estimation + control + learning**.

> Goal: help researchers quickly map the space, compare assumptions, and find practical baselines, code, and open problems.

---

## Contents
- [Scope](#scope)
- [How to Use This List](#how-to-use-this-list)
- [Core Papers](#core-papers)
  - [Distributed / Decentralized MPC for UAV–USV Landing](#distributed--decentralized-mpc-for-uavusv-landing)
  - [MPC for Harsh Winds & Open Waters](#mpc-for-harsh-winds--open-waters)
- [Perception & State Estimation](#perception--state-estimation)
- [Learning-Enhanced Control](#learning-enhanced-control)
- [Simulation, Benchmarks, and Evaluation](#simulation-benchmarks-and-evaluation)
- [Open Problems](#open-problems)
- [Contributing](#contributing)
- [Citation](#citation)

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

## Perception & State Estimation
> Add papers on relative pose estimation, deck tracking, onboard-only sensing, latency & dropout robustness.

- (paper) **Title** — link  
  - **What it estimates**: (relative pose / deck tilt / wave phase / etc.)
  - **Sensors**: (AprilTag / VIO / RTK / IMU / radar / etc.)
  - **Why it matters for landing**: (1 line)

---

## Learning-Enhanced Control
> Residual learning, GP uncertainty, robust learning MPC, safety filters / CBFs for hard constraints.

- (paper) **Title** — link  
  - **Base controller**: (MPC / PID / geometric control / etc.)
  - **Learning component**: (residual NN / GP / RL policy / etc.)
  - **Safety mechanism**: (CBF / safety filter / tube MPC / etc.)

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
- Wave + vessel motion generators: (fill)
- UAV dynamics / wind disturbance models: (fill)
- Vision + AprilTag / marker simulation: (fill)
- (Optional) repo links for reproducible baselines

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


