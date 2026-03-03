# Day 3: Satellite Hardware and Onboard Computing
**Phase:** FUNDAMENTALS  
**Date:** March 03, 2026

---

## Prediction Prompt
> Modern smartphones contain AI chips that far outperform most radiation-hardened space processors — so why do you think satellite engineers don't simply launch commercial phone hardware into orbit instead of using slower, purpose-built chips?

## Lesson

Here is the Day 3 email lesson:

---

MAKEMEANLEXPERT: AI/ML IN SATELLITES AND SPACE SYSTEMS
Day 3 of 30 — Satellite Hardware and Onboard Computing


When ESA's Phi-Sat-1 launched in September 2020, it carried something no spacecraft had ever flown before: a commercial AI chip — originally designed for smartphone cameras — repurposed to make autonomous decisions in the harshest computing environment imaginable. That chip worked. But getting a neural network accelerator to survive radiation, vacuum, and temperature swings of hundreds of degrees required understanding every physical constraint that defines satellite hardware design. Today we cover exactly that: the machines behind the missions.


ANATOMY OF A SATELLITE

A satellite is not a single device. It is a collection of tightly integrated subsystems, each solving a different engineering problem, all constrained by the same merciless budget: mass, power, and volume.

The bus is the spacecraft platform — the structure, power systems, computers, attitude control, and communications hardware that keep the satellite alive and pointed in the right direction. Every satellite has a bus. What makes a mission unique is the payload: the instrument, sensor, or transponder that does the actual science or service. Think of the bus as the car and the payload as the cargo. The bus exists to deliver the payload to the right place and keep it operational.

The power subsystem collects energy from solar panels (in sunlight) and stores it in batteries (during eclipse). A small 3U CubeSat might generate 5 to 10 watts average. A large geostationary communications satellite can generate 20 kilowatts. Everything onboard — computers, heaters, sensors, radios — must fit within that power envelope. When an engineer wants to add an AI accelerator, they are not asking "can we do the computation?" They are asking "can we afford the watts?"

The thermal control subsystem manages heat. In low Earth orbit, a satellite rotates in and out of sunlight every 90 minutes. The sun-facing side can reach +120 degrees Celsius; the shadow side can drop to -170 degrees Celsius. Electronics have much tighter operating ranges, typically -40 to +85 degrees Celsius. Thermal engineers use radiator panels, Multi-Layer Insulation blankets, heat pipes, and resistance heaters to keep the internals stable. Adding a power-hungry AI chip creates new thermal challenges — more heat must be conducted away or radiated into space.

The Attitude Determination and Control System (ADCS) keeps the satellite pointed correctly. Earth observation satellites must aim cameras at precise ground targets. Astronomical telescopes must hold a fixed pointing to within fractions of an arcsecond. ADCS uses combinations of star trackers, sun sensors, magnetometers, gyroscopes, reaction wheels, and magnetic torque rods to measure and correct orientation. The ADCS is often the most computationally active subsystem on a traditional satellite, running control loops many times per second.

The Telemetry, Tracking, and Command (TT&C) subsystem is the radio link to the ground — what we covered in Day 1. And the Onboard Computer (OBC) sits at the center of it all, interpreting commands, running flight software, managing fault detection, and — increasingly — running AI inference.


ONBOARD COMPUTERS: RADIATION-HARDENED PROCESSORS VS. COTS HARDWARE

Here is a fact that surprises most software engineers when they first encounter it: the central processor in many spacecraft is slower than a pocket calculator from the 1990s.

The BAE Systems RAD750 processor, which flew aboard the Mars Curiosity rover and the Lunar Reconnaissance Orbiter, runs at around 200 MHz with roughly 400 MIPS of compute throughput. It consumes about 5 to 10 watts and costs somewhere between $200,000 and $500,000 per unit. By comparison, a $50 Raspberry Pi 4 runs at 1,500 MHz, delivers far more throughput, consumes 5 watts, and would be destroyed within weeks in the radiation environment of deep space.

Why are radiation-hardened processors so expensive and so slow? The answer is physics. Standard CMOS transistors shrink as chip manufacturing advances. Smaller transistors are faster and more power-efficient — but they are also more vulnerable to ionizing radiation. Radiation-hardened chips deliberately use older, larger manufacturing processes (sometimes 150-nanometer or even 250-nanometer feature sizes, compared to 3-5 nm in modern commercial chips). They also add redundant logic, error-correcting circuits, and special transistor layouts that resist the effects of energetic particles. The result is a chip that may run a dozen times slower than comparable commercial silicon but can survive a decade in orbit.

