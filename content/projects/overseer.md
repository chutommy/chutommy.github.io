---
title: "Digital surveillance in the 21st Century"
description: "Project Overseer - The George Orwell's crusade"
date: "2019-04-11T16:22:02+02:00"
weight: 100

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

Project Overseer is a year-long project undertaken in honor of [George Orwell](https://en.wikipedia.org/wiki/George_Orwell) and his mission to warn humanity about the dangers of surveillance. In his book [1984](https://en.wikipedia.org/wiki/Nineteen_Eighty-Four), he laid the groudwork for omnipresent government surveillance and mass global-scope manipulation. The Overseer project aims to convey the message through an art work featuring a disturbing surveillance.

{{< figure src="/projects/overseer/processed/06.png" caption="Conceptual design drafts for the enclosure" >}}

{{< figure src="/projects/overseer/processed/09.png" >}}

> {{< collapse summary="Expand **Introduction**" >}}

## Introduction

Every one of us leaves a digital footprint in today's world. Computers have seamlessly integrated into our daily lives, whether we're using online services, navigation tools, payment options, electronic wearables, or smart devices.

Around 80 years ago, brilliant scientists and engineers of the 20th century introduced computers, reshaping the world beyond recognition. This happened to connect the entire globe, accelerate scientific advancements, automate numerous manual tasks, and propel healthcare to new heights. However, this transformation has also drastically affected our security. The spread of digital cameras, which offer the ability to 'peek into the past', has given birth to an entirely new industry.

In recent decades, the number of cameras in public spaces has risen dramatically ([CCTVs](https://en.wikipedia.org/wiki/Closed-circuit_television)). These cameras are deployed for various purposes, with crime prevention being the most common. Managing the recordings from these cameras presents its own set of challenges. While there are legal limitations on how long such recordings can be retained and how they can be handled, practical control over them is often elusive due to restricted public access.

Technology companies with massive user base have gathered a huge amount of data. Even those people who don't actively use these services still contribute data passively. This dense web of information enables to gain insights into society and open possibilities for potential mass manipulation.

In essence, every individual who leaves behind even the tiniest digital trace is unknowingly part of a vast, shadowy industry. This industry churns billions of dollars annually, fueled by the data we generate. Free usage of these services comes at the cost of our personal information, which when combined with data from millions of other users, becomes the key to power and influence.

{{</ collapse >}}

> {{< collapse summary="Expand **Motivation**" >}}

## Motivation

When I was introduced to George Orwell's classic novel 1984 I couldn't help but notice similarities between the dystopian world of Big Brother and our own reality. I began to envision our world as a parallel to this nightmarish setting, particularly within the colossal corporations that seemed to echo [Orwell's Party](https://en.wikipedia.org/wiki/Political_geography_of_Nineteen_Eighty-Four). It's not so much the act of surveillance where the resemblance lies, but rather the immense power these entities wield.

A tangible example of the influence held by these companies can be found in recent events, such as the notorious case [Facebook-Cambridge Analytica](https://en.wikipedia.org/wiki/Facebook%E2%80%93Cambridge_Analytica_data_scandal). The social media platform managed to amass personal data from millions of American citizens without their consent, crafting a model that helped them to effectively manipulate a substantial portion of the population through influential videos and news articles. In collaboration with Donald Trump's 2016 election campaign, they possessed a deep understanding of what resonated with voters and what information to feed them to shape the election outcome to their liking. In 2018, a former Cambridge Analytica employee disclosed that the data used by their analysts to construct this model was derived from leaked Facebook user data - a revelation marking the largest known data breach in history.

We find ourselves as one of the first generations handling this form of manipulation, facing a challenge with no clear strategy for defense. Abandoning the services of large technology companies isn't a feasible solution, primarily due to the immense convenience they provide. Once again, I can't help but draw parallels between our situation and the citizen's of Orwell's society.

The Overseer is to serve as a reminder to the audience that there are, at the very least, surveillance cameras from which they cannot escape.

{{</ collapse >}}

## Implementation

To bring the project to life, it was needed to develop a software capable of efficiently controlling the robot arms and orchestrating the entire system's movements. This required several key functionalities.

### Software driver

First and foremost, the software had to perform the following tasks:

1. Capture frames from the camera.
2. Identify all targets within the frames.
3. Select the nearest target based on size.

It's worth noting that not every person has the same head size, which could potentially lead to errors in selecting more distant faces. However, I concluded that these discrepancies would be minimal. The primary aim of this function was to ensure that the algorithm consistently focused on a single object.

### First setbacks

A crucial step was to determine how much the servo motors should rotate based on the coordinates of the target. This presented a significant challenge during the software development process, since only the number of degrees of the camera movement was computationally attainable.

{{< figure src="/projects/overseer/processed/01.png" caption="Initial sketches of servo-motor motions and arm movements, angle calculations" >}}

This presented a complex problem where a triangle with vertices representing the camera, the target, and the movable joint needed to be synchronized. Having only two pieces of information: one angle, and one length, this was geometrically impossible to calculate.

Ultimately, to solve this challenge the motor was positioned in a way that the camera's rotating axis aligned with it. This, in practice, meant that the axes of both motors had to intersect precisely at the camera's lens. While this resolved the data deficiency for calculating arm movements, it added complexity to the construction of the device.

{{< figure src="/projects/overseer/processed/02.png" caption="Initial drafts of the casing design" >}}

As the project evolved, additional features were incorporated, such as target tolerance adjustments, the ability to draw rectangles around detected targets, and automated arm centering both before and after extended periods of inactivity.

{{< figure src="/projects/overseer/processed/03.png" caption="Final concept - dimension calculations and assembly planning" >}}

### CPU compatibility

Everything was planned to be run on a [Raspberry Pi Zero W](https://www.raspberrypi.com/products/raspberry-pi-zero-w/), which was initially intended to fit inside the same enclosure as the camera. However, due to the decision to employ the [OpenCV4](https://opencv.org/) technology (v4.3.0) for facial recognition, which is only compatible with ARMv7+ processors, it was ultimately opted for the [Raspberry Pi 3B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/) model with an ARMv8 CPU.

This choice, however, came with a trade-off as it was considerably larger. Consequently, it was necessary to add another enclosure to the rear of the assembly. This adjustment also led to the abandonment of the original idea of mounting the camera as an arm on a board that could be hung on the wall.

## Assemble

{{< figure src="/projects/overseer/processed/04.png" caption="Hand-cutting particleboard and cardboard for arms and casing" >}}

To assemble the Overseer robot, I chose lightweight particleboard for the frame, prioritizing algorithm speed to ensure smooth operation. However, the wood proved to be weaker and more prone to splitting than I'd anticipated, requiring additional reinforcement for some parts.

For the casing, I used more suitable materials like cardboard to cover the electrical components and thin, sturdy laminated boards for the technical enclosure. I aimed for a consistent and unobtrusive color scheme to convey my message subtly, showing that not everything needs to be flashy or noticeable.

{{< figure src="/projects/overseer/processed/05.png" caption="Assembling the main components of the arm" >}}

I faced a few unexpected challenges such as a nonoptimal cable-management, which made surrounding wires too loose and got them tangled in the arms and joints. I solved this by using plastic zip ties to secure the wires along the arms.

{{< figure src="/projects/overseer/processed/07.png" caption="Complete assembly of the Overseer" >}}

{{< youtube egSJn1_CyDM >}}

### Final touch

In the final phase of my project, a critical step was testing and fine-tuning the configuration. I conducted experiments by adjusting various parameters, including the tolerance, and different facial patterns.

Once I achieved the desired functionality, I had to reduce the frame rate and image resolution to optimize the device's speed. I encountered a significant challenge as the computer struggled to execute all necessary tasks swiftly for real-time performance. Consequently, I had to make compromises and lower the image quality, resulting in reliable recognition within a range of up to three meters.

During testing, I also observed that the processor's average temperature sat around 80°C. While it could safely operate up to 85°C, I decided to add a fan as an extra precaution.

## Conclusion

{{< figure src="/projects/overseer/processed/08.png" caption="Complete assembly of the Overseer" >}}

I consider my final project to be a success overall. I set out to explore the theme of surveillance, following in the footsteps of George Orwell, who masterfully delved into it in his iconic work 1984 and served as a significant source of inspiration for my own project. My creation operates as intended.

The goal has never been to disturb viewers. Rather, it seeks to momentarily nudge them out of their comfort zones and offer a taste of unfiltered reality.

{{< youtube aPQUcmxKYXk >}}

### Components used

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
