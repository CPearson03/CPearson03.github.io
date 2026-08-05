---
layout: default
title: Modeling Buildings Seismic Response
description: Numerical Methods Project
technologies: [Python, NumPy, Matplotlib]
image: /assets/images/Seismic_Response/Thumbnail.png
show_header_image: false
---

<style>
  .project-hero {
  text-align: center;
  margin: 20px 0 30px 0;
}

.project-hero img {
  max-width: 100px;
  width: 100%;
  height: auto;
  border-radius: 8px;
}

table {
  border-collapse: collapse;
  width: 100%;
  margin: 15px 0;
}
th, td {
  padding: 8px 20px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.project-hero img {
  max-width: 350px;
  width: 100%;
  height: auto;
  border-radius: 8px;
}

figure {
  text-align: center;
  margin: 25px 0;
}
figcaption {
  font-size: 0.9rem;
  color: #555;
  margin-top: 5px;
}
figure img {
  max-width: 100%;
  height: auto;
  display: block;
  margin: 0 auto;
}
.img-row {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
  align-items: flex-start;
  margin: 25px 0;
}
.img-row figure {
  margin: 0;
  flex: 0 0 auto;
}
.img-row img {
  width: 100%;
  height: auto;
  display: block;
  border-radius: 8px;
  }
</style>


## Numerical Simulation of a Two-Story Building's Seismic Response

Nonlinear dynamic simulation of a two-story shear building under seismic loading, solving a coupled system of ODEs with a hand-written 4th order Runge-Kutta integrator, built in **Python**.

<img src="/assets/images/Seismic_Response/Thumbnail.png" alt="Thumbnail image" class="inline-image-r" style="max-width: 220px;">

**[View the code on GitHub →](https://github.com/cpearson03/seismic-response-two-story-building)**


### Contents

- [Overview](#overview)
- [Building Mathematical Model](#building-mathematical-model)
- [Results](#results)
- [Verification](#verification)
- [Discussion](#discussion)


### Overview

<img src="/assets/images/Seismic_Response/SMD.png" alt="Thumbnail image" class="inline-image-r" style="max-width: 240px;">

A two-story building can be modelled as two lumped floor masses connected by a nonlinear spring and damper, sitting on a linear-elastic foundation — a standard simplification for studying the structural response to forcing caused by an earthquake.

 The goal of this project was to build the full numerical pipeline needed to simulate that response from first principles:
- Fit the nonlinear force models from data
- Derive the governing equations as a system of first-order ODEs
- Implement a 4th order Runge-Kutta solver without relying on built-in ODE solvers
- Use the solver to study how the building responds to earthquakes of different intensities
- Verify results against both a known analytical solution and against a simpler Forward Euler method


### Building Mathematical Model

**Assumptions**
-  Structural stiffness of girders is assumed to be infinite
-  No active controller is used to enhance the efficiency of any
internal damping mechanism
- Any vertical motion of the building is neglected


**Governing equations**

The building is modelled as two floors of mass $m_1$ and $m_2$, connected by a nonlinear spring and damper carrying forces $F_{sp}$ and $F_d$, with the first floor also connected to the foundation via linear stiffness $k_f$ and damping $c_f$ :

$$m_2\ddot{x}_2 + F_d + F_{sp} = -m_2\ddot{x}_g$$

$$m_1\ddot{x}_1 - F_d - F_{sp} = -m_1\ddot{x}_g - c_f\dot{x}_1 - k_fx_1$$

where $x_1, x_2$ are the floor displacements and $\ddot{x}_g$ is the earthquake ground acceleration, modelled as a sine wave of amplitude $A$, active for one period $T$. In this project, I used the following parameters to simulate responses for amplitudes of $A=4.4m/s^2$ and $A=16m/s^2$.


| Parameter | Value |
|---|---|
| $m_1$ | 533.5 Kg |
| $m_2$ | 552.5 Kg |
| $k_f$ | 456000 N/m |
| $c_f$ | 68.7 N sec/m |
| $T$ | 2.5 s |


**Nonlinear spring & damper fit**

$F_{sp}$ and $F_d$ are nonlinear functions of the relative displacement ($\Delta x$) and velocity ($\Delta \dot{x}$) between the two stories:

$$F_{sp} = k_1\Delta x + k_2\Delta x^2 + k_3 \Delta x^3, \qquad F_d = c_1\Delta \dot{x} + c_2\Delta \dot{x}^2$$

The coefficients were found by fitting supplied force-displacement and force-velocity datasets, using a polynomial least-squares regression implemented directly from the normal equations (design matrix, Gram matrix, `numpy.linalg.solve`) rather than a built-in curve-fitting function.

<div class="img-row">
  <figure style="width: 450px;">
    <img src="/assets/images/Seismic_Response/spring_force_fit.png" alt="Spring force least-squares fit">
    <figcaption>Cubic least-squares fit of spring force vs. relative displacement.</figcaption>
  </figure>
  <figure style="width: 450px;">
    <img src="/assets/images/Seismic_Response/damping_force_fit.png" alt="Damping force least-squares fit">
    <figcaption>Quadratic least-squares fit of damping force vs. relative velocity.</figcaption>
  </figure>
</div>

| Coefficient | Value |
|---|---|
| $k_1$ | 1.10 × 10⁵ |
| $k_2$ | −6.31 × 10⁶ |
| $k_3$ | 3.36 × 10⁹ |
| $c_1$ | 9.09 × 10¹ |
| $c_2$ | 2.54 |

**Implementing the RK4 solver**

The two coupled 2nd order ODEs were recast as four coupled 1st order ODEs using the state vector $\mathbf{y} = [x_1, \dot{x}_1, x_2, \dot{x}_2]$, which is the form required by a Runge-Kutta solver.

I then implemented a fixed-step 4th order Runge-Kutta integrator directly from its defining stages (k₁–k₄ RHS evaluations per step) rather than using a library solver such as `scipy.integrate.solve_ivp`.

### Results

**Timestep independence**

Before analysing results, I conducted a timestep independense test to reduce truncation error and ensure the solver was stable. For $A=16m/s^2$, the RK4 timestep was halved successively (from h = T/200 down to h = T/800) until the x₁ and x₂ response curves became visually indistinguishable between refinements. This resulted in a converged step size of h = T/400 (6.25 ms) which I used for all following runs of the simulation.

<figure style="max-width: 700px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Seismic_Response/timestep_independence_A16.0.png" alt="Timestep independence test">
  <figcaption>x₁ and x₂ response converge as the RK4 timestep is halved; h = T/400 was selected as timestep-independent.</figcaption>
</figure>


**Running the simulation**

The converged RK4 solver was run for both earthquake forcing amplitudes, A = 4.4 m/s² and A = 16 m/s², over a 10 s simulation window.

<figure style="max-width: 700px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Seismic_Response/dynamic_response_A4.4.png" alt="Dynamic response at A=4.4">
  <figcaption>Displacement and velocity response of both floors, A = 4.4 m/s².</figcaption>
</figure>

<figure style="max-width: 700px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Seismic_Response/dynamic_response_A16.0.png" alt="Dynamic response at A=16">
  <figcaption>Displacement and velocity response of both floors, A = 16 m/s² — larger amplitude and slower decay than the weaker forcing case.</figcaption>
</figure>


### Verification

**Comparing against known analytical solution**

To verify the solver, I tested it against the known analytical solution of $\dot{y} = y$ ($y = e^t$), alongside a hand-written Forward Euler integrator, to confirm both were implemented correctly.

<figure style="max-width: 550px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Seismic_Response/solver_verification.png" alt="Solver verification against analytical solution">
  <figcaption>Both integrators reproduce the exact solution of a known ODE, confirming correct implementation.</figcaption>
</figure>




**Forward Euler vs. RK4**

I then wanted to test my RK4 solver against the simpler Forward Euler method to analyse the difference in accuracy and computational cost between the two methods. I first ran the Forward Euler solver with the same time step as used in the RK4 solver ($h=T/400=6.25ms$). With this time step, the solution blew up due to its instability.

I then decreased the time step by a factor of 16 to get $h=T/6400=0.391ms$. At this timestep, I compared the solution with the RK4 solution at its origional timestep of $h=6.25ms$, getting the following plots.

<figure style="max-width: 700px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Seismic_Response/euler_vs_rk4_comparison.png" alt="Forward Euler vs RK4 comparison">
  <figcaption>Forward Euler (h = T/6400) vs. RK4 (h = T/400). Forward Euler is numerically unstable at RK4's timestep and needs a far smaller step just to remain stable.</figcaption>
</figure>

| Quantity | Value |
|---|---|
| Absolute difference at t = 2T (x₁, v₁, x₂, v₂) | 0.00066 m, 0.02168 m/s, 0.00020 m, 0.02720 m/s |
| Forward Euler step size for error < 10⁻² | 0.264 ms (37,822 total steps) |
| Forward Euler runtime at matched accuracy | 0.166 s |
| RK4 runtime | 0.031 s |


### Discussion

**Accuracy and stability.** Forward Euler is only 1st-order accurate and becomes numerically unstable for this stiff, nonlinear system at anything close to RK4's timestep — it had to be run at h = T/6400 just to produce a stable result at all, and needed an even smaller step (0.264 ms) to match RK4's accuracy to within 10⁻².

**Computational cost.** Despite RK4 requiring 4 right-hand-side evaluations per step versus Forward Euler's 1, RK4 was still roughly 5× faster overall (0.031 s vs. 0.166 s) at matched accuracy. This is because RK4's 4th-order global accuracy means it needs far fewer total steps to converge than a 1st-order method — the number of steps saved outweighs the extra per-step cost by a wide margin, which is the central practical argument for using higher-order integrators on stiff nonlinear systems like this one.

**Response to forcing amplitude.** Increasing the forcing amplitude from A = 4.4 to 16 m/s² produced proportionally larger displacement and velocity amplitudes and a markedly slower decay after the earthquake forcing ends at t = T, consistent with the nonlinear spring/damper engaging more strongly at larger relative displacements.


### Tools Used

Python, NumPy (linear algebra for the least-squares fit), Matplotlib
