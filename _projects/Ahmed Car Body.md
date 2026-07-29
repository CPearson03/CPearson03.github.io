---
layout: default
title: Ahmed Car Model Flow
description: Anysy Analysis Project
technologies: [Ansys Fluent]
image: /assets/images/Ahmed_Car/Thumbnail.png
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


## CFD Analysis of Ahmed Car Body Flow

3D RANS simulation of turbulent flow over an Ahmed car body, predicting drag and lift and visualising the wake structure using Ansys Fluent.


#### Contents

<img src="/assets/images/Ahmed_Car/Front_image.png" alt="Streamlines showing recirculation" class="inline-image-r" style="max-width: 400px;">

- [Objective](#objective)
- [Methodology & Pre-analysis](#methodology-pre-analysis)
- [Results](#results)
- [Discussion](#discussion)
- [Limitations](#limitations)


### Objective

The Ahmed body is a simplified car geometry widely used as a benchmark for bluff-body aerodynamics, because its rear slant produces flow separation and a recirculation region similar to a real vehicle. The goal of this project was to solve for the 3D turbulent flow field around the body, compute the drag coefficient (Cd) and lift coefficient (Cl), and build physical intuition for the flow by visualising velocity and pressure fields.


### Methodology & Pre-analysis {#methodology-pre-analysis}

**Geometry**

| Dimension | Value |
|---|---|
| Length | 1044 mm |
| Width | 389 mm |
| Height | 288 mm |
| Back Angle | 25° |


<figure style="max-width: 450px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Ahmed_Car/Geometry.png" alt="Ahmed body in SpaceClaim">
  <figcaption>Ahmed body geometry (25° slant configuration) modeled in Ansys SpaceClaim.</figcaption>
</figure>

**Governing equations:** Reynolds-Averaged Navier-Stokes (RANS), closed with the k-ω GEKO turbulence model. The RANS approach assumes the time-averaged flow statistics are steady, and models turbulent stresses via an eddy-viscosity approximation. The fluid was treated as Newtonian air, with:
- Density ρ = 1.193 kg/m³
- Dynamic viscosity μ = 1.7894 × 10⁻⁵ Pa·s


**Domain and symmetry:** Since the Ahmed body is symmetric about its centerline, only half the geometry was modeled, with a symmetry boundary condition on the centerplane to halve the mesh size without losing accuracy. This requires Cl, Cd and drag forces reported by the solver to be doubled.

**Domain sizing:** The recommended practice for external aerodynamics is to place the front, side, and top boundaries at 10 characteristic lengths (L) from the body and the outlet at 20L, to keep the boundary conditions from artificially influencing the flow near the body. Due to time/computational constraints, a reduced domain was used instead: 2.5L upstream, 5L for the top, side, and downstream boundaries, and the ground modeled at true height (0). This is a known limitation — see below.

**Boundary conditions:**
- Inlet: velocity-inlet, Vx = 40 m/s
- Turbulence intensity = 1%, turbulent viscosity ratio = 2 at the inlet, top, and side boundaries (low-turbulence freestream values — results are not expected to be sensitive to these, since turbulence quantities grow much larger near the body and walls)
- Ahmed body & ground: no-slip wall
- Outlet: pressure outlet
- Symmetry plane: zero-gradient (symmetry) condition

**Solver settings:** Pressure-based solver, second-order upwind discretisation for turbulent kinetic energy and specific dissipation rate, with pseudo-transient and high-order term relaxation enabled for stability. Solution initialised using standard initialisation computed from the inlet, followed by Full Multi-Grid (FMG) initialisation via the solver's text interface for a more robust initial guess. Run for 100 iterations, with residuals converging below 10⁻³.

**Force coefficients:** Cd and Cl were computed using reference values matching the Ahmed body's frontal area (0.112 m²), free-stream velocity (40 m/s), and density (1.193 kg/m³).


**Mesh**
- Boundary layer resolved using 5 inflation layers (12+ recommended — see Limitations), with a first-cell height targeting y+ ≈ 50, appropriate for a wall-function-based (law of the wall) approach rather than fully resolving the viscous sublayer.
- Mesh quality assessed via inverse orthogonal quality, with a target maximum below 0.95 (higher values make the discretised equations increasingly stiff and hurt convergence).
- Total cell count: *151,758* cells
- Inverse orthogonal quality: max *0.83*, average *0.05*

<div class="img-row">
  <figure style="width: 380px;">
    <img src="/assets/images/Ahmed_Car/Mesh.png" alt="Generated Mesh">
    <figcaption>Mesh generated around car body.</figcaption>
  </figure>
  <figure style="width: 300px;">
    <img src="/assets/images/Ahmed_Car/Mesh_boundary_layers.png" alt="Mesh cross-section">
    <figcaption>Boundary layer inflation near the Ahmed car body wall, 5 layers.</figcaption>
  </figure>
</div>


**Expected physical trends to check results against:** Flow decelerates (low velocity, high pressure) at the front stagnation point, accelerates around the front curvature (high velocity, low pressure), and separates at the back corner, producing a recirculation zone with reduced velocity and pressure due to viscous dissipation. Form drag is expected to dominate over skin-friction drag, driven by the pressure differential from separation.

### Results

**Convergence**

<div class="img-row">
  <figure style="width: 350px;">
    <img src="/assets/images/Ahmed_Car/Residuals.png" alt="Residuals">
    <figcaption>Residuals converge below 10⁻³ after 58 iterations.</figcaption>
  </figure>
  <figure style="width: 350px;">
    <img src="/assets/images/Ahmed_Car/Force_coef_convergence.png" alt="Residual / Cd & Cl convergence history">
    <figcaption>Coeficients stabalise after ~45 iterations.</figcaption>
  </figure>
</div>


**Wall y+**

<figure style="max-width: 700px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Ahmed_Car/y_plus.png" alt="Wall y+ distribution">
  <figcaption>y+ distribution on the ahmed body wall, ranging from 3–211. Most of the body falls between 3–55; higher values are localised to the support stilts.</figcaption>
</figure>


**Velocity contours**

<figure style="max-width: 700px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Ahmed_Car/Velocity_mag_cont.png" alt="Velocity magnitude on symmetry plane">
  <figcaption>Flow decelerates at the front of the body, accelerates around the front curvature and separates near the rear corner, consistent with expected bluff-body behavior.</figcaption>
</figure>

**Pressure contours**

<figure style="max-width: 700px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Ahmed_Car/Pressure_mag_cont.png" alt="Pressure on symmetry plane">
  <figcaption>High-pressure region at the front of the body and low-pressure region visible in the wake due to recirculation zone. Results in pressure differential leading to significant form drag.</figcaption>
</figure>

**Streamlines**

<figure style="max-width: 700px; margin-left: auto; margin-right: auto;">
  <img src="/assets/images/Ahmed_Car/Velocity_streamlines.png" alt="Streamlines showing recirculation">
  <figcaption>Recirculation bubble visible downstream of the rear corner.</figcaption>
</figure>


**Force coefficients & drag forces**

| Coefficient | Value |
|---|---|
| Cd | *0.335* |
| Cl | *0.385* |
| Form drag | *29.9 N* |
| Skin friction drag | *5.8 N* |


### Discussion


*[To be added once results are in — compare computed Cd against the published experimental value of 0.298 for this Ahmed body configuration, and discuss agreement/discrepancy in light of mesh resolution and domain size.]*

### Limitations

- **Boundary layer resolution:** 5 inflation layers were used rather than the recommended 12+, which likely under-resolves the near-wall region and affects y+ and wall shear accuracy.
- **Domain size:** The far-field boundaries were placed closer to the body than standard practice (2.5L–5L vs. the recommended 10L–20L) to reduce mesh size and solve time. This is a good candidate for a mesh/domain independence study if extended further.

### Tools Used

Ansys Workbench, Fluent Meshing, Fluent Solver (k-ω GEKO turbulence model), CFD-Post, SpaceClaim
