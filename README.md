# 🦾 Humanoid Motion Learning

A video-driven humanoid motion-learning pipeline for **human motion recovery, motion retargeting, reinforcement-learning-based tracking, cross-simulation validation, and real-robot deployment preparation** using the Unitree G1 humanoid robot.

This project was developed as part of a broader research effort on reinforcement-learning platforms for humanoid robots operating in confined-space welding environments.

> 📌 **Project status:** The motion-learning and deployment pipeline has been established and validated in simulation.  
> Real-robot deployment is currently at the preliminary integration and safety-validation stage.

---

## 🔍 Overview

Learning whole-body motion for humanoid robots requires several stages beyond simply training a policy.

Human motion obtained from ordinary videos must first be reconstructed, converted to a robot-compatible representation, retargeted to the humanoid morphology, and organized into trajectories suitable for reinforcement-learning-based tracking.

This project develops a staged pipeline:

```text
Monocular Video
      ↓
Human Motion Recovery
      ↓
Motion Retargeting
      ↓
Motion Data Conversion
      ↓
Tracking Policy Training
      ↓
Isaac Lab Validation
      ↓
Cross-Simulation Validation
      ↓
Motion Refinement
      ↓
Real-Robot Deployment Preparation
```

The pipeline is designed as an intermediate step between human demonstration data and future task-specific humanoid manipulation or welding policies.

---

## 🧠 System Pipeline

The project combines several tools and simulation environments into one motion-learning workflow.

### 1. Video-Driven Human Motion Recovery

Ordinary monocular video is used as the input source.

Human whole-body motion is reconstructed using **GVHMR**, converting natural video into structured motion sequences that can be used for downstream processing.

This enables motion demonstrations to be collected without requiring a dedicated motion-capture system.

---

### 2. Motion Retargeting

Recovered human motion cannot be directly executed by a humanoid robot because the human body and robot have different:

- joint structures
- link lengths
- coordinate conventions
- motion limits
- kinematic constraints

The project therefore uses **GMR** to retarget reconstructed human motion to the target humanoid robot.

The resulting motion sequences are transformed into robot-compatible references for subsequent tracking-policy training.

---

### 3. Motion Data Conversion

Motion data produced by the recovery and retargeting stages must be reorganized before reinforcement-learning training.

The conversion pipeline includes:

- motion-sequence organization
- coordinate transformation
- robot-specific format conversion
- trajectory preprocessing
- training-input preparation

This stage connects reconstructed human demonstrations with the motion representation required by the target humanoid model.

---

## 🤖 G1 Tracking Policy Training

The retargeted motion sequences are used to train a whole-body tracking policy for the **Unitree G1 humanoid robot**.

Training is performed using **Isaac Lab** and the `whole_body_tracking` workflow.

The policy learns to reproduce reference motions while maintaining stable whole-body behavior.

The training process focuses on:

- body-position tracking
- joint-position tracking
- pose consistency
- motion continuity
- episode stability
- whole-body coordination

The resulting policy provides a foundation for later task-specific humanoid control.

---

## 🎥 Demo

A motion-tracking demonstration will be added here.

<!--
<p align="center">
  <img src="assets/g1_tracking_demo.gif" width="750">
</p>

<p align="center">
  <i>Unitree G1 whole-body motion tracking after video-driven motion recovery, retargeting, and policy training.</i>
</p>

▶️ [Watch the full demonstration](assets/g1_tracking_demo.mp4)
-->

---

## 🔄 Cross-Simulation Validation

After policy training, the learned tracking policy is transferred to **RoboJuDo** for cross-simulation validation.

The purpose of this stage is to examine whether the learned motion behavior is tied entirely to the original simulation environment or can remain executable after being transferred to another simulator.

The cross-simulation workflow is:

```text
Motion Reference
      ↓
Isaac Lab Policy Training
      ↓
Trained Policy
      ↓
RoboJuDo Deployment
      ↓
Cross-Simulation Motion Validation
```

This stage acts as an intermediate validation step between simulation training and eventual deployment on physical hardware.

---

## 🎥 Cross-Simulation Demo

A RoboJuDo cross-simulation demonstration will be added here.

<!--
<p align="center">
  <img src="assets/g1_robojudo_demo.gif" width="750">
</p>

<p align="center">
  <i>Cross-simulation execution of the trained G1 tracking policy in RoboJuDo.</i>
</p>
-->

---

## 🛠 Motion Refinement

Automatically recovered and retargeted motion may contain:

- unnatural local poses
- motion discontinuities
- joint-pose deviations
- artifacts introduced during motion reconstruction

To address these issues, the project also investigates **Blender-based motion refinement**.

The workflow supports:

```text
Motion Sequence
      ↓
Blender Import
      ↓
Pose / Motion Editing
      ↓
Motion Re-export
      ↓
Retraining
```

This provides a practical way to manually correct problematic demonstrations before they are reused for policy training.

---

## 🦿 Real-Robot Deployment Preparation

