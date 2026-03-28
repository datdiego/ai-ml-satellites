# Day 28: AI for Active Debris Removal and In-Orbit Servicing
**Phase:** EXPERT-LEVEL AND CUTTING-EDGE RESEARCH  
**Date:** March 28, 2026

---

## Prediction Prompt
> When a robotic spacecraft needs to grab a piece of tumbling space debris, it can't rely on the debris having cooperative markers or known spin rates — so how do you think a computer vision system might estimate the debris object's 3D orientation and rotation from just a single camera feed, and what makes this harder than standard object pose estimation on Earth?

## Lesson

Here is the complete Day 28 lesson. I'll paste it below since the write to the archived repo was blocked:

---

**Subject: Day 28 — AI for Active Debris Removal and In-Orbit Servicing**

---

**OPENING: THE JUNKYARD ABOVE US**

Imagine trying to catch a spinning refrigerator hurtling through space at 7.5 kilometers per second, with no GPS transponder, no cooperative docking port, and no second chances if you bump it wrong. That is the problem active debris removal engineers are trying to solve — and machine learning is quickly becoming their most important tool.

Low Earth orbit is filling up fast. As of 2025, the US Space Surveillance Network tracks over 27,000 objects larger than 10 centimeters. Estimates put the total population of debris larger than 1 centimeter at roughly 900,000 pieces — each capable of destroying a satellite on impact. The Kessler Syndrome, proposed by NASA scientist Donald Kessler in 1978, describes a cascade scenario: enough collisions generate enough debris to trigger more collisions, potentially rendering certain orbital shells unusable for decades.

The solution is not just stopping the launch of new debris (though that helps). We also need to actively remove what is already there. This is the domain of Active Debris Removal (ADR) — and autonomously rendezvous-ing with, grasping, and deorbiting an uncooperative, tumbling chunk of metal is one of the hardest robotics problems ever attempted.

---

**KEY CONCEPT 1: ACTIVE DEBRIS REMOVAL — THE CAPTURE TECHNOLOGIES**

Active debris removal broadly means any mission designed to capture a piece of non-functional hardware and either deorbit it, move it to a graveyard orbit, or otherwise reduce the collision risk it poses. Several capture mechanisms are under development.

NETS: A chaser spacecraft deploys a weighted net at short range that wraps around the target. The University of Surrey and Airbus tested this concept during the RemoveDEBRIS mission in 2018. A small CubeSat was released as a target and successfully captured with a net in orbit — a world first. The net is then used to drag the target into the atmosphere using a sail or thruster.

HARPOONS: A barbed projectile is fired into the target and physically embedded. RemoveDEBRIS also demonstrated a harpoon capture in 2019, firing into a target panel at roughly 20 meters range. Harpoons are well-suited to rigid debris like rocket bodies but carry a fragmentation risk if the impact is too energetic.

ROBOTIC ARMS: A multi-degree-of-freedom arm extends from the chaser and grabs the target directly. This requires the most precise relative navigation — the arm tip must intercept a slowly tumbling object within centimeters — but it is the most controllable and reduces the risk of generating additional fragments during capture.

LASERS: Ground-based and space-based laser systems can ablate the surface of small debris, generating a tiny thrust impulse that changes the object's orbit enough to cause reentry. Japan's RIKEN institute and others have studied this for sub-centimeter debris too small to capture mechanically but still dangerous. Lasers are best suited for small pieces; each approach has a different sweet spot depending on target size, altitude, and acceptable risk.

---

**KEY CONCEPT 2: COMPUTER VISION FOR NON-COOPERATIVE RENDEZVOUS**

Here is where machine learning enters the picture. A cooperative spacecraft — one designed for servicing — has GPS, visible docking targets, radio beacons, and known geometry. A piece of debris has none of these. It may be slowly tumbling, covered in multi-layer insulation that reflects light unpredictably. The chaser must figure out where the target is, how it is oriented, and how fast it is rotating — from camera images alone.

This problem is called pose estimation: given an image (or sequence of images) of a known object, determine its 6-DOF pose — three-dimensional position (x, y, z) and orientation (roll, pitch, yaw) simultaneously.

Classical approaches used feature matching (finding corners or texture patches and matching to a 3D model) or model-based tracking (fitting a wireframe to detected edges). These work when lighting is cooperative and the target is at close range. They struggle with:

- HARSH LIGHTING: In space, the Sun can be directly behind or to the side of the target, creating extreme contrast. Shadows fall in unusual places that do not match Earth-based templates.
- UNKNOWN TUMBLE RATES: A decommissioned satellite may be spinning at anywhere from fractions of a degree per second to several degrees per second. The pose estimate must track this rotation in real time.
- FEATURELESS SURFACES: Multi-layer insulation (the gold or silver foil covering most spacecraft) is nearly featureless and highly specular, defeating texture-based matching.
- MONOCULAR CONSTRAINT: Most close-range sensors are single cameras. Without a stereo baseline, depth must be inferred from object size and known geometry.

