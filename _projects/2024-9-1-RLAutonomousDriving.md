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
header:
  teaser: /assets/projects/rl-autonomous-dirving/rl-autonomous-driving-thumbnail.gif
excerpt: "<ul><li>Trained DQN (MDP) and DRQN (POMDP) agents in SUMO simulator for autonomous intersection navigation with agents of unknown driving styles.</li><li>Achieved 88% success rate; DRQN learned to infer latent driver behaviors using recurrent memory for safer decision-making.</li></ul>"
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

This project explores this question by modeling intersection navigation as a **multi-agent reinforcement learning problem**. Each vehicle exhibits a latent **driving style**—ranging from aggressive to cautious—that is not directly observable, but significantly impacts safety. The goal is to train a policy that can safely and efficiently cross intersections, even when the intentions of other drivers are unknown.

To tackle this, the task is framed under two decision-making paradigms:
- A **Markov Decision Process (MDP)**, where driving styles are fully observable
- A more realistic **Partially Observable MDP (POMDP)**, where the agent must **infer hidden driver behaviors from past observations**

The resulting system, **SHIELD** (*Safe Handling of Intersection Events under Latent Driving Styles*), is trained in a SUMO-based traffic simulator with diverse traffic patterns and driving styles. The agent must learn to adapt its behavior dynamically to ensure safety in the presence of uncertainty—just as a human driver would.

## Method

The SHIELD system was trained using deep reinforcement learning to handle intersection scenarios involving multiple agents with unknown intent. Two distinct formulations were used to investigate the trade-offs between full observability and realistic, partial information.

### 1. Combined Deep Q-Network (DQN): Full Observability (MDP)

In the first approach, we assume a fully observable environment where the driving style of every other vehicle is known. This scenario is modeled as a **Markov Decision Process (MDP)**.

The state is represented by a high-dimensional **105-D vector**, which includes the ego vehicle's state plus the states of up to 20 surrounding vehicles (position, velocity, acceleration, and impatience level). A single **Deep Q-Network (DQN)** is trained on this combined state to learn a centralized policy. While this gives the agent complete information, the high dimensionality makes it challenging for the model to learn efficiently.

### 2. Deep Recurrent Q-Network (DRQN): Partial Observability (POMDP)

To model a more realistic driving scenario, we treat other agents' intentions as unobservable, reframing the task as a **Partially Observable MDP (POMDP)**. In this setup, the agent must infer a vehicle's driving style from its behavior over time.

A **Deep Recurrent Q-Network (DRQN)** with an LSTM layer is used to process sequences of observations. At each timestep, the DRQN receives only the physical states of nearby vehicles (position, speed, etc.), without their true impatience labels. By maintaining a memory of past actions, the DRQN learns to implicitly identify aggressive versus cautious drivers and adapt its strategy accordingly.

### Action Space & Training Details

In both formulations, the models were trained with a consistent setup to ensure a fair comparison.

-   **Action Space**: The ego vehicle selects from a discrete set of 7 acceleration commands, ranging from **-3 m/s² to +3 m/s²**.

-   **Training Details**:
    -   **Episodes**: 5,000 per policy
    -   **Simulator**: SUMO (Simulation of Urban Mobility) with Python/TraCI bindings
    -   **Reward Function**: Designed to promote safe and efficient navigation:
        -   **+10** for successfully crossing the intersection
        -   **-100** for a collision
        -   **-0.5 per timestep** to penalize delays
        -   Additional penalties for speed limit violations and causing other vehicles to brake suddenly

This dual-method approach allows for a direct comparison between an agent with perfect information and a more practical agent that must navigate the uncertainty of real-world driving by inferring latent intent.

## Demo Videos & Results

The policies were trained over 5,000 episodes and evaluated on their ability to safely and efficiently cross the intersection. In the videos below:
- **Ego vehicle**: Yellow
- **Cautious (patient)**: Green
- **Aggressive (impatient)**: Red

### 1. Random Policy (Baseline)
The ego vehicle selects its acceleration from a uniform random distribution. This policy is completely unaware of its surroundings and, as the video clearly shows, definitely results in frequent and dangerous collisions.

<iframe width="560" height="315" src="https://www.youtube.com/embed/g9itaHs5lTc?si=Ao3RBgU0p8tPsQF7" frameborder="0" allowfullscreen></iframe>

