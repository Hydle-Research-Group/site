---
title: How we're designing the TAAR platform 
date: 2026-06-11
excerpt: A quick update on the design of TAARABLE and future plans for TAAR-x models
image: /images/image2.png
tags: [update, design, taar, inside-look]
---

The TAAR platform is where our primary robotics research is happening, and the ecosystem is growing _fast_. From the prototype [TAAR-1 model](https://github.com/Hydle-Research-Group/taar1), we've spawned [TAARABLE](https://github.com/Hydle-Research-Group/taarable), a UART communication interface for the TAAR-x models.

## Rust-Based Firmware

TAAR is allowing us to explore the applications of Rust-based firmware in robotics, and the use of asynchronous runtimes like [Embassy](https://github.com/embassy-rs/embassy).

The advantages of using Rust in these projects are very apparent; memory safety, no need for a heavy RTOS, and of course, the Cargo build system. Development speed and memory safety are both equally important, and Rust fits this piece of the puzzle flawlessly.

Having an asynchronous model and runtime powered by Embassy makes most of the firmware design straightforward. Stepper motor control became a breeze with async functions, due to the fact stepper motor pulses require timing and hardware loops. 

## TAAR-1

TAAR-1 is the "launchpad" of the TAAR platform.

<figure>
  <img src="https://raw.githubusercontent.com/Hydle-Research-Group/media/main/taar/taar1/taar1-freecad-assembly.png" alt="TAAR-1 Assembly" />
  <figcaption>The TAAR-1 Assembly in FreeCAD</figcaption>
</figure>

Entirely 3D-printed, TAAR-1 leverages press-fit helical gears and Nema 17 stepper motors to achieve 2 Degrees of Freedom (2DOF) motion. The base has a 14:1 common ratio, with the arm at a ratio of about ~5:1. 
  
Mounted to the bottom of the arm is a standard limit switch used for performing a "homing" sequence on the arm. The base does not perform any sort of homing sequence, as it can achieve full 360° motion (although it is limited to 90° both directions in firmware)

<figure>
  <img src="https://raw.githubusercontent.com/Hydle-Research-Group/media/main/taar/taar1/taar1-homing.gif" alt="TAAR-1 Homing" />
  <figcaption>TAAR-1 performing a homing sequence</figcaption>
</figure>

TAAR-1 is powered by its own firmware running on an STM32G474RE-Nucleo. This board was chosen due to the amount of peripherals exposed, and the speed given by the 32-bit processor. Robotics generally requires a significant amount of motion calculation, and this board ensures we can perform stepper motor operations, inverse kinematics, and floating point division at high-speed.

## TAARABLE

TAARABLE is ultimately leading the TAAR project. Our idea is to use TAARABLE as the primary communication interface for the TAAR-x models. TAARABLE actually implements the [command standard](https://github.com/Hydle-Research-Group/taarable#command-standard) that TAAR-x models are intended to follow.

<figure>
  <img src="https://raw.githubusercontent.com/Hydle-Research-Group/media/main/taar/taarable/taarable-ui.png" alt="TAARABLE User Interface" />
  <figcaption>A screenshot of the TAARABLE user interface</figcaption>
</figure>

## In The Future: TAAR-2

Soon, we plan to build our next model, TAAR-2. TAAR-2 is designed to be a 4DOF arm, with a significantly larger reach. TAAR-2 will take inspiration from TAAR-1, with the same STM32 board variant and Nema 17 steppers being reused.
