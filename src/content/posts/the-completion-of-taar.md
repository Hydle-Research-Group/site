---
title: The completion of TAAR
date: 2026-08-19
excerpt: An analysis of TAAR + our first research paper
image: /images/image6.png
tags: [update, taar]
---

TAAR is in a completed state, with all of the design files, firmware, and research finalized!

We encourage you to read the full paper [here](https://github.com/Hydle-Research-Group/media/blob/main/papers/paper-1.pdf), but we've put together a quick overview of everything below.

<hr>

TAAR was designed as an experiment to see how far we could push a relatively simple collection of mechanical, electrical, and software components to create a functioning robotic arm. The completed system has four degrees of freedom: base rotation, shoulder rotation, elbow rotation, and hand rotation. Approximately 78% of the robot's mass is 3D-printed, with the remainder consisting primarily of motors, electronics, bearings, and other hardware. The project took roughly three months to complete, and was finished on August 13, 2026.

The mechanical structure uses PETG for its printed components. We compared three common FDM materials: PLA, PETG, and ASA. While PLA has a higher tensile strength, its brittle properties made it less suitable for moving components. ASA had a lower tensile strength and presented additional difficulties during the printing process. We ultimately chose PETG because it provided a useful compromise between strength and flexibility while remaining relatively easy to print.

The arm consists of a base and two 100 mm arm segments. NEMA-17 stepper motors provide the movement, while a 6:1 gear ratio gives the motors additional mechanical advantage. Each motor has a 1.8° step angle, allowing for relatively precise positioning, with microstepping available for finer control.

Electrically, the system runs from a 24 V DC power supply. We chose 24 V because of the voltage/current tradeoff: for a given amount of power, increasing the voltage reduces the current required. Lower current means less heat is produced by electrical resistance in the wiring and components.

An STM32G474RE NUCLEO board acts as the main controller, while TMC2209 stepper drivers control the motors. We also incorporated MT6701 magnetic encoders for rotational feedback. However, in the final design, only the hand joint retained an encoder rather than having one installed on every motor.

TAAR's embedded software was written in Rust, using the Embassy framework for hardware abstraction and asynchronous programming. Our firmware handles motor control, UART communication, command parsing, and feedback without relying on a traditional RTOS.

The robot receives GCODE commands over UART. A communicating computer can send commands to the arm, and TAAR responds with JSON messages indicating whether it should continue executing the sequence or stop because of an error. This gives TAAR a relatively simple interface for communicating with external software.

For motion control, our firmware generates pulses on the TMC2209 STEP inputs. The delay between each pulse determines the speed of the motor. When multiple joints need to move simultaneously, the firmware calculates different pulse delays for each motor so that the joints can begin and finish their movements together rather than simply moving each axis independently.

TAAR uses inverse kinematics to determine the motor angles required to place the end effector at a desired `(x, y, z)` position. The calculation uses trigonometry, triangle geometry, and the law of cosines to determine the base, shoulder, and elbow angles. The hand angle is then calculated so that it remains parallel to the ground.

We also implemented forward kinematics. This performs the reverse calculation: given the known joint angles, we can calculate where the end effector should be located in three-dimensional space. The arm's geometry is first reduced to a radial distance and vertical position, which are then projected onto the X and Y axes using the base rotation.

The entire system cost approximately $179.78 in listed components, with no predetermined budget.

The reported positional accuracy is approximately ±3 mm. This is an estimate rather than a rigorous measurement. We obtained this value by locking each motor axis, applying a small force to "bump" the arm, and measuring the resulting displacement with calipers. A more rigorous test would involve commanding the arm to known Cartesian coordinates and measuring its actual position with precision instruments.

Throughout development, we encountered several failed designs and had to make a number of changes. Our proposed 3D-printed PCB was abandoned because of the precision machining requirements and problems with copper foil. The original 150 mm arm length overloaded the stepper motors and introduced weight problems, so we reduced the arm length to 100 mm. Magnetic encoders on the geared joints produced angle-wrapping problems because of the 6:1 gear ratio, so we removed those encoders from the affected joints. Finally, the TMC2209 drivers experienced excessive heat, which led us to add larger heatsinks and impose movement-speed limits.

TAAR was never intended to replace industrial robotic arms. Its primary purpose is education and experimentation. Through TAAR, we were able to combine embedded firmware, motor control, machine-to-machine communication, geometry, trigonometry, and mechanical engineering into a single physical system.

More importantly, TAAR demonstrates how a seemingly complicated robotic system can be broken down into relatively understandable pieces: motors, gears, electronics, software, and mathematics. By combining these pieces, we can build a functional robotic system without requiring every individual component to be particularly complicated.

## DIY to Research Project

There were originally plans to develop TAAR into a project that anyone could build themselves. We ultimately decided to designate TAAR as a **research platform** instead. 

This decision is not permanent. With further development, TAAR could eventually become a desktop robotic arm that anyone can build. For now, however, we believe its value is greater as a platform for experimentation and research. The design files, firmware implementation, and research still remain publicly available inside the [TAAR repository](https://github.com/Hydle-Research-Group/taar).
