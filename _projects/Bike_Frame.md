---
layout: default
title: Electric Bike-Share Frame — FEA Design & Optimisation
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
  .results-grid {
  display: grid;
  grid-template-columns: minmax(0, 1.3fr) minmax(0, 1fr);
  column-gap: 20px;
  row-gap: 30px;
  align-items: start;
  margin: 25px 0;
}
.results-grid figure {
  margin: 0;
}
.results-grid img {
  width: 100%;
  height: auto;
  display: block;
  border-radius: 8px;
}
.results-grid .detail-img {
  height: 180px;
  object-fit: contain;
}
.results-grid .main-img {
  max-width: 88%;
  margin: 0 auto;
}
</style>


## Electric Bike-Share Frame — FEA Design & Optimisation

Structural design and finite element analysis of an electric bike frame for a bike-share scheme, built in **Fusion 360** (CAD) and **ANSYS Mechanical** (FEA).


### Contents

<img src="/assets/images/Bike/Thumbnail_2.png" alt="Thumbnail image" class="inline-image-r" style="max-width: 350px;">

- [Overview](#overview)
- [Methodology](#methodology)
- [Baseline Design](#baseline-design)
- [Redesigned Geometry](#redesigned-geometry)
- [Parametric optimisation](#parametric-optimisation)
- [Verification](#verification)
- [Takeaways](#takeaways)
- [Limitations](#limitations)



### Overview

Bike-share schemes replace short car journeys and improve public health, air quality, and access to affordable transport — but removing the frame's top tube to make the bike easier to mount, and adding a battery pack, changes how loads travel through the structure. This project designs a frame that stays lightweight and safe under that constraint.

**Goals:** easy to mount, lightweight, safety factor ≥ 2.

**Process:**
1. Analyse a generic bike frame with the top tube removed to understand load paths and critical regions within the frame.
2. Redesign the frame geometry to make it easier to mount and compensate for the missing top tube.
3. Run a parametric optimisation on thickness of tube walls to minimise mass while keeping the safety factor above 2.

### Methodology

The frame is built from thin-walled tubes, so it's modeled with **shell elements** rather than 3D solid elements. Shell theory assumes plane sections remain plane and displacements stay small - valid here since the frame is expected to stay within its elastic regime. This lets each tube be represented as a curved 2D mid-surface rather than a full 3D solid — each node carries 6 degrees of freedom (3 translations, 3 rotations) instead of the finer mesh a solid model would need, which keeps the model far cheaper to solve without sacrificing accuracy.

The solution follows the standard FE energy-minimisation route:
1. Express the displacement field from the shell kinematics, then derive strain and (via Hooke's Law) stress in terms of the unknown mid-surface displacement.
2. Build the total potential energy, Π = W_int − W_ext, integrating strain energy through the tube thickness.
3. Discretise with a mesh, assemble element stiffness contributions, and minimise Π with respect to each nodal DOF (∂Π/∂dᵢ = 0), giving the system **[K][d] = [f]**.
4. Solve for unknown DOF using the essential boundary conditions, back out reaction forces, then post-process for stress, strain, and safety factor.


### Baseline Design 

**Geometry**
<figure style="max-width: 400px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Bike/V1_geom.png" alt="Baseline geometry">
  <figcaption>Generic bike frame with the top tube removed, modelled as a 2D mid-surface.</figcaption>
</figure>


**FEA Setup**

| | |
|---|---|
| Material | Aluminium Alloy (E = 71 GPa, ν = 0.33, ρ = 2770 kg/m³) |
| Tube&nbsp;thickness | 2mm (all tubes) | 
| Element size | 4 mm |
| Loads | Rider weight: 700 N · Pedal force: 150 N · Battery weight: 30 N |
| Essential BCs | Zero displacement at the rear dropout and head tube (bike is supported as it would be by the wheels) |
| Target | Minimum safety factor ≥ 2 |



<div class="img-row">
  <figure style="max-width: 350px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Bike/V1_BCs.png" alt="Boundary conditions">
  <figcaption>Boundary conditions applied to bike frame.</figcaption>
</figure>
  <figure style="width: 350px;">
    <img src="/assets/images/Bike/V1_mesh.png" alt="Bike frame mesh">
    <figcaption>Generated mesh with shell elements.</figcaption>
  </figure>
</div>


**Results**

<div class="results-grid">
  <figure>
    <img class="main-img" src="/assets/images/Bike/V1_def.png" alt="Total deformation of frame">
    <figcaption>Total deformation of frame.</figcaption>
  </figure>
  <figure>
    <img class="detail-img" src="/assets/images/Bike/V1_def_max.png" alt="Maximum deformation">
    <figcaption>Maximum deformation occured at the top of the seat tube.</figcaption>
  </figure>

  <figure>
    <img class="main-img" src="/assets/images/Bike/V1_VM.png" alt="von-Mises stress">
    <figcaption>von-Mises Stress on frame.</figcaption>
  </figure>
  <figure>
    <img class="detail-img" src="/assets/images/Bike/V1_VM_max.png" alt="Maximum von-Mises stress">
    <figcaption>Maximum von-Mises stress occured at the connection of tubes on the bottom bracket shell.</figcaption>
  </figure>

  <figure>
    <img class="main-img" src="/assets/images/Bike/V1_SF.png" alt="Safety factor">
    <figcaption>Safety factor on frame.</figcaption>
  </figure>
  <figure>
    <img class="detail-img" src="/assets/images/Bike/V1_VM_max.png" alt="Maximum von-Mises stress">
    <figcaption>Minimum safety factor occured at the connection of tubes on the bottom bracket shell.</figcaption>
  </figure>
</div>




| Results Summary | Value |
|---|---|
| Total mass | 1.16 kg |
| Max total deformation | 0.165 mm |
| Max von-Mises stress | 63.4 MPa |
| **Minimum safety factor** | **4.41** |


Removing the top tube leaves the frame comfortably above the safety threshold for a uniform tube thickness of 2mm. The critical region shifts to the bottom bracket shell, where the loss of the top tube's bracing is felt most.

### Redesigned Geometry
To make the bike easier to mount, I curved the lower edge of the down tube, so riders can step over it without stretching over a steep incline — a small change that matters if you're wearing restrictive clothing. I relocated the battery to the midpoint of the down tube to suit this new shape, which also made for a convenient mounting position. Based on insight from the baseline run, I also dropped the wall thickness of all tubes to a uniform 1.5 mm to cut weight.

<figure style="max-width: 350px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Bike/V2_geom.png" alt="Redesigned geometry">
  <figcaption>Redesigned geometry for easier mounting.</figcaption>
</figure>


Using the same FEA setup, I got the following results for the new geometry. All the critical maximum/minimum values occured at the same locations as the baseline geometry.

| Results Summary | Value |
|---|---|
| Total mass | 2.47 kg |
| Max total deformation | 0.330 mm |
| Max von-Mises stress | 190.0 MPa |
| Minimum safety factor | 1.32 (below the target of 2) |


### Parametric Optimisation

The redesigned geometry didn't clear the safety factor target at a uniform thickness of 1.5mm. Rather than increasing the thickness of all tubes, I ran a parametric optimisation on the thickness of the three tubes most critical to the minimum safety factor, where I minimised the mass whilst acheiving a safety factor ≥ 2.


<div class="img-row">
  <figure style="max-width: 450px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Bike/optimisation.png" alt="Opitimised tubes">
  <figcaption>Tubes with optimised thickness whilst maintaining a safety factor > 2.</figcaption>
</figure>
  <figure style="width: 300px;">
    <img src="/assets/images/Bike/V2_SF.png" alt="Safety factor of final frame">
    <figcaption>Final frame design with a minimum safety factor > 2.</figcaption>
  </figure>
</div>

| Tube | Optimised thickness | Rounded (manufacturable) |
|---|---|---|
| Seat Tube | 1.267 mm | 1.2 mm |
| Down Tube | 1.739 mm | 1.8 mm |
| Bottom Bracket | 2.995 mm | 3.0 mm |

**Impact:** 
- Final mass 2.60 kg
- Minimum safety factor 2.04
- A **55% increase in safety factor for only a 5% increase in mass** over the uniform-thickness redesign.

### Verification

Every model was checked three ways before trusting the results:

- **Reaction forces** — reactions at the essential BCs summed to exactly balance the 880 N of applied load in the load direction, for both the baseline and final designs.
- **Boundary conditions** — displacement at all constrained nodes confirmed to be exactly zero.
- **Mesh convergence** — total deformation and von-Mises stress were tracked across four levels of mesh refinement; both converged to within 5% (deformation after 1 refinement, stress after 4).

<div class="img-row">
  <figure style="max-width: 350px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Bike/Def_conv.png" alt="Total deformation convergence">
  <figcaption>Total deformation converged to within 5% after one mesh refinement.</figcaption>
</figure>
  <figure style="width: 350px;">
    <img src="/assets/images/Bike/VM_conv.png" alt="von-Mises convergence">
    <figcaption>von-Mises stress converged to within 5% after 4 refinements</figcaption>
  </figure>
</div>


### Takeaways

- Representing the frame as shell mid-surfaces rather than solid geometry was the appropriate choice for thin-walled tubes, keeping the mesh coarse and the solution efficient without sacrificing accuracy. This does however required the CAD to be constructed around shell theory from the start, rather than adapted from an existing solid model
- The first redesign attempt improved some regions of the frame but made others worse — a reminder that structural design is iterative, and a single "improvement" pass rarely gets everything right at once.
- Parametric optimisation was the highest-leverage step here: a 55% gain in safety factor for a 5% mass penalty would never have been possible by simply taking educated guesses at shell thicknesses.
- limitations over welding - probably weakest part of the frame which isn't modeled in simulation, transient shock analysis - e.g. hitting a curb/bump, different loads


## Limitations

- **Welded joints not modeled.** The joints are almost certainly the weakest point of the real frame, but the simulation treats the geometry as continuous — it doesn't capture the stress concentrations, residual stresses, or reduced material strength typical of a weld.
- **Static analysis only.** The model doesn't capture dynamic or impact loading, such as hitting a curb or pothole, which can produce transient stresses well above the steady-state loads analysed here.
- **Single, idealized load case.** Only one symmetric loading condition (seated rider, steady pedaling) was tested. Real riding introduces combined and asymmetric loads — braking, cornering, or standing to pedal — that weren't evaluated.
- **No fatigue assessment.** A bike frame experiences millions of load cycles over its service life. Cyclic loading can cause failure well below the static yield strength, so a frame that passes this static safety-factor check isn't guaranteed to hold up long-term.

### Tools
Fusion 360, ANSYS Mechanical, ANSYS parametric optimisation