Deep learning approaches — specifically CNNs trained on synthetic renderings of the target object — have shown marked improvements on all these failure modes. A network trained on thousands of synthetic images rendered under varied lighting, background, and viewpoint conditions generalizes better to the real space environment than hand-crafted feature matchers.

---

**KEY CONCEPT 3: ML FOR RELATIVE NAVIGATION**

Pose estimation is a snapshot; relative navigation is the continuous, closed-loop problem of maintaining an accurate estimate of where the target is and how it is moving as the chaser approaches.

MONOCULAR DEPTH ESTIMATION: Because most ADR cameras are single-lens, depth cannot be read directly from disparity. Networks trained on synthetic datasets learn to infer depth from apparent object size against the known 3D model, along with shading cues and motion parallax as the chaser moves. Combined with an orbital mechanics model, even a rough initial depth estimate can be refined rapidly as range closes.

6-DOF POSE TRACKING: Once an initial pose is established, a tracking module propagates the estimate forward using the inertia model of the target (how fast it is expected to be tumbling) and corrects it with each new camera frame. Recurrent architectures and Kalman filter hybrids are common here — the physics provides a strong prior that prevents the network from making physically impossible jumps between frames.

DOMAIN ADAPTATION: Because training data is predominantly synthetic (no one has a tumbling debris field available for labeling), domain adaptation bridges the gap between rendered images and real camera imagery. Domain randomization — randomizing textures, lighting, and background during training, popularized by OpenAI for robotics and adopted extensively in the space community — is a common technique. Adversarial domain adaptation is another, where the network learns feature representations indistinguishable between synthetic and real domains.

The SPEED dataset (Spacecraft Pose Estimation Dataset), released by Stanford's Space Rendezvous Laboratory (SLAB) in 2019, became the community benchmark for this problem. It contains approximately 15,000 synthetic images and 300 real lightbox images of the Tango spacecraft from the ESA PRISMA mission, with annotated 6-DOF poses. It was the basis for the ESA Pose Estimation Challenge, which drew over 150 competing solutions.

---

**KEY CONCEPT 4: IN-ORBIT SERVICING**

Active debris removal is one application of proximity operations technology; in-orbit servicing is the commercial cousin. Instead of capturing debris for deorbit, servicing missions extend the life of functional — but fuel-depleted — satellites in geostationary orbit (GEO).

GEO satellites are expensive ($300–500 million each), operate at 35,786 km altitude where retrieval is costly, and typically end their lives not because of hardware failure but because they run out of propellant for station-keeping. A servicing vehicle that docks with such a satellite and takes over station-keeping duties can extend its revenue-generating life by five to seven years.

Northrop Grumman's Mission Extension Vehicle (MEV) proved this commercially viable. MEV-1 successfully docked with Intelsat 901 in February 2020 — the first commercial in-orbit servicing mission ever performed. The MEV inserted its docking probe into the satellite's liquid apogee engine nozzle (a standardized interface most GEO satellites happen to share) and took over attitude control and station-keeping. The satellite, which had been moved to a disposal orbit awaiting deorbit, was returned to commercial service. MEV-2 repeated the feat with Intelsat 10-02 in April 2021.

---

**KEY CONCEPT 5: CURRENT AND PLANNED MISSIONS**

DARPA RSGS (Robotic Servicing of Geosynchronous Satellites): A DARPA program to develop a robotic servicing vehicle capable of inspecting, repairing, and repositioning GEO satellites. DARPA partnered with Maxar (formerly SSL) on the spacecraft design. RSGS aimed to demonstrate multi-arm manipulation and tool change-out in orbit — capabilities that go beyond MEV's passive docking and represent the most ambitious manipulation capability planned for GEO.

Astroscale ELSA-d (End-of-Life Services by Astroscale — Demonstration): Launched in March 2021, ELSA-d demonstrated a magnetic capture mechanism for debris removal. The mission included a chaser spacecraft and a client (target) spacecraft, both equipped with a magnetic docking plate. The chaser demonstrated the ability to release, track, and re-capture the client — including under tumbling conditions — validating the technology for future operational missions. Astroscale is now developing ELSA-M, a multi-client version targeting OneWeb constellation debris.

ClearSpace-1: ESA's first debris removal mission, contracted to Swiss startup ClearSpace SA, targets the VESPA (Vega Secondary Payload Adapter) upper-stage rocket body left in orbit by a 2013 Vega launch. The mission, targeted for no earlier than 2026, uses four robotic arms to grab the roughly 100-kilogram VESPA object and guide it to reentry together. It is notable as the first ESA mission to purchase debris removal as a service rather than developing the capability in-house.

---

**REAL-WORLD APPLICATIONS**

