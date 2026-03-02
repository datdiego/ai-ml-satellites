# Day 2: Orbital Mechanics for the AI Engineer
**Phase:** FUNDAMENTALS  
**Date:** March 02, 2026

---

## Prediction Prompt
> If minimizing communication latency is your goal, would you place your satellite in a low Earth orbit (~550 km) or a geostationary orbit (~36,000 km) — and what trade-off do you predict you'd have to accept to get that lower latency?

## Lesson

Here is the Day 2 lesson:

---

When SpaceX's Starlink constellation began providing internet to remote areas of Ukraine in 2022, it highlighted something most engineers take for granted: where a satellite lives in space is not just a technical footnote — it is the single most consequential design decision a mission will ever make.


ORBITAL REGIMES: LEO, MEO, GEO, AND HEO

Think of Earth as the center of a set of nested shells, each with its own character, tradeoffs, and tenants.

Low Earth Orbit (LEO) spans roughly 160 to 2,000 km altitude. At this range, a satellite completes an orbit in about 90 to 120 minutes. The International Space Station sits at approximately 408 km. Planet Labs' Dove imaging satellites operate near 475 to 500 km. SpaceX Starlink's primary operational shell sits at around 550 km. The defining advantage of LEO is proximity: objects are close, so signals travel fast and sensors see fine detail. The disadvantage is that no single satellite stays overhead for long — it sweeps by in minutes, then disappears over the horizon.

Medium Earth Orbit (MEO) occupies the band from roughly 2,000 km up to just below geostationary altitude. The most famous residents are navigation satellites. GPS satellites orbit at 20,200 km with a period of approximately 11 hours and 58 minutes — exactly half a sidereal day. This elegant choice means the satellite constellation repeats its ground track every day, simplifying ground-based receiver design. Europe's Galileo system orbits at 23,222 km, and Russia's GLONASS at 19,100 km. MEO is a navigation sweet spot: far enough to see large swaths of Earth simultaneously, close enough to maintain adequate signal strength.

Geostationary Earth Orbit (GEO) sits at precisely 35,786 km altitude over the equator. Here, an orbital period exactly matches Earth's rotation: one sidereal day, or 23 hours, 56 minutes, and 4 seconds. The result is a satellite that appears to hover motionless above a fixed point on the ground. Arthur C. Clarke proposed this concept in 1945, which is why GEO is also called the Clarke orbit. Weather satellites like NOAA's GOES-16 and GOES-18 live here, providing continuous imagery of the Western Hemisphere. So do most commercial TV broadcast satellites and military communications systems. A single GEO satellite can see approximately 40% of Earth's surface (excluding the polar regions). The price: it is a very long way up.

Highly Elliptical Orbit (HEO) is the specialist's tool for situations where GEO has blind spots. The Molniya orbit — named after the Soviet communications satellite series — has a perigee of about 600 km and an apogee of roughly 40,000 km, with a 12-hour period and a critical inclination of 63.4 degrees. At this specific inclination, the gravitational effects of Earth's equatorial bulge cause no apsidal precession, meaning the high point of the orbit stays locked over the northern hemisphere. A satellite spends about 8 of its 12 hours near apogee, drifting slowly over high-latitude regions where GEO satellites sit too low on the horizon. Russia still relies on Molniya-heritage orbits for communications in Siberia and the Arctic. The more recent Tundra orbit (also called Quasi-Zenith) operates similarly with a 24-hour period, used by Japan's QZSS navigation augmentation system.


THE ORBIT-MISSION TRADEOFF MATRIX

Orbit choice is not preference — it is physics imposing constraints on every downstream design decision.

Latency is the most visceral difference. A signal traveling to a GEO satellite and back covers roughly 72,000 km round-trip, introducing a minimum one-way delay of about 240 milliseconds. Round-trip latency exceeds 600 milliseconds before adding any processing time. That is why satellite internet from GEO made video calls painful for decades. A LEO satellite at 550 km imposes a one-way delay of under 2 milliseconds — roughly comparable to a cross-continental fiber hop. Starlink's end-to-end latency of 20 to 40 milliseconds feels like broadband by comparison.

Coverage per satellite scales with altitude. From GEO you see 40% of Earth. From 550 km LEO you see a footprint with a radius of roughly 2,200 km at a modest minimum elevation angle. You need many LEO satellites to cover the globe continuously, but each is cheaper to build and launch.

