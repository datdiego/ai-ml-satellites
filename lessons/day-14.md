# Day 14: Space Debris Tracking and Collision Avoidance
**Phase:** INTERMEDIATE TO ADVANCED  
**Date:** March 14, 2026

---

## Prediction Prompt
> Given that there are over 1.2 million pieces of debris larger than 1cm orbiting Earth, and traditional physics-based collision probability calculations can take hours to run for each potential conjunction, what do you think is the biggest challenge ML faces when trying to automate collision avoidance — and why might it produce too many false alarms rather than too few?

## Lesson

Here is the complete Day 14 lesson:

---

**Subject: Day 14 — Space Debris Tracking and Collision Avoidance**

**MakeMeAnExpert: AI/ML in Satellites and Space Systems**
**Day 14 of 30 — Space Debris Tracking and Collision Avoidance**

---

OPENING

On February 10, 2009, two satellites collided 790 kilometers above Siberia. One was an active Iridium communications satellite. The other was a defunct Russian military spacecraft called Cosmos 2251. Neither operator received a timely warning. The collision destroyed both spacecraft instantly and generated over 2,000 trackable debris fragments — a cloud that still threatens satellites in low Earth orbit today, more than 15 years later.

That incident was a wake-up call. Space is not as empty as it looks, and the problem is getting dramatically worse. Today you will learn why debris tracking is one of the most urgent applications of machine learning in the space industry, what tools exist to address it, and how ML researchers are now turning this into a competitive benchmark problem.

---

THE KESSLER SYNDROME: WHEN SPACE BECOMES A MINEFIELD

In 1978, NASA scientist Donald Kessler and colleague Burton Cour-Palais published a paper describing a terrifying chain reaction scenario. At some threshold of debris density, collisions between objects would generate more debris, which would trigger more collisions, which would generate even more debris — a self-sustaining cascade that could render entire orbital shells permanently unusable. This is the Kessler syndrome.

We are not in a runaway Kessler cascade yet. But we are uncomfortably close to the tipping point in some orbits, particularly low Earth orbit (LEO) between 500 and 1,000 km altitude — exactly where most Earth observation satellites and the Starlink, OneWeb, and Amazon Kuiper mega-constellations operate.

The scale of the problem is staggering. As of 2024, the US Space Surveillance Network tracks approximately 27,000 objects larger than 10 cm in Earth orbit. However, current radar systems cannot detect smaller debris, and statistical models estimate there are over 500,000 objects larger than 1 cm and over 100 million objects larger than 1 mm. An object as small as 1 cm traveling at orbital velocity (roughly 7–8 km/s) carries the kinetic energy of a bowling ball dropped from a skyscraper. A 1 mm particle can penetrate a spacesuit.

Three events dramatically worsened the situation. China's 2007 anti-satellite test, which deliberately destroyed the Fengyun-1C weather satellite, created over 3,000 trackable fragments — the single largest debris-generating event in history. The 2009 Iridium-Cosmos collision added another 2,000+. And Russia's 2021 ASAT test against Cosmos 1408 added roughly 1,500 more trackable pieces, forcing ISS crew to shelter in their Soyuz capsule multiple times.

Without active intervention and smarter traffic management, the orbital environment will only become more dangerous as mega-constellations add thousands of satellites.

---

SPACE SITUATIONAL AWARENESS: KNOWING WHERE EVERYTHING IS

Space situational awareness (SSA) is the collective effort to track, catalog, and predict the positions of objects in orbit. It is the foundation on which any collision avoidance system must rest.

The primary tracking infrastructure in the United States is the Space Surveillance Network (SSN), operated by US Space Command (USSPACECOM). It consists of roughly 30 ground-based radars and optical telescopes spread globally, including the powerful Cobra Dane phased-array radar in Alaska and the Space Fence radar on Kwajalein Atoll in the Marshall Islands. Space Fence, which became fully operational in 2020, can track objects as small as a marble in LEO. The SSN publishes unclassified tracking data — Two-Line Element sets, or TLEs — through Space-Track.org, freely accessible to satellite operators worldwide.

