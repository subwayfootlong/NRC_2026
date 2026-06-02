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