# Day 9: Satellite Constellation Basics and Network Topology
**Phase:** FUNDAMENTALS  
**Date:** March 09, 2026

---

## Prediction Prompt
> Starlink has thousands of satellites in low Earth orbit — do you think they communicate directly with each other in space to route data, or does every data packet have to travel down to a ground station and back up again before reaching its destination? What are the trade-offs of each approach?

## Lesson

Here is the Day 9 lesson:

---

**Subject: Day 9 — Satellite Constellation Basics and Network Topology**

When SpaceX launched its 7,000th Starlink satellite in 2024, it crossed a threshold no company had ever reached: operating a fully functional internet backbone built entirely in orbit. The internet you take for granted on the ground took decades and trillions of dollars of cable to build — Starlink is doing something equivalent in low Earth orbit, and the engineering behind it is far stranger and more clever than most people realize.


**WHAT IS A SATELLITE CONSTELLATION?**

A single satellite in geostationary orbit (GEO, at 35,786 km altitude) can cover about one-third of Earth's surface from its fixed position. For decades, this was the dominant model for satellite communications. But GEO has a brutal limitation: the round-trip signal delay is roughly 600 milliseconds. Try playing a real-time video game or having a VoIP call over that. You cannot.

The solution is to use many smaller satellites at much lower altitudes — low Earth orbit (LEO), typically between 400 and 2,000 km. At 550 km (where Starlink operates), round-trip latency drops to about 20–40 ms, comparable to fiber broadband. The catch: a single LEO satellite only sees a small patch of Earth at any moment and sweeps over any given location in minutes. To maintain continuous coverage, you need dozens or hundreds of them organized in a coordinated pattern — a constellation.

A constellation is not just a bunch of satellites scattered randomly. It is a precisely designed geometric arrangement that guarantees coverage continuity, minimizes gaps, balances capacity across users, and enables handoffs between satellites and ground stations without interrupting service.

Two dominant geometric patterns are used in constellation design:

**Walker-Delta** configurations arrange satellites in inclined circular orbits, all at the same altitude and inclination relative to the equator. The classic notation is T/P/F, where T is total satellites, P is orbital planes, and F is the phasing parameter. Starlink Phase 1 uses a Walker-Delta variant with 72 planes of 22 satellites each at 53-degree inclination. Walker-Delta is good for mid-latitude coverage and is easier to maintain because all planes share the same altitude.

**Walker-Star** configurations use polar or near-polar orbits where planes are distributed longitudinally around Earth, like the ribs of a globe. OneWeb uses a near-polar Walker-Star at about 1,200 km altitude with 648 operational satellites. The Walker-Star pattern provides better coverage at high latitudes and poles — critical for Arctic shipping routes — but creates seams at the poles where adjacent planes converge, which must be managed carefully.


**STARLINK, ONEWEB, KUIPER: ARCHITECTURE COMPARISON**

These three systems represent different bets on how to solve the same problem, and their architectural choices reflect real engineering trade-offs.

**Starlink (SpaceX)** operates at 550–570 km in a multi-shell configuration. The low altitude minimizes latency and requires less transmit power, but satellites experience significant atmospheric drag and have orbital lifetimes of roughly 5 years before they naturally deorbit — so SpaceX must continuously launch replacements. The business advantage: SpaceX owns its own rockets, making this economically viable in a way it would not be for competitors. Starlink uses Ku-band for user terminals and Ka-band for gateway links, with laser inter-satellite links (ISLs) deployed on recent generations — making it the first mass-market constellation to route traffic through space rather than only through ground stations.

**OneWeb** operates at 1,200 km in 12 polar planes. The higher altitude provides longer orbital lifetime (tens of years versus 5) but increases latency compared to Starlink. OneWeb does not currently use ISLs, meaning every packet must touch the ground before being routed onward. OneWeb targets enterprise, government, maritime, and aviation customers rather than mass consumer broadband. After bankruptcy in 2020, it was acquired by a consortium led by the UK government and Bharti Global — a reminder that satellite broadband infrastructure carries real geopolitical weight.

**Amazon Kuiper** will operate at three shells: 590 km, 610 km, and 630 km, with 3,236 satellites total. Kuiper uses Ka-band and plans to leverage AWS ground infrastructure for seamless integration — if your application already runs in AWS, routing traffic through Kuiper directly into AWS data centers becomes architecturally simpler. Kuiper had launched initial prototype satellites and was ramping toward commercial service as of early 2025.

The key design trade-offs across all three: altitude (latency vs. lifetime vs. drag), orbital inclination (coverage pattern vs. collision risk at polar seams), spectrum choice (throughput vs. rain fade vulnerability vs. regulatory constraints), and ground segment strategy (how many gateway stations, where, and how traffic is aggregated).


