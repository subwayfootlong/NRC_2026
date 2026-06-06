# 🤖 Robotics & Coding Curriculum Logs

Welcome to the lesson tracking repository. This document serves as a rolling log of our hands-on robotics sessions, detailing the core concepts learned, engineering challenges faced, and the practical solutions explored by the students.

---

## 📊 Live Progress Tracker
We use a digital Kanban board to teach the students project management and task accountability. You can view our live progress, completed milestones, and upcoming competition tasks here:
👉 **[View Our Live Competition Progress Board](https://github.com/users/subwayfootlong/projects/3/views/1)**

---

## 📅 Lesson History & Milestones

| Lesson | Date | Core Topic | Key Takeaway / Hands-On Activity |
| :---: | :--- | :--- | :--- |
| **1** | 23 May 2026 | Drivetrain Movement & Gyroscopic Precision | Mastered straight-line driving using closed-loop sensor feedback. |
| **2** | 30 May 2026 | Manipulators & Object Interaction | Designed and prototyped unique, custom grabbing mechanisms. |
| **3** | 6 June 2026 | Playfield Familiarisation | Learnt about the rules, the game objects and stratergised a plan. |
| **4** |13 June 2026 | Vertical Lifting Mechanisms & Center of Gravity | Explored vertical motorized lifts (rack-and-pinion/arms) to stack blocks without tipping over. |
| **5** | 20 June 2026 | Color Sensing & Smart Sorting Logic | Programmed color sensors to scan blocks and create an internal memory buffer for sorting. |

---

## 🔍 Detailed Lesson Breakdowns

### 📘 Lesson 1: Movement & Its Challenges (23 May 2026)
In this foundational session, we transitioned from basic timing-based movements to sensor-driven navigation.

* **The Core Concept:** Understanding why physical robots rarely drive in a perfectly straight line (due to battery variance, wheel slippage, and surface friction).
* **The Technology:** Introduced the **Internal Gyroscope Sensor** to detect spatial orientation and heading drift.
* **Engineering Challenges:**
    * Overcoming the $+180^\circ$ to $-180^\circ$ sensor "boundary flip" when the robot spins fully around.
    * Implementing a Closed-Loop Controller to dynamically adjust motor speeds on the fly to correct steering errors.

---

### 📘 Lesson 2: Grabbing Field Objects & Mechanical Challenges (30 May 2026)
This session shifted focus from navigation to active field interaction, focusing heavily on mechanical design and prototyping.

* **The Core Concept:** How to translate rotational motor force into linear gripping force to securely capture mission objects.
* **The Activity (Design Thinking):** Instead of following a rigid step-by-step manual, every student acted as a lead engineer. Each student experimented, iterated, and built a **completely unique grabber attachment** customized to their ideas.
* **Engineering Challenges:**
    * **Gear Ratios & Torque:** Finding the sweet spot between a grabber that closes quickly versus one that clamps tightly enough without stalling the motor.
    * **Form Factor:** Adjusting mechanical reach and claw clearance to ensure objects wouldn't slip out during rapid chassis turns.

---

### 📘 Lesson 3: Playfield Familiarization & Strategy (6 June 2026)

This session shifts the focus to spatial awareness, rules interpretation, and strategic debate, encouraging students to critically analyze and defend their tactical approaches to the competitive field.

* **The Core Concept:** Evaluating risk versus reward through collaborative strategy formation, forcing students to weigh immediate, low-scoring certainties against high-yield, high-risk maneuvers.
* **Engineering & Tactical Challenges:**
* **The Efficiency vs. Risk Dilemma:** Navigating intense team debates on whether to prioritize a "safe and steady" approach (clearing simple tasks first for guaranteed, lower points) or an aggressive, high-risk strategy (tackling complex tasks immediately for a massive point advantage).
* **Task Sequencing & Concurrency:** Analyzing optimal pathing and workflow—deciding whether the robot should attempt tasks sequentially (one by one for precision) or concurrently (multitasking to maximize efficiency within the match time limit).

---

### 📘 Lesson 4: Vertical Lifting Mechanisms & Center of Gravity (13 June 2026)
This session expands on physical manipulators, moving away from simple grabbing to high-altitude stacking mechanisms necessary for this year's block task.

* **The Core Concept:** Converting rotational motor motion into vertical linear lift travel using structural components (such as rack-and-pinion tracks, dual-axis lift linkages, or scissor extensions).
* **Engineering Challenges:**
    * **Center of Gravity (CoG) Shift:** As the lift ascends with a payload, the robot’s center of mass shifts upward and forward. Students must design counterweights or balance the battery placement to prevent the robot from tipping over forward when stacking.
    * **Friction & Binding:** Managing linear alignment so the elevator tracks slide smoothly without catching under heavy block loads.

---

### 📘 Lesson 5: Color Sensing & Smart Sorting Logic (20 June 2026)
This session prepares the software base to handle the randomized target blocks on the game field.

* **The Core Concept:** Transitioning the robot from blind navigation to autonomous decision-making using active color scanning.
* **The Technology:** Leveraging the hardware **Color Sensor** to distinguish and record surface/block reflections.
* **Engineering Challenges:**
    * **Ambient Light Variance:** Calibrating sensor read reliability to handle differences in external overhead light settings.
    * **Memory Buffering Logic:** Programming the robot to temporarily hold an object in a physical "buffer zone" while evaluating its color array against an internal memory rule before driving to its matching drop stack location.
