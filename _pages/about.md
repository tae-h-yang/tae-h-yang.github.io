---
permalink: /
title: "About"
excerpt: "About"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
classes: wide
# homepage_feature_row:
#   - image_path: publications/nfls.png
#     title: "Neural Feedback Loop Verification"
#   - image_path: publications/cadrl_stata.gif
#     title: "Socially Aware Motion Planning"
#   - image_path: publications/jackal_offroad_small.png
#     title: "High-Speed Off-Road Navigation"
---

Hi! I'm Tae, a Master's student in Computer Science (AI specialization) at Stanford University. I earned my B.S. in Computer Engineering with a minor in Robotics from Northeastern University. I'm passionate about developing and researching embodied AI, with a focus on robot learning.

---

# Publications

<style>
.publication-card {
  display: flex;
  gap: 20px;
  margin-bottom: 30px;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
  align-items: center;
}

.publication-thumbnail {
  flex-shrink: 0;
  width: 250px;
}

.publication-thumbnail img {
  width: 100%;
  height: auto;
  border-radius: 4px;
}

.publication-info {
  flex-grow: 1;
}

.publication-title {
  font-size: 1.1em;
  font-weight: bold;
  margin-bottom: 8px;
  color: #2c3e50;
}

.publication-authors {
  margin-bottom: 8px;
  color: #555;
}

.publication-authors a {
  text-decoration: none;
}

.publication-authors a:hover {
  text-decoration: underline;
}

.publication-venue {
  margin-bottom: 12px;
  font-style: italic;
  color: #666;
}