TLEs are compact numerical representations of a satellite's orbit. They encode six Keplerian orbital elements plus a drag term and a timestamp, updated regularly as new observations come in. Think of a TLE as a GPS fix for a satellite — except it tells you not just where the satellite is now but where it will be at any future time, within the limits of the model's accuracy.

Commercial SSA is growing rapidly. LeoLabs operates a network of phased-array radars specifically designed to track small debris in LEO, claiming the ability to reliably catalog objects down to 2 cm. ExoAnalytic Solutions runs a global network of optical telescopes focused on geosynchronous orbit (GEO). A consortium of European agencies including ESA, DLR, and CNES operates the European Space Surveillance and Tracking (EUSST) system. The more sensors watching the sky, the better the catalog quality — and the less likely a real warning gets missed.

---

ML FOR CONJUNCTION ASSESSMENT: PREDICTING CLOSE APPROACHES

A conjunction is when two tracked objects pass close enough that collision risk must be evaluated. With 27,000+ tracked objects, the number of possible pairings is enormous. Most are obviously safe — different orbital shells, wildly divergent inclinations. But filtering down to pairings within, say, 5 km of each other still yields hundreds to thousands of events per day across the entire catalog.

The standard tool for quantifying risk is the conjunction data message (CDM), a standardized format used by USSPACECOM and ESA to alert satellite operators. A CDM contains the estimated closest approach distance, the time of closest approach (TCA), and — critically — the covariance matrices describing positional uncertainty for both objects.

Here is why covariance matters so much: a TLE is not a perfect position fix. Atmospheric drag, solar radiation pressure, and limited observation cadence all introduce uncertainty in where an object actually is. The covariance matrix is a 6×6 structure encoding uncertainty in each dimension of the state vector (position and velocity in three axes) and how those uncertainties are correlated. Collision probability (Pc) is then calculated by integrating a probability distribution over the combined hard-body radius of the two objects.

This sounds straightforward, but the math is extremely sensitive to covariance quality. A poorly calibrated covariance that underestimates uncertainty produces dangerously low Pc values. One that overestimates generates floods of false alarms that desensitize operators. Neither extreme is acceptable.

This is where machine learning enters. Several research groups have demonstrated that ML models — gradient boosted trees, random forests, and neural networks — can learn to predict whether a conjunction will escalate into a serious risk or safely dissolve as updated TLEs refine the orbital estimates. The key insight is that the trajectory of Pc over time carries a lot of predictive signal. Conjunctions that start with moderate Pc and steadily climb tend to be real risks. Those that start high and quickly drop are often artifacts of covariance miscalibration. A recurrent neural network or LSTM trained on historical CDM time series can learn this pattern and flag only the events that truly warrant operator attention.

---

ESA'S CREAM PROJECT: AUTOMATING COLLISION RISK ESTIMATION

ESA's Space Safety Programme has been at the forefront of applying ML to conjunction assessment. The CREAM project (Collision Risk Estimation and Avoidance for Multiple objects) specifically targeted the problem of false alarms overwhelming operators.

Traditional rule-based filters using fixed Pc thresholds generate large numbers of actionable warnings, most of which turn out to be non-events as orbital data improves. Operators at satellite control centers have learned to largely ignore low-probability warnings — a dangerous habituation effect where real risks can get lost in the noise.

CREAM applied machine learning to rerank conjunction events by predicted severity, incorporating features beyond just Pc: object type (active spacecraft vs. debris), TLE age and observation quality, covariance realism scores, and the historical behavior of Pc for similar conjunction geometries. The results were striking: the ML approach achieved approximately 22% fewer false positives compared to the baseline rule-based system while maintaining the same detection rate for true high-risk events. For a satellite operator managing a fleet of dozens or hundreds of spacecraft, that reduction translates directly into fewer unnecessary maneuvers, real fuel savings, and reduced operator fatigue.

