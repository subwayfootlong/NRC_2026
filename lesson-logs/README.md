# 🤖 Robotics & Coding Curriculum Logs

Welcome to the lesson tracking repository. This document serves as a rolling log of our hands-on robotics sessions, detailing the core concepts learned, engineering challenges faced, and the practical solutions explored by the students.

---

## 📊 Live Progress Tracker

We use a digital Kanban board to teach the students project management and task accountability. You can view our live progress, completed milestones, and upcoming competition tasks here:
👉 **[View Our Live Competition Progress Board](https://github.com/users/subwayfootlong/projects/3/views/1)**

---

## 📅 Lesson History & Milestones

| Lesson | Date | Core Topic | Key Takeaway / Hands-On Activity |
| --- | --- | --- | --- |
| **1** | 23 May 2026 | Drivetrain Movement & Gyroscopic Precision | Mastered straight-line driving using closed-loop sensor feedback. |
| **2** | 30 May 2026 | Manipulators & Object Interaction | Designed and prototyped unique, custom grabbing mechanisms. |
| **3** | 6 June 2026 | Playfield Familiarisation | Learnt about the rules, the game objects and stratergised a plan. |
| **4** | 13 June 2026 | Vertical Lifting Mechanisms & CoG | Explored vertical motorized lifts to stack blocks without tipping. |
| **5** | 20 June 2026 | Color Sensing & Smart Sorting Logic | Programmed sensors to scan blocks and create an internal memory buffer. |
| **6** | 27 June 2026 | Hardware Iteration & Navigation | Refined chassis stability and implemented open-source navigation. |
| **7** | 4 July 2026 | Hardware Iteration | Attempted to resolve movement reliability by adjusting Center of Gravity (CoG) and wheel positioning. |
| **8** | 11 July 2026 | Hardware Iteration | Continued efforts to stabilize movement through further CoG and wheel configuration refinements. |
| **9** | 18 July 2026 | Coding Tasks | Starting to solve all the NRC Tasks. |

---

## 🔍 Detailed Lesson Breakdowns

### Lesson 1: Movement & Its Challenges (23 May 2026)

This session focused on transitioning from basic timing-based movements to sensor-driven navigation.

* **Concept learnt:** Understanding the physics of robot movement and the necessity of the **Internal Gyroscope Sensor** to detect spatial orientation and drift.
* **Challenges experienced:** Unable to reliably move straight. Need to find out otherways to improve reliable movement

### Lesson 2: Grabbing Field Objects & Mechanical Challenges (30 May 2026)

This session shifted focus from navigation to active field interaction, focusing on mechanical design and prototyping.

* **Concept learnt:** Translating rotational motor force into linear gripping force, with each student prototyping a unique grabber attachment.
* **Challenges experienced:** Not all grabbers are equal. Some can grab heavier while others can grab more.

### Lesson 3: Playfield Familiarization & Strategy (6 June 2026)

This session focused on spatial awareness, rules interpretation, and collaborative tactical planning.

* **Concept learnt:** Evaluating risk versus reward and weighing "safe and steady" scoring against high-yield, aggressive maneuvers.
* **Challenges experienced:** Navigating team debates.

### Lesson 4: Vertical Lifting Mechanisms & Center of Gravity (13 June 2026)

This session expanded on physical manipulators, focusing on high-altitude stacking mechanisms.

* **Concept learnt:** Converting rotational motor motion into vertical linear lift travel using rack-and-pinion or linkage systems.
* **Challenges experienced:** Managing the **Center of Gravity (CoG)** shift to prevent the robot from tipping forward when carrying payloads, and solving friction issues within the elevator tracks.

### Lesson 5: Color Sensing & Smart Sorting Logic (20 June 2026)

This session prepared the software base to handle randomized target blocks on the game field.

* **Concept learnt:** Transitioning the robot from blind navigation to autonomous decision-making using active color scanning.
* **Challenges experienced:** Calibrating sensor reliability against ambient light variance and building the logic to buffer objects in memory before sorting.

### Lesson 6: Hardware Iteration & Navigation Fundamentals (27 June 2026)

This session focused on refining the physical integrity of the chassis and mastering foundational software for autonomous movement.

* **Concept learnt:** Bridging the gap between structural stability—specifically regarding the robot's **Center of Gravity (CoG)**—and reliable software execution via open-source movement libraries.
* **Challenges experienced:** Auditing mechanical gear slippage caused by an unstable CoG and mapping hardware motor ports to match the requirements of the new navigation code.

### Lesson 7: Hardware Iteration (4 July 2026)

This session focused on troubleshooting persistent movement reliability issues.

* **Concept learnt:** The direct correlation between chassis weight distribution and driving base
* **Challenges experienced:** Identifying whether the instability stems from weight distribution or wheel grip, leading to initial adjustments in the robot's physical configuration.

### Lesson 8: Hardware Iteration (11 July 2026)

This session served as a continuation of the previous week

* **Concept learnt:** Methodical iteration of physical hardware components to isolate mechanical failure points.
* **Challenges experienced:** Continuing to refine wheel positions and shifting the Center of Gravity (CoG) to ensure the robot maintains predictable movement patterns during autonomous tasks.
* **Solved** Adding weight solves center of gravity problem

### Lesson 9: Coding Tasks (18 July 2026)

Trying to solve all the tasks for NRC 2026

* **Currently at** : Yellow Blocks
---

