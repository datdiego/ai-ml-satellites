# Day 26: AI-Powered Mega-Constellations and Space-Based Computing
**Phase:** EXPERT-LEVEL AND CUTTING-EDGE RESEARCH  
**Date:** March 26, 2026

---

## Prediction Prompt
> If you wanted to run a data center in orbit, what do you think would be the hardest engineering challenge to solve — and how does it differ from building a data center on Earth?

## Lesson

Here is the Day 26 lesson, written in plain text per your format requirements:

---

MAKEMEANEXPERT: AI/ML IN SATELLITES AND SPACE SYSTEMS
Day 26 of 30: AI-Powered Mega-Constellations and Space-Based Computing

---

OPENING

When Amazon built its first data center, nobody predicted it would eventually rent computing power to half the internet. Now, multiple governments and corporations are asking the same question one level up: what if the data center was in orbit?

In the past two years, this has gone from science fiction to regulatory filings, feasibility studies, and funded engineering programs. Today we unpack why moving AI compute to space is being taken seriously, who is building what, and what the physics actually allows.

---

WHY ORBIT AT ALL?

Before diving into the players, it's worth asking the obvious question: why would anyone put a data center in space when land-based data centers are cheaper, easier to cool, and already exist?

There are three serious answers.

First, solar power. In low Earth orbit, the sun shines about 90% of the time (you pass through Earth's shadow only briefly), and there is no atmosphere to absorb or scatter sunlight. Solar irradiance in orbit is roughly 1,361 watts per square meter, compared to a global average of around 170 W/m² on the surface after accounting for weather, night, and angle. If you can generate power cheaply via large thin-film solar arrays in orbit and convert it directly to compute, you sidestep the land, permitting, cooling infrastructure, and grid costs that dominate terrestrial data center economics.

Second, latency to global users. A mega-constellation at low Earth orbit (roughly 300–600 km altitude) can place a satellite within milliseconds of any point on Earth. If inference is happening on-orbit rather than being routed to a ground data center, you can achieve edge-compute latency for any user, anywhere, without building physical infrastructure in every country.

Third, strategic and sovereignty arguments. Nations that cannot build competitive semiconductor fabs are nonetheless capable of launching satellites. Compute in orbit, especially if distributed, is harder for any single government to seize or shut down.

---

SPACEX'S ORBITAL AI COMPUTE CONCEPT

In 2024, documents and filings associated with SpaceX began sketching out an extraordinarily ambitious concept: a constellation of up to one million satellites functioning collectively as a distributed orbital data center. To put that in perspective, Starlink currently operates roughly 6,000 satellites — the largest operational constellation ever built. A million-satellite compute network would be two orders of magnitude larger.

The core idea is that Starship's dramatically reduced cost-to-orbit (targeting under $100 per kilogram to LEO at scale) changes the economic calculus entirely. If launch costs fall enough, you can treat satellite hardware the way cloud providers treat commodity servers: deploy in massive volume, accept individual failures, and manage the network as a software-defined resource pool.

The practical engineering challenges are severe. At a million satellites, orbital debris management, spectrum coordination, and inter-satellite link topology become extraordinary problems. But SpaceX's filings signal that they view this not as a distant dream but as an eventual commercial product category. Given that Starlink already operates the world's largest satellite network, SpaceX has both the launch capacity and operational experience to move faster than any competitor.

---

PROJECT SUNCATCHER (GOOGLE)

Google Research's Project Suncatcher is the most technically detailed public exploration of space-based AI compute to date. The concept centers on deploying satellites equipped with Google's Tensor Processing Units (TPUs) — the same custom silicon that runs Google's largest AI models on the ground — into orbit, powered by large deployable solar arrays.

The name "Suncatcher" reflects the power generation strategy. Rather than trying to compete with terrestrial data centers on a cost-per-FLOP basis using conventional power, the design leans into the unique advantage of space: abundant, uninterrupted solar power. Large thin-film or rigid solar panels in orbit can generate significant power per kilogram of deployed mass, available around the clock without fuel, cooling towers, or grid infrastructure.

The key inference use case is global AI service delivery. A constellation of TPU-equipped satellites could run ML inference workloads — image classification, natural language processing, recommendation systems — with low latency for any user on Earth without routing through a regional ground data center. The model weights live on-orbit; queries go up, answers come down.

---

ASCEND: ESA'S ORBITAL DATA CENTER STUDY

The European Space Agency's ASCEND program — Advanced Space Cloud for European Net zero emission and Data sovereignty — takes a different angle. Rather than framing orbital compute purely as an AI infrastructure play, ASCEND is explicitly motivated by Europe's energy and sovereignty concerns.

The central environmental argument is that space-based solar-powered data centers could offload energy-intensive compute from Earth's grid. European data centers currently consume roughly 2.7% of the continent's electricity, a figure growing rapidly with AI workloads. If orbital solar can power exascale compute without drawing on terrestrial power infrastructure, the net carbon impact could be favorable even accounting for rocket launch emissions — though this depends heavily on launch vehicle reusability.

The sovereignty angle reflects a European concern that dominance in cloud compute is currently concentrated in American and Chinese providers. Orbital infrastructure governed under ESA's multinational framework would give European institutions a compute layer not subject to foreign data-access laws.

ESA completed feasibility studies in 2023–2024 and is targeting an in-orbit demonstration of key enabling technologies by 2028. The demonstration is expected to validate thermal management systems, deployable solar arrays at the required scale, and high-bandwidth optical inter-satellite links.

---

CHINA'S THREE-BODY COMPUTING CONSTELLATION

China's Three-Body Computing Constellation represents the largest publicly announced space compute initiative by satellite count. The program envisions approximately 2,800 satellites forming a distributed computing network in low Earth orbit, capable of providing AI inference and data processing services globally.

The name is a nod to Liu Cixin's celebrated science fiction trilogy, which is deeply embedded in Chinese tech culture — several major Chinese AI and space initiatives have adopted references from it.

The technical architecture emphasizes distributed processing: rather than each satellite being an independent powerful compute node, the constellation is designed to function as a networked system where workloads are partitioned and processed collaboratively across multiple satellites using inter-satellite links. This mirrors the approach taken in distributed ground-based computing clusters, adapted for the orbital environment where individual node capability is constrained by power and mass budgets.

The program is proceeding as part of China's broader push to establish sovereign digital infrastructure independent of Western semiconductor supply chains. Chinese domestic AI accelerators are expected to form the compute substrate, making the Three-Body constellation not just an infrastructure project but a demonstration of China's ability to deploy competitive AI hardware at scale outside the Western ecosystem.

---

ECONOMICS AND PHYSICS: THE HARD CONSTRAINTS

All of these programs run up against the same three physical constraints. Understanding them is essential for evaluating which concepts are viable.

POWER GENERATION. In orbit, solar panels can generate roughly 150–200 watts per kilogram of panel mass for thin-film designs. A modern server-class AI accelerator consumes 300–700 watts. This means generating enough power for serious compute requires significant panel mass per satellite. At constellation scale, launch costs for this mass are the dominant economic factor. The Google Suncatcher paper estimates that at Starship-era launch costs, the economics become viable for certain workload types — but only barely, and only at very large scale.

HEAT DISSIPATION. This is arguably the trickiest constraint. In space, there is no air to convect heat away. The only way to shed waste heat is thermal radiation. A radiator panel at 300 Kelvin can reject roughly 460 watts per square meter. A GPU-class chip generating 300W of waste heat therefore requires more than half a square meter of radiator. At scale, radiator area becomes a serious design driver, competing with solar panel area for spacecraft surface. Adding radiator mass cuts directly into the power-per-kilogram advantage that makes space solar attractive in the first place.

LATENCY. LEO satellites at 550 km altitude have a round-trip signal delay to the ground of roughly 3.6 milliseconds for electromagnetic propagation alone, before any processing time. This is actually competitive with many terrestrial cloud regions for users in underserved geographies. However, the satellite is moving at 7.5 km/s, meaning any given satellite is overhead for only a few minutes before handing off to the next one. Seamless handoff without dropping inference sessions in progress requires sophisticated session management. Inter-satellite optical links (laser crosslinks) can extend session continuity, but this adds cost and complexity.

---

REAL-WORLD APPLICATIONS

APPLICATION 1: GLOBAL EDGE INFERENCE FOR UNDERSERVED REGIONS

Sub-Saharan Africa, rural Southeast Asia, and large parts of South America lack the terrestrial data center infrastructure to deliver low-latency AI services. A dense LEO compute constellation could serve AI-powered medical diagnosis, translation, and educational tools to these regions with latencies competitive with what urban users in developed countries receive from regional cloud data centers today. The business model would resemble satellite broadband — subscription access to compute rather than connectivity.

APPLICATION 2: REAL-TIME EARTH OBSERVATION PROCESSING

Currently, Earth observation satellites collect raw imagery and downlink it to ground stations for processing. Putting AI compute directly on or near the observation satellites allows onboard inference — detecting wildfires, monitoring crop health, tracking ship traffic — before the data hits the ground. This cuts downlink bandwidth requirements dramatically and enables near-real-time alerts without waiting for a ground station pass. ESA's Phi-Sat-2 mission, launched in 2023, demonstrated onboard AI inference for cloud detection as a proof of concept. A compute constellation takes this from single-satellite demonstrations to a networked infrastructure.

APPLICATION 3: JURISDICTION-NEUTRAL FEDERATED LEARNING

While training large foundation models requires dense interconnects difficult to replicate in space, federated learning — where many nodes collaboratively train on local data without sharing raw data — is more network-tolerant. A compute constellation could serve as a neutral, jurisdiction-independent infrastructure for federated training across organizations in different countries, useful for medical imaging or climate modeling consortia that cannot pool data due to legal constraints. The satellite layer provides a trusted intermediary that no single nation's law fully governs.

---

PAPER SUMMARY

Aguera y Arcas, B., and collaborators at Google Research published "Towards a future space-based, highly scalable AI infrastructure system design" in November 2025 (arXiv:2511.19468). The paper addresses whether space-based AI compute is physically and economically feasible at a scale relevant to global AI workloads. The authors perform a detailed engineering analysis of a TPU-equipped LEO constellation architecture, examining power generation via deployable solar arrays, thermal rejection via radiator panels, communication architectures for ground-to-satellite and inter-satellite links, and total-cost-of-ownership projections across a range of launch cost assumptions. Their key finding is that at Starship-class launch costs (below $200/kg to LEO), space-based AI inference becomes cost-competitive with ground data centers for certain high-value, globally-distributed workloads — primarily because the solar power advantage eliminates land, grid, and cooling costs that dominate terrestrial economics. The paper is candid about remaining engineering challenges, particularly thermal management, and frames Project Suncatcher as a long-term research direction rather than an imminent product.

Full citation: Aguera y Arcas, B. et al. "Towards a future space-based, highly scalable AI infrastructure system design." Google Research, 2025. arXiv:2511.19468.
URL: https://arxiv.org/abs/2511.19468

---

FURTHER READING

1. The Project Suncatcher paper (primary source for today's technical content):
   https://arxiv.org/abs/2511.19468

2. ESA's ASCEND initiative — ESA's official program page covering the feasibility study, partner institutions, and the 2028 in-orbit demonstration plan:
   https://www.esa.int/Enabling_Support/Preparing_for_the_Future/Discovery_and_Preparation/ASCEND

3. ESA Phi-Sat-2 mission page — a real, already-operational example of onboard AI inference for Earth observation, useful for grounding the mega-constellation concepts in demonstrated technology:
   https://www.esa.int/Applications/Observing_the_Earth/Phi-sat-2

4. "Space-Based Solar Power" — the European Space Agency's broader space solar power study, which provides the energy generation physics and economics that underpin all orbital compute proposals:
   https://www.esa.int/Enabling_Support/Space_Engineering_Technology/SOLARIS/Space-Based_Solar_Power

---

KEY TAKEAWAY

The race to put AI compute in orbit is no longer a thought experiment — it is an active engineering and policy competition among the US, Europe, and China, driven by the convergence of falling launch costs, rising AI energy demands, and strategic sovereignty concerns, with the laws of physics (especially heat dissipation) serving as the ultimate arbiter of what is actually buildable.

---

*End of Day 26. Tomorrow: Day 27.*

## Callback Question
> On Day 13, we explored the severe constraints of edge AI on individual satellites — limited power budgets, thermal management challenges, and restricted compute capacity. Given those constraints, how does the mega-constellation compute model (like Project Suncatcher or China's Three-Body Computing Constellation) fundamentally reframe the problem, and what new engineering trade-offs emerge when you aggregate thousands of satellites into a distributed orbital data center rather than optimizing a single node?

## Feynman Prompt
> In 2-3 sentences, explain space-based computing trade-offs as if describing it to a colleague who has never heard of it — specifically, why putting a data center in orbit creates unique engineering challenges around power generation, heat dissipation, and latency that don't exist for ground-based systems.

## Visual References

![TDRS-M Liftoff](https://images-assets.nasa.gov/image/KSC-20170818-PH_KAA01_0007/KSC-20170818-PH_KAA01_0007~thumb.jpg)
*TDRS-M Liftoff* — Source: [NASA](https://images.nasa.gov/details/KSC-20170818-PH_KAA01_0007)

![Space-based solar power](https://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/6min-RGB-C2023_P1_%28Nishimura%29.png/960px-6min-RGB-C2023_P1_%28Nishimura%29.png)
*Space-based solar power* — Source: [Wikipedia (SomeAstroStuff, CC BY-SA 4.0)](https://en.wikipedia.org/wiki/Space-based%20solar%20power)