Revisit time — how often a satellite passes over a specific target — is the critical variable for Earth observation. A single LEO satellite in a 500 km orbit passes over any given point on Earth at most once or twice per day, and the window is short. Planet Labs solved this by deploying more than 200 Dove satellites across multiple orbital planes, achieving at least once-daily global coverage. Radar systems needing frequent revisit have driven companies like Capella Space and ICEYE to build large SAR constellations in LEO.

Power requirements for a communications link scale with the square of distance. The free-space path loss between a LEO satellite at 550 km and a GEO satellite at 35,786 km differs by about 36 decibels — a factor of roughly 4,000 in received power. A GEO communications satellite must either carry a much larger transmitter, use a narrower directional antenna beam, or accept a lower data rate. For onboard machine learning inference, proximity to Earth also reduces the latency of getting data to a ground station for processing.


SUN-SYNCHRONOUS ORBITS: THE EARTH OBSERVER'S WORKHORSE

Earth is not a perfect sphere. Its equatorial bulge — described mathematically by the J2 zonal harmonic coefficient — causes the orbital plane of any non-equatorial satellite to precess over time. For prograde orbits (inclinations below 90 degrees), this precession is westward. For retrograde orbits (inclinations slightly above 90 degrees), it is eastward.

Sun-synchronous orbit (SSO) is an engineered exploit of this effect. By choosing the right combination of altitude and inclination — typically around 97 to 98 degrees for orbits between 400 and 800 km — the orbital plane precesses eastward at exactly the same rate Earth revolves around the Sun: about 0.9856 degrees per day. The result is a satellite whose orbital plane maintains a constant angle relative to the Sun throughout the year.

In practice this means the satellite always crosses the equator at the same local solar time on every pass. A satellite crossing at 10:30 AM local time will always cross at 10:30 AM local time regardless of season. This provides consistent solar illumination angles — the shadows fall the same way on every overpass. For machine learning applications in Earth observation, this is enormously valuable: pixel-level change detection models do not have to account for wildly varying lighting conditions, reducing a major source of false positives. Landsat, Sentinel-2, SPOT, WorldView, and Planet's Dove constellation all use sun-synchronous orbits for this reason.


CONSTELLATION GEOMETRIES

A single satellite is a camera on a moving truck. A constellation is a surveillance network.

Walker constellations are the mathematical framework for designing uniform satellite networks. The Walker Delta notation — written T/P/F — specifies T total satellites distributed across P orbital planes, with F as a phasing parameter controlling the relative angular offset between adjacent planes. GPS's baseline design was a Walker 24/6/2 arrangement: 24 satellites in 6 planes, with 4 per plane. The phasing ensures that at any time, at least four GPS satellites are visible from almost anywhere on Earth's surface — the minimum needed for a 3D position fix.

Polar orbits (inclination near 90 degrees) pass over both poles on every orbit. Because Earth rotates beneath them, a polar satellite eventually samples every latitude strip. Iridium's 66-satellite constellation uses 6 polar planes with 11 satellites each, providing truly global voice and data coverage including Antarctica — the only commercial system that works reliably at the poles. Weather agencies favor polar orbits for atmospheric sounders that need global, uniform sampling.

Inclined planes strike a balance. GPS at 55 degrees inclination covers the populated mid-latitudes more densely than the poles, which see fewer navigation satellites simultaneously. SpaceX's initial Starlink consumer service at 53 degrees inclination similarly prioritized coverage between roughly 53 degrees north and south latitude, encompassing most of the world's population. Subsequent polar shells extended coverage for high-latitude users.


TWO-LINE ELEMENT SETS: THE LANGUAGE OF SATELLITE TRACKING

Every object in Earth orbit has a two-line element (TLE) set — a compact, standardized representation of its orbital state. TLEs were developed by NORAD and remain the universal lingua franca of satellite tracking, published continuously by the US Space Force's 18th Space Control Squadron via Space-Track.org.

A TLE consists of two 69-character lines. Line 1 encodes the satellite catalog number, classification, epoch (the date and time the element set was valid), the first derivative of mean motion (used to model atmospheric drag), and a drag-related term called B-star. Line 2 encodes the six classical orbital elements: inclination, right ascension of the ascending node (RAAN), eccentricity, argument of perigee, mean anomaly, and mean motion in revolutions per day.

From mean motion alone you can immediately compute orbital period. The ISS's mean motion of approximately 15.5 revolutions per day gives a period of about 93 minutes. A GEO satellite's mean motion of 1.0027 revolutions per day corresponds to its ~24-hour period.

TLEs are propagated forward in time using the SGP4 algorithm (Simplified General Perturbations 4) for near-Earth objects and SDP4 for deep-space objects above roughly 6,000 km altitude. SGP4 accounts for atmospheric drag, the J2 and J4 zonal harmonics of Earth's gravitational field, and solar and lunar gravitational perturbations — all in a computationally inexpensive closed-form approximation. Accuracy degrades with time: fresh TLEs predict position to within roughly 1 to 3 km; week-old TLEs for LEO satellites experiencing significant atmospheric drag can be off by tens of kilometers. This matters for collision avoidance, for scheduling ground station contacts, and for georeferencing satellite sensor data.

For AI/ML engineers, this is the practical punchline: TLE propagation with SGP4 is how you answer the question "where exactly was this satellite when it captured this image?" Every georeferenced Earth observation product depends on accurate orbit determination at the time of acquisition. Tools like the Skyfield Python library let you do this in ten lines of code.


REAL-WORLD APPLICATIONS

Planet Labs' 200+ satellite Dove constellation demonstrates Walker geometry in commercial practice. By deploying satellites across multiple sun-synchronous planes at approximately 475 to 500 km altitude, Planet achieves daily global coverage at 3-meter resolution. The consistent illumination from SSO design directly enables their ML pipeline for cloud masking, crop monitoring, and change detection — without SSO, a model trained on morning images would systematically fail on afternoon acquisitions.

The GPS constellation demonstrates Walker geometry's power for navigation: 31 operational satellites in 6 orbital planes at 55 degrees inclination provide global navigation with better than 99.9% availability. Each satellite broadcasts ephemeris data — precise orbital parameters — allowing receivers to compute satellite positions. The underlying mechanics are TLE propagation at higher accuracy, using the broadcast navigation message format defined by the Interface Control Document.

LeoLabs, a commercial space situational awareness company, operates a network of phased-array radars to track LEO objects and generates its own orbital element catalog for objects as small as 2 centimeters in diameter. Their conjunction analysis service — predicting close approaches between satellites — processes millions of orbital state pairs daily and uses machine learning to prioritize collision alerts. This is a live example of orbital mechanics and AI working together in an operational context, at scale, right now.


PAPER SUMMARY

Vallado, D.A. Fundamentals of Astrodynamics and Applications, 4th edition. Microcosm Press/Springer, 2013. ISBN: 978-1881883180.

This textbook addresses the complete mathematical foundation of spacecraft orbit analysis — from the classical two-body problem through numerical integration of perturbed equations of motion to operational orbit determination and prediction. Vallado's approach ties rigorous derivations to practical implementation, including production-quality source code for SGP4/SDP4 propagation in multiple languages. The chapters on orbital elements and propagation models detail precisely how TLEs are generated from radar and optical observations, how the SGP4 algorithm applies perturbation corrections, and where the model's accuracy degrades — knowledge essential for anyone who processes satellite imagery, schedules ground contacts, or builds collision avoidance systems. This is the reference used by US Space Force analysts, mission designers, and academic researchers worldwide; its treatment of coordinate systems (ECI, ECEF, topocentric) and time standards is indispensable for building correctly georeferenced AI/ML pipelines on satellite data.


FURTHER READING

Space-Track.org — the official US government source for TLE data on all tracked Earth-orbiting objects, maintained by the 18th Space Control Squadron, US Space Force. Free registration required.
https://www.space-track.org/

Celestrak (Dr. T.S. Kelso) — long-running TLE archive with extensive documentation on the SGP4 algorithm, downloadable source code for propagation in multiple languages, and supplemental catalog data.
https://celestrak.org/

Skyfield — a pure-Python astronomical library for computing satellite positions, pass predictions, and ground track intersections from TLEs. Clean API, actively maintained, and well-suited for data science workflows.
https://rhodesmill.org/skyfield/

NASA General Mission Analysis Tool (GMAT) — free, open-source flight dynamics software for mission design, orbit propagation, and trajectory optimization. Used by NASA on real missions and excellent for hands-on learning of orbital mechanics concepts.
https://gmat.gsfc.nasa.gov/


KEY TAKEAWAY

Before you write a single line of ML code for a satellite application, know which orbital regime your data comes from — because the orbit determines the latency, the coverage gaps, the illumination geometry, and the positional accuracy of every data point you will ever train on.

## Callback Question
> On Day 1, we examined how satellites communicate with Earth, including the role of signal propagation delay and ground station contact windows. Given today's material on orbital altitudes and periods, explain how the choice between a LEO constellation and a GEO satellite fundamentally changes both the latency characteristics of communication links and the engineering complexity required to maintain continuous ground contact.

## Feynman Prompt
> In 2-3 sentences, explain Two-Line Element (TLE) sets as if describing it to a colleague who has never heard of them.
