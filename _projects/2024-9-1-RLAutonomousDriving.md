---
title: "Safe Autonomous Driving at Uncontrolled Intersections with Latent Style Awareness"
# toc: true
# toc_sticky: true
collection: projects
type: 
permalink: /projects/rl-autonomous-driving/ 
# date: 2023-5-8
period: Sep 2024 - Dec 2024
location: 
classes: wide
excerpt: "Developed and compared MDP and POMDP-based reinforcement learning agents to navigate an autonomous vehicle through an uncontrolled intersection with other agents of unknown, latent driving styles."
---
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

{% include toc %}

<!-- https://docs.google.com/document/d/1IRCccUOwx-f1--FEx6aE-qLXbhfnBbiPrwmLhwSZVj0/edit?usp=sharing -->


This page is based on the following resources:<br />
<a style="text-decoration: none;" href="/assets/projects/rl-autonomous-dirving/AA228_CS238_Project_Report.pdf" target="_blank">Report <i class="fa fa-file"></i></a><br />
<a style="text-decoration: none;" href="https://github.com/tae-h-yang/SHIELD" target="_blank">Code <i class="fa fa-code"></i></a><br />

## Introduction

The increasing adoption of autonomous vehicles (AVs) presents significant challenges for ensuring safety and efficiency in complex traffic scenarios. Among these, navigating uncontrolled four-way intersections is particularly challenging due to the absence of traffic signals and the presence of diverse human driving styles.

We simulate such scenarios using SUMO (Simulation of Urban Mobility), where **latent driving styles** are governed by an **"impatience" parameter**:
- **Aggressive (Impatient)**: Impatience = `1.0`, fast acceleration and risky decisions.
- **Cautious (Patient)**: Impatience = `off`, slow, defensive driving.

This project formulates the task as both a fully observable **Markov Decision Process (MDP)** and a more realistic **Partially Observable Markov Decision Process (POMDP)** to account for hidden intent.

---

## Method

### MDP: Full Observability

We assume the ego vehicle knows each vehicle’s impatience level and train using **Deep Q-Networks (DQN)**:

- **Combined MDP**: One high-dimensional state space (ego + 20 agents). Difficult to train due to state complexity.
- **Independent MDPs**: Separate small DQNs per foe agent. Final action chosen conservatively based on worst-case Q-value, improving safety.

### POMDP: Hidden Styles

We model hidden impatience as a latent state and use:

- **Deep Recurrent Q-Network (DRQN)**: Uses LSTM to infer latent styles from observation history.
- **DQN + LSTM Estimator**: An LSTM explicitly predicts impatience levels; decisions are made using the pretrained Independent DQN.

---

## Demo Videos & Results

Trained over 5,000 episodes. In the videos:
- **Ego vehicle**: Yellow  
- **Cautious (patient)**: Green  
- **Aggressive (impatient)**: Red

### Random Policy (Baseline)
<iframe width="560" height="315" src="https://www.youtube.com/embed/g9itaHs5lTc" frameborder="0" allowfullscreen></iframe>  
Random acceleration decisions. Results in frequent collisions due to poor reaction to other vehicles.

### DQN Policy (Full Observability)
<iframe width="560" height="315" src="https://www.youtube.com/embed/CE1KN5p64ow" frameborder="0" allowfullscreen></iframe>  
Access to true driving styles allows cautious and efficient crossing, outperforming the random baseline.

### DRQN Policy (Partial Observability)
<iframe width="560" height="315" src="https://www.youtube.com/embed/QDEmzOe_8p8" frameborder="0" allowfullscreen></iframe>  
Learns to infer behavior from past trajectories. Performs well but slightly less efficient due to uncertainty and undertraining.

---

## Conclusion

This project highlights trade-offs in safe autonomous navigation:
- **Independent MDPs** outperform high-dimensional DQNs by simplifying the state space.
- **Explicit Latent State Estimation** improves policy reliability under partial observability.
- **POMDP methods** require more training data but offer realism for real-world deployment.

---

## Future Work

- Improve reward design to better balance safety and efficiency.
- Add complexity: left turns, lane merges, and human AV mix.
- Explore probabilistic belief update methods for latent state inference.
