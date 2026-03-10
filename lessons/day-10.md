# Day 10: Defense vs. Commercial Satellite Systems
**Phase:** FUNDAMENTALS  
**Date:** March 10, 2026

---

## Prediction Prompt
> The U.S. National Reconnaissance Office (NRO) has traditionally operated its own classified imaging satellites, but in recent years it has started purchasing imagery from commercial companies like Planet Labs and Maxar. Why do you think a secretive intelligence agency would pay private companies for satellite imagery instead of relying solely on its own classified systems — and what does this shift suggest about how AI might be changing military intelligence gathering?

## Lesson

Here is the Day 10 email lesson:

---

PREDICTION PROMPT (Answer this BEFORE reading today's lesson)

When commercial satellite companies like Planet Labs or Maxar image an active conflict zone, should the U.S. government have the legal authority to order them to stop selling that imagery — even to allied nations and journalists? What competing interests are at stake, and how would you resolve them?

Write your prediction in answers/day-10.md before reading on.

---

DAY 10: DEFENSE VS. COMMERCIAL SATELLITE SYSTEMS

Welcome to the end of Phase 1. Today we zoom out from the technical and look at the political and economic landscape that shapes which satellites get built, who controls them, and who gets to see what they see. This tension has never been more visible than it was during the 2022 Russian invasion of Ukraine, when commercial satellite operators were publishing imagery of troop movements that would have been classified state secrets a decade ago.

---

MILITARY SATELLITE CATEGORIES

Defense satellites are not a monolith. They fall into several distinct mission types, each with different orbital requirements, sensor packages, and classification levels.

SIGINT (Signals Intelligence) satellites intercept electronic emissions — radio communications, radar pulses, telemetry from missile tests. The U.S. National Reconnaissance Office (NRO) operates SIGINT systems under programs historically codenamed Mentor and Trumpet. These are typically large, geosynchronous satellites with massive deployable antennas that can eavesdrop on communications across entire continents.

IMINT (Imagery Intelligence) satellites are the reconnaissance platforms most people picture — optical and radar systems photographing ground targets. The NRO's KH (Keyhole) series, including the still-operational KH-11, uses mirror-based telescopes comparable in size to the Hubble Space Telescope but pointed downward. Resolution is classified, but open-source analysis puts it below 10 centimeters. Synthetic Aperture Radar (SAR) variants, like the future GEOSAR program, can image through clouds and at night.

COMSAT (Military Communications Satellites) provide secure, jam-resistant communications for military forces. The U.S. Advanced Extremely High Frequency (AEHF) constellation and the Wideband Global SATCOM (WGS) system together handle the bulk of U.S. military data traffic. AEHF uses jam-resistant frequency hopping and anti-spoofing protections that commercial comsats do not.

Early Warning satellites detect ballistic missile launches using infrared sensors that spot the intense heat signature of rocket plumes. The U.S. Space-Based Infrared System (SBIRS) in geosynchronous and highly elliptical orbits provides global launch detection within 25 to 30 seconds of ignition. Russia operates an equivalent system called Tundra (Kupol).

Navigation constellations are the most dual-use category of all. GPS (United States, 31 operational satellites), GLONASS (Russia, 24 satellites), Galileo (European Union, 28 satellites), and BeiDou (China, 35+ satellites) provide positioning, navigation, and timing services to both military and civilian users. Military signals use encrypted codes with higher accuracy; civilian signals are deliberately less precise, though the gap has narrowed.

---

CLASSIFICATION LEVELS AND DATA HANDLING

In U.S. defense practice, intelligence data is classified at three primary levels: Confidential, Secret, and Top Secret. Above Top Secret sits the Sensitive Compartmented Information (SCI) tier, which requires access to specific "compartments" — named programs — beyond a general clearance. NRO imagery products are typically Top Secret/SCI.

The physical and digital infrastructure protecting this data is extensive. Classified networks like SIPRNet (Secret Internet Protocol Router Network) and JWICS (Joint Worldwide Intelligence Communications System) are entirely air-gapped from the public internet. Data from reconnaissance satellites flows through encrypted downlinks to special-purpose ground stations, into secure processing facilities, and then onto these closed networks. Analysts working with raw NRO products do so in Sensitive Compartmented Information Facilities (SCIFs) — rooms with shielded walls, no windows, and strict device policies.

One underappreciated complexity is the "sources and methods" problem. Even telling an ally that a certain factory is producing weapons can reveal what the satellite can see and when it passes over — which itself is classified. This constrains intelligence sharing and creates persistent friction in coalition operations.

---

COMMERCIAL EARTH OBSERVATION: THE NEW LANDSCAPE

The commercial remote sensing industry has undergone a transformation since the early 2000s that would have seemed implausible to Cold War satellite designers. Three companies define the current landscape.

Planet Labs (now Planet) operates the largest fleet of Earth-imaging satellites ever flown — over 200 Dove and SuperDove CubeSats in sun-synchronous low Earth orbit, supplemented by the higher-resolution SkySat constellation. Planet images the entire Earth's landmass every day. Resolution is 3 to 5 meters for the Dove fleet and 50 centimeters for SkySat. The business model is a data subscription: customers pay for access to a data stream rather than individual image orders. Customers include agricultural companies monitoring crop health, financial firms watching parking lots at retail stores, and governments tracking deforestation.

Maxar Technologies operates the WorldView constellation — WorldView-1 through WorldView-4, plus Legion — providing 30-centimeter native resolution imagery. This is the sharpest commercially available optical imagery on the market. Maxar's heritage comes from DigitalGlobe, which was for years the primary NRO commercial imagery contractor. The U.S. government is Maxar's largest single customer by revenue.

BlackSky operates a smaller constellation of 16 satellites optimized for rapid revisit — imaging a specific location multiple times per day rather than maximizing resolution. Their analytics platform combines imagery with AI to monitor port activity, troop movements, and industrial sites with minimal human intervention.

Revenue models differ: Planet sells subscriptions to data streams; Maxar sells high-value individual tasking plus long-term government contracts; BlackSky sells analytics-as-a-service. All three are publicly traded and subject to shareholder pressure that government satellite programs are not.

---

THE DUAL-USE DILEMMA: ITAR, EAR, AND SHUTTER CONTROL

Commercial satellite imagery is inherently dual-use. A 50-centimeter image of a military base provides tactically useful information to any state or non-state actor willing to pay the subscription fee. This has forced regulatory frameworks to evolve quickly.

ITAR (International Traffic in Arms Regulations), administered by the U.S. State Department, controls the export of defense articles and services listed on the U.S. Munitions List. Satellite systems designed primarily for military use, and technical data about them, fall under ITAR. Violating ITAR can result in criminal prosecution and export license revocation. Many commercial satellite companies navigate a careful line to keep their hardware and software under the less restrictive EAR (Export Administration Regulations), which covers dual-use items administered by the Commerce Department.

Shutter control is a specific regulatory power under the Land Remote Sensing Policy Act: the U.S. government can order domestic commercial operators to stop collecting or distributing imagery of specific areas during national security events. In practice, this power has rarely been formally invoked since 2001 — partly because unilateral shutter control is ineffective when European (Airbus, Satellogic) and Asian operators face no such restriction. The government prefers to be a large paying customer, effectively buying exclusivity on sensitive imagery through commercial contracts, rather than ordering companies to go dark.

The Ukraine conflict exposed the limits of all these frameworks. Planet, Maxar, and BlackSky published imagery of Russian troop buildups before and after the invasion began, enabling open-source analysts at Bellingcat and OSINT organizations to track battlefield developments in near-real-time. Russia could not suppress this — the satellites belong to U.S. companies, but the data flows globally. This is a fundamentally new geopolitical reality.

---

HOW AI IS CHANGING THE DEFENSE-COMMERCIAL BOUNDARY

The NRO's Commercial Systems Program Office (CSPO) has been purchasing commercial satellite imagery for years, but the scale and sophistication of this relationship is growing. The intelligence community now formally recognizes commercial imagery as a tier of intelligence collection, not just a supplement to classified systems. The advantage: commercial systems are updated faster and proliferate risk across many satellites rather than concentrating it in billion-dollar exquisite systems.

Project Maven, launched by the Department of Defense in 2017, was the first large-scale DoD deployment of AI for imagery analysis. Initially focused on full-motion video from drones in ISIS surveillance missions, Maven used convolutional neural networks to automatically identify vehicles, people, and structures — tasks that previously required hundreds of human analysts. It created internal controversy when Google, a Maven contractor, withdrew in 2018 following employee protests over military AI work. Maven has since continued under other contractors and expanded to include satellite imagery analysis.

The convergence is clear: NRO buys Planet imagery; Planet data gets ingested into AI pipelines similar to Maven; analysts using JWICS terminals now see commercial and classified imagery side by side. The wall between "defense" and "commercial" is more porous than it has ever been.

---

REAL-WORLD APPLICATIONS

Case 1 — Ukraine: Planet's daily global coverage documented Russian military logistics hubs, hospital destruction, and mass grave sites in Bucha before any government officially acknowledged them. The OSINT community's use of commercial imagery as accountability documentation has permanently changed expectations for future conflicts.

Case 2 — North Korea nuclear monitoring: The James Martin Center for Nonproliferation Studies at Middlebury Institute regularly publishes analysis of North Korean nuclear and missile facilities using commercial Maxar imagery. Activity at the Yongbyon enrichment facility — trucks, steam from cooling towers, excavation — is now tracked by university researchers, not just intelligence agencies.

Case 3 — NRO EOCL contract: The NRO awarded a series of Electro-Optical Commercial Layer (EOCL) contracts to Planet, Maxar, and BlackSky starting in 2020, worth hundreds of millions of dollars, to provide a baseline of unclassified imagery that can be shared with allies and partners who lack Top Secret clearances. This is a deliberate institutional acknowledgment that commercial data fills a role classified systems cannot — shareable intelligence.

---

PAPER SUMMARY

Paikowsky, Deganit. The Power of the Space Club. Cambridge: Cambridge University Press, 2017. DOI: https://doi.org/10.1017/9781108159883

Paikowsky asks why states invest in expensive, strategically marginal space programs and what geopolitical benefits they receive in return. Drawing on case studies from Israel, India, Brazil, and others, she argues that membership in the "Space Club" — the set of nations with indigenous space launch capability — confers prestige, diplomatic leverage, and internal legitimacy that cannot be reduced to purely military or economic utility. Critically for today's topic, she analyzes how intelligence satellite programs became a key currency of strategic signaling during the Cold War, and how that logic persists as commercial capabilities erode the exclusivity that once made space-based intelligence a near-monopoly of superpowers. The book's key finding is that space capability functions as a power multiplier: states with independent access to space can act, verify, and coerce in ways closed to those who cannot see the world from orbit.

---

FURTHER READING

1. NRO.gov — The National Reconnaissance Office's public-facing site includes declassified histories of CORONA, GAMBIT, and HEXAGON imagery satellite programs, giving essential context for how defense IMINT evolved.
   https://www.nro.gov

2. Secure World Foundation — Publishes annual reports on space sustainability, commercial remote sensing policy, and dual-use technology governance.
   https://swfound.org

3. Aerospace Security Project (CSIS) — Center for Strategic and International Studies tracker of space launches, satellite capabilities, and space policy analysis.
   https://aerospace.csis.org

4. Planet Labs Insights Blog — Technical and policy writing directly from one of the major commercial operators, including how they think about responsible data sharing in conflict zones.
   https://www.planet.com/pulse

---

KEY TAKEAWAY

The most important shift in space intelligence is not a new sensor — it is that the data advantage once reserved for superpowers can now be subscribed to by anyone with a credit card.

---

CALLBACK QUESTION

On Day 9 we covered constellation design and network topology for systems like Starlink. How do the tradeoffs in constellation architecture — specifically the choice between many small satellites versus few large ones — differ when optimizing for military survivability versus commercial cost efficiency? What does this suggest about how defense and commercial operators might evolve toward (or away from) similar designs?

Write your answer in answers/day-10.md.

---

FEYNMAN PROMPT

In 2-3 sentences, explain the dual-use dilemma in commercial satellite imagery as if describing it to a colleague who works in software but has never thought about space policy.

Write your explanation in answers/day-10.md.

---

*Day 10 of 30 — MakeMeAnExpert: AI/ML in Satellites and Space Systems*

---

That's the full Day 10 lesson. It covers all five syllabus bullet points, includes the Paikowsky paper summary with DOI, three real-world case studies (Ukraine OSINT, North Korea monitoring, NRO EOCL contracts), four further reading links to real organizations, a prediction prompt, callback question tied to Day 9's constellation topology content, and a Feynman prompt. Approximately 1,750 words in the core lesson.

## Callback Question
> On Day 7, we covered how Synthetic Aperture Radar (SAR) can image through clouds and darkness — capabilities that give it significant tactical advantages over optical sensors. Given today's discussion of the dual-use dilemma and export control frameworks like ITAR and EAR, how might the inherently military-relevant nature of SAR's all-weather, day-night imaging affect how regulators treat commercial SAR providers (like Capella Space) compared to optical Earth observation companies (like Planet Labs), and what implications does this have for AI models trained on SAR data being sold internationally?

## Feynman Prompt
> In 2-3 sentences, explain the dual-use dilemma in satellite imagery as if describing it to a colleague who has never heard of it.

## Visual References

![Planet Labs](https://upload.wikimedia.org/wikipedia/commons/thumb/9/92/Dukono%2C_Indonesia%2C_2016-08-24_by_Planet_Labs.jpg/960px-Dukono%2C_Indonesia%2C_2016-08-24_by_Planet_Labs.jpg)
*Planet Labs* — Source: [Wikipedia (Planet Labs, Inc., CC BY-SA 4.0)](https://en.wikipedia.org/wiki/Planet%20Labs)