Case Study 1 — RemoveDEBRIS (Surrey/Airbus, 2018–2019): The RemoveDEBRIS mission demonstrated net and harpoon capture in orbit and also tested a vision-based navigation system using LiDAR and cameras for pose estimation of the target CubeSat. The onboard computer ran a feature-based pose estimator in real time, providing proof of concept for autonomous proximity operations without ground-in-the-loop control. The full mission results were published in Nature Communications in 2020.

Case Study 2 — Northrop Grumman MEV-1/2 (2020–2021): MEV uses an autonomous rendezvous system for the final approach and docking. The system combines GPS (available at GEO), star trackers, and short-range optical sensors to guide the docking probe to the target engine nozzle with centimeter accuracy over the final meter of approach — directly analogous to the ADR navigation problem, but with a cooperative target.

Case Study 3 — ESA PRISMA Mission and the SPEED Dataset (2010–2019): PRISMA was a Swedish Space Corporation mission flying two spacecraft in formation. The Tango spacecraft (the passive member) was imaged extensively from the Mango chaser during proximity operations. These real on-orbit images became the ground-truth foundation for the SPEED dataset, providing the community with the only publicly available real-imagery benchmark for spacecraft pose estimation. The SPEED+ dataset (2021) expanded this with images under more varied lighting conditions.

---

**PAPER SUMMARY**

Sharma, S. & D'Amico, S. (2020). "Neural Network-Based Pose Estimation for Noncooperative Spacecraft Rendezvous." IEEE Transactions on Aerospace and Electronic Systems, 56(6), 4638–4658. DOI: 10.1109/TAES.2020.2999148. arXiv preprint: https://arxiv.org/abs/1906.09868

This paper addresses the core challenge of estimating the 6-DOF pose of a non-cooperative spacecraft from monocular camera imagery, targeting the rendezvous and proximity operations problem. The authors, from Stanford's Space Rendezvous Laboratory (SLAB), propose a two-stage CNN pipeline: a first network (based on MobileNet architecture) predicts a coarse pose by regressing the image locations of known 3D keypoints on the spacecraft, and a second stage refines the estimate by solving the Perspective-n-Point problem to fit those keypoints to the known 3D model geometry. The approach was trained entirely on synthetic images of the Tango spacecraft and evaluated on the SPEED dataset — including real lightbox imagery the network had never seen during training. The key finding is that the keypoint-based formulation achieves state-of-the-art pose accuracy on both synthetic and real images, and is significantly more robust to domain shift than direct pose regression — validating the training-on-synthetic, evaluating-on-real paradigm that has since become standard in the field.

---

**FURTHER READING**

1. SPEED+ Dataset (Stanford SLAB, 2021): The expanded Spacecraft Pose Estimation Dataset with more challenging lighting conditions, available at https://zenodo.org/record/6327547 — essential if you want to benchmark your own pose estimation model.

2. Kisantal, M. et al. (2020). "Satellite Pose Estimation Challenge: Dataset, Competition Design, and Results." IEEE Transactions on Aerospace and Electronic Systems. arXiv preprint: https://arxiv.org/abs/1911.02074 — Summarizes the ESA-sponsored Pose Estimation Challenge results across 150+ competing teams, providing the most comprehensive benchmark comparison available.

3. ESA ClearSpace-1 Mission Overview: https://www.esa.int/Safety_Security/Clean_Space/ClearSpace-1 — ESA's official mission pages describe the mission design, the VESPA target, and the four-arm capture approach, updated as the mission progresses toward launch.

4. Opromolla, R. et al. (2017). "A Review of Cooperative and Uncooperative Spacecraft Pose Determination Techniques for Close-Proximity Operations." Progress in Aerospace Sciences, 93, 53–72. DOI: 10.1016/j.paerosci.2017.07.001 — A thorough survey covering both classical and learning-based approaches, essential background before diving into current deep learning papers.

---

**KEY TAKEAWAY**

The hardest part of cleaning up space is not the physics — it is the perception: teaching a robot to find, track, and safely grasp a tumbling, uncooperative object using only camera images, and machine learning trained on synthetic data is currently the most promising path to making that possible at scale.

## Callback Question
> On Day 13, we explored edge AI and onboard processing constraints for satellites — limited compute, power budgets, and the need for real-time autonomy. How do these same constraints shape the design of AI systems for active debris removal, where a chaser spacecraft must perform 6-DOF pose estimation of a tumbling, non-cooperative target in real time without ground-in-the-loop latency?

## Feynman Prompt
> In 2-3 sentences, explain non-cooperative rendezvous pose estimation as if describing it to a colleague who has never heard of it.

## Visual References

![1200737](https://images-assets.nasa.gov/image/1200737/1200737~thumb.jpg)
*1200737* — Source: [NASA](https://images.nasa.gov/details/1200737)
