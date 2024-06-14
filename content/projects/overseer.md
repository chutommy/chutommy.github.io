---
title: "Digital surveillance in the 21st Century"
description: "Project Overseer - The George Orwell's crusade"
# date: "2019-04-11T16:22:02+02:00"
weight: 50

hideMeta: false
showPostNavLinks: false
showReadingTime: false

showToc: true
tocOpen: false
disableHLJS: false
disableAnchoredHeadings: true

cover:
  image: "/projects/overseer/cover2.png"
---

Project Overseer is a year-long initiative honoring [George Orwell](https://en.wikipedia.org/wiki/George_Orwell) and his warning about the dangers of surveillance. Inspired by his book [1984](https://en.wikipedia.org/wiki/Nineteen_Eighty-Four), which describes a world under constant government watch, the project Overseer aims to highlight the threat of surveillance through a thought-provoking artwork.

{{< figure src="/projects/overseer/processed/06.png" caption="Conceptual design drafts for the enclosure" >}}

<!-- {{< figure src="/projects/overseer/processed/09.png" >}} -->

In this project, we create a robot that tracks faces in real-time. To bring this to life, we needed software capable of efficiently planning and controlling the robot's arm. This required several key functionalities.

### Software driver

The program had to perform the following tasks:

1. Capture a frame from the camera.
2. Identify all faces within the frame.
3. Select the closest face.
4. Update the arm's position accordingly.

### Setbacks

One major challenge was determining the precise rotation needed for the servo motors based on the target's coordinates. Initially, the software could only calculate the degrees of camera movement, which was insufficient for precise motor control.

{{< figure src="/projects/overseer/processed/01.png" caption="Initial sketches of servo-motor motions and arm movements, angle calculations" >}}

This problem required synchronizing a triangle formed by the camera, the target, and the movable joint. With only one angle and one length, it was geometrically impossible to make accurate calculations.

To overcome this, we aligned the motor's axis with the camera's rotating axis, ensuring both intersected precisely at the camera's lens. While this alignment resolved the calculation issue, it added complexity to the device's construction.

{{< figure src="/projects/overseer/processed/02.png" caption="Initial drafts of the casing design" >}}

As the project progressed, we added features like target tolerance adjustments, drawing rectangles around detected targets, and automated arm centering before and after periods of inactivity.

{{< figure src="/projects/overseer/processed/03.png" caption="Final concept - dimension calculations and assembly planning" >}}

### CPU compatibility

Everything was initially planned to run on a [Raspberry Pi Zero W](https://www.raspberrypi.com/products/raspberry-pi-zero-w/), which was supposed to fit inside the same enclosure as the camera. However, we decided to use [OpenCV4](https://opencv.org/) technology (v4.3.0) for facial recognition, which requires ARMv7+ processors. Therefore, we switched to the [Raspberry Pi 3B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/) with an ARMv8 CPU.

This choice had a downside since the Raspberry Pi 3B+ is much larger. We had to add another enclosure to the rear of the assembly, which also meant abandoning the original idea of mounting the camera as an arm on a board that could be hung on the wall.

## Assembling

{{< figure src="/projects/overseer/processed/04.png" caption="Hand-cutting particleboard and cardboard for arms and casing" >}}

To build the Overseer robots, we used lightweight particleboard for the frames to prioritize fast algorithms for smooth operation. However, the wood proved weaker than expected, so we reinforced some parts.

For casings, we opted for cardboard to cover electrical components and thin, sturdy laminated boards for technical enclosures. We chose a subtle color scheme to convey our message discreetly.

{{< figure src="/projects/overseer/processed/05.png" caption="Assembling the main components of the arm" >}}

We encountered challenges with cable management, leading to loose wires tangling in the arms and joints. We resolved this by securing the wires with plastic zip ties along the arms.

{{< figure src="/projects/overseer/processed/07.png" caption="Complete assembly of the Overseer" >}}

{{< youtube egSJn1_CyDM >}}

In the final phase of our project, we focused on testing and refining our robot's configuration for optimal facial recognition. We adjusted parameters, optimized speed by lowering frame rates and image quality, and added a fan to manage processor temperature during testing.

## Conclusion

{{< figure src="/projects/overseer/processed/08.png" caption="Complete assembly of the Overseer" >}}

In conclusion, our project has been a success. Inspired by George Orwell's insights in 1984, we aimed to explore the theme of surveillance. Our creation operates effectively as intended.

Rather than aiming to disturb, our goal was to gently provoke thought and offer a glimpse into a different perspective on reality. We hope our project encourages reflection on the impacts of surveillance in today's world.

{{< youtube aPQUcmxKYXk >}}

### Used components

#### 2x Servo Motors Waveshare MG996R
torque: 9kg/cm (4.8V), 11kg/cm (6V),
speed: 0.19s/60° (4.8V), 0.18s/60° (6V),
rotation: 180°,
voltage: 4.8V ~ 6V,
weight: 55g,
dimensions: 40.7 × 19.7 × 42.9mm

#### Raspberry Pi 3 Model B+
cpu: 1.4GHz 64-bit quad-core ARM Cortex-A53,
cpu architecture: ARMv8-A (64/32-bit),
gpu: Broadcom VideoCore IV @ 250MHz,
sdram: 1GB,
weight: 45g,
dimensions: 85.60 × 56.5 × 17mm

#### Raspberry Pi Camera V2
video FPS: 1080p30, 720p60,
sensor: 8MP Sony IMX219 CCD,
connector: CSI,
field of view: 62.2° x 48.8°,
weight: 3g,
dimensions: 25 × 20 × 9mm

#### Raspberry Pi Fan with 30cm Cable
consumption: 0.35W,
power: 5V,
reduced: 3V3,
noise: 22dBA (5V),
dimensions: 35 x 35 x 10mm

#### Power Supply 5.1V⎓3A
power: 15.3W,
voltage: 5.1V,
current: 3A