The new space economy is forcing a rethink of this trade-off. Companies like Planet Labs, SpaceX (for Starlink), and the many CubeSat manufacturers of the past decade are using Commercial Off-The-Shelf (COTS) components that were never designed for space. A COTS processor might offer ten to one hundred times the compute performance of a radiation-hardened equivalent at one-hundredth the price. The trade-off is reliability. COTS parts in space will experience bit errors, occasional computation mistakes, and sometimes catastrophic failures. Operators mitigate this through redundancy (flying multiple units), watchdog timers that reboot the processor if it hangs, and careful software design that can recover from transient faults. For short-lived missions of a year or two, COTS reliability is often acceptable. For a 15-year geostationary satellite, it may not be.


PROCESSING CONSTRAINTS IN SPACE: POWER BUDGETS, THERMAL LIMITS, AND RADIATION EFFECTS

Understanding radiation effects is non-negotiable for anyone designing AI systems for space. Three phenomena dominate.

A Single Event Upset (SEU) occurs when a single energetic particle — typically a cosmic ray or a proton trapped in Earth's radiation belts — passes through a memory cell or logic circuit and deposits enough charge to flip a bit from 0 to 1 or vice versa. An SEU does not physically damage the device; it is a transient error. In your laptop, bit flips are so rare as to be negligible. In a satellite orbiting through the South Atlantic Anomaly (a region where Earth's radiation belts dip unusually close to the surface), a processor might experience thousands of SEUs per day. In memory, this shows up as data corruption. In a stored neural network weight, it can silently shift a model's behavior in unpredictable ways.

Single Event Latch-up (SEL) is more serious. A heavy ion can trigger a parasitic current path inside a CMOS transistor, causing it to short-circuit and conduct excessive current. Without intervention, the chip destroys itself. Hardware designers respond by monitoring current draw and cutting power to the affected device — then powering it back up. This latch-up protection circuitry adds complexity, but it is essential for COTS components in high-radiation orbits.

Total Ionizing Dose (TID) is cumulative damage. Over months and years, radiation gradually degrades oxide layers in transistors, shifting their electrical characteristics. A component might be rated for a TID of 100 kilorads before it falls out of specification. In low Earth orbit at 500 km altitude, a satellite accumulates roughly 5 to 10 kilorads per year behind moderate shielding. In geostationary orbit or near the radiation belts, the dose rate is far higher.

Beyond radiation, every watt of power consumed inside a satellite becomes heat that must be radiated away. There is no convection in the vacuum of space — heat can only leave through conduction to radiator surfaces or direct thermal emission. A GPU drawing 75 watts on the ground is trivial to cool with a fan. In orbit, evacuating those 75 watts requires a substantial radiator panel. Compact satellite designs simply cannot accommodate this, which is why AI accelerators for space are evaluated not just on TOPS (tera-operations per second) but on TOPS per watt.


FROM SIMPLE COMMAND EXECUTION TO ONBOARD AUTONOMY

Early satellites were essentially passive relay stations. Engineers sent a command; the satellite executed it exactly, period. The onboard computer — often not much more than a timer and a relay controller — had no capacity for reasoning about its state or adapting its behavior.

By the 1970s and 1980s, attitude control systems became sophisticated enough to manage pointing autonomously, following control laws without waiting for a command uplink. By the 1990s, fault detection, isolation, and recovery (FDIR) software gave satellites the ability to identify anomalies and reconfigure around failed components — the first real form of onboard autonomy.

A landmark demonstration came in 2003 with NASA's Autonomous Sciencecraft Experiment (ASE) aboard the Earth Observing 1 (EO-1) satellite. ASE ran an onboard expert system capable of identifying scientifically interesting events — volcanic eruptions, flooding, wildfire spread — in sensor data and autonomously retasking the satellite to collect additional observations, all without waiting for a ground pass. For a satellite with 90-minute orbits and limited downlink windows, this kind of onboard intelligence fundamentally changed what was scientifically possible.