.publication-links {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.publication-links a {
  padding: 6px 14px;
  background: #007bff;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  font-size: 0.9em;
  transition: background 0.3s;
}

.publication-links a:hover {
  background: #0056b3;
}
</style>

<div class="publication-card">
  <div class="publication-thumbnail">
    <img src="/assets/papers/locomotion_beyond_feet_thumbnail.gif" alt="Locomotion Beyond Feet">
  </div>
  <div class="publication-info">
    <div class="publication-title">Locomotion Beyond Feet</div>
    <div class="publication-authors">
      <strong><a href="https://tae-h-yang.github.io/">Tae Hoon Yang</a></strong><sup>*</sup>,
      <a href="https://hshi74.github.io/">Haochen Shi</a><sup>*</sup>,
      <a href="https://www.linkedin.com/in/jc-hu/">Jiacheng Hu</a><sup>*</sup>,
      <a href="https://www.linkedin.com/in/zhicongzhang/">Zhicong Zhang</a>,
      <a href="https://www.linkedin.com/in/danieljiangdj/">Daniel Jiang</a>,
      <a href="http://me.weizhuowang.com/">Weizhuo Wang</a>,
      <a href="https://shockwavehe.github.io/">Yao He</a>,
      <a href="https://zhenwu061.github.io/">Zhen Wu</a>,
      <a href="https://www.linkedin.com/in/matt-chen-ym/">Yuming Chen</a>,
      <a href="https://yifan-hou.github.io/">Yifan Hou</a>,
      <a href="https://monroekennedy3.com/">Monroe Kennedy III</a>,
      <a href="https://shurans.github.io/">Shuran Song</a><sup>†</sup>,
      <a href="https://tml.stanford.edu/people/karen-liu">C. Karen Liu</a><sup>†</sup>
      <br><small><sup>*</sup>Equal contribution, <sup>†</sup>Equal advising</small>
    </div>
    <div class="publication-venue">
      In submission
    </div>
    <div class="publication-links">
      <a href="https://locomotion-beyond-feet.github.io/">Project Page</a>
      <a href="https://arxiv.org/abs/2601.03607">Paper</a>
      <!-- <a href="/assets/papers/locomotion_beyond_feet.mp4">Video</a> -->
    </div>
  </div>
</div>

<div class="publication-card">
  <div class="publication-thumbnail">
    <img src="/assets/papers/tmtt_thumbnail.png" alt="MEMS Acoustic Wave Resonators">
  </div>
  <div class="publication-info">
    <div class="publication-title">Toward Intelligent Design and Measurement of MEMS Acoustic Wave Resonators</div>
    <div class="publication-authors">
      Xingyu Liu, Tianxi Meng, Shuhan Chen, <strong>Tae Hoon Yang</strong>, Jun Zhang, Songbin Gong, Yansong Yang
    </div>
    <div class="publication-venue">
      Manuscript in revision for IEEE Transactions on Microwave Theory and Techniques (TMTT)
    </div>
    <div class="publication-links">
      <a href="/assets/papers/TMTT_Xingyu_Final.pdf">PDF</a>
    </div>
  </div>
</div>

---

# Selected Projects

<style>
.project-card {
  display: flex;
  gap: 20px;
  margin-bottom: 30px;
  padding: 20px;
  background: #f8f9fa;
  border-radius: 8px;
  align-items: center;
}

.project-thumbnail {
  flex-shrink: 0;
  width: 250px;
}

.project-thumbnail img {
  width: 100%;
  height: auto;
  border-radius: 4px;
}

.project-info {
  flex-grow: 1;
}

.project-title {
  font-size: 1.1em;
  font-weight: bold;
  margin-bottom: 8px;
  color: #2c3e50;
}

.project-period {
  margin-bottom: 8px;
  color: #666;
  font-size: 0.9em;
}

.project-affiliation {
  margin-bottom: 12px;
  color: #555;
  font-size: 0.95em;
}

.project-description {
  margin-bottom: 12px;
}

.project-description ul {
  margin: 0;
  padding-left: 20px;
}

.project-description li {
  margin-bottom: 6px;
  color: #555;
}

.project-links {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  align-items: center;
}

.project-links a {
  padding: 6px 14px;
  background: #007bff;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  font-size: 0.9em;
  transition: background 0.3s;
}

.project-links a:hover {
  background: #0056b3;
}

</style>

<div class="project-card">
  <div class="project-thumbnail">
    <img src="/assets/projects/toddlerbot-2.0/keyframe.gif" alt="ToddlerBot 2.0">
  </div>
  <div class="project-info">
    <div class="project-title">ToddlerBot 2.0: Open-Source ML-Compatible Humanoid Platform</div>
    <div class="project-period">Dec 2024 - Aug 2025</div>
    <div class="project-affiliation">Robotics and Embodied Artificial Intelligence (REAL) Lab and The Movement Lab, Stanford University</div>
    <div class="project-description">
      <ul>
        <li>Contributed to the 2.0 release by improving the custom keyframe animation app and adding non-physics motion generating and RL training tools.</li>
        <li>Built an RL training pipeline with custom terrain generation, integrated stereo depth perception using the deep learning model FoundationStereo, and added RGB-D rendering and elevation mapping features in simulation.</li>
      </ul>
    </div>
    <div class="project-links">
      <a href="https://toddlerbot.github.io/">Website</a>
      <a href="https://github.com/hshi74/toddlerbot">Code</a>
      <a href="https://github.com/hshi74/toddlerbot/stargazers" style="background: none; padding: 0;">
        <img src="https://img.shields.io/github/stars/hshi74/toddlerbot?style=social" alt="Stars" style="height: 30px; display: block;">
      </a>
      <a href="https://github.com/hshi74/toddlerbot/network/members" style="background: none; padding: 0;">
        <img src="https://img.shields.io/github/forks/hshi74/toddlerbot?style=social" alt="Forks" style="height: 30px; display: block;">
      </a>
    </div>
  </div>
</div>

<div class="project-card">
  <div class="project-thumbnail">
    <img src="/assets/projects/go1-navigation/go1-thumbnail.gif" alt="Go1 Navigation">
  </div>
  <div class="project-info">
    <div class="project-title">Go1 DLIOM-Based Autonomous Navigation</div>
    <div class="project-period">Jan 2024 - Aug 2024</div>
    <div class="project-affiliation">Autonomy & Intelligence Lab, Northeastern University</div>
    <div class="project-description">
      <ul>
        <li>Configured Unitree Go1 robot dog with 3D LiDAR sensor fixture and ROS2 environment on robot's Jetson board.</li>
        <li>Integrated state estimation from DLIOM (Direct LiDAR Inertial Odometry and Mapping) with ROS2 Nav2.</li>
        <li>Achieved robot navigation with custom nodes that convert velocities from Nav2 to robot's high-level SDK commands.</li>
      </ul>
    </div>
    <div class="project-links">
      <a href="https://www.youtube.com/watch?v=rXD-VD9xano&t=1s">Video</a>
    </div>
  </div>
</div>

