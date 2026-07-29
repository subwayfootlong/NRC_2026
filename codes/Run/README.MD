# Robot Control System & Mission Procedures README

A comprehensive markdown documentation explaining the functions, procedures, and mission routines for the LEGO SPIKE Prime NRC 2026 Run

---

## Table of Contents

* Core Setup & Initialization
* Spike Hub Button Triggers
* Sensor & Heading Functions
* Movement & Turning Functions
* Manipulator & Calibration Procedures
* Mission Routine Procedures (Task Blocks)

---

## Core Setup & Initialization

### initialise

* **Purpose:** Sets up the robot hardware environment when the program starts.
* **Actions:** Resets the hub's orientation, pairs drivetrain motors to ports A and E, configures braking behavior, defines control gain variables (KPYaw, KPTurn), initializes tracking variables to zero, and broadcasts the continuous heading background loop (`calculateYawTrue`).

---

## Spike Hub Button Triggers

### Program Start & Event Handlers

* **When Program Starts:** Automatically turns on a grid pattern on the hub screen and runs the `Init` block to prepare the robot.
* **When Right Button Pressed:** Turns on a specific custom icon on the hub display and triggers the main sequence (`DoTasks`).
* **When Left Button Pressed:** Turns on a check/cross icon pattern, broadcasts `Reset Grabber and Arm` to re-align the manipulator mechanisms, and plays an alert sound twice to confirm the reset action is running.

---

## Sensor & Heading Functions

### calculateYawTrue

* **Purpose:** Runs continuously in a background thread to prevent heading errors.
* **Actions:** Reads the raw gyroscope yaw value (-180 to 180 degrees), compares it to the previous frame, and updates a full rotation counter when crossing the boundary. This generates an unwrapped continuous heading (`yawTrue`) that can go beyond normal angular limits.

---

## Movement & Turning Functions

### YawDistance (speed, distance)

* **Purpose:** Drives the robot straight using proportional gyro correction.
* **Actions:** Scales target distance into encoder degrees, monitors motor movement, and continuously adjusts left and right motor speeds based on heading error to prevent drifting off a straight path.

### TurnYaw (angle)

* **Purpose:** Executes precise point turns on the robot's center axis.
* **Actions:** Rotates the robot toward a target angle using proportional control, automatically slowing down as it approaches the destination to prevent overshooting.

### ZeroYaw

* **Purpose:** Utility routine to reset the robot's alignment against a wall or barrier.
* **Actions:** Drives briefly forward into a physical barrier, stops to settle, and resets internal tracking and gyro variables to absolute zero.

---

## Manipulator & Calibration Procedures

### Calibrate Grabber / Grabber Close / Grabber Open

* **Purpose:** Controls the mechanism used to grab objects.
* **Actions:**
* `Calibrate Grabber`: Uses a stall-detection routine to find the physical limit of the grabber mechanism and zeroes its motor position.
* `Grabber Close`: Moves the grabber motor to a closed position to secure an object.
* `Grabber Open`: Opens the grabber to release an item.



### Calibrate Lift / Lift Up / Lift Down

* **Purpose:** Controls the vertical arm mechanism.
* **Actions:**
* `Calibrate Lift`: Stalls the motor against its lower physical boundary to establish a baseline reference position.
* `Lift Up`: Raises the arm mechanism to an upper score/travel height.
* `Lift Down`: Lowers the arm back to the baseline position.



---

## Mission Routine Procedures (Task Blocks)

### DoTasks

* **Purpose:** High-level master sequence wrapper that triggers specific mission scripts.
* **Actions:** Calls individual color-coded task routines sequentially.

### DoBlocksYellow

* **Purpose:** Autonomous sequence for handling yellow items.
* **Actions:** Uses structured distance drives (`YawDistance`), closes the grabber, raises the lift arm, drives to position, and opens the grabber.

### DoBlocksBlue

* **Purpose:** Autonomous sequence for handling blue items.
* **Actions:** Sets movement speed, navigates through a series of precise straight drives and point turns (`TurnYaw`), operates the lift and grabber mechanisms, and resets orientation using `ZeroYaw`.

### DoBlocksPink

* **Purpose:** Autonomous sequence for handling pink items.
* **Actions:** Combines straight-line movements, directional turns, and manipulator actions to complete the pink block challenge sequence.