ESA has also developed SOCRATES-successor tools that apply these ML techniques to the entire public catalog, providing operators with prioritized, ML-scored conjunction reports as a practical daily operational product.

---

AUTONOMOUS MANEUVER PLANNING: REINFORCEMENT LEARNING FOR AVOIDANCE

Knowing a collision risk exists is only half the problem. The other half is deciding what to do about it. A collision avoidance maneuver (CAM) must be executed early enough to be effective, must not increase risk with other objects, must conserve fuel, and ideally should not disrupt the satellite's mission profile.

Traditional CAM planning is a human-in-the-loop process: an analyst examines the CDM, consults with mission operations, selects a maneuver strategy, and commands the spacecraft. This works for a small constellation, but with Starlink alone deploying thousands of satellites, manual review at scale becomes impossible.

Reinforcement learning is the natural framework for autonomous CAM planning. An RL agent learns a policy — a mapping from observed state to maneuver action — by interacting with a simulated orbital environment and receiving rewards for avoiding collisions while penalizing excessive fuel use and mission disruption.

The challenge in applying RL to this domain is the long time horizon and the multi-body nature of the problem. A single maneuver to avoid one debris object may create a new conjunction with a different object. The agent must reason over future conjunctions simultaneously. Researchers have applied Deep Q-Networks (DQN) and Proximal Policy Optimization (PPO) to simplified versions of this problem, demonstrating that RL agents can learn non-obvious maneuver sequences that avoid multiple debris objects more efficiently than greedy single-conjunction strategies.

SpaceX has implemented a fully automated collision avoidance system for Starlink. It uses TLE data from Space-Track.org, evaluates all conjunctions autonomously, and executes maneuvers without human review when Pc exceeds a threshold. The company has reported executing hundreds of avoidance maneuvers in a single year across its fleet — the first large-scale deployment of autonomous SSA-driven collision avoidance in history.

---

REAL-WORLD APPLICATIONS

The most immediate application is fleet management for mega-constellations. With Starlink targeting 40,000+ satellites and Amazon Kuiper planning 3,236, the conjunction assessment problem scales combinatorially. ML-based Pc prediction and autonomous maneuver planning are not luxuries — they are necessities for operating at this scale safely.

A second application is debris removal mission planning. Proposals for active debris removal (ADR) — such as ClearSpace-1, ESA's first contracted debris removal mission targeting the VESPA upper stage adapter — require precise tracking and approach trajectory planning. ML is being applied to predict the attitude (tumbling) state of debris objects from light curve observations, which is critical for designing a safe capture strategy.

A third application is onboard autonomy for small satellites that lack continuous ground contact. A CubeSat in LEO may pass over a ground station only a few minutes per day, yet a conjunction could occur at any time. Onboard ML inference against a locally cached TLE catalog allows the satellite itself to evaluate risk and potentially execute an avoidance maneuver without waiting for ground command. This is an active research area, particularly for polar-orbit constellations operating at high debris density altitudes.

---

PAPER SUMMARY

Uriot, T., Izzo, D., et al. — "Spacecraft Collision Avoidance Challenge: Design and Results of a Machine Learning Competition." Astrodynamics, 2022.

This paper documents the design, execution, and results of a Kaggle-style machine learning competition organized by ESA's Advanced Concepts Team focused on predicting collision probability from CDM time series. The challenge presented participants with historical sequences of CDMs for real conjunctions and asked them to predict the final maximum Pc value from early in the event's evolution — essentially: given only the first few CDMs in a conjunction, how dangerous will this ultimately become? The winning solutions used gradient boosted models and neural networks that extracted temporal features from the CDM sequence, including the trend and rate of change of Pc over time, covariance scaling factors, and miss distance evolution. The competition demonstrated that ML approaches could achieve substantially better predictive accuracy than simple baseline rules, validating data-driven methods for operational conjunction assessment and providing a reusable public benchmark dataset for the research community.

Full citation: Uriot, T., Izzo, D., Simões, L. F., Abay, R., Einecke, N., Rebhan, S., Martinez-Heras, J., Letizia, F., Siminski, J., & Merz, K. (2022). Spacecraft collision avoidance challenge: Design and results of a machine learning competition. Astrodynamics, 6(2), 121–140. https://doi.org/10.1007/s42064-021-0101-5 | arXiv: https://arxiv.org/abs/2008.03069

---

FURTHER READING

1. Space-Track.org — the official US government source for TLE data and conjunction data messages. Free registration required. Essential for anyone working on SSA problems.
   https://www.space-track.org

2. ESA DISCOS (Database and Information System Characterising Objects in Space) — ESA's public-facing space object catalog with metadata beyond TLEs, including object dimensions and mission history.
   https://discosweb.esoc.esa.int

3. Alfano, S. (2005). "A Numerical Implementation of Spherical Object Collision Probability." Journal of the Astronautical Sciences. The foundational paper on computing collision probability from covariance matrices that underpins nearly all operational CDM processing.
   https://doi.org/10.1007/BF03546397

4. LeoLabs Space Safety Blog — accessible technical articles on radar tracking, debris catalog quality, and close approach analysis from a commercial SSA perspective.
   https://leolabs.space/blog

---

KEY TAKEAWAY

The debris environment is already beyond what human operators can manage manually — ML-powered conjunction assessment and autonomous maneuver planning are not future aspirations but present operational requirements for anyone putting hardware in orbit.

---

Tomorrow: Day 15 — Onboard Processing and Edge AI for Satellites.

## Callback Question
> On Day 13, we explored the severe constraints of running AI models at the edge aboard satellites — limited compute, memory, and power budgets. Given those limitations, what challenges would engineers face when trying to deploy a reinforcement learning model for autonomous collision avoidance directly onboard a spacecraft? Why might those constraints be worth overcoming compared to relying solely on ground-based conjunction assessment, especially as debris populations grow?

## Feynman Prompt
> In 2-3 sentences, explain conjunction assessment as if describing it to a colleague who has never heard of it.

## Visual References

![KSC-05pd-1425](https://images-assets.nasa.gov/image/05pd1425/05pd1425~thumb.jpg)
*KSC-05pd-1425* — Source: [NASA](https://images.nasa.gov/details/05pd1425)

![Kessler syndrome](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Gabbard_diagram.png/500px-Gabbard_diagram.png)
*Kessler syndrome* — Source: [Wikipedia (NASA Orbital Debris Program Office, Public domain)](https://en.wikipedia.org/wiki/Kessler%20syndrome)

![Aegopodium podagraria](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Aegopodium_podagraria1_ies.jpg/500px-Aegopodium_podagraria1_ies.jpg)
*Aegopodium podagraria* — Source: [Wikipedia (Frank Vincentz, CC BY-SA 3.0)](https://en.wikipedia.org/wiki/Space%20debris)

## Quiz (week-2)

1. What is the Doppler shift problem in LEO satellite communications, and why does it require special handling compared to geostationary satellites?
2. Compare the Walker-Delta and Walker-Star constellation configurations. What is one key trade-off between these two approaches in terms of coverage or connectivity?
3. Explain the dual-use dilemma in commercial satellite imagery. Give a specific example of how this tension plays out in practice, and name one regulatory framework designed to manage it.
4. What is the bandwidth bottleneck problem that motivates onboard satellite processing? Describe how ESA's PhiSat-1 mission addressed this challenge with edge AI.
5. What is the Kessler syndrome, and why does it represent an existential risk to the usability of low Earth orbit? How does machine learning help address this threat?
