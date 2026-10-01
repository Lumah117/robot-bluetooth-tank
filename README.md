# Arduino Bluetooth-Controlled Robot Tank

An Arduino-based tracked robot project extending an existing **Keyestudio Mini Tank Robot** platform with **Bluetooth smartphone control**.

The original robot platform included autonomous obstacle-avoidance and light-tracking functionality. My contribution was to extend the existing Arduino software so that the tank could also receive commands over a Bluetooth connection and be **manually driven using a smartphone**.

This project provided practical experience working with an existing embedded codebase, integrating an additional communication interface and translating wireless user commands into physical robot movement.

---

## Project Overview

The original Keyestudio platform provided several autonomous behaviours:

```text
Keyestudio Mini Tank
        │
        ├── Obstacle Avoidance
        │      └── Ultrasonic Sensing
        │
        ├── Light Tracking
        │      └── Photoresistor Sensors
        │
        ├── Motor Control
        │
        └── LED Matrix
        │
        ▼
Existing Robotic Platform
```

I extended the system by adding a Bluetooth-controlled operating mode:

```text
             Smartphone
                 │
                 ▼
        Bluetooth Connection
                 │
                 ▼
         Arduino Controller
                 │
          Interpret Command
                 │
        ┌────────┼────────┐
        │        │        │
        ▼        ▼        ▼
      Forward   Turn    Reverse
        │        │        │
        └────────┼────────┘
                 │
                 ▼
            Motor Control
                 │
                 ▼
             Robot Tank
```

This allowed the robot to be manually controlled wirelessly from an old smartphone while retaining the functionality of the original robotic platform.

---

## My Contribution

The distinction between the original platform and my work is important.

### Existing Keyestudio Functionality

The starting code/platform already included functionality such as:

- Ultrasonic obstacle detection
- Autonomous obstacle avoidance
- Servo-based ultrasonic scanning
- Light tracking
- Motor control
- LED matrix functionality
- Existing autonomous behaviours

### My Extension

My work extended this existing system to add:

- Bluetooth communication
- Wireless command reception
- Smartphone-based robot control
- Interpretation of incoming control commands
- Manual forward and reverse control
- Manual left and right steering
- Remote stopping
- Integration of manual control with the existing robot software

The objective was therefore not to replace the original autonomous functionality, but to **extend the robot with an additional human-controlled operating capability**.

---

# System Architecture

The completed system can be viewed as three layers:

```text
┌─────────────────────────────┐
│       USER INTERFACE        │
│                             │
│         Smartphone          │
└──────────────┬──────────────┘
               │
               │ Bluetooth
               ▼
┌─────────────────────────────┐
│       ROBOT CONTROL         │
│                             │
│           Arduino           │
│                             │
│   Receive → Decode → Act    │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│       ROBOT HARDWARE        │
│                             │
│ Motors │ Sensors │ Display  │
└─────────────────────────────┘
```

The Bluetooth interface provides a communication link between the user and the embedded robot controller.

---

# Bluetooth Control

The main extension to the original project was the ability to receive control instructions wirelessly.

Conceptually:

```text
Smartphone
    │
    ▼
User Selects Command
    │
    ▼
Bluetooth Transmission
    │
    ▼
Arduino Receives Command
    │
    ▼
Command Interpretation
    │
    ▼
Robot Behaviour
```

The received command determines the required movement of the robot.

Typical commands correspond to:

```text
        FORWARD
           ▲
           │
           │
LEFT ◄─── STOP ───► RIGHT
           │
           │
           ▼
        REVERSE
```

This turns the smartphone into a simple wireless remote controller for the tank.

---

# Differential Drive Control

The tracked robot uses independent left and right motor control.

By changing the direction of each side, several movement behaviours can be produced.

### Forward

```text
Left Motor  → Forward
Right Motor → Forward

       ↑
     ROBOT
```

### Reverse

```text
Left Motor  → Reverse
Right Motor → Reverse

     ROBOT
       ↓
```

### Left Turn

```text
Left Side   ←
Right Side  →

      ↺
    ROBOT
```

### Right Turn

```text
Left Side   →
Right Side  ←

    ROBOT
      ↻
```

This allows Bluetooth commands to be translated directly into physical movement.

---

# Existing Autonomous Obstacle Avoidance

The underlying Keyestudio platform includes ultrasonic obstacle-avoidance behaviour.

Conceptually:

```text
Ultrasonic Sensor
        │
        ▼
Measure Distance
        │
        ▼
Obstacle Detected?
      /       \
    No         Yes
    │           │
    ▼           ▼
Continue       Scan
Driving      Environment
                │
                ▼
           Select Turn
                │
                ▼
          Avoid Obstacle
```

This functionality formed part of the existing platform rather than the Bluetooth extension developed for this project.

---

# Existing Light Tracking

The platform also includes light-sensitive sensors that can be used to influence robot movement.

```text
Left Light Sensor      Right Light Sensor
        │                     │
        └──────────┬──────────┘
                   │
                   ▼
           Compare Readings
                   │
             ┌─────┴─────┐
             ▼           ▼
          Steer         Steer
           Left          Right
```

This allows the robot to react to differences in illumination.

Again, this behaviour was part of the original Keyestudio implementation and is retained here as part of the complete robot software.

---

# Multiple Robot Behaviours

The resulting software contains several forms of robot control:

