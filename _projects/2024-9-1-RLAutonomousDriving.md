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

<!-- https://docs.google.com/document/d/1IRCccUOwx-f1--FEx6aE-qLXbhfnBbiPrwmLhwSZVj0/edit?usp=sharing -->


This page is based on the following resources:<br />
<a style="text-decoration: none;" href="/assets/projects/rl-autonomous-dirving/AA228_CS238_Project_Report.pdf" target="_blank">Report <i class="fa fa-file"></i></a><br />
<a style="text-decoration: none;" href="https://github.com/tae-h-yang/SHIELD" target="_blank">Code <i class="fa fa-code"></i></a><br />

## Introduction
[cite_start]The increasing adoption of autonomous vehicles (AVs) presents significant challenges for ensuring safety and efficiency in complex traffic scenarios[cite: 10]. [cite_start]Among these, navigating uncontrolled four-way intersections poses a critical problem[cite: 11]. [cite_start]Such intersections lack explicit signaling, requiring AVs to make real-time decisions while interacting with other road users who exhibit diverse and latent driving styles, ranging from aggressive to cautious[cite: 13].

[cite_start]In our SUMO (Simulation of Urban Mobility) environment, these latent driving styles are configured using an **"impatience" parameter**[cite: 31]. This parameter governs the behavior of other vehicles:
* **Aggressive (Impatient) Vehicles**: Assigned an impatience value of `1.0`, these vehicles accelerate more aggressively and make rapid decisions when navigating the intersection.
* **Patient (Cautious) Vehicles**: With impatience set to `off`, these vehicles drive more conservatively, with slower acceleration and more cautious decision-making.

[cite_start]This project explores this problem by first formulating it as a fully observable Markov Decision Process (MDP), where driving styles are known, and then as a more realistic Partially Observable Markov Decision Process (POMDP), where these styles are hidden and must be inferred from behavior[cite: 4, 6].

## Method

### MDP for Full Observability

[cite_start]We first model the problem as an MDP, assuming the ego vehicle has full knowledge of other drivers' impatience levels[cite: 17]. We explored two model-free approaches using Deep Q-Networks (DQN).

1.  [cite_start]**Combined MDP**: A single, high-dimensional MDP is solved, where the state includes the ego vehicle and up to 20 nearby road users[cite: 90]. [cite_start]This results in a large state space of 105 dimensions (ego state + 20 * foe vehicle states)[cite: 97]. While comprehensive, this approach can be difficult to train effectively.
2.  [cite_start]**Independent MDPs**: To reduce complexity, we solve separate, smaller MDPs for each surrounding vehicle, considering only the ego vehicle and one other road user at a time[cite: 73]. [cite_start]The final action is chosen by selecting the best action for the state with the worst predicted Q-value, ensuring a safe, defensive driving strategy[cite: 124].

### POMDP for Hidden Driving Styles

[cite_start]To create a more realistic scenario, we treat the impatience of other drivers as a hidden state, reformulating the problem as a POMDP[cite: 77].

1.  **Deep Recurrent Q-Network (DRQN)**: A DRQN is used to handle partial observability directly. [cite_start]Its recurrent structure, using an LSTM layer, allows the agent to maintain a memory of past observations to infer the hidden driving styles and make informed decisions[cite: 137, 138].
2.  [cite_start]**DQN with Impatience-Estimating LSTM**: This hybrid approach first uses an LSTM network to explicitly estimate the impatience level of each vehicle based on its recent behavior[cite: 158]. [cite_start]This estimation transforms the POMDP back into an MDP, allowing us to reuse the pre-trained Independent DQN for decision-making[cite: 160].

## Demo Videos & Results

The policies were trained over 5,000 episodes and evaluated on their ability to safely and efficiently cross the intersection. In the videos, the **ego vehicle is yellow**, **patient vehicles are green**, and **aggressive (impatient) vehicles are red**.

### 1. Random Policy (Baseline)
The ego vehicle selects its acceleration from a uniform random distribution. As seen in the video, this policy is highly unstable and frequently leads to collisions because it fails to react to the presence and behavior of other vehicles.

<iframe width="560" height="315" src="https://www.youtube.com/embed/g9itaHs5lTc?si=Ao3RBgU0p8tPsQF7" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### 2. DQN Policy (Full Observability)
The DQN agent has access to the true impatience levels of other vehicles. The video demonstrates that the agent learns a robust policy, successfully navigating the intersection by being cautious, especially around the aggressive (red) vehicles. [cite_start]It achieves a higher success rate and is more efficient (faster crossing time) than the random policy[cite: 206].

<iframe width="560" height="315" src="https://www.youtube.com/embed/CE1KN5p64ow?si=RDAabycyFirEW9-D" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### 3. DRQN Policy (Partial Observability)
The DRQN agent must infer driving styles from observed behavior. While still successful, its performance is less efficient than the fully observable DQN, as it must contend with uncertainty. [cite_start]The report notes that the POMDP-based methods were likely undertrained due to the high data requirements of LSTM models, resulting in weaker overall performance compared to their MDP counterparts[cite: 228].

<iframe width="560" height="315" src="https://www.youtube.com/embed/QDEmzOe_8p8?si=m2Jk4vOo-pqe7pVz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Conclusion

This project highlights the trade-offs between model complexity, observability, and performance in autonomous intersection navigation. Our findings indicate that:
-   [cite_start]**Managing State Space is Key**: The Independent DQN outperformed the Combined DQN, suggesting that reducing state-space dimensionality is highly effective[cite: 234].
-   **Explicit Estimation Can Outperform End-to-End**: The POMDP approach of first estimating impatience and then using a DQN was more successful than the end-to-end Combined DQN. [cite_start]This suggests that tackling state-space complexity is more critical than having perfect, but high-dimensional, information[cite: 229, 231].

Ultimately, understanding the latent states of other agents is crucial for both safety and efficiency. However, effectively managing the observable state space appears to be a more practical and impactful first step.

## Future Work

Future work could focus on several key areas for improvement:
-   [cite_start]**Refined Reward Engineering**: To better balance the trade-offs between safety and efficiency[cite: 239].
-   [cite_start]**More Complex Environments**: Incorporating more complex scenarios, such as vehicles turning or changing lanes[cite: 239].
-   [cite_start]**Advanced Belief Updates**: Exploring more sophisticated architectures for inferring hidden states in partially observable environments[cite: 240].