**INTER-SATELLITE LINKS: THE SPACE-BASED INTERNET BACKBONE**

In early constellations, every bit of data had to touch the ground. A packet from a user in London would go up to a satellite, down to a gateway in the UK, through the terrestrial internet to a data center, and back. The satellite was just a bent pipe — a repeater in the sky.

Inter-satellite links (ISLs) change this entirely. With ISLs, satellites talk directly to each other, forming a mesh network in orbit. A packet from London can travel up to a Starlink satellite, hop across multiple satellites via laser links, and come down near the destination — potentially never touching the public terrestrial internet.

**Radio frequency ISLs** use microwave or millimeter-wave radio to connect satellites. They are technically mature and work in all weather, but bandwidth is limited by available spectrum. Iridium NEXT uses Ka-band radio ISLs across its 66-satellite constellation, enabling pole-to-pole voice routing with no ground infrastructure in the path — which is why Iridium works in Antarctica.

**Optical ISLs** (laser communication terminals, or LCTs) use narrow infrared laser beams. Starlink's optical ISLs operate at 100 Gbps per link and require no spectrum licensing. The challenge is pointing: you are aiming a laser beam less than 10 microradians wide at a target moving at several km/s relative to you, from a platform that is itself vibrating. Modern systems use fine-pointing mechanisms and dedicated acquisition protocols to maintain lock. Because optical ISLs operate in vacuum, atmospheric absorption is irrelevant — the laser only passes through atmosphere at the very start and end of a ground-to-satellite link.

Within a constellation, ISL topology comes in two flavors. **Intra-plane ISLs** connect adjacent satellites within the same orbital plane — these are relatively stable because satellites in the same plane maintain nearly constant separation. **Inter-plane ISLs** connect satellites in adjacent planes — more complex because relative geometry changes as satellites orbit, and near polar convergence zones links may need to be disabled temporarily to avoid pointing conflicts.


**ROUTING IN SATELLITE NETWORKS: THE TOPOLOGY PROBLEM**

Here is the core routing problem: satellites move at roughly 7.5 km/s in LEO. The network topology — which satellite is adjacent to which — changes continuously. In terrestrial networking, protocols like OSPF converge when topology changes, but they assume changes are rare events. In a LEO constellation, topology change is the baseline condition, not the exception.

Several approaches address this:

**Proactive routing** precomputes routing tables for all future states of the constellation and loads them onto satellites in advance. Because satellite orbits are deterministic, you can compute exactly where every satellite will be at every future moment. Starlink reportedly uses a variant of this approach. The limitation is memory and update frequency — tables must be refreshed as the precomputed future becomes the present.

**Adaptive routing** allows satellites to make real-time decisions based on current link states, requiring satellites to exchange topology information as in OSPF. This adds overhead and requires significant onboard processing. Iridium uses a form of adaptive routing across its mesh.

**Ground-assisted routing** keeps routing decisions centralized on the ground, with satellites acting on instructions pushed to them. This simplifies satellite hardware but requires constant uplink contact.

The **intermittent connectivity problem** arises because some links — particularly inter-plane ISLs near the poles and certain ground-to-satellite links — are only available some of the time. Delay-tolerant networking (DTN) protocols, originally developed for deep space missions, are increasingly studied for LEO constellations to handle these disruptions without data loss.


**GROUND SEGMENT ARCHITECTURE**

The space segment is only half the system. The ground segment is where most operational complexity lives:

**Gateway stations** are large antenna installations, typically 2.4–9 meters in diameter, that aggregate traffic between the satellite network and the terrestrial internet. Their geographic distribution is a hard constraint on system capacity — a user can only be served if a gateway can see the same satellite simultaneously. Starlink has hundreds of gateways globally; Kuiper plans to leverage existing AWS data center locations as anchor points.

**User terminals** are the customer-end antennas. Starlink's flat-panel phased-array terminal uses electronically steered beams to track satellites across the sky with no moving parts — a significant engineering achievement at a consumer price point. The terminal automatically selects which satellite to connect to and performs handoffs every 15–20 minutes as satellites pass overhead, seamlessly from the user's perspective.

**Network operations centers (NOCs)** monitor the entire constellation in real time: satellite health, link quality, interference, capacity allocation, and software updates. Managing thousands of autonomous nodes simultaneously has pushed heavy investment in automation and AI-driven monitoring — an area where the space industry is increasingly overlapping with the operations tooling used by cloud hyperscalers.


**REAL-WORLD APPLICATIONS**

