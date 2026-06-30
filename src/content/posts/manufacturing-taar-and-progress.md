---
title: Manufacturing, TAAR, and Progress
date: 2026-06-29
excerpt: How the TAAR platform is evolving
image: /images/image3.png
tags: [news, taar]
---

In our last post, we briefly mentioned TAAR-2 and future plans. A lot, and we mean **_a lot_**, has changed since then.

## TAAR-2 to TAAR

As we developed TAAR-2, we decided to move the project and reform it into TAAR. The [TAAR repository](https://github.com/Hydle-Research-Group/taar) is our designated place for hosting design files, firmware, and instructions (so you can build your own TAAR arm!) 

We chose to do this because we believe constantly versioning each TAAR implementation gets messy. As the project evolves, so should the firmware, the design files, and instructions.

## Moving to magnets, not switches

TAAR-2 was originally designed with limit switches, which (can) get the job done. As a hard stop, they fail to provide any form of closed-loop control. Step skipping is popular with both stepper motors and robotic arms, so the choice to move to a [MT6701 Magnetic Encoder](https://uelectronics.com/wp-content/uploads/2026/01/AR4799-MT6701-Encoder-Magnetico-14-Bits-Datasheet.pdf?__cf_chl_f_tk=6K.5sfm5_uBChRikLZh.MG6gPWUJIJgPes7MU7Dt40c-1782786119-1.0.1.1-TiI0mUGzNmdjXh9lwqOFjU5YIIQ3e.lMNGfQbjv7yFE) on each axis was one we considered wise for TAAR. With closed-loop control, we can implement hardware loops similar to the psuedo-code below:

```rust
while current_position() != desired_angle {
    step_motor()
}
```

This ideology of "move until we reach this position" is much similar to limit switches, except magnetic encoders provide the _exact_ angle of the motor shaft. Not only do we increase precision, but we can also account for missed steps - something that previously would've held us back.

## Manufacturing Research

We are now applying research to manufacturing techniques and tool & die. In the near future, we plan to push the limits of both 3D-printing and milling.

TAAR is our primary introduction to manufacturing as a whole, as we are exploring ways to create our own custom 3D printed PCBs (more on that in a future post)