### 2. Combined DQN Policy
With full observability of other agents' driving styles, the DQN learns an effective policy. It passes through safely, clearly aware of the other vehicles, especially the aggressive (red) ones. This allows it to navigate the intersection both safely and efficiently.

<iframe width="560" height="315" src="https://www.youtube.com/embed/CE1KN5p64ow?si=RDAabycyFirEW9-D" frameborder="0" allowfullscreen></iframe>

### 3. Combined DRQN Policy 
This agent must infer driving styles from behavior alone. It exhibits more careful behavior, such as a brief slowdown at the beginning, because it needs more time to observe and figure out the other vehicles' intentions. This demonstrates a more realistic and cautious approach, highlighting that the agent learned the importance of being aware of latent driving styles for safe decision-making.

<iframe width="560" height="315" src="https://www.youtube.com/embed/QDEmzOe_8p8?si=m2Jk4vOo-pqe7pVz" frameborder="0" allowfullscreen></iframe>

---

### Quantitative Analysis

The table below summarizes the performance of **Combined DQN** and **Combined DRQN** models over 500 evaluation episodes.

<!-- | Metric | Combined DQN | Combined DRQN |
| :--- | :---: | :---: |
| **Successful Passes** | 442 | 439 |
| **Average Time on Successful Passes (s)** | 6.81 | 9.14 |
| **Number of Collisions** | 58 | 61 |
| **Average Number of Emergency Stoppings from Road Users** | 0.79 | 0.97 |

<p style="text-align: center;"><strong>Table 1: Comparison of Evaluation Results for Models</strong></p> -->

<div style="display: table; margin: 0 auto;">
  <table>
    <caption><strong>Table 1: Comparison of Evaluation Results for Models</strong></caption>
    <thead>
      <tr>
        <th>Metric</th>
        <th>Combined DQN</th>
        <th>Combined DRQN</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><strong>Successful Passes</strong></td>
        <td>442</td>
        <td>439</td>
      </tr>
      <tr>
        <td><strong>Average Time on Successful Passes (s)</strong></td>
        <td>6.81</td>
        <td>9.14</td>
      </tr>
      <tr>
        <td><strong>Number of Collisions</strong></td>
        <td>58</td>
        <td>61</td>
      </tr>
      <tr>
        <td><strong>Average Number of Emergency Stoppings from Road Users</strong></td>
        <td>0.79</td>
        <td>0.97</td>
      </tr>
    </tbody>
  </table>
</div>


As expected, the **Combined DQN**, with its access to perfect information, performs well. It is significantly faster than DRQN model (6.81s vs. 9.14s) and results in fewer collisions (58 vs. 61).

The **Combined DRQN's** longer crossing time reflects the cautious behavior seen in the demo video. Because it must infer intent, it acts more defensively, leading to a safer but less efficient policy. The slightly higher number of collisions and the greater number of emergency stops it causes (0.97) suggest that inferring latent states from noisy observations is a challenging task. These results effectively quantify the trade-off between having complete information and the necessity of navigating real-world uncertainty.

## Conclusion

This project successfully demonstrates that deep reinforcement learning can produce robust policies for navigating complex, multi-agent intersection scenarios. The works on the **Combined DQN** and **DRQN** models highlights two key findings:

1.  **The Challenge of High-Dimensional States**: While the Combined DQN, with its perfect information, provides a strong performance baseline, its effectiveness is challenged by the sheer complexity of a 105-dimensional state space, which makes efficient learning difficult.

2.  **The Power of Inferring Latent States**: The DRQN model proves that it is possible to learn a safe and functional policy even with incomplete information. By using its recurrent memory to analyze behavior over time, it can implicitly infer the hidden driving styles of other agents. This represents a crucial step toward building autonomous systems that can safely coexist with unpredictable human drivers in real-world environments.

Interestingly, the results suggest that effectively managing the observable state may be more critical than having perfect but overwhelmingly complex information about latent states.

## Future Work

To build upon this project, future work could focus on several key areas for improvement:

-   **Refined Reward Engineering**: The reward function could be further refined to better balance the trade-offs between safety, such as avoiding collisions, and traffic efficiency, like minimizing crossing time.
-   **Increased Scenario Complexity**: The simulation environment could be enhanced to include more complex and realistic traffic scenarios, such as vehicles turning, merging, or changing lanes.
-   **Advanced Belief Update Mechanisms**: More sophisticated architectures could be explored for inferring hidden states, which could lead to more effective and reliable solutions for partially observable environments.