**Ukraine and Starlink:** When Russia disrupted terrestrial and geostationary satellite communications in Ukraine beginning in 2022, Starlink terminals provided critical connectivity for military coordination, civil communications, and journalism. This demonstrated both the resilience of LEO mesh networks (harder to jam than a single GEO satellite) and the geopolitical sensitivity of privately owned space infrastructure — Elon Musk's decisions about service availability became foreign policy decisions, whether anyone intended that or not.

**Arctic shipping and OneWeb:** The Northern Sea Route through the Arctic has become commercially significant as ice retreats. Terrestrial connectivity there is nearly nonexistent; GEO satellites cannot see the poles. OneWeb's near-polar Walker-Star constellation provides broadband coverage all the way to 90 degrees north, enabling real-time monitoring, weather updates, and crew communications on ice-class vessels navigating the route.

**Aviation connectivity:** Airlines including Delta, United, and Lufthansa have announced or deployed Starlink terminals for in-flight Wi-Fi. The challenge involves low-profile terminal design for aircraft fuselages and regulatory approval across national airspaces. This is driving demand for higher-throughput Starlink service tiers and pushing new antenna miniaturization work.


**PAPER SUMMARY**

del Portillo, I., Cameron, B.G. & Crawley, E.F. — "A Technical Comparison of Three Low Earth Orbit Satellite Constellation Systems to Provide Global Broadband." Acta Astronautica, 2019. DOI: 10.1016/j.actaastro.2019.03.040

This paper conducts a rigorous side-by-side technical comparison of the LEO broadband constellations proposed by SpaceX (Starlink), OneWeb, and Telesat LEO, evaluating each against consistent metrics: coverage continuity, capacity, latency, and link budget performance. The authors use orbital simulation and link budget analysis to assess global coverage and per-user capacity at different latitudes — not just aggregate figures. They find that all three systems can achieve global coverage, but differ substantially in capacity distribution (Starlink's denser, lower constellation provides higher throughput at mid-latitudes), and that ground station gateway placement is a critical bottleneck that constellation orbital design alone cannot solve. The paper is one of the few peer-reviewed analyses comparing commercial LEO constellation designs using consistent technical methodology rather than marketing projections.

Full citation: del Portillo, I., Cameron, B.G. & Crawley, E.F. (2019). A technical comparison of three low Earth orbit satellite constellation systems to provide global broadband. Acta Astronautica, 159, 123–135. https://doi.org/10.1016/j.actaastro.2019.03.040


**FURTHER READING**

1. Bhattacherjee, D. et al. — "Network topology design at 27,000 km/hour" (ACM CoNEXT 2019): A systems networking perspective on Starlink's ISL routing problem, from researchers who modeled the constellation before ISLs were deployed. https://dl.acm.org/doi/10.1145/3359989.3365407

2. Walker, J.G. — "Satellite Constellations" (Journal of the British Interplanetary Society, 1984): The original paper defining the Walker constellation formalism that the industry still uses today. Foundational for understanding T/P/F notation and coverage geometry.

3. Ansys STK (Systems Tool Kit) — the industry standard tool for constellation coverage analysis, link budget modeling, and orbital simulation. Free academic licenses are available with extensive tutorials: https://www.ansys.com/products/missions/ansys-stk

4. FCC International Bureau filings: All US-licensed satellite constellations file technical parameters publicly. The Starlink and Kuiper license applications contain the actual engineering specs — frequency plans, power levels, orbital parameters — that marketing materials never mention. Searchable at https://licensing.fcc.gov/


**KEY TAKEAWAY**

A satellite constellation is not just a collection of satellites — it is a dynamic, software-defined network topology that happens to orbit at 7.5 km/s, and the hardest engineering problems in building one have less to do with rockets than with routing, ground station placement, and the challenge of maintaining a stable network when nothing about your infrastructure is standing still.

## Callback Question
> On Day 3, we covered onboard computing constraints aboard satellites, including limited processing power and memory. How do these hardware limitations influence the design choices for inter-satellite link (ISL) routing protocols in a large LEP constellation like Starlink, where dynamic topology changes require frequent routing updates that must be computed with minimal onboard resources?

## Feynman Prompt
> In 2-3 sentences, explain inter-satellite links (ISLs) as if describing it to a colleague who has never heard of them.

## Visual References

![A bright artificial satellite flare is visible above the Unit Telescopes of the VLT.](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6a/Flare_at_Paranal.jpg/960px-Flare_at_Paranal.jpg)
*A bright artificial satellite flare is visible above the Unit Telescopes of the VLT.* — Source: [Wikipedia (R. Wesson/ESO, CC BY 4.0)](https://en.wikipedia.org/wiki/Satellite%20constellation)
