# Gyroscope-Assisted Closed-Loop Robot Control System

A comprehensive, robust proportional control system (P-Controller) designed for LEGO SPIKE Prime / MINDSTORMS platforms. This codebase implements continuous heading calculation to overcome hardware layout limitations, proportional straight-line driving calibration, auto-slowing point turns, and wall-squaring reset functions.

---

## 📋 Table of Contents
- [System Variables](#-system-variables)
- [Mathematical Formulas & Control Theory](#-mathematical-formulas--control-theory)
  - [1. Continuous Heading Tracking (`yawTrue`)](#1-continuous-heading-tracking-yawtrue)
  - [2. Distance Scaling & Progress Tracking](#2-distance-scaling--progress-tracking)
  - [3. Proportional Straight-Line Drive (`moveGyro`)](#3-proportional-straight-line-drive-movegyro)
  - [4. Proportional Point Turns (`turnGyro`)](#4-proportional-point-turns-turngyro)
- [Core Procedure Documentation](#-core-procedure-documentation)
  - [`initialise`](#initialise)
  - [`calculateYawTrue`](#calculateyawtrue)
  - [`moveGyro(speed, distance)`](#movegyrospeed-distance)
  - [`turnGyro(angle)`](#turngyroangle)
  - [`resetGyroForward` / `resetGyroReverse`](#resetgyroforward--resetgyroreverse)
- [Execution Order](#-execution-order)

---

## 🎛️ System Variables

| Variable Name | Initial Value | Type / Units | Description |
| :--- | :---: | :---: | :--- |
| `KPYaw` | `5` | Constant (Gain) | Multiplier for straight driving. Determines how aggressively the robot corrects back onto its path when drifting. |
| `KPTurn` | `1.5` | Constant (Gain) | Multiplier for point turns. Controls deceleration based on the robot's remaining angular distance to the target. |
| `yaw` | `0` | Degrees ($-180^\circ$ to $180^\circ$) | The raw, unedited direction directly reading from the hub's internal gyroscope. |
| `yawOld` | `0` | Degrees ($-180^\circ$ to $180^\circ$) | Stores the previous frame's `yaw` angle to check for boundary crossovers. |
| `counter` | `0` | Integer | Tracks full $360^\circ$ rotations. Increments clockwise ($+1$) and decrements counter-clockwise ($-1$). |
| `yawTrue` | `0` | Degrees ($-\infty$ to $+\infty$) | Calculated unwrapped continuous direction. Prevents math breaking at bounds. |
| `yawTarget` | `0` | Degrees ($-\infty$ to $+\infty$) | The ideal target orientation the robot aims to achieve or maintain. |
| `yawError` | Dynamic | Degrees | The direct algebraic variance between the actual heading and the intended path. |
| `PID` / `Pturn` | Dynamic | Power Offset | Computed output used to modify raw electrical motor distributions. |

---

## 🧮 Mathematical Formulas & Control Theory

### 1. Continuous Heading Tracking (`yawTrue`)
Standard internal hub gyroscopes clip absolute headings at a strict boundary condition ($[-180^\circ, 180^\circ]$). Spinning across this boundary causes a step discontinuity ($180^\circ 	o -180^\circ$), causing naive arithmetic logic to flip directions erratically.

To unwrap this, a frame-by-frame change ($\Delta	heta$) is calculated:
$$\Delta	heta = 	ext{yaw} - 	ext{yawOld}$$

* **Clockwise Crossing Check:** If $\Delta	heta < -180^\circ$, the system detects a clockwise rollover across the boundary line.
  $$	ext{counter} = 	ext{counter} + 1$$
* **Counter-Clockwise Crossing Check:** If $\Delta	heta > 180^\circ$, the system detects a counter-clockwise rollover.
  $$	ext{counter} = 	ext{counter} - 1$$

The total continuous structural angle is evaluated as:
$$	ext{yawTrue} = 	ext{yaw} + (360 	imes 	ext{counter})$$

### 2. Distance Scaling & Progress Tracking
Motors measure distances internally via rotational degrees rather than spatial coordinates like centimeters. A constant scaling metric of $19.5$ coordinates requested distances into target target values:
$$	ext{Target Degrees } (D) = |	ext{distance} 	imes 19.5|$$

The real-time absolute progress tracked through the encoder cycle is evaluated using the baseline marker position ($	ext{motorPosB}$) established right before the loop:
$$	ext{degreesTraveled} = |	ext{Current Motor B Position} - 	ext{motorPosB}|$$

The control sequence condition terminates once:
$$	ext{degreesTraveled} > D$$

### 3. Proportional Straight-Line Drive (`moveGyro`)
The routine acts as a **Proportional (P)** controller loop. The heading error $	heta_e$ is calculated dynamically:
$$	heta_e = 	ext{yawTrue} - 	ext{yawTarget}$$

The corrective output profile $u(t)$ is directly proportional to this current systemic error:
$$PID = 	ext{KPYaw} 	imes 	heta_e$$

This offset value dynamically shifts energy balancing between the left and right drive lines (assuming Port A handles Left, Port B handles Right):

#### **Forward Travel Mode ($	ext{distance} > 0$):**
$$M_{	ext{left}} = 	ext{speed} - PID$$
$$M_{	ext{right}} = 	ext{speed} + PID$$

#### **Backward Travel Mode ($	ext{distance} \le 0$):**
To ensure correcting turns steer into the proper direction when driving backwards, structural power distributions are mathematically inverted:
$$M_{	ext{left}} = -1 	imes (	ext{speed} - PID)$$
$$M_{	ext{right}} = -1 	imes (	ext{speed} + PID)$$

### 4. Proportional Point Turns (`turnGyro`)
Unlike driving straight, structural pivots use targeted scaling factors to decelerate smoothly as the robot completes its rotation, preventing target overshoot.

The targeted systemic error matches standard processing:
$$	heta_e = 	ext{yawTrue} - 	ext{yawTarget}$$

The dynamic motor power profiles computed across structural paths vary depending on the angle configuration:

#### **Clockwise Rotations ($	ext{angle} > 0$):**
The loop runs continuously until the error condition clears ($	heta_e > -0.5^\circ$).
$$P_{	ext{turn}} = (	ext{KPTurn} 	imes 	heta_e) - 1$$
$$M_{	ext{left}} = -P_{	ext{turn}}, \quad M_{	ext{right}} = P_{	ext{turn}}$$

#### **Counter-Clockwise Rotations ($	ext{angle} \le 0$):**
The loop runs continuously until the error condition clears ($	heta_e < 0.5^\circ$).
$$P_{	ext{turn}} = (	ext{KPTurn} 	imes 	heta_e) + 1$$
$$M_{	ext{left}} = -P_{	ext{turn}}, \quad M_{	ext{right}} = P_{	ext{turn}}$$

*Note: The hardcoded $\pm 1$ offset acts as a minimum base voltage boost to overcome physical motor stall torque/friction when the error value drops near zero.*

---

## 📂 Core Procedure Documentation

### `initialise`
Resets the hardware hub's integrated gyro orientation, pairs up the physical drivetrain channels to ports **A** and **B**, locks default coasting configurations to explicit brake methods (`stop: "1"`), establishes default controller gain thresholds, and activates background telemetry tracking by broadcasting `calculateYawTrue`.

### `calculateYawTrue`
An execution thread intended to run inside a `forever` loop structure. It analyzes positional deltas between polling samples to unwrap raw tracking boundaries. This updates the absolute global measurement space (`yawTrue`) smoothly without risk of calculations breaking during multi-spin tasks.

### `moveGyro(speed, distance)`
Executes straight-line movements using closed-loop correction feedback. It monitors physical travel using motor wheel encoders. If structural drift occurs due to wheel slippage, floor friction variance, or physical collisions, the feedback algorithm recalculates internal power balancing to restore original alignments instantly.

### `turnGyro(angle)`
Executes point turns on the robot's physical center axis. It increases or decreases `yawTarget` by the requested `angle`. The code tracks the remaining angular delta and applies proportional deceleration, smoothing out approach tracking and minimizing physical chassis momentum overshooting.

### `resetGyroForward` / `resetGyroReverse`
Alignment utility macros used to square the robot's chassis flush against a rigid field wall. The robot drives deliberately into a barrier for a brief window ($0.3$ rotations), stops to settle mechanical oscillations, resets the internal sensor plane to absolute zero ($0^\circ$), and clears out all global rotation tracking counters.

---

## 🔄 Execution Flow Diagram

```
[Program Starts] 
       │
       ▼
 [initialise] ──► Sets Gain Values & Resets Hardware Hub
       │
       ├─► [Broadcast: calculateYawTrue] ──► (Runs infinitely in background)
       │                                       Tracks continuous unwrapped angle
       ▼
 [Main Mission Steps]
       │
       ├─► moveGyro(speed: 50, distance: 10) ──► Uses KPYaw to track straight line
       │
       └─► turnGyro(angle: 90)              ──► Uses KPTurn to decelerate into turn
```