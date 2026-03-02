# Day 1: How Satellites Communicate
**Phase:** FUNDAMENTALS  
**Date:** March 02, 2026

---

## Prediction Prompt
> If you were designing a radio link between a satellite 36,000 km away and a ground station, what do you think would be the single biggest limiting factor on how much data you can receive — the transmitter power on the satellite, the size of the antenna on the ground, or the frequency you choose — and why?

## Lesson

Here is the Day 1 lesson:

---

When Ukrainian forces coordinated battlefield operations using commercial Starlink terminals in 2022, or when a research vessel in the Southern Ocean pulls down oceanographic data from a polar-orbiting satellite, the underlying physics is the same — electromagnetic waves negotiating hundreds to thousands of kilometers of near-vacuum. Understanding how that works, and why the choices engineers make are the ones they make, is the foundation for everything in this course.


THE BASICS: UPLINK, DOWNLINK, TRANSPONDERS, AND GROUND STATIONS

Every satellite communication system has three core elements: a ground station, a spacecraft in orbit, and the radio links between them.

The uplink is the signal traveling from the ground up to the satellite. The downlink is the signal traveling from the satellite back to Earth. These two directions almost always use different frequencies — a practice called frequency division duplexing — to prevent the satellite's powerful outgoing transmission from drowning out the much weaker signal arriving from Earth.

At the heart of most communications satellites is the transponder: a device that receives an uplink signal, amplifies it, shifts it to a new frequency, and retransmits it as a downlink. A single large geostationary satellite might carry 24 to 48 transponders operating in parallel, each handling a different channel, customer, or geographic beam. The frequency shift is deliberate — it prevents the onboard transmitter from interfering with the onboard receiver.

Ground stations complete the link. For traditional geostationary broadcast satellites, a ground station antenna might be 7 to 15 meters in diameter. For Starlink's low-Earth-orbit network, the user terminal is the size of a laptop — feasible because the satellites are only 550 km away rather than 35,786 km. Mission operations centers typically operate a separate set of dedicated ground stations specifically for TT&C functions, covered next.


TT&C AND CCSDS STANDARDS

Every satellite needs a dedicated communication channel just to stay alive. This is the TT&C subsystem — Telemetry, Tracking, and Command.

Telemetry is the continuous downlink stream of health and status data: battery voltage, temperature at key nodes, attitude angles, memory usage, payload status. This is the satellite's heartbeat. Engineers watch telemetry before issuing any commands.

Tracking is how ground stations determine the satellite's position and velocity. By measuring the Doppler shift of the received signal and the round-trip travel time of a ranging tone, operators compute an accurate state vector and can propagate the orbit forward in time.

Command is the reverse flow. Ground operators send instructions: repoint the antenna, toggle a payload, dump stored observation data, adjust the orbit. Security on the command uplink is critical — unauthorized command injection can end a mission. Authentication and encryption schemes are standard in modern systems.

To make all of this interoperable across agencies and manufacturers worldwide, the Consultative Committee for Space Data Systems (CCSDS) publishes a family of open international standards. The Space Packet Protocol defines packet format and structure for data exchanged between ground and spacecraft. The CCSDS File Delivery Protocol (CFDP) provides reliable file transfers over an unreliable radio link. The Advanced Orbiting Systems (AOS) standard defines a frame-based data link layer suited for high-rate downlinks. NASA, ESA, JAXA, ISRO, and the vast majority of civil and scientific space agencies have adopted these standards, which means a European ground station can — in principle — exchange data with a Japanese spacecraft using the same protocol stack you would read in a publicly available document.


FREQUENCY BANDS: L, S, C, X, KU, KA, V

Choosing the right frequency band is one of the earliest and most consequential decisions in satellite system design. Each band involves a fundamental tradeoff between propagation behavior, available bandwidth, antenna size, and regulatory constraints.

L-band (1–2 GHz): Low frequencies penetrate vegetation, rain, and the ionosphere well. GPS operates at L1 (1575.42 MHz) and L2 (1227.60 MHz). Inmarsat's maritime and aeronautical communications use L-band. The tradeoff is limited bandwidth and large antenna apertures needed for high gain.

S-band (2–4 GHz): The workhorse of TT&C for many spacecraft. NASA uses S-band for the International Space Station's primary link. Weather surveillance radar (the U.S. NEXRAD network) operates here. Good propagation, moderate bandwidth, well-understood regulatory landscape.

C-band (4–8 GHz): Traditional home of satellite television distribution and VSAT networks. Rain fade resistance is better than at higher frequencies, which is why broadcast networks and tropical operators favor it. The European Sentinel-1 synthetic aperture radar uses C-band for its imaging payload.

X-band (8–12 GHz): Favored by military and government Earth observation operators. The German TerraSAR-X and Italian COSMO-SkyMed radar constellations use X-band for their SAR payloads. NASA's Deep Space Network (DSN) uses X-band at 8.4 GHz to communicate with interplanetary probes. Higher frequencies enable narrower beams and more precise pointing with a given antenna size.

Ku-band (12–18 GHz): This is what brought direct-to-home satellite television to consumers — the smaller wavelength allows a 45 cm dish instead of a 3-meter C-band installation. First-generation Starlink uses Ku-band for user downlinks. Rain fade becomes a design concern at Ku and is addressed through link margin and adaptive coding.

Ka-band (26.5–40 GHz): High-throughput satellite (HTS) systems live here. The available bandwidth is large, enabling hundreds of Mbps per spot beam. HughesNet, second-generation Starlink, and Viasat's commercial broadband satellites all use Ka-band. The penalty is severe rain fade and more demanding antenna pointing tolerances.

V-band (40–75 GHz): Millimeter-wave frequencies with very high bandwidth potential. Atmospheric oxygen absorption creates an unusable window near 60 GHz, but flanking sub-bands remain viable. Experimental inter-satellite laser and RF links, as well as future ultra-high-capacity ground links, are being tested here. This band will matter more as spectrum congestion drives the industry upward in frequency.


SCIENTIFIC VS. DEFENSE VS. COMMERCIAL SATELLITES

The same physics governs all three categories. The mission priorities diverge significantly.

Scientific satellites prioritize data fidelity and volume. The James Webb Space Telescope downlinks approximately 28.6 gigabytes of science and engineering data per day through a 4-hour Ka-band communications window routed through NASA's Space Network relay satellites. Latency is essentially irrelevant — data collected 1.5 million kilometers from Earth ages just fine while waiting for a downlink window.

Defense satellites prioritize security, resilience, and availability under adversarial conditions. The U.S. Advanced Extremely High Frequency (AEHF) constellation uses frequency-hopping spread spectrum, nuclear hardening, jam-resistant antennas with null-steering, and end-to-end encryption rated for the most sensitive traffic. The communications architecture is largely classified, but the physics — and the requirement to operate when an adversary is actively trying to deny the link — shapes every design choice.

Commercial satellites are cost-optimized. They use commercially available frequency allocations, standardized off-the-shelf modems, and aggressive frequency reuse through spot-beam technology. A modern high-throughput satellite uses hundreds of narrow spot beams to reuse the same Ka-band frequencies dozens of times across different geographic regions, achieving aggregate throughput measured in terabits per second from a single spacecraft.


LINK BUDGETS: THE MATH BEHIND THE SIGNAL

A link budget is the engineer's accounting ledger for a radio link. It tracks every gain and loss a signal experiences from the transmitter to the receiver and tells you whether the link closes — that is, whether the received signal will be strong enough to decode correctly.

The received power at the terminal is the transmitted power, plus the transmit antenna gain, minus the free-space path loss, plus the receive antenna gain, minus atmospheric attenuation, pointing losses, and polarization mismatch, minus the noise power at the receiver.

Free-space path loss dominates and grows with both distance and frequency. The formula is: FSPL = 20 * log10(4 * pi * d / lambda), where d is the distance and lambda is the wavelength. At Ka-band (30 GHz) communicating with a geostationary satellite at 35,786 km, the free-space path loss alone exceeds 213 dB. That is a factor of roughly 10^21 in power — you need very efficient antennas on both ends of the link.

The signal-to-noise ratio at the receiver must exceed the threshold required by the modulation and coding scheme. Higher-order modulations like 64-QAM pack more bits per hertz but require much better SNR to decode without errors. This is why link budgets drive so many satellite design decisions: they determine required transmitter power, antenna diameter, orbital altitude, and the choice of modulation. A link budget is not a single calculation — it is a living document that gets revised throughout a satellite's design life.


REAL-WORLD APPLICATIONS