```text
                    ROBOT
                      │
       ┌──────────────┼──────────────┐
       │              │              │
       ▼              ▼              ▼
   Bluetooth       Obstacle        Light
    Control        Avoidance      Tracking
       │              │              │
       ▼              ▼              ▼
    Manual        Autonomous     Autonomous
 Teleoperation     Navigation     Response
```

The Bluetooth extension therefore adds a **human-in-the-loop teleoperation capability** alongside the platform's existing autonomous behaviours.

---

# LED Matrix

The robot also includes an LED matrix used by the existing software to display status and directional patterns.

This provides a simple visual indication of robot behaviour.

```text
Robot Command
      │
      ├── Forward
      ├── Reverse
      ├── Left
      ├── Right
      └── Stop
      │
      ▼
 LED Matrix Pattern
```

This is useful for visually confirming the command or behaviour being executed by the robot.

---

# Repository Structure

```text
arduino-bluetooth-robot-tank/
│
├── README.md
├── LICENSE
│
├── src/
│   └── bluetooth_robot_tank.ino
│
└── media/
    └── ...
```

Photographs of the physical robot and smartphone control setup can be stored in the `media` directory.

---

# Source Code Attribution

This project was developed using the **Keyestudio Mini Tank Robot** and its existing example software as a starting point.

The original software contained functionality for the robot hardware and autonomous behaviours, including obstacle avoidance and light tracking.

My contribution was the extension of this existing system to provide **Bluetooth-based smartphone teleoperation**.

Original Keyestudio attribution present in the source code has intentionally been retained.

This repository is therefore intended to demonstrate my experience in:

```text
Existing Embedded System
          │
          ▼
Understand Existing Code
          │
          ▼
Identify Integration Point
          │
          ▼
Implement New Functionality
          │
          ▼
Integrate Bluetooth Control
          │
          ▼
Test on Physical Hardware
```

rather than claiming authorship of the complete manufacturer-provided software.

---

# Engineering Skills Demonstrated

Although relatively small, this project provided practical experience with several useful engineering concepts.

### Embedded Programming

- Arduino
- C/C++
- Digital I/O
- PWM motor control
- Sensor integration
- Servo control

### Robotics

- Differential-drive control
- Mobile robotics
- Robot teleoperation
- Autonomous behaviours
- Sensor-actuator integration

### Communications

- Bluetooth communication
- Wireless command reception
- Command interpretation
- Smartphone-to-microcontroller communication

### Software Engineering

- Understanding an existing codebase
- Extending existing functionality
- Integrating new behaviour without replacing existing features
- Hardware/software debugging
- Testing software on physical hardware

---

# Why This Project Was Useful

One of the useful aspects of this project was that it involved **modifying an existing robotic system rather than starting with an empty program**.

In practical engineering, software frequently needs to be integrated into systems containing existing hardware, libraries and code.

The development process therefore involved:

```text
Existing System
      │
      ▼
Understand Architecture
      │
      ▼
Understand Existing Behaviour
      │
      ▼
Add New Requirement
      │
      ▼
Bluetooth Interface
      │
      ▼
Integrate With Motor Control
      │
      ▼
Physical Testing
```

This provided early experience with the type of software integration work commonly required in larger robotics projects.

---

# Known Limitations

The Bluetooth control implementation is relatively simple and was developed as an educational robotics project.

Potential limitations include:

- Simple command protocol
- No command acknowledgement
- No communication-loss failsafe
- No authentication
- No telemetry feedback to the smartphone
- No closed-loop position control
- No wheel odometry
- No localisation
- No mapping
- No remote sensor visualisation

The smartphone acts primarily as a **command transmitter**, rather than a complete bidirectional robot-control interface.

---

# How I Would Extend It Today

A more advanced version could implement a structured communication protocol between the robot and remote controller.

```text
              Smartphone
                  │
          Command + Telemetry
                  │
          ┌───────┴───────┐
          ▼               ▲
       Commands         Status
          │               │
          ▼               │
             Robot
          Controller
              │
      ┌───────┼────────┐
      ▼       ▼        ▼
    Motors  Sensors   Battery
```

Potential improvements could include:

- Bidirectional communication
- Connection-loss detection
- Emergency stop behaviour
- Adjustable motor speed
- Live ultrasonic-distance telemetry
- Battery monitoring
- Robot status feedback
- Mode selection from the smartphone
- Wi-Fi control
- ESP32 integration
- ROS / ROS2 integration

---

# From Teleoperation to Autonomy

The project also demonstrates an important distinction in robotics.

### Teleoperation

```text
Human
  │
  ▼
Smartphone
  │
  ▼
Bluetooth
  │
  ▼
Robot
```

The human makes the navigation decisions.

### Autonomous Control

```text
Sensors
  │
  ▼
Robot Controller
  │
  ▼
Decision
  │
  ▼
Robot
```

The robot makes decisions from sensor information.

The platform contains examples of both approaches, making it a useful early experiment in different forms of mobile-robot control.

---

# Portfolio Context

This project represents another stage in my progression through embedded systems and robotics:

```text
Basic Arduino Programming
          │
          ▼
Sensors & Actuators
          │
          ▼
Autonomous Obstacle Avoidance
          │
          ▼
Existing Robotic Platform
          │
          ▼
Software Extension
          │
          ▼
Bluetooth Communication
          │
          ▼
Smartphone Teleoperation
          │
          ▼
More Advanced Robotic Systems
```

The main value of the project was not the complexity of the individual control commands, but the experience of taking an **existing physical robotic platform and embedded software stack and extending it with new functionality**.

It provided practical experience in embedded software modification, wireless communication, mobile robot control and hardware/software integration.
