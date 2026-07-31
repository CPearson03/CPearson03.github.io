---
layout: default
title: Ahmed Car Model Flow
description: Ansys Analysis Project
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


### Contents

<img src="/assets/images/Ahmed_Car/Front_image.png" alt="Streamlines showing recirculation" class="inline-image-r" style="max-width: 400px;">

- [Objective](#objective)
- [Methodology & Pre-analysis](#methodology-pre-analysis)
- [Results](#results)
- [Discussion](#discussion)
- [References](#references)


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

**Domain sizing:** The recommended practice for external aerodynamics is to place the front, side, and top boundaries at 10 characteristic lengths (L) from the body and the outlet at 20L, to keep the boundary conditions from artificially influencing the flow near the body. Due to time/computational constraints, a reduced domain was used instead: 2.5L upstream, 5L for the top, side, and downstream boundaries, and the ground modeled at true height (0). This is a known limitation — see discussion.

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


**Expected physical trends to check results against:** Flow decelerates (low velocity, high pressure) at the front stagnation point, accelerates around the front curvature (high velocity, low pressure), and separates at the back corner, producing a recirculation zone with reduced velocity and pressure due to viscous dissipation. We may also see separation at the back window due to the sudden change in geometry. Form drag is expected to dominate over skin-friction drag, driven by the pressure differential from separation.

### Results

**Convergence**

<div class="img-row">
  <figure style="width: 350px;">
    <img src="/assets/images/Ahmed_Car/Residuals.png" alt="Residuals">
    <figcaption>Residuals converge below 10⁻³ after 58 iterations.</figcaption>
  </figure>
  <figure style="width: 350px;">
    <img src="/assets/images/Ahmed_Car/Force_coef_convergence.png" alt="Residual / Cd & Cl convergence history">
    <figcaption>Coefficients stabilise after ~45 iterations.</figcaption>
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
  <figcaption>Flow remains attached at the rear window. A recirculation bubble is visible downstream of the rear corner after separation at this point. </figcaption>
</figure>


**Force coefficients & drag forces**

| Coefficient | Value |
|---|---|
| Cd | *0.335* |
| Cl | *0.385* |
| Form drag | *29.9 N* |
| Skin friction drag | *5.8 N* |


### Discussion

**Comparison to literature**

Experimental results for the same geometry and configuration report Cd = 0.299 and Cl = 0.345 [1]. The simulation overestimates both coefficients by approximately 12%. The flow remains attached along the rear slant in both the simulation and experiment, consistent with the documented behavior of Ahmed bodies with slant angles below the critical ~30° threshold, above which the flow transitions to a fully separated regime [1].

**Physical trend validation**

The pressure and velocity contours, along with the streamline visualisation, reproduce all of the physical trends anticipated in the Methodology section: a stagnation point at the front face, flow acceleration and a corresponding pressure drop around the front curvature, and separation at the rear corner producing a recirculation region. Form drag (29.9 N) is over 5x larger than skin friction drag (5.8 N), consistent with the expectation that pressure-driven separation dominates drag for bluff bodies of this geometry.

**Attribution of discrepancy**

Three modeling simplifications likely contribute to the 12% overestimation, ranked by expected impact:

1. **Domain size** — the far-field boundaries were placed at 2.5L–5L rather than the recommended 10L–20L. A domain this constrained can artificially accelerate flow around the body (a blockage effect), which would tend to increase the predicted pressure differential and inflate both Cd and Cl. This is likely the largest contributor given how far the reduction is from standard practice.
2. **Turbulence model & near-wall resolution** — the k-ω GEKO model, combined with only 5 inflation layers (versus the recommended 12+) and a y+ ≈ 50 wall-function approach, introduces uncertainty in the predicted separation point on the slant. RANS models are known to struggle with accurately capturing separation location on slanted rear surfaces, and an under-resolved boundary layer compounds this by increasing uncertainty in near-wall shear and pressure recovery.
3. **Symmetry assumption** — modeling only half the body assumes perfectly symmetric flow, which removes any 3D instability or asymmetric wake dynamics that could be present in the full-body case, though this effect is typically minor compared to the two above.

A logical next step to isolate these effects would be a domain independence study (re-running with the recommended 10L–20L domain) to check whether the overestimation shrinks — this would help confirm domain size as the dominant factor.

**y+ and mesh quality**

The y+ distribution ranges from 3–211, with most of the body wall falling between 3–55 — comfortably within the valid range for a wall-function-based approach (typically valid for 30 < y+ < 300). Localised values above this range on the support stilts likely reflect their reduced boundary layer resolution rather than the main body's surface, and given the stilts' small contribution to overall wetted area and force generation, this is unlikely to meaningfully affect the global Cd/Cl values reported.

**Overall takeaway**

A 12% overestimation in both Cd and Cl is a reasonable result for a first-pass RANS simulation using a reduced domain and a coarser than recommended boundary layer mesh. The consistent overestimation across both coefficients (rather than a mixed over/under-prediction) supports domain blockage as the most likely dominant cause, since it would be expected to inflate both coefficients simultaneously in the same direction. 


### References

[1] Moghimi, P., & Rafee, R. (2018). Numerical and Experimental Investigations on Aerodynamic Behavior of the Ahmed Body Model with Different Diffuser Angles. *Journal of Applied Fluid Mechanics*, 11(4), 1101–1113. https://doi.org/10.29252/jafm.11.04.27923


### Tools Used

Ansys Workbench, Fluent Meshing, Fluent Solver (k-ω GEKO turbulence model), CFD-Post, SpaceClaim
