# Gyroscope-Assisted Closed-Loop Robot Control System

A comprehensive, robust proportional control system (P-Controller) designed for LEGO SPIKE Prime / MINDSTORMS platforms. This codebase implements continuous heading calculation to overcome hardware layout limitations, proportional straight-line driving calibration, auto-slowing point turns, and wall-squaring reset functions.

---

## 📋 Table of Contents

* [System Variables](https://www.google.com/search?q=%23-system-variables)
* [Mathematical Formulas & Control Theory](https://www.google.com/search?q=%23-mathematical-formulas--control-theory)
* [1. Continuous Heading Tracking (`yawTrue`)](https://www.google.com/search?q=%231-continuous-heading-tracking-yawtrue)
* [2. Distance Scaling & Progress Tracking](https://www.google.com/search?q=%232-distance-scaling--progress-tracking)
* [3. Proportional Straight-Line Drive (`moveGyro`)](https://www.google.com/search?q=%233-proportional-straight-line-drive-movegyro)
* [4. Proportional Point Turns (`turnGyro`)](https://www.google.com/search?q=%234-proportional-point-turns-turngyro)


* [Core Procedure Documentation](https://www.google.com/search?q=%23-core-procedure-documentation)
* [`initialise`](https://www.google.com/search?q=%23initialise)
* [`calculateYawTrue`](https://www.google.com/search?q=%23calculateyawtrue)
* [`moveGyro(speed, distance)`](https://www.google.com/search?q=%23movegyrospeed-distance)
* [`turnGyro(angle)`](https://www.google.com/search?q=%23turngyroangle)
* [`resetGyroForward` / `resetGyroReverse](https://www.google.com/search?q=%23resetgyroforward--resetgyroreverse)`


* [Execution Order](https://www.google.com/search?q=%23-execution-order)

---

## 🎛️ System Variables

| Variable Name | Initial Value | Type / Units | Description |
| --- | --- | --- | --- |
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

Standard internal hub gyroscopes clip absolute headings at a strict boundary condition ($[-180^\circ, 180^\circ]$). Spinning across this boundary causes a step discontinuity ($180^\circ \to -180^\circ$), causing naive arithmetic logic to flip directions erratically.

To unwrap this, a frame-by-frame change ($\Delta\theta$) is calculated:


$$\Delta\theta = \text{yaw} - \text{yawOld}$$

* **Clockwise Crossing Check:** If $\Delta\theta < -180^\circ$, the system detects a clockwise rollover across the boundary line.

$$\text{counter} = \text{counter} + 1$$


* **Counter-Clockwise Crossing Check:** If $\Delta\theta > 180^\circ$, the system detects a counter-clockwise rollover.

$$\text{counter} = \text{counter} - 1$$



The total continuous structural angle is evaluated as:


$$\text{yawTrue} = \text{yaw} + (360 \times \text{counter})$$

### 2. Distance Scaling & Progress Tracking

Motors measure distances internally via rotational degrees rather than spatial coordinates like centimeters. A constant scaling metric of $19.5$ coordinates requested distances into target values:


$$\text{Target Degrees } (D) = \vert{}\text{distance} \times 19.5\vert{}$$

The real-time absolute progress tracked through the encoder cycle is evaluated using the baseline marker position ($\text{motorPosB}$) established right before the loop:


$$\text{degreesTraveled} = \vert{}\text{Current Motor B Position} - \text{motorPosB}\vert{}$$

The control sequence condition terminates once:


$$\text{degreesTraveled} > D$$

### 3. Proportional Straight-Line Drive (`moveGyro`)

The routine acts as a **Proportional (P)** controller loop. The heading error $\theta_e$ is calculated dynamically:


$$\theta_e = \text{yawTrue} - \text{yawTarget}$$

The corrective output profile is directly proportional to this current systemic error:


$$\text{PID} = \text{KPYaw} \times \theta_e$$

This offset value dynamically shifts energy balancing between the left and right drive lines (assuming Port A handles Left, Port B handles Right):

#### **Forward Travel Mode ($\text{distance} > 0$):**

$$M_{\text{left}} = \text{speed} - \text{PID}$$

$$M_{\text{right}} = \text{speed} + \text{PID}$$

#### **Backward Travel Mode ($\text{distance} \le 0$):**

To ensure correcting turns steer into the proper direction when driving backwards, structural power distributions are mathematically inverted:


$$M_{\text{left}} = -1 \times (\text{speed} - \text{PID})$$

$$M_{\text{right}} = -1 \times (\text{speed} + \text{PID})$$

### 4. Proportional Point Turns (`turnGyro`)

Unlike driving straight, structural pivots use targeted scaling factors to decelerate smoothly as the robot completes its rotation, preventing target overshoot.

The targeted systemic error matches standard processing:


$$\theta_e = \text{yawTrue} - \text{yawTarget}$$

The dynamic motor power profiles computed across structural paths vary depending on the angle configuration:

#### **Clockwise Rotations ($\text{angle} > 0$):**

The loop runs continuously until the error condition clears ($\theta_e > -0.5^\circ$).


$$P_{\text{turn}} = (\text{KPTurn} \times \theta_e) - 1$$

$$M_{\text{left}} = -P_{\text{turn}}, \quad M_{\text{right}} = P_{\text{turn}}$$

#### **Counter-Clockwise Rotations ($\text{angle} \le 0$):**

The loop runs continuously until the error condition clears ($\theta_e < 0.5^\circ$).


$$P_{\text{turn}} = (\text{KPTurn} \times \theta_e) + 1$$

$$M_{\text{left}} = -P_{\text{turn}}, \quad M_{\text{right}} = P_{\text{turn}}$$

*Note: The hardcoded  pm 1  offset acts as a minimum base voltage boost to overcome physical motor stall torque/friction when the error value drops near zero.*

---

## 📂 Core Procedure Documentation

### `Init`

Resets the hardware hub's integrated gyro orientation, pairs up the physical drivetrain channels to ports **A** and **B**, locks default coasting configurations to explicit brake methods (`stop: "1"`), establishes default controller gain thresholds, and activates background telemetry tracking by broadcasting `calculateYawTrue`.

### `YawTrue`

An execution thread intended to run inside a `forever` loop structure. It analyzes positional deltas between polling samples to unwrap raw tracking boundaries. This updates the absolute global measurement space (`yawTrue`) smoothly without risk of calculations breaking during multi-spin tasks.

### `YawDistance(speed, distance)`

Executes straight-line movements using closed-loop correction feedback. It monitors physical travel using motor wheel encoders. If structural drift occurs due to wheel slippage, floor friction variance, or physical collisions, the feedback algorithm recalculates internal power balancing to restore original alignments instantly.

### `TurnYaw(angle)`

Executes point turns on the robot's physical center axis. It increases or decreases `yawTarget` by the requested `angle`. The code tracks the remaining angular delta and applies proportional deceleration, smoothing out approach tracking and minimizing physical chassis momentum overshooting.

### `ZeroYaw`

Alignment utility macros used to square the robot's chassis flush against a rigid field wall. The robot drives deliberately into a barrier for a brief window ($0.3$ rotations), stops to settle mechanical oscillations, resets the internal sensor plane to absolute zero ($0^\circ$), and clears out all global rotation tracking counters.

---

## 🔄 Execution Flow Diagram

```text
[Program Starts] 
        │
        ▼
[initialise] ──► Sets Gain Values & Resets Hardware Hub
        │
        ├─► [Broadcast: calculateYawTrue] ──► (Runs infinitely in background)
        │                                      Tracks continuous unwrapped angle
        ▼
[Main Mission Steps]
        │
        ├─► moveGyro(speed: 50, distance: 10) ──► Uses KPYaw to track straight line
        │
        └─► turnGyro(angle: 90)              ──► Uses KPTurn to decelerate into turn

```