Today the goal is deeper intelligence: not just reacting to predefined events, but running trained neural networks that generalize to new situations. Rather than an expert system rule that says "if cloud cover exceeds 80%, discard the image," a convolutional neural network can classify cloud patterns with a confidence score, handle ambiguous cases, and be updated as new training data becomes available. This shift — from rule-based autonomy to learning-based autonomy — is what makes AI/ML integration with satellite hardware so consequential.


OVERVIEW OF SPACE-QUALIFIED AI ACCELERATORS

Three hardware categories are actively competing to become the standard for AI inference in orbit.

The Intel Movidius Myriad 2 Vision Processing Unit (VPU) is a low-power chip originally designed for drone and augmented reality applications. It delivers roughly 1 TOPS (tera-operations per second) at under 1 watt of power — an extraordinary efficiency ratio for image classification and object detection tasks. It is not radiation-hardened, but its power profile makes it manageable for CubeSat and small satellite applications with appropriate latch-up protection. It is the chip that flew aboard Phi-Sat-1, where it ran a cloud-detection neural network in real time on hyperspectral imagery.

Xilinx FPGAs (now AMD-Xilinx) are programmable logic devices that can be configured in hardware to implement custom compute pipelines. They excel at fixed-function, highly parallel workloads — which is exactly what neural network inference looks like when you unroll the matrix operations. Xilinx offers radiation-hardened variants such as the Virtex-5QV as well as COTS devices like the Kintex UltraScale+ that are flown with careful latch-up protection. FPGAs can implement entire neural network accelerators in reprogrammable logic, meaning that the AI architecture can potentially be updated in flight by uploading a new configuration bitstream. Their power consumption is tunable: a small network uses little power; scale up the parallelism and power rises accordingly.

Ubotica's CogniSAT module is a space-qualified integration of the Movidius Myriad 2. Ubotica, an Irish company, addresses the gap between commercial AI chips and the reliability requirements of spaceflight. The CogniSAT module packages the Myriad 2 with power conditioning, thermal management interfaces, and radiation characterization data, and provides software tools for deploying neural network models on orbit. It was the hardware behind the Phi-Sat-1 AI payload. Ubotica's approach — characterizing and qualifying COTS AI chips rather than building radiation-hardened silicon from scratch — represents a pragmatic middle path that could dramatically accelerate the adoption of onboard AI.

Beyond these, NASA's High-Performance Spaceflight Computing (HPSC) initiative is developing a next-generation processor based on RISC-V cores, targeting orders-of-magnitude improvements in compute-per-watt for deep space applications. The HPSC chip, developed with SiFive, is intended to support autonomous science operations, terrain-relative navigation, and real-time data reduction on future planetary missions — representing the next evolution beyond what current rad-hard processors can offer.


REAL-WORLD APPLICATIONS

ESA's Phi-Sat-1, launched in September 2020, is the most concrete demonstration of onboard AI in practice. The satellite carried a hyperspectral Earth observation camera alongside the Ubotica CogniSAT module running a compressed convolutional neural network trained to detect cloud coverage. Before Phi-Sat-1, every image had to be downlinked to Earth for ground-based quality screening — the satellite had no way to know whether a cloudy scene had made the image scientifically worthless. With onboard AI, the satellite evaluated image quality in real time and discarded heavily clouded frames before they ever entered the downlink queue. The result was an estimated 30 percent reduction in wasted downlink bandwidth — directly translating to more useful science per contact window.

NASA's Autonomous Sciencecraft Experiment on EO-1 (2003-2011) demonstrated rule-based onboard autonomy before deep learning was a practical option. ASE successfully detected volcanic eruptions, flooding events, and wildfire spread in real time, autonomously retasking the satellite's instruments to capture follow-up observations without waiting for human approval from the ground. Although ASE used expert systems rather than neural networks, it proved the operations concept that has since motivated the entire onboard AI field: a satellite can do science without waiting for instructions from Earth.

DARPA's Blackjack program, initiated around 2018, is pushing COTS hardware much further. Blackjack aims to build a low Earth orbit military satellite constellation where each satellite uses commercial processors to run complex mission autonomy, sensor fusion, and time-sensitive processing entirely onboard. Rather than treating radiation tolerance as a hard requirement and paying the performance penalty of radiation-hardened silicon, Blackjack designs for graceful degradation: use cheap COTS hardware, accept that some components will occasionally fail, and design software that keeps the mission running through faults. If validated at scale, this philosophy would fundamentally change the economics of space-based AI.


PAPER OF THE DAY

Furano, G., Meoni, G., Dunne, A. et al. — "Towards the Use of Artificial Intelligence on the Edge in Space Systems: Challenges and Opportunities." IEEE Aerospace and Electronic Systems Magazine, 2020. DOI: https://doi.org/10.1109/MAES.2020.3008468

This paper, authored by researchers from ESA, the University of Florence, and Ubotica Technologies, asks a deceptively simple question: which currently available hardware platforms can actually run AI inference onboard a spacecraft, and what stands in the way of doing so? The authors systematically evaluate candidate accelerators — including the Intel Movidius Myriad 2, various FPGAs, and GPU-based options — against the triple constraints of space deployment: radiation tolerance, power consumption, and the availability of AI development toolchains. They find that while no existing commercial AI chip was designed for space, the Movidius Myriad 2 offers a compelling combination of inference performance and power efficiency that makes it a viable candidate for low Earth orbit missions with appropriate qualification testing. Crucially, the paper also identifies the lack of space-qualified AI development frameworks and the difficulty of updating deployed neural network models in orbit as significant open challenges — challenges that remain only partially solved today. This paper is effectively the founding document for the ESA new space AI program that produced Phi-Sat-1.


FURTHER READING

ESA Phi-Sat-1 mission page — Official ESA documentation on the first operational AI chip in orbit, including technical details on the hyperspectral camera payload and the cloud-detection convolutional neural network:
https://www.esa.int/Enabling_Support/Space_Engineering_Technology/Phi-sat

Ubotica Technologies — Website of the Irish company that developed the CogniSAT space-qualified AI module; includes technical documentation and use cases for the Myriad-2-based hardware:
https://www.ubotica.com

"Orbital Edge Computing: Nanosatellite Constellations as a New Class of Computer System" (Denby and Lucia, ACM ASPLOS 2020) — A systems research perspective on satellite constellations as distributed computing infrastructure, with rigorous analysis of compute constraints, communication windows, and what kinds of AI workloads are actually feasible in orbit. Free preprint available via ACM:
https://doi.org/10.1145/3373376.3378473

NASA High-Performance Spaceflight Computing (HPSC) project — Overview of NASA's program to develop next-generation space processors using RISC-V architecture, targeting 100x improvements in compute-per-watt over current radiation-hardened processors for future planetary missions:
https://www.nasa.gov/directorates/spacetech/game_changing_development/projects/HPSC


KEY TAKEAWAY

Every architectural decision you make about AI models for space — model size, numerical precision, update frequency, fault tolerance — ultimately traces back to a single physics constraint: in orbit, every watt you spend on computation is a watt that must be radiated away into vacuum, and there is a hard limit to how large your radiator can be.

---

## Callback Question
> On Day 2, we explored how a satellite's orbital regime determines where it spends its time — including prolonged exposure to the Van Allen radiation belts. Given what you've learned today about radiation effects like single-event upsets (SEUs) and latch-up, how does a satellite's choice of orbit influence the decision between radiation-hardened processors and COTS hardware for onboard computing? What trade-offs would an engineer face when deploying a COTS AI accelerator on a mission to geostationary orbit versus low Earth orbit?

## Feynman Prompt
> In 2-3 sentences, explain radiation-hardened processors as if describing it to a colleague who has never heard of them.

## Visual References

![Animation of International Space Station's trajectory from 14 September 2018 to 14 November 2018. Earth is not shown.](https://upload.wikimedia.org/wikipedia/commons/c/c8/Animation_of_International_Space_Station_trajectory.gif)
*Animation of International Space Station's trajectory from 14 September 2018 to 14 November 2018. Earth is not shown.* — Source: [Wikipedia (Phoenix7777, CC BY-SA 4.0)](https://en.wikipedia.org/wiki/International%20Space%20Station)

![Artist's concept of 2001 Mars Odyssey spacecraft over Syrtis Major Planum.](https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/2001_mars_odyssey_wizja.jpg/960px-2001_mars_odyssey_wizja.jpg)
*Artist's concept of 2001 Mars Odyssey spacecraft over Syrtis Major Planum.* — Source: [Wikipedia (NASA/JPL/Corby Waste, Public domain)](https://en.wikipedia.org/wiki/Curiosity%20%28rover%29)
