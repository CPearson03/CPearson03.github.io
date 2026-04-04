---
layout: project
title: Torque Wrench Design Project
description: Anysy Design Project
technologies: [Autodesk, Fusion, MATLAB]
image: assets/images/Torque_Wrench/Torque Cover Image.png
show_header_image: true
---





<style>
  .project-hero {
  text-align: center;
  margin: 20px 0 30px 0; /* spacing around image */
}

.project-hero img {
  max-width: 350px;   /* adjust size as needed */
  width: 100%;
  height: auto;
  border-radius: 8px; /* optional for nicer look */
}

  .img-row {
    display: flex;
    gap: 15px;
    flex-wrap: wrap;
    justify-content: center;
  }
  .img-row img {
    max-height: 180px;
    width: auto;
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
</style>
## Introduction

In this project I designed a torque wrench that maximized strain-gauge output while also meeting safety requirements for yielding, fracture, and fatigue. I first performed analytical hand calculations using beam theory, selecting geometry and material to satisfy the design requirements. I then modeled the design in Fusion 360 and imported it into ANSYS to simulate stress and strain under the maximum applied torque.

---

## CAD Model

<div class="img-row">
  <img src="/fa25-portfolio-CPearson03/assets/images/Torque_Wrench/CAD-frontview.png" alt="Front view">
  <img src="/fa25-portfolio-CPearson03/assets/images/Torque_Wrench/CAD-isometric.png" alt="Isometric view">
  <img src="/fa25-portfolio-CPearson03/assets/images/Torque_Wrench/CAD-sideview.png" alt="Side view">
</div>

---

## Material Selection

I selected **Aluminum 7075-T6** for its high strength-to-modulus ratio and fatigue performance.  
The relevant mechanical properties are:

- **Young’s modulus (E):** 10.5 × 10⁶ psi  
- **Poisson’s ratio:** 0.33  
- **Ultimate tensile strength:** 70 ksi  
- **Fracture toughness (Kᵢc):** 24.2 ksi·√in  
- **Fatigue strength:** 27 ksi  

---

## Loads and Boundary Conditions

<figure>
  <div class="img-row">
    <img src="/fa25-portfolio-CPearson03/assets/images/Torque_Wrench/Applying-load.png" alt="Applying load" style="width:45%;">
    <img src="/fa25-portfolio-CPearson03/assets/images/Torque_Wrench/Applying-BC.png" alt="Applying boundary condition" style="width:45%;">
  </div>
  <figcaption>
    Applied end torque and clamped boundary condition at the drive.
  </figcaption>
</figure>

---

## Plots from ANSYS
<figure>
  <img src="/fa25-portfolio-CPearson03/assets/images/Torque_Wrench/Normal Strain Contors.png"
       alt="Normal Strain Contours"
       style="width: 80%;">
  <figcaption>Normal strain (in gauge direction ) contour plot</figcaption>
</figure>

<figure>
  <img src="/fa25-portfolio-CPearson03/assets/images/Torque_Wrench/Maximum Priniciple Stress.png"
       alt="Maximum Principal Stress"
       style="width: 80%;">
  <figcaption>Maximum principal stress contor plot</figcaption>
</figure>



---

## Summary of FEM Results

- **Maximum normal stress:** 59.6 ksi  
- **Tip deflection:** 0.52 in  
- **Strain at gauge location:** 1.46 × 10<sup>−3</sup>  
- **Torque wrench sensitivity:** 1.46 × 10 mV/V  

---

## Strain Gauge Selection

I selected a **half-bridge strain gauge** with dimensions:  
- **Length:** 0.25 in  
- **Width:** 0.125 in  

This size fits comfortably on the side of the wrench and produces a sensitivity which exceeds the design requirement.

---

## Conclusion

This project demonstrated a complete workflow from analytical design to CAD modeling to finite element analysis. The final torque wrench design met all safety factor requirements while maximizing strain-gauge output for improved measurement sensitivity.


