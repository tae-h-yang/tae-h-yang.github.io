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

Navigating uncontrolled intersections remains a significant challenge for autonomous vehicles (AVs). These intersections lack signaling infrastructure, forcing AVs to make safe and efficient decisions while interacting with other road users whose **driving styles** are not directly observable.

This project introduces **SHIELD** (**S**afe **H**andling of **I**ntersection **E**vents under **L**atent **D**riving Styles), a reinforcement learning framework designed to handle such situations. Agents are trained in a SUMO simulation where the driving style of each vehicle is governed by an **"impatience" parameter**:

- **Aggressive (Impatient)**: Fast acceleration, riskier crossing behavior.
- **Cautious (Patient)**: Slow, defensive behavior.

The task is modeled both as a **Markov Decision Process (MDP)** with full observability and as a **Partially Observable MDP (POMDP)** where driving styles must be inferred from behavior.

## Methodology

### MDP with Known Driving Styles

In the MDP formulation, the agent has access to true impatience levels of all vehicles. Two strategies are explored:

- **Combined DQN**: A Deep Q-Network trained on a large joint state space representing the ego vehicle and up to 20 nearby vehicles.
- **Independent DQNs**: One Q-network per ego–foe pair; the final action is chosen conservatively based on the minimum Q-value across pairs.

### POMDP with Latent Driving Styles

For the more realistic scenario where impatience is hidden:

- **DRQN (Deep Recurrent Q-Network)**: An LSTM-based Q-network uses temporal sequences of vehicle states to infer driving styles and guide decision-making under uncertainty.

## Evaluation

Agents were trained for 5,000 episodes and evaluated over 200 randomized test cases.

| **Policy**            | **Visibility**     | **Success Rate** | **Collision Rate** | **Avg. Time (s)** |
|-----------------------|--------------------|------------------|--------------------|-------------------|
| Random Baseline       | None               | 24.5%            | 63.2%              | 9.81              |
| Combined DQN          | Full (MDP)         | 91.8%            | 4.7%               | 5.62              |
| Independent DQNs      | Full (MDP)         | 95.4%            | 1.3%               | 5.41              |
| DRQN                  | Partial (POMDP)    | 88.0%            | 7.2%               | 6.31              |

- *Success Rate*: Ego vehicle safely crosses the intersection.
- *Collision Rate*: Episode ends with a crash.
- *Avg. Time*: Time to cross intersection.

## Demo Videos

Vehicles are color-coded:
- Ego: **Yellow**
- Cautious (patient): **Green**
- Aggressive (impatient): **Red**

### Random Policy

No learned behavior; chooses random acceleration.

<iframe width="560" height="315" src="https://www.youtube.com/embed/g9itaHs5lTc?si=Ao3RBgU0p8tPsQF7" frameborder="0" allowfullscreen></iframe>

### Combined DQN Policy

Trained with full access to impatience levels.

<iframe width="560" height="315" src="https://www.youtube.com/embed/CE1KN5p64ow?si=RDAabycyFirEW9-D" frameborder="0" allowfullscreen></iframe>

### DRQN Policy

Infers latent driver intent from temporal behavior.

<iframe width="560" height="315" src="https://www.youtube.com/embed/QDEmzOe_8p8?si=m2Jk4vOo-pqe7pVz" frameborder="0" allowfullscreen></iframe>

## Conclusion

This work demonstrates that reinforcement learning can enable safe autonomous intersection handling under uncertainty. Key takeaways:

- **Reducing state complexity improves robustness**: Independent Q-networks outperform large combined models.
- **Inference from partial observability is feasible**: DRQN can infer latent behaviors and achieve safe navigation without direct style labels.

## Future Work

- **Reward shaping**: Improve trade-off between safety and speed.
- **Scenario complexity**: Add turning, occlusion, and lane changes.
- **Belief tracking**: Explore probabilistic intent inference mechanisms.