The project also progressed from simulation toward preliminary deployment on a physical Unitree platform.

The deployment workflow includes:

- establishing remote communication with the robot
- transferring the trained policy model
- configuring robot-side execution files
- preparing the policy for real-device inference
- testing the deployment workflow under controlled conditions

The trained policy was exported for deployment and the communication and execution pipeline with the physical platform was established.

At the current stage, this work should be considered **deployment preparation rather than completed real-robot policy validation**.

Motion stability, configuration consistency, and execution safety require further validation before full autonomous execution.

---

## 🔗 Connection to Confined-Space Welding

This humanoid motion pipeline was developed as a foundation for future humanoid welding research.

Confined-space welding may require coordination between:

- whole-body posture
- upper-limb motion
- welding-tool pose
- obstacle avoidance
- workspace constraints
- task stability

Instead of immediately training a complete welding policy, this project first establishes the general motion-learning and deployment infrastructure required for high-degree-of-freedom humanoid control.

Future task-specific policies can extend this framework by incorporating welding-related states, actions, rewards, and safety constraints.

---

## 🧩 Project Architecture

```text
                ┌───────────────────────┐
                │    Monocular Video    │
                └───────────┬───────────┘
                            ↓
                ┌───────────────────────┐
                │        GVHMR          │
                │ Human Motion Recovery │
                └───────────┬───────────┘
                            ↓
                ┌───────────────────────┐
                │         GMR           │
                │  Motion Retargeting   │
                └───────────┬───────────┘
                            ↓
                ┌───────────────────────┐
                │   Data Conversion     │
                │  & Preprocessing      │
                └───────────┬───────────┘
                            ↓
                ┌───────────────────────┐
                │      Isaac Lab        │
                │ Tracking Policy Train │
                └───────────┬───────────┘
                            ↓
          ┌─────────────────┴─────────────────┐
          ↓                                   ↓
┌───────────────────────┐          ┌───────────────────────┐
│       RoboJuDo        │          │        Blender        │
│ Cross-Sim Validation  │          │  Motion Refinement    │
└───────────┬───────────┘          └───────────┬───────────┘
            │                                  │
            └─────────────────┬────────────────┘
                              ↓
                  ┌───────────────────────┐
                  │ Unitree G1 Deployment │
                  │      Preparation      │
                  └───────────────────────┘
```

---

## 🧰 Technologies

### Humanoid Robotics

- Unitree G1
- whole-body motion tracking
- motion retargeting
- humanoid policy learning

### Reinforcement Learning & Simulation

- NVIDIA Isaac Lab
- RoboJuDo
- reinforcement-learning-based motion tracking
- sim-to-sim policy validation

### Human Motion Processing

- GVHMR
- GMR
- monocular motion recovery
- motion-sequence conversion

### Motion Editing & Deployment

- Blender
- ONNX
- Linux
- SSH-based robot deployment workflow

### Programming

- Python

---

## 👩‍💻 Project Role

This work was completed as part of a team Keystone project on reinforcement-learning platforms for confined-space humanoid welding robots.

I served as the **group leader** and participated in the development, integration, testing, and organization of the staged technical pipeline.

The project was completed collaboratively with contributions from team members across simulation deployment, motion processing, policy training, data processing, and experimental validation.

---

## 🚧 Current Scope

This repository focuses specifically on the **humanoid motion-learning pipeline**.

Completed stages include:

- video-based human motion recovery
- humanoid motion retargeting
- motion-data preprocessing
- G1 tracking-policy training
- simulation-based policy validation
- cross-simulation verification
- motion-refinement workflow
- preliminary real-device deployment preparation

The following are considered future work:

- task-specific welding reinforcement learning
- full humanoid welding policy design
- systematic sim-to-real policy transfer
- stable real-robot motion validation
- autonomous welding execution
- safety-aware task-level policy evaluation

---

## 📁 Repository Structure

The repository will gradually include selected project materials.

```text
humanoid-motion-learning/
│
├── assets/
│   ├── images/
│   ├── gifs/
│   └── videos/
│
├── motion/
│   └── selected motion-processing examples
│
├── scripts/
│   └── selected utility scripts
│
├── README.md
│
└── .gitignore
```

Some project components depend on external research repositories and simulation frameworks and are therefore not redistributed directly here.

---

## 📄 Project

**Reinforcement Learning Platform for Confined-Space Humanoid Welding Robots**

Keystone Project  
Fan Gongxiu Honors College  
Beijing University of Technology

---

## 🔗 Related Projects

This humanoid motion-learning pipeline is part of a broader robotics research workflow.

- 🤖 **Task-Registered Robotic Welding Framework** — sim-to-real perception, RGB-D geometry recovery, weld-path generation, and robot interfaces
- 🎮 **Tetris Closed-Loop Control** — foundational perception–decision–execution verification platform

Links will be added after the corresponding repositories are completed.

---

## 📬 Contact

**Weiran Wang**  
Beijing University of Technology  
Mechanical Engineering  
Second Bachelor's Degree in Computer Science and Technology