Starlink's LEO Architecture: SpaceX's Starlink constellation exceeded 7,000 satellites in orbit by early 2026 and achieves round-trip latency of 20–40 ms — a factor of 20 improvement over geostationary broadband — precisely because of its 550 km altitude. The shorter path dramatically reduces free-space path loss, enabling small, flat user terminals. The tradeoff is that each satellite is visible from any ground point for only a few minutes, requiring a dense constellation with inter-satellite handoffs and routing logic, plus a large ground station network to handle traffic.

NASA's Deep Space Network and Voyager 1: The DSN communicates with Voyager 1, now over 24 billion kilometers from Earth. At X-band (8.4 GHz), the one-way signal travel time exceeds 22 hours. The DSN uses 70-meter dish antennas and cryogenically cooled low-noise amplifiers to detect Voyager's 23-watt transmitter signal after it has been attenuated across interstellar space. This is the link budget problem taken to its absolute limit — every fraction of a decibel in antenna efficiency or receiver noise temperature matters.

Iridium NEXT for Global IoT: The Iridium NEXT constellation of 66 operational satellites uses L-band links to user devices and Ka-band cross-links between satellites, creating a mesh network with no blind spots — including the poles. This architecture supports aircraft Automatic Dependent Surveillance-Broadcast (ADS-B) tracking, maritime AIS data collection, and personal emergency locator beacons. The low data rates (kilobits per second) are a deliberate choice: L-band propagation makes it reliable through weather and for small, low-power devices that need global reach above everything else.


PAPER SUMMARY

Kodheli, O., Lagunas, E., Maturo, N., Sharma, S.K., Shankar, B., Querol, J., Duncan, J.C.M., Chatzinotas, S., Ottersten, B., Gomez-Vilardebo, J., and Bhavani Shankar, M.R. "Satellite Communications in the New Space Era: A Survey and Future Challenges." IEEE Communications Surveys and Tutorials, vol. 23, no. 1, pp. 70–109, first quarter 2021. DOI: 10.1109/COMST.2020.3028247

This paper asks how satellite communications must evolve as low-cost LEO mega-constellations, miniaturized satellites, and 5G integration pressure incumbent architectures that were designed around geostationary satellites in a regulated, slow-moving industry. The authors survey the full protocol stack — physical layer waveforms and multiple-access schemes, constellation design, channel modeling, spectrum coexistence, and emerging applications from broadband internet to narrowband IoT. Their central finding is that non-terrestrial networks (NTN) — satellites, high-altitude platform stations, and UAVs — are transitioning from isolated infrastructure to integrated layers of a unified global network, creating new interference management, handover, and spectrum coordination challenges that the space industry has not had to solve at the scale that constellations like Starlink introduce.


FURTHER READING

1. CCSDS Blue Books (official space data systems standards): The complete set of published protocol standards, including the Space Packet Protocol, CFDP, and AOS, is freely available at https://public.ccsds.org/publications/bluebooks.aspx — the primary reference for anyone implementing or integrating with satellite data systems.

2. ITU Radio Regulations and Space Services: The International Telecommunication Union publishes binding international frequency allocation agreements. The ITU-R Space Services page at https://www.itu.int/en/ITU-R/space/Pages/default.aspx provides access to the Radio Regulations, orbit filing databases, and coordination procedures that govern every satellite frequency allocation on Earth.

3. Pratt, T., Bostian, C., and Allnutt, J. — "Satellite Communications," 2nd edition (Wiley, 2003): The standard engineering textbook on satellite communications, covering link budgets, modulation, multiple-access techniques, and system design with worked numerical examples. Available through university library access or standard technical booksellers.

4. Maral, G. and Bousquet, M. — "Satellite Communications Systems: Systems, Techniques and Technology," 5th edition (Wiley, 2009): A comprehensive engineering reference that complements the Pratt textbook with strong coverage of VSAT networks, broadcast architectures, and mobile satellite systems. The link budget analysis chapters are among the most thorough in the open literature.


KEY TAKEAWAY

Every decision in satellite communications — which frequency band, which orbit, how big the antenna, how much transmit power — ultimately traces back to a single equation: the link budget, which is just the universe's accounting of how much signal survives the journey.

## Feynman Prompt
> In 2-3 sentences, explain the link budget as if describing it to a colleague who has never heard of it.
