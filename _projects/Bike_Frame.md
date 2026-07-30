---
layout: default
title: Electric Bike-Share Frame — FEA Design & Optimization
description: FEA Analysis Project
technologies: [Ansys Mechanical, Fusion 360]
image: /assets/images/Bike/Thumbnail.png 
show_header_image: false
---





<style>
  .project-hero {
  text-align: center;
  margin: 20px 0 30px 0; /* spacing around image */
}

.project-hero img {
  max-width: 100px;   /* adjust size as needed */
  width: 100%;
  height: auto;
  border-radius: 8px; /* optional for nicer look */
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


# Electric Bike-Share Frame — FEA Design & Optimization

Structural design and finite element analysis of an electric bike frame for a bike-share scheme, built in **Fusion 360** (CAD) and **ANSYS Mechanical** (FEA). University project, university coursework.

<p align="center">
  <img src="assets/09-geometry-final.png" width="480" alt="Final bike frame geometry">
</p>

## Overview

Bike-share schemes replace short car journeys and improve public health, air quality, and access to affordable transport — but removing the frame's top tube to make the bike easier to mount, and adding a battery pack, changes how loads travel through the structure. This project designs a frame that stays lightweight and safe under that constraint.

**Goals:** easy to mount (no top tube), lightweight, safety factor ≥ 2.

**Process:**
1. Analyze a generic bike frame under normal riding loads to understand load paths.
2. Remove the top tube and add the battery mass to see how a bike-share frame's response differs from a standard frame.
3. Redesign the frame geometry to compensate for the missing top tube.
4. Run a parametric optimization on tube wall thickness to minimize mass while keeping the safety factor above 2.

## Method: shell elements, not solid elements

The frame is built from thin-walled tubes, so it's modeled with **shell elements** rather than 3D solid elements. Shell theory assumes plane sections remain plane and displacements stay small (valid here since the frame is expected to stay elastic). This lets each tube be represented as a curved 2D mid-surface rather than a full 3D solid — each node carries 6 degrees of freedom (3 translations, 3 rotations) instead of the finer mesh a solid model would need, which keeps the model far cheaper to solve without sacrificing accuracy.

The solution follows the standard FE energy-minimization route:
1. Express the displacement field from the shell kinematics, then derive strain and (via Hooke's Law) stress in terms of the unknown mid-surface displacement.
2. Build the total potential energy, Π = W_int − W_ext, integrating strain energy through the tube thickness.
3. Discretize with a mesh, assemble element stiffness contributions, and minimize Π with respect to each nodal DOF (∂Π/∂dᵢ = 0), giving the system **[K][d] = [f]**.
4. Solve for unknown DOF using the essential boundary conditions, back out reaction forces, then post-process for stress, strain, and safety factor.

## FEA Setup

| | |
|---|---|
| Material | Aluminium Alloy (E = 71 GPa, ν = 0.33, ρ = 2770 kg/m³) |
| Element size | 4 mm |
| Loads | Rider weight: 700 N · Pedal force: 150 N · Battery weight: 30 N |
| Essential BCs | Zero displacement at the rear dropout and head tube (bike is supported as it would be by the wheels) |
| Target | Minimum safety factor ≥ 2 |

<p align="center">
  <img src="assets/03-boundary-conditions-initial.png" width="480" alt="Boundary conditions and applied loads">
</p>

## Baseline Design (No Top Tube)

Starting point: the generic frame geometry with the top tube removed and the battery load added, at a uniform 2 mm wall thickness.

<p align="center">
  <img src="assets/01-geometry-initial.png" width="380" alt="Baseline geometry"> 
  <img src="assets/02-mesh-initial.png" width="380" alt="Baseline mesh">
</p>

| Result | Value |
|---|---|
| Total mass | 1.16 kg |
| Max total deformation | 0.165 mm |
| Max von-Mises stress | 63.4 MPa |
| **Minimum safety factor** | **4.41** |

<p align="center">
  <img src="assets/04-deformation-initial.png" width="330" alt="Total deformation"> 
  <img src="assets/05-vonmises-initial.png" width="330" alt="Von-Mises stress">
  <img src="assets/06-safety-factor-initial.png" width="330" alt="Safety factor">
</p>

Removing the top tube leaves the frame comfortably above the safety threshold — the critical region shifts to the joint between the seat tube and the down tube, where the loss of the top tube's bracing is felt most.

## Redesigned Geometry

Using the insight from the baseline run, I re-profiled the frame to route load more efficiently around the missing top tube (including relocating the battery), at a uniform 1.5 mm wall thickness to start.

<p align="center">
  <img src="assets/09-geometry-final.png" width="380" alt="Redesigned geometry"> 
</p>

| Result | Value |
|---|---|
| Total mass | 2.47 kg |
| Max total deformation | 0.330 mm |
| Max von-Mises stress | 190.0 MPa |
| Minimum safety factor | 1.32 (below the target of 2) |

<p align="center">
  <img src="assets/11-deformation-final.png" width="330" alt="Total deformation, redesign"> 
  <img src="assets/12-vonmises-final.png" width="330" alt="Von-Mises stress, redesign">
  <img src="assets/13-safety-factor-final.png" width="330" alt="Safety factor, redesign">
</p>

## Parametric Optimization

The redesigned geometry didn't clear the safety factor target at a uniform thickness, so I ran a parametric optimization on the wall thickness of the three tubes most critical to the minimum safety factor, minimizing mass subject to safety factor ≥ 2.

<p align="center">
  <img src="assets/14-optimized-tube-thicknesses.png" width="700" alt="Optimized tube thicknesses">
</p>

| Tube | Optimized thickness | Rounded (manufacturable) |
|---|---|---|
| Tube 1 | 1.267 mm | 1.2 mm |
| Tube 2 | 1.739 mm | 1.8 mm |
| Tube 3 | 2.995 mm | 3.0 mm |

**Result:** final mass **2.60 kg**, minimum safety factor **2.04** — a 55% increase in safety factor for only a 5% increase in mass over the uniform-thickness redesign.

## Verification

Every model was checked three ways before trusting the results:

- **Reaction forces** — reactions at the essential BCs summed to exactly balance the 880 N of applied load in the load direction, for both the baseline and final designs.
- **Boundary conditions** — displacement at all constrained nodes confirmed to be exactly zero.
- **Mesh convergence** — total deformation and von-Mises stress were tracked across four levels of mesh refinement; both converged to within 5% (deformation after 1 refinement, stress after 3–4).

<p align="center">
  <img src="assets/07-convergence-deformation.png" width="330" alt="Deformation convergence"> 
  <img src="assets/08-convergence-vonmises.png" width="330" alt="Von-Mises convergence">
</p>

## Tools

- **Fusion 360** — CAD geometry
- **ANSYS Mechanical** — shell-element FEA, static structural analysis
- **ANSYS parametric optimization** — tube thickness sweep

## Key Takeaways

- Modeling the frame as shell mid-surfaces (rather than solid geometry) was the right call for thin-walled tubes — it kept the mesh coarse and the solve fast without giving up accuracy, but it also meant rethinking the CAD from the start rather than just importing a solid model.
- The first redesign attempt improved some regions of the frame but made others worse — a reminder that structural design is iterative, and a single "improvement" pass rarely gets everything right at once.
- Parametric optimization was the highest-leverage step here: a 55% gain in safety factor for a 5% mass penalty, and it made clear how much value there is in understanding *why* an optimizer converges where it does rather than treating it as a black box.