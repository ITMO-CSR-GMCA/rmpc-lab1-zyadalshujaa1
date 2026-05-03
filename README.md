[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/L0glmnMn)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23589763&assignment_repo_type=AssignmentRepo)


# RMPC-Lab 1: Robot Motion Planning and Control

## 1. Project Overview
This laboratory work focuses on the implementation of the Recursive Newton-Euler Algorithm (RNEA) for a **6-DOF** manipulator. The goal is to analyze the inverse dynamics of the robot under different kinematic scenarios (Dynamic, Quasi-static, and Static).

Selected Robot: **Universal Robots UR10 (6-DOF)**

## 2. Manipulator Model and Parameters

### Denavit-Hartenberg (DH) Parameters
The robot model is loaded using the **Standard DH** convention. 
- Type: **6 Revolute joints**.
- Link Lengths: Defined according to the official manufacturer specifications.

### Dynamic and Drive Parameters
The following physical properties were assigned to each link to ensure a realistic simulation:
*   Masses ($m$): Identified link masses ranging from **8.39kg to 0.19kg**.
*   Centers of Mass ($r$): 3D vectors relative to each link frame.
*   Inertia Tensors ($I$): $3 \times 3$ symmetric matrices representing mass distribution.
*   Drive Inertia ($J_m$): $1 \times 10^{-5} \text{ kg}\cdot\text{m}^2$.
*   Friction: Viscous ($B=0.0015$) and Coulomb ($T_c=[0.2, -0.2]$).
*   Gear Ratios ($G$): $1:100$ for all joints (typical approximation).
*   Constraints ($q_{lim}$): Generalized coordinate limits set to $\pm 2\pi$ rad.

## 3. Trajectory Planning
A smooth trajectory was planned between two arbitrary configurations:
*   $q_{start}$: [0, 0, 0, 0, 0, 0] (Zero Pose)
*   $q_{end}$: [pi/4, -pi/3, -pi/4, pi/3, -pi/3, pi/4]

The trajectory was generated using a 5th-order polynomial (quintic) over a 5-second interval ($N=100$ points), ensuring continuous velocity and acceleration profiles.

## 4. Inverse Dynamics Scenarios
The inverse dynamics problem was solved for three specific cases:
1.  Full Dynamics ($\dot{q} \neq 0, \ddot{q} \neq 0$): Total torque required for motion.
2.  Quasi-statics ($\dot{q} \neq 0, \ddot{q} \approx 0$): Torques required ignoring inertial acceleration.
3.  Static ($\dot{q} = 0, \ddot{q} = 0$): Pure gravity compensation ($G(q)$).

Numerical values for the Inertia Matrix $M(q)$, Coriolis Matrix $C(q, \dot{q})$, and Gravity Vector $G(q)$ were calculated at each time step.

## 5. Results and Visualization
Joint torque graphs were plotted for all **6 joints**. These plots illustrate the contribution of various forces (Inertia, Friction, Gravity) over the course of the trajectory.

*(Note: Refer to the .ipynb file for the high-resolution torque plots.)*

## 6. Conclusions

### Dominance of Gravity
The static scenario ($\tau_{static}$) reveals that gravity is the primary source of joint torque. Joints supporting the largest mass segments (**Joint 2 and Joint 3**) show significant static torques.

### Influence of Inertia
The difference between Full Dynamics and Quasi-Static scenarios represents the torque required to overcome link inertia. At moderate speeds, gravitational forces significantly outweigh inertial forces.

### Impact of Friction
The proximity of the Full Dynamics and Quasi-Static curves suggests that Coriolis forces and friction are secondary to the weight of the links for the selected motion profile.

### Final Summary
The experiment confirms that for the **Universal Robots UR10**, gravity compensation is the most critical component of the control law, though full dynamics are required for high-speed tracking accuracy.
