---
layout: project
title: Active Suspension Project
description: Modeling and designing an active suspension system
technologies: [MATLAB]
image: /assets/images/Active_Suspension/Suspension System.png


---

<style>
  .project-hero {
  text-align: center;
  margin: 20px 0 30px 0; /* spacing around image */
}

.project-hero img {
  max-width: 200px;   /* adjust size as needed */
  width: 100%;
  height: auto;
  border-radius: 8px; /* optional for nicer look */
}

  /* Figures and images */
  figure {
    display: inline-block;       /* shrink figure to image width */
    text-align: center;          /* center image & caption */
    margin: 25px auto;
  }

  figure img {
    display: block;
    margin: 0 auto;
    max-width: 100%;
  }

  figcaption {
    font-size: 0.9rem;
    color: #555;
    text-align: center;
    margin-top: 5px;
  }

  /* Image sizes */
  .img-small { width: 20%; }
  .img-medium { width: 40%; }
  .img-large { width: 80%; }

  /* Left-aligned text */
  .text-left { text-align: left; }

  /* Table styling */
  table {
    border-collapse: collapse;
    width: 60%;
    margin: 15px 0;
  }

  table, th, td {
    border: 1px solid #ddd;
  }

  th, td {
    padding: 8px;
    text-align: left;
  }

  th {
    background-color: #f2f2f2;
  }

  /* Flex row for multiple figures on same line */
  .figure-row {
    display: flex;
    justify-content: center;
    gap: 20px;
    flex-wrap: wrap; /* allows wrap on small screens */
  }

  .figure-row figure {
    flex: 1;           
    max-width: 120px;  /* optional: max width for each figure */
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 0;
  }

  .figure-row img {
    width: 100%;
    height: auto;
  }

  /* Vertically stacked, centered figures */
  .figure-centered {
    display: block;        
    text-align: center;    
    margin: 25px auto;     
  }

  .figure-centered img {
    display: block;
    margin: 0 auto;        
    max-width: 100%;
  }

  .figure-centered figcaption {
    font-size: 0.9rem;
    color: #555;
    text-align: center;
    margin-top: 5px;
  }
</style>


### Introduction
This project focuses on the analysis and design of an active suspension system. We take a state-space approach to modeling the system and then use active control to optimize its performance. 

---

### Assumptions and Design Requirements
The following assumptions were made to simplify the analysis:

<ul>
  <li>Quarter-car model represents one wheel and associated mass; only vertical motion considered.</li>
  <li>Yaw, pitch, roll, and aerodynamic effects are neglected.</li>
  <li>Masses are rigid; suspension components are linear and frictionless.</li>
  <li>Road input and actuator are ideal (perfect force application, no delays).</li>
  <li>Performance targets: driver acceleration &lt;1.5 m/s², body displacement &lt;1 cm, shock travel &lt;10 cm.</li>
</ul>

---

### Model of the System
The following analysis shows how the state space model is formed from the governing equations and then how the transfer function and poles are extracted from this.

<figure>
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/ODEs.png" class="img-large" alt="ODEs">
  <figcaption>Ordinary differential equations of the system.</figcaption>
</figure>

<div class="figure-row">
  <figure>
    <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/X_vector.png" alt="X_vector">
    <figcaption>State vector, X.</figcaption>
  </figure>

  <figure>
    <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/Y_vector.png" alt="Y_vector">
    <figcaption>Output vector, Y.</figcaption>
  </figure>

  <figure>
    <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/U_vector.png" alt="U_vector">
    <figcaption>Input vector, U.</figcaption>
  </figure>
</div>



<figure>
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/state_space1.png" class="img-large" alt="state_space1">
  <br>
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/state_space2.png" class="img-large" alt="state_space2">
  <figcaption>State-space representation of the system.</figcaption>
</figure>

<figure class="figure-centered">
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/Transfer function.png" class="img-medium" alt="Transfer function">
  <figcaption>Closed-form transfer function of the system.</figcaption>
</figure>

<figure class="figure-centered">
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/Poles.png" class="img-small" alt="Poles">
  <figcaption>Poles of the system.</figcaption>
</figure>




<figure>
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/Bode.png" class="img-large" alt="Bode">
  <figcaption>Bode plot of body displacement.</figcaption>
</figure>

---

### Closed Loop Analysis
We analyzed the closed-loop dynamics using the block diagram of the system:

<figure>
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/BD.png" class="img-large" alt="Block_diagram">
  <figcaption>Closed-loop block diagram of the system.</figcaption>
</figure>

<figure>
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/X_closedloop.png" class="img-large" alt="X_closedloop">
  <figcaption>State vector with closed-loop control.</figcaption>
</figure>

---

### Parameters
In our model, we used the following parameters:

<table>
  <tr><th>Parameter</th><th>Value</th></tr>
  <tr><td>Acceleration (a(t))</td><td>&lt; 5 m/s²</td></tr>
  <tr><td>Sprung Mass</td><td>300 kg</td></tr>
  <tr><td>Displacement y(t)</td><td>&lt; 1.5 cm</td></tr>
  <tr><td>Unsprung Mass</td><td>60 kg</td></tr>
  <tr><td>Shock Travel</td><td>&lt; 10 cm</td></tr>
  <tr><td>Tire Stiffness</td><td>190,000 N/m</td></tr>
  <tr><td>Road Height</td><td>0.05 m</td></tr>
  <tr><td>Suspension Stiffness</td><td>17,000 N/m</td></tr>
  <tr><td>Suspension Damping</td><td>2,500 N·s/m</td></tr>
</table>

---

### Results
We initially modeled the suspension system in open-loop form without any control. We then implemented skyhook control to improve performance. Below is a comparison between the open-loop and closed-loop response of the system:

<figure>
  <img src="/fa25-portfolio-CPearson03/assets/images/Active_Suspension/Control results.png" class="img-large" alt="Control results">
  <figcaption>Comparison of open-loop vs. closed-loop suspension response.</figcaption>
</figure>

---

### Conclusions
The active suspension system showed clear improvements over the passive model, reducing body displacement and improving ride comfort through skyhook control. Modeling the system using a state-space approach provided a clear framework for analyzing its dynamics and evaluating control performance.