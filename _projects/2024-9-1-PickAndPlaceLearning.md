---
title: "Action Affordance Learning for Robotic Pick-and-Place"
# toc: true
# toc_sticky: true
collection: projects
type: 
permalink: /projects/pick-and-place-learning/ 
# date: 2023-5-8
period: Sep 2024 - Dec 2024
location: 
classes: wide
excerpt: "Trained a robotic manipulator for pick-and-place tasks using an action affordance model, achieving high success rates on both seen and unseen objects."
---
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>

{% include toc %}


## Introduction

Teaching a robot to pick up and move arbitrary objects is a fundamental challenge in robotics. Instead of training a model to recognize specific objects, a more generalizable approach is to teach it the concept of "graspability," or **action affordances**. This allows the robot to identify suitable grasping points on any object, whether it has seen it before or not.

This project implements a deep learning model for a robotic pick-and-place task. The model uses a Mini-UNet architecture to predict spatial action maps from visual input, enabling a robotic arm to grasp a variety of objects in a simulated environment.

## Method

The core of this project is an affordance-based model that predicts optimal grasping poses directly from images. The model is designed to be equivariant to translation and rotation, allowing it to generalize its understanding of a "good grasp" across different object positions and orientations.

The network architecture is a **Mini-UNet**, which is well-suited for generating spatial heatmaps that correspond to the input image. Key aspects of the method include:
-   **Affordance-Based Learning**: The model is trained not on object identities, but on "affordances"—predicting regions on an object that are suitable for grasping. [cite_start]This is achieved by generating Gaussian scoremaps that provide dense supervision, guiding the network to focus on generalizable geometric features rather than object-specific appearances[cite: 4262, 4263].
-   **Rotational Data Augmentation**: To ensure the model can identify grasps at any orientation, the training data is augmented with random rotations. [cite_start]This forces the model to learn a rotation-invariant understanding of graspability[cite: 4264].
-   **Test-Time Improvement**: A key improvement was implemented to handle failed grasp attempts. After a failure, the affordance map is updated to remove the failed action. [cite_start]This prevents the robot from getting stuck in a loop of repeating the same unsuccessful grasp and encourages it to explore other promising options[cite: 4269].

## Results and Demonstrations

The agent was trained using supervised learning for over 100 epochs, achieving high prediction accuracy. The evaluation focused on clearing a bin containing diverse objects, both seen and unseen during training.

- **Training Loss**: Final value of **0.0011**
- **Test Loss**: Final value of **0.0010**

### Quantitative Results

| Evaluation Set     | Success Rate |
|--------------------|--------------|
| Seen Objects       | **86.7%**     |
| Unseen Objects     | **76.7%**     |

The success rates confirm that the affordance-based model generalizes well beyond its training set by focusing on spatial geometry rather than specific object identity.

### Video Demonstrations

The videos below illustrate the effectiveness of the model in simulation.

**1. Pick-and-Place Task (No Test-Time Buffer)**  
Without failure handling, the robot occasionally repeats the same failed grasp, reducing efficiency.  
_Result: 4 objects remained in the bin after 10 attempts._  

<iframe width="560" height="315" src="https://www.youtube.com/embed/UUQe6OvuvCw?si=dXieN5a7e5KZnuV9" title="Pick and Place - No Buffer" frameborder="0" allowfullscreen></iframe>

**2. Pick-and-Place with Test-Time Improvement**  
In this version, a buffer prevents repeated failed grasps by removing failed actions from the affordance map.  
_Result: 4 objects remained (due to a physics engine issue with a hammer sticking to the bin), but grasp attempts were more strategic and diverse._  

<iframe width="560" height="315" src="https://www.youtube.com/embed/Zywt3zYZ1Fk?si=63q4FdmgWHc-rbgJ" title="Pick and Place - With Buffer" frameborder="0" allowfullscreen></iframe>

## Conclusion

This project successfully demonstrates the effectiveness of an action affordance model for robotic grasping. By focusing on the geometric properties of graspable surfaces rather than object identities, the Mini-UNet model generalized remarkably well to unseen objects.

The implementation of a test-time improvement, which prevents the robot from repeating failed actions, proved crucial for creating a more intelligent and efficient system. This strategic adaptation allows the manipulator to handle failures gracefully and avoid getting stuck in unproductive loops, showcasing a more robust approach to real-world robotic interaction.