# Day 15: Autonomous Satellite Operations
**Phase:** INTERMEDIATE TO ADVANCED  
**Date:** March 15, 2026

---

## Prediction Prompt
> Starlink satellites autonomously avoid collisions using a neural network — before reading, take a guess: what inputs do you think that neural network uses to decide whether to fire thrusters, and why might a rule-based system alone be insufficient for this decision?

## Lesson

It looks like the write permission wasn't granted. Here is the full lesson text — you can copy it or let me know where to save it:

---

**MAKEMEАНEXPERT: AI/ML IN SATELLITES AND SPACE SYSTEMS**
Day 15 of 30 — Autonomous Satellite Operations

---

In September 2019, SpaceX made headlines by revealing that one of its Starlink satellites had autonomously maneuvered away from a European Space Agency satellite after ESA's operators chose not to act on a conjunction alert — because Starlink's onboard AI had already handled it. No human involved, no ground command uplinked, just a machine deciding to move itself out of danger. That moment crystallized what the aerospace community had been quietly building toward for decades: satellites that can think and act for themselves.

Today we explore how that autonomy works — from the basic rungs on the autonomy ladder all the way up to fully self-driving spacecraft.

---

**LEVELS OF AUTONOMY IN SPACE SYSTEMS**

Not all autonomous systems are equal. The space community uses a tiered framework to describe how much a satellite can do on its own. The European Cooperation for Space Standardization (ECSS) defines four levels, often labeled E1 through E4, and NASA uses a similar ladder internally.

At the bottom (E1), the satellite executes pre-uploaded time-tagged command sequences. Ground operators plan everything, and the spacecraft just follows orders. This is how early satellites like Landsat-1 worked — essentially a remote-controlled camera in orbit.

At E2, the satellite can execute stored procedures and respond to simple onboard triggers. For example, "if the battery drops below 20%, switch to safe mode." Still mostly scripted, but the spacecraft can react to predefined conditions without waiting for a ground command.

At E3, the satellite performs onboard replanning. It has a model of its own resources and goals, and if something disrupts the plan — a sensor failure, a power anomaly, an unexpected science opportunity — it can autonomously replan its schedule. This is where things get genuinely interesting.

At E4, the satellite operates goal-directed autonomy. You tell it *what* you want accomplished, not *how* to do it. The spacecraft figures out the sequence, allocates its own resources, handles contingencies, and executes without further human input. Only a handful of spacecraft have reached this level in practice.

Most operational satellites today sit somewhere between E2 and E3. The trend is unmistakably upward, driven by communication latency (you cannot wait 20 minutes for a round-trip signal to Mars and back when something goes wrong), the explosion in constellation sizes (no one can babysit 6,000 Starlink satellites individually), and growing onboard computing capability.

---

**TASK SCHEDULING AND RESOURCE ALLOCATION**

Running a satellite is a resource management problem wrapped in a physics problem. At any given moment, the spacecraft has limited power (constrained by solar panel output and battery state), limited data storage, limited downlink bandwidth, and thermal constraints that change as it moves in and out of eclipse. Meanwhile, there are dozens of competing demands: science observations to make, housekeeping telemetry to gather, contact windows to exploit, and maneuvers to execute.

This is formally a Constraint Satisfaction Problem (CSP) — find an assignment of activities to time slots that satisfies all constraints simultaneously — and often also an optimization problem, because among all feasible schedules, you want the one that maximizes science return, minimizes fuel use, or achieves some other objective.

JPL developed one of the most influential tools in this space: ASPEN (Automated Scheduling and Planning ENvironment), later extended into CASPER (Continuous Activity Scheduling Planning Execution and Replanning). ASPEN represents goals, activities, and constraints declaratively, then uses heuristic search to find feasible schedules. CASPER adds the ability to replan continuously as new information arrives — a critical capability when onboard sensors detect unexpected events.

Under uncertainty, scheduling gets harder. Power generation depends on how clean the solar panels are and the angle to the sun. Science opportunities (a storm system forming over the ocean, a wildfire starting in California) are not predictable in advance. Modern approaches combine Mixed Integer Linear Programming (MILP) for exact optimization over short horizons with heuristic methods like genetic algorithms or Monte Carlo Tree Search for longer-horizon planning where exact methods become computationally intractable.

For large constellations, the problem scales dramatically. Scheduling which Starlink satellite downloads data through which ground station, while coordinating inter-satellite links, while managing each satellite's power budget — this is solved not by human operators but by centralized automated systems running continuously on the ground, with increasing autonomy being pushed to the edge (the satellites themselves).

---

**ONBOARD FAULT DETECTION, DIAGNOSIS, AND RECOVERY (FDIR)**

Space is trying to kill your satellite at all times. Cosmic rays flip bits in memory. Thermal cycling fatigues components. Solar particles upset electronics. Micrometeorites can puncture solar panels. A spacecraft that requires a human to diagnose and recover from every fault simply cannot survive the communication latency to deep space, let alone the pace of failures in a large LEO constellation.

FDIR — Fault Detection, Isolation, and Recovery — is the discipline of making spacecraft self-healing. Traditional FDIR is rule-based: engineers write exhaustive if-then trees based on their understanding of failure modes. "If pressure sensor A reads above threshold X AND temperature sensor B is nominal, then isolate valve C and switch to redundant pressure sensor A2." This approach is well-understood, verifiable, and certifiable — critical virtues for flight software — but it only handles faults the engineers anticipated.

ML-based FDIR extends this to anomalies that were never explicitly catalogued. Autoencoders trained on nominal telemetry learn what "healthy" looks like across hundreds of sensor channels simultaneously, and flag deviations that no individual threshold would catch. Recurrent neural networks (LSTMs) can learn temporal patterns — a gradual drift in reaction wheel speed that precedes a bearing failure — and issue warnings before the fault becomes critical. The Mars Science Laboratory (Curiosity rover) team has used anomaly detection algorithms to automatically screen the massive telemetry streams coming from the rover and prioritize what human analysts should investigate.

The challenge is that in flight software, certification matters enormously. A rule-based FDIR system can be formally verified. An LSTM cannot, at least not with current tools. The practical approach emerging in the industry is hybrid: keep rule-based systems as the primary safety backstop for critical fault responses, while running ML anomaly detectors in a monitoring role that alerts operators or suggests (but does not automatically execute) recovery actions.

---

**AUTONOMOUS ORBIT MAINTENANCE**

Maintaining a satellite in its intended orbit requires fuel, and using that fuel intelligently requires planning.

Station-keeping for geostationary satellites (GEO) is one of the oldest forms of satellite autonomy. A GEO bird drifts due to the gravitational pull of the Moon and Sun, and solar radiation pressure. Operators upload maneuver plans periodically, but modern GEO platforms increasingly execute these maneuvers based on onboard calculations. Electric propulsion satellites — which use ion thrusters far more efficient than chemical thrusters but must fire for weeks at a time — essentially require automated maneuver planning because the sequences are too complex and too long to micromanage from the ground.

Formation flying takes autonomy to another level. Missions like ESA's Cluster (four spacecraft measuring Earth's magnetosphere) and GRACE-FO (two satellites measuring gravity field variations by tracking the precise distance between them) require spacecraft to maintain centimeter-level relative positioning accuracy. Each spacecraft continuously estimates its own position and the relative geometry, computes correction maneuvers, and executes them — all autonomously. The Prisma mission (2010–2014) was a dedicated technology demonstration of autonomous formation flying and rendezvous algorithms in LEO.

Rendezvous and proximity operations (RPO) — bringing two spacecraft together — represents the most demanding autonomy challenge. NASA's Orbital Express (2007) demonstrated fully autonomous rendezvous and docking between a servicer and a client satellite. More recently, Northrop Grumman's Mission Extension Vehicle has autonomously docked with commercial GEO satellites to extend their operational lives. The autonomous GNC (Guidance, Navigation, and Control) algorithms for RPO combine relative navigation sensors (LIDAR, cameras, GPS differential) with trajectory optimization and control laws that must handle the complex orbital mechanics of close-proximity flight.

---

**CASE STUDY: STARLINK'S AUTONOMOUS COLLISION AVOIDANCE**

SpaceX operates more than 6,000 active Starlink satellites as of early 2026, with plans to grow to tens of thousands. No human workforce could manage collision avoidance for a constellation that size. SpaceX addressed this by building what they describe as a "self-driving" satellite system.

Each Starlink satellite receives conjunction data — predictions of close approaches with other objects in the Space-Track catalog — and runs a neural network that evaluates the collision probability, the uncertainty in the predicted trajectories, the cost of maneuvering (fuel, schedule disruption, downstream conjunction risks), and the risk of creating new conjunctions by maneuvering. If the system determines a maneuver is warranted, the satellite executes it autonomously, without waiting for ground approval.

This is a significant departure from historical practice, where every maneuver decision was a human call. The tradeoff is real: autonomous systems can react faster and at a scale impossible for human operators, but they also introduce coordination risks. In 2021, China filed a complaint with the UN Committee on the Peaceful Uses of Outer Space, alleging that Chinese space station crew members had to take emergency evasive action twice to avoid Starlink satellites. The collision avoidance automation that makes Starlink manageable at scale also creates new coordination challenges that the space community is still working through.

The underlying ML approach — using neural networks to evaluate complex, multi-variable decisions under uncertainty in real time — is directly analogous to how autonomous vehicles handle hazard avoidance. The physics is different (orbital mechanics vs. road geometry) but the architectural pattern is the same.

---

**PAPER SUMMARY**

Chien, S., Sherwood, R., Tran, D., Cichy, B., Rabideau, G., Castano, R., Davies, A., Mandl, D., Frye, S., Trout, B., Shulman, S., and Boyer, D. "Using Autonomy Flight Software to Improve Science Return on Earth Observing One." AIAA Journal of Aerospace Computing, Information, and Communication, Vol. 2, No. 4, 2005, pp. 196–216. DOI: 10.2514/1.12923. PDF: https://ai.jpl.nasa.gov/public/documents/papers/chien-JACIC2005-UsingAutonomy.pdf

This paper answers a central question in spacecraft autonomy: can onboard AI actually improve science return compared to traditional ground-controlled operations? The authors describe the Autonomous Sciencecraft Experiment (ASE) deployed on NASA's Earth Observing One (EO-1) satellite, which used JPL's ASPEN/CASPER planning system to enable the satellite to detect scientifically interesting events — volcanic eruptions, floods, wildfires — directly from onboard sensor data, replan its own observation schedule, and downlink the most valuable data during its next ground contact. The key finding is a clear empirical yes: EO-1 with ASE autonomously detected and responded to dozens of events (including the 2002 eruption of Nyiragongo volcano in the Democratic Republic of Congo) that would have been missed under conventional pre-scheduled operations. This paper remains a landmark reference because it moved spacecraft autonomy from theoretical promise to demonstrated operational benefit on a real NASA mission.

---

**FURTHER READING**

1. Rabideau, G., and Benowitz, E. "Prototyping an Onboard Scheduler for the Mars 2020 Rover." ICAPS Workshop on Planning and Scheduling for Space, 2017.
   https://ai.jpl.nasa.gov/public/documents/papers/rabideau_icaps_2017.pdf
   A look at how JPL extended scheduling autonomy to Mars rovers, where communication delays make onboard replanning essential.

2. ESA — Autonomy in Space Systems: Technology and Concepts
   https://www.esa.int/Enabling_Support/Space_Engineering_Technology/Onboard_Computers_and_Data_Handling/Autonomy_and_On-Board_Software
   ESA's overview of autonomy frameworks, the ECSS autonomy levels, and ongoing autonomy technology development programs.

3. Jewison, C., and Erwin, R.S. "A Problem Formulation for Stochastic Rendezvous and Proximity Operations." AIAA SPACE 2016 Conference.
   https://arc.aiaa.org/doi/10.2514/6.2016-5677
   Covers the stochastic optimization formulation for autonomous RPO under uncertainty — the mathematical foundations of proximity operations GNC.

4. Krage, F., and Sherwood, R. "Onboard Autonomy on the IPEX CubeSat Mission." IEEE Aerospace Conference, 2015.
   https://ieeexplore.ieee.org/document/7119109
   A compact case study of deploying CASPER-derived autonomy software on a CubeSat, showing that spacecraft autonomy is no longer limited to flagship NASA missions.

---

**KEY TAKEAWAY**

The most important shift in spacecraft autonomy is not any single algorithm — it is the move from spacecraft that execute plans to spacecraft that make plans, transforming satellites from passive instruments into active agents pursuing scientific and operational goals on behalf of their operators.

---

*Coming up on Day 16: Reinforcement Learning for Orbital Mechanics and Maneuver Planning*

## Callback Question
> On Day 13, we covered Edge AI and onboard satellite processing, including the constraints of running inference with limited compute and power budgets. How does moving toward fully autonomous satellite operations — including onboard FDIR and real-time decision-making — amplify or change those edge computing constraints compared to running a single inference task like cloud detection?

## Feynman Prompt
> In 2-3 sentences, explain onboard fault detection, isolation, and recovery (FDIR) as if describing it to a colleague who has never heard of it.

## Visual References

![KSC-04pd1638](https://images-assets.nasa.gov/image/KSC-04pd1638/KSC-04pd1638~thumb.jpg)
*KSC-04pd1638* — Source: [NASA](https://images.nasa.gov/details/KSC-04pd1638)
