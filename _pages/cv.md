---
layout: default
title: Charlie Pearson - CV
permalink: /cv/
---

<div style="display:flex; justify-content:space-between;">
  <a href="mailto:C.Pearson-9@sms.ed.ac.uk">C.Pearson-9@sms.ed.ac.uk</a>
  <span>07492505324</span>
</div>

---

## Education

**University of Edinburgh** – MEng Mechanical Engineering (2023 – 2028)
- First year class medal, awarded to best performing student in the cohort
- Year-long exchange at Cornell University (3rd year) with 3.96 GPA
- Predicted 1st class honours
- Relevant Modules: Solid Mechanics, Fluid Mechanics, Engineering Design, Material Processing, Thermodynamics, System Dynamics, Numerical Methods

**The Sixth Form College Farnborough** (2020 – 2022)
- A* in Mathematics, Physics and Economics
- President of the Engineering Society

---

## Projects

**Electric Bike-Share Frame – FEA Design & Optimisation**
- Designed and analysed a bike-share frame in Fusion 360 and ANSYS Mechanical, removing the top tube for easier mounting while targeting a safety factor ≥ 2
- Modelled the frame using shell elements to capture thin-walled tube behaviour, keeping the mesh coarse and the solution computationally efficient without sacrificing accuracy
- Ran a parametric thickness optimisation on the three most critical tubes, raising minimum safety factor by 55% for just a 5% mass increase over the original uniform-thickness design
- Verified results via reaction force balance, boundary condition checks, and mesh convergence studies, with deformation and stress converging within 5%

**CFD Analysis of Ahmed Car Body Flow**
- Simulated 3D turbulent flow over an Ahmed car body in ANSYS Fluent, solving the RANS equations with the k-ω GEKO turbulence model to predict drag and lift coefficients
- Built a half-domain mesh using a symmetry boundary condition and boundary-layer inflation, reaching residual convergence below 10⁻³ within 58 iterations
- Validated C<sub>D</sub> and C<sub>L</sub> against published experimental data, landing within 12% of literature values while correctly reproducing separation and wake behaviour
- Diagnosed the source of discrepancy through a ranked analysis of modelling simplifications (domain size, turbulence closure, symmetry assumption), identifying domain size as the dominant contributor

**Numerical Simulation of a Two-Story Building's Seismic Response**
- Modelled a two-story shear building under seismic loading as a coupled nonlinear ODE system, fitting nonlinear spring and damper force models from data via least-squares regression
- Implemented a 4th order Runge-Kutta integrator from scratch in Python, confirming timestep independence and validating the solver against a known analytical solution
- Benchmarked against a hand-written Forward Euler integrator, showing RK4 held stable at a 24x larger timestep for equivalent accuracy, while running 5.4x faster overall
- Analysed the building's dynamic response across two earthquake intensities, characterising how forcing amplitude affects decay behaviour and peak displacement

---

## Experience

**BUiD Sustainability Spring School** (2025)
- Collaborated with engineers, architects, and designers to develop a sustainable lighting solution, integrating mechanical design and energy efficiency principles
- Co-presented the final design to a panel of engineers and academics, achieving 1st place
- Communicated technical concepts within an international team, supporting members for whom English was not a first language

**CFH Tutoring, Co-Founder and Tutor** (2020 – 2025)
- Co-founded a tutoring company educating and mentoring children from ages 8–18
- Grew the business to a revenue of over £15,000 and employed up to 8 tutors
- Developed a structured curriculum to ensure quality and consistency, sustaining a 100% pass rate over a 5-year period

**Costa Coffee, Barista** (2022 – Present)
- Led shifts of 4–6 staff in a fast-paced environment fostering teamwork and efficiency
- Analysed sales data to forecast demand and implement stock management strategies, reducing monthly waste by 15%

---

## Technical Skills

**Software**
- ANSYS Fluent, ANSYS Mechanical, Fusion 360, Granta EduPack

**Programming & Numerical Methods**
- Python (NumPy, Matplotlib), MATLAB, nonlinear ODE modelling, least-squares regression

**Manufacturing & Testing**
- Mill and lathe operation, tensile testing, ASTM standards

**Engineering Analysis**
- FEA, CFD, mesh convergence, simulation validation

---

[Download my CV](/assets/CV.pdf) in pdf format.