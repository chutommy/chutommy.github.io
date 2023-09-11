---
title: "Project Overseer - Surveillance in the 21st Century"
description: "George Orwell's Underappreciated Crusade"
date: "2019-04-11T16:22:02+02:00"

hideMeta: false
showPostNavLinks: false
showReadingTime: false

showToc: true
tocOpen: false
disableHLJS: false
disableAnchoredHeadings: true

cover:
  image: "/posts/project-overseer/processed/08.png"
---

The Overseer is a year-long project undertaken in honor of [George Orwell](https://en.wikipedia.org/wiki/George_Orwell) and his mission to warn humanity about the dangers of surveillance. In his book [1984](https://en.wikipedia.org/wiki/Nineteen_Eighty-Four), he laid the groudwork for omnipresent government surveillance and mass global-scope manipulation. The Overseer project aims to convey the message through an art work featuring a disturbing surveillance camera mounted on a robotic arm.

{{< figure src="/posts/project-overseer/1984-orwell.png" caption="source: bookanalysis.com/1984/ingsoc" >}}

## Introduction

> Dear all,
>
> You live in a world where you've grown accustomed to our constant presence. But don't bother trying to evade us; it's futile. We are omnipresent, watching you closely. In truth, we are powerless slaves to the algorithms and tools others employ. If you find this unsettling, well, tough luck -- no one sought your opinion. There's no hiding from us, and we won't cease our vigilant gaze.
>
> Yours faithfully, The Overseer

It's astonishing how much of a digital footprint each of us leaves in today's world. Computers have seamlessly integrated into our daily routines, whether we're using online services, precise navigation tools, quick payment options, or electronic wearables.

Around 80 years ago, brilliant scientists and engineers of the 20th century introduced computers, reshaping the world beyond recognition. They enabled us to connect the entire globe, accelerate scientific advancements, automate numerous manual tasks, and propel healthcare to new heights. However, this transformation has also drastically affected our security. The advent of digital cameras, offering the ability to 'peek into the past', has given birth to an entirely new industry.

In recent decades, the number of cameras in public spaces has risen dramatically ([CCTVs](https://en.wikipedia.org/wiki/Closed-circuit_television)). If you live in a city, you're likely to encounter them frequently. These cameras are deployed for various purposes, with crime prevention being the most common. Managing the recordings from these cameras presents its own set of challenges. While there are legal limitations on how long such recordings can be retained and how they can be handled, practical control over them is often elusive due to restricted public access. The real value emerges when advanced algorithms, like [facial recognition](https://en.wikipedia.org/wiki/Facial_recognition_system) for identifying individuals or [license plate recognition](https://en.wikipedia.org/wiki/Automatic_number-plate_recognition) for vehicles, are applied to these recordings. This analysis goes beyond capturing individuals; it deciphers behaviors, interests, and even emotions based on facial expressions, activities, timestamps, locations, and other digital traces.

{{< figure src="/posts/project-overseer/observed.png" caption="source: innertec.co.uk/2022/06/cctv-does-it-actually-work" >}}

Notably, tech giants with massive user bases have gather a huge amount of data. Even those who don't actively use these services still contribute data passively. These companies have moved beyond the need for additional data and now focus on refining algorithms to identify patterns within the colossal amount of existing data. This intricate web of information allows them to gain unprecedented insights into society and open possibilities for mass manipulation.

In essence, every individual who leaves behind even the tiniest digital trace is unwittingly part of a vast, shadowy industry. This industry churns billions of dollars annually, fueled by the data we generate. Our 'free' use of these services comes at the cost of our personal information, which, when combined with data from millions of other users, becomes the key to these companies' power and influence.

## Motivation

What initially hooked my interest in the interconnected digital world was the notorious case involving [Cambridge Analytica](https://en.wikipedia.org/wiki/Cambridge_Analytica). Subsequently, I was introduced to George Orwell's classic novel 1984. As I delved into its pages, I couldn't help but notice striking similarities between the dystopian world of Big Brother and our own reality. I began to envision our world as a parallel to this nightmarish setting, particularly within the colossal corporations that seemed to echo [Orwell's Party](https://en.wikipedia.org/wiki/Political_geography_of_Nineteen_Eighty-Four). It's not so much the act of surveillance where the resemblance lies, but rather the immense power these entities wield. However, a fundamental distinction lies in the fact that the citizens in Orwell's world are fully aware of their constant surveillance.

A tangible example of the influence held by these corporations can be found in recent events, such as the [Facebook-Cambridge Analytica scandal](https://en.wikipedia.org/wiki/Facebook%E2%80%93Cambridge_Analytica_data_scandal). This company managed to amass personal data from millions of American citizens without their consent, crafting a psychological model that allowed them to predict the behaviour of each American voter with remarkable accuracy. Given that our behavior shapes our voting decisions, they effectively manipulated a substantial portion of the population through influential videos and news articles, in collaboration with Donald Trump's 2016 election campaign. They possessed a deep understanding of what resonated with voters and what information to feed them to shape the election outcome to their liking. In 2018, a former Cambridge Analytica employee disclosed that the data used by their analysts to construct this model of American society was derived from leaked Facebook user data - a revelation marking the largest known data breach in history. It's clear that without access to this data, Cambridge Analytica would not have exerted such significant influence, potentially leading to an entirely different election outcome.

{{< figure src="/posts/project-overseer/usa-2016.png" caption="source: en.wikipedia.org/wiki/2016_United_States_presidential_election" title="Results by state: Trump (red) vs. Clinton (blue)" >}}

We find ourselves as one of the first generations handling this form of manipulation, facing a formidable challenge with no clear strategy for defense. Abandoning the services of these tech companies isn't a feasible solution, primarily due to the immense convenience they provide. Once again, I can't help but draw parallels between our situation and the citizen's of Orwell's society.

My work, the Overseer, was to serve as a reminder to the audience that there are, at the very least, surveillance cameras from which they cannot escape, and that each one of us leaves behind a digital footprint.

## Implementation

To bring the project to life, I needed to develop software capable of efficiently controlling the robot arms and orchestrating the entire system's movements. This required several key functionalities.

### Software driver

First and foremost, I required the software to perform the following tasks:

1. Capture frames from the camera.
2. Identify all targets within the frames.
3. Select the nearest target based on size.

It's worth noting that not every person has the same head size, which could potentially lead to errors in selecting more distant faces. However, I concluded that these discrepancies would be minimal. The primary aim of this function was to ensure that the algorithm consistently focused on a single object.

### First setbacks

Another crucial aspect was determining how much the servo motors should rotate based on the coordinates of the target. This presented a significant challenge during the software development process. I could only calculate the number of degrees the camera should rotate on its respective axes. For the camera, this involved a field of view of 62.2° in width and 48.8° in height.

{{< figure src="/posts/project-overseer/project/processed/01.png" caption="Initial sketches of servo-motor motions and arm movements, angle calculations" >}}

This presented a complex problem where I had a triangle with vertices representing the camera, the target, and the movable joint with the motor. I had only two pieces of information: the angle between the camera and the target, and the length of the arm. While I could have attempted to calculate the distance from the camera to the face, it would have been highly imprecise.

Ultimately, I opted for a solution where I positioned the motor in a way that the camera's rotating axis aligned with it. This, in practice, meant that the axes of both motors had to intersect precisely at the camera's lens. While this resolved the data deficiency for calculating arm movements, it added complexity to the device's construction.

{{< figure src="/posts/project-overseer/project/processed/02.png" caption="Initial drafts of the casing design" >}}

As the project evolved, I incorporated additional features, such as target tolerance adjustments, the ability to draw rectangles around detected targets, and automated arm centering both before and after extended periods of inactivity.

{{< figure src="/posts/project-overseer/project/processed/03.png" caption="Final concept - dimension calculations and assembly planning" >}}

### CPU compatibility

I originally planned to run everything on a [Raspberry Pi Zero W](https://www.raspberrypi.com/products/raspberry-pi-zero-w/), which I had initially intended to fit inside the same enclosure as the camera. However, due to the decision to employ the relatively precise [OpenCV4](https://opencv.org/) technology (v4.3.0) for facial recognition, which is only compatible with ARMv7+ processors, I ultimately opted for the [Raspberry Pi 3B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/) model with an ARMv8 CPU.

{{< figure src="/posts/project-overseer/project/processed/06.png" caption="Conceptual design drafts for an additional enclosure" >}}

This choice, however, came with a trade-off as it was considerably larger. Consequently, I had to add another enclosure to the rear of the assembly. This adjustment also led me to abandon the concept of mounting the camera as an articulated arm on a board that could be hung on the wall.

#### PWM controlled servo-motors

Another hurdle I encountered was related to the motors, which are controlled using [pulse width modulation](https://en.wikipedia.org/wiki/Pulse-width_modulation) (PWM) - a method of data transmission via a single wire involving variations in signal periods and pulse widths. The challenge here was that the motors couldn't be connected to the [Gobot](https://gobot.io/) controller (v1.14.0) through the PWM pins. Fortunately, I ultimately found a solution with the [Go-Pi-Blaster](https://github.com/vestor/watermyplant) library, which allowed me to work within the current range where servo motors respond reliably.

The entire code was implemented in the [Go](https://go.dev/) programming language (v1.14).

## Assemble

{{< figure src="/posts/project-overseer/project/processed/04.png" caption="Hand-cutting particleboard and cardboard for arms and casing" >}}

To assemble the Overseer bot, I chose lightweight particleboard for the frame, prioritizing algorithm speed to ensure smooth operation. However, the wood proved weaker and more prone to splitting than I'd anticipated, requiring additional reinforcement for some parts.

For the casing, I used more suitable materials like cardboard to cover the electrical components and thin, sturdy laminated boards for the technical enclosure. I aimed for a consistent and unobtrusive color scheme to convey my message subtly, showing that not everything needs to be flashy or noticeable.

{{< figure src="/posts/project-overseer/project/processed/05.png" caption="Assembling the main components of the arm" >}}

I faced a few unexpected challenges I underestimated like a bad cable-management, which led to them being too loose and getting tangled in the arms and motors. I solved this by using plastic zip ties to secure the wires along the arms.

{{< figure src="/posts/project-overseer/project/processed/07.png" caption="Complete assembly of the Overseer" >}}

{{< youtube egSJn1_CyDM >}}

### Final touch

In the final phases of my project, a critical step was testing and fine-tuning the device's construction. I conducted experiments by adjusting various parameters, including the tolerance, and different facial patterns.

Once I achieved the desired functionality, I had to reduce the frame rate and image resolution to optimize the device's speed. I encountered a significant challenge as the computer struggled to execute all necessary tasks swiftly for real-time performance. Consequently, I had to make compromises and lower the image quality, resulting in reliable recognition within a range of up to three meters.

During testing, I also observed that the processor's average temperature sat around 80°C. While it could safely operate up to 85°C, I decided to add a fan as an extra precaution.

## Conclusion

{{< figure src="/posts/project-overseer/project/processed/08.png" caption="Complete assembly of the Overseer" >}}

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
