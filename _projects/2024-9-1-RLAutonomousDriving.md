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

Navigating uncontrolled intersections is a critical challenge for autonomous vehicles. Without traffic signals, an AV must make real-time decisions by interacting with other road users, each with their own unique, unstated driving style. How can a car learn to be safe when it doesn't know if the other driver is cautious or aggressive?

This project, **SHIELD**, explores this problem by training reinforcement learning agents in a SUMO simulation. We model **latent driving styles** using an "impatience" parameter:
- **Aggressive (Impatient)**: High impatience, leading to faster acceleration and riskier maneuvers.
- **Cautious (Patient)**: Low impatience, resulting in more defensive driving.

My work focused on two primary approaches: first, tackling the problem with full information (an MDP), and second, addressing the more realistic scenario where driving styles are hidden (a POMDP).

## My Approach: From Full Information to Hidden Intent

### 1. The Combined DQN Model (MDP)
My first approach modeled the intersection as a fully observable **Markov Decision Process (MDP)**. Here, the ego vehicle knows the exact impatience level of every other car. I developed a **Deep Q-Network (DQN)** agent that processes a high-dimensional state, including the ego vehicle and up to 20 surrounding vehicles, to make a single, holistic decision. While this provides a strong performance baseline, the sheer size of the state space presents a significant learning challenge.

### 2. The DRQN Model (POMDP)
To address the more realistic scenario where driving styles are unknown, I formulated the problem as a **Partially Observable MDP (POMDP)**. My solution was to implement a **Deep Recurrent Q-Network (DRQN)**. This model uses an LSTM layer to maintain a memory of past observations (like a vehicle's speed and acceleration over time). By analyzing these behavioral patterns, the DRQN learns to **infer the hidden driving style** of other agents and adapt its strategy accordingly, without ever being explicitly told whether another driver is aggressive or cautious.

---

## Demo Videos & Results

The agents were trained for 5,000 episodes. The videos below demonstrate the performance of different policies.
- **Ego vehicle**: Yellow
- **Cautious (patient)**: Green
- **Aggressive (impatient)**: Red

### Random Policy (Baseline)
The ego vehicle randomly samples acceleration values. This approach is completely unaware of its surroundings and, as expected, results in frequent and dangerous collisions.

<iframe width="560" height="315" src="https://www.youtube.com/embed/g9itaHs5lTc?si=Ao3RBgU0p8tPsQF7" frameborder="0" allowfullscreen></iframe>

### Combined DQN Policy (My MDP Solution)
This agent has full knowledge of other vehicles' driving styles. It learns to be cautious, especially around aggressive (red) vehicles, and successfully crosses the intersection much more safely and efficiently than the random baseline.

<iframe width="560" height="315" src="https://www.youtube.com/embed/CE1KN5p64ow?si=RDAabycyFirEW9-D" frameborder="0" allowfullscreen></iframe>

### Combined DRQN Policy (My POMDP Solution)
This agent must **infer** the driving styles from behavior alone. The video shows it successfully navigating the intersection, demonstrating its ability to handle uncertainty. While slightly less efficient than the fully observable DQN due to the complexity of its task, it represents a more robust and realistic approach for real-world scenarios.

<iframe width="560" height="315" src="https://www.youtube.com/embed/QDEmzOe_8p8?si=m2Jk4vOo-pqe7pVz" frameborder="0" allowfullscreen></iframe>

---

## Conclusion

This project demonstrates that reinforcement learning can produce robust policies for complex, multi-agent navigation tasks. My work specifically highlights two key takeaways:

1.  **Handling High-Dimensional States**: While the Combined DQN provides a holistic view, its performance can be limited by the complexity of its state space.
2.  **Inferring Latent States is Key**: The DRQN shows that it is possible to learn safe and effective policies even with incomplete information by using memory to infer the hidden intentions of other drivers. This is a crucial step toward building autonomous systems that can safely coexist with unpredictable human drivers.

## Future Work

- **Refined Reward Engineering**: To better balance safety with traffic efficiency.
- **Increased Scenario Complexity**: Incorporate more complex maneuvers like left turns and lane merges.
- **Advanced Belief Update Mechanisms**: Explore more sophisticated probabilistic methods for inferring latent states to improve the agent's understanding under uncertainty.