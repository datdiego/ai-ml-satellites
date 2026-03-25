# Day 25: Quantum Communications in Space
**Phase:** EXPERT-LEVEL AND CUTTING-EDGE RESEARCH  
**Date:** March 25, 2026

---

## Prediction Prompt
> China's Micius satellite demonstrated quantum key distribution between space and Earth in 2017 — but the photons carrying quantum keys must travel through the turbulent atmosphere. Based on what you know about photon physics and atmospheric interference, do you think it's easier to send quantum keys from the satellite down to the ground, or beam them up from the ground to the satellite? Why might one direction be harder than the other?

## Lesson

I'll paste the lesson directly here since the lessons directory doesn't exist in the main repo (it lives in the `ai-ml-satellites` content repo). Here is Day 25:

---

**Subject: Day 25 — Quantum Communications in Space**

---

DAY 25 OF 30: QUANTUM COMMUNICATIONS IN SPACE

---

OPENING

In September 2017, two scientists sat down for a video call. One was in Beijing, the other in Vienna. Nothing unusual about that — except the cryptographic key protecting their conversation had been distributed across 7,600 kilometers of space using individual photons. That call, secured by China's Micius satellite, demonstrated something that would have seemed like science fiction a decade earlier: intercontinental quantum-secured communication via satellite. Today, multiple countries are racing to put their own quantum satellites in orbit, and the geopolitical stakes could not be higher.

---

THE PROBLEM QUANTUM COMMUNICATIONS SOLVES

Before diving into the technology, it is worth understanding why satellite-based quantum communications matters so much. Modern encryption — the kind protecting your banking session and national intelligence channels — relies on mathematical hardness. RSA, for example, depends on the practical impossibility of factoring large numbers with classical computers. Quantum computers, once mature, will break this assumption. Post-quantum cryptography (new math-based algorithms resistant to quantum attacks) is one response. Quantum key distribution (QKD) is the other, and it takes a fundamentally different approach: instead of making eavesdropping computationally hard, it makes it physically detectable.

QKD does not encrypt data directly. It distributes a shared secret key between two parties in a way that any interception disturbs the quantum states being transmitted, revealing the presence of an eavesdropper. Once a clean key is established, it can be used with a classical one-time pad for theoretically unbreakable encryption. The security is guaranteed by the laws of physics, not computational assumptions — which is why some organizations consider it a hedge against future quantum computers breaking classical encryption.

---

THE BB84 PROTOCOL: HOW QUANTUM KEY DISTRIBUTION ACTUALLY WORKS

The workhorse of satellite QKD is BB84, proposed by Charles Bennett and Gilles Brassard in 1984. It is elegant in its simplicity.

The sender (traditionally called Alice) encodes key bits onto individual photons using polarization states. BB84 uses two bases: a rectilinear basis (horizontal = 0, vertical = 1) and a diagonal basis (45 degrees = 0, 135 degrees = 1). Alice randomly picks which basis to use for each photon and randomly picks which bit to send within that basis.

The receiver (Bob) also randomly chooses a basis to measure each incoming photon. After all photons are sent, Alice and Bob publicly compare which bases they used — but not the actual bit values. They keep only the bits where they happened to choose the same basis, discarding the rest. This subset becomes the raw key.

Here is where physics enforces security: if an eavesdropper (Eve) intercepts a photon and measures it, she inevitably disturbs its quantum state because she does not know which basis to use. When Alice and Bob later compare a sample of their key bits, they will find an elevated error rate — typically around 25% if Eve is intercepting everything. A clean channel should have very low error rates, typically under 5-10% depending on optical quality. Above an agreed threshold, the parties abandon the key and try again.

Adapting BB84 for satellite-ground links introduces serious engineering challenges. The satellite must point its telescope at a ground station with sub-arcsecond precision while traveling at roughly 7.5 km/s. Atmospheric turbulence scatters and absorbs photons. The satellite is overhead for only minutes per pass. Daylight operation is nearly impossible because solar photons overwhelm the single-photon detectors. Despite all of this, Micius proved it works.

---

CHINA'S MICIUS SATELLITE: THE LANDMARK DEMONSTRATION

Micius (also called QUESS, Quantum Experiments at Space Scale) launched on August 16, 2016, into a sun-synchronous orbit at roughly 500 km altitude. Named after a 5th-century BC Chinese philosopher who conducted early optical experiments, the satellite carried a quantum key transmitter, an entangled photon source, and a quantum teleportation experiment payload.

The 2017 Nature paper by Liao et al. reported the first satellite-to-ground QKD demonstration. Over ground stations in China's Qinghai province, Micius achieved a secure key rate of approximately 1.1 kilobits per second at a slant distance of 645 km — an improvement of more than 20 orders of magnitude in channel efficiency compared to optical fiber over those distances (because fiber attenuation scales exponentially with length while the satellite-to-ground channel scales with the inverse square of distance plus fixed atmospheric losses).

Perhaps even more impressive, a separate experiment demonstrated entanglement distribution between two ground stations 1,203 km apart, with each station receiving one photon from an entangled pair generated aboard the satellite. This confirmed that quantum entanglement survives the journey from orbit to Earth — a crucial result because entanglement distribution is the basis for quantum repeaters and more advanced QKD protocols.

The September 2017 intercontinental video call between Chinese Academy of Sciences president Chunli Bai in Beijing and Austrian Academy of Sciences president Anton Zeilinger in Vienna used Micius to relay the quantum-secured key, then applied it with classical AES-128 encryption for the audio and video stream. The total quantum-link distance, counting both ground segments and the satellite relay, was approximately 7,600 km.

---

THE GROUND-TO-SATELLITE CHALLENGE: UPLINK QKD

For most of Micius's early experiments, the satellite transmitted photons downward to ground stations — a downlink configuration. This is easier for a practical reason: large telescope apertures needed to collect single photons can be placed on the ground where size and weight are unconstrained, while the satellite carries a more compact transmitter.

Uplink QKD — sending photons from the ground up to the satellite — is inherently harder. The photon beam must traverse the turbulent lower atmosphere before entering the relatively clean upper atmosphere and vacuum. Atmospheric turbulence causes beam wander, spreading, and phase distortions that increase loss and error rates. This matters enormously for building practical networks: a ground-based transmitter is far easier to operate, maintain, and upgrade than a satellite-borne one, and a single satellite equipped with quantum receivers could theoretically accept connections from many different ground nodes.

Research groups in China, Europe, and Canada have demonstrated progressive improvements in uplink QKD, and by 2025 experiments had shown that bidirectional quantum links are operationally feasible — meaning a satellite equipped with quantum receivers can accept QKD sessions from ground stations rather than only distributing keys downward. This is a significant architectural shift toward hub-and-spoke quantum networks where satellites serve as relay nodes accepting uplinks from distributed ground stations.

---

UPCOMING MISSIONS: THE QUANTUM SATELLITE RACE

The success of Micius has triggered a wave of national and commercial quantum satellite programs.

Eagle-1 is the European Space Agency's first dedicated quantum communications satellite, developed under the EuroQCI initiative — the European Quantum Communication Infrastructure. Targeting launch in the late 2026 to early 2027 timeframe, Eagle-1 aims to demonstrate satellite-based QKD across European ground stations and establish interoperability standards for the broader European quantum network. The program reflects both European concern about dependence on non-European satellite communications infrastructure and the long-term vulnerability of classical encryption.

QEYSSat (Quantum Encryption and Science Satellite) is Canada's contribution to the field, developed by the Canadian Space Agency with the Institute for Quantum Computing at the University of Waterloo. QEYSSat is designed primarily as an uplink receiver — the satellite carries the quantum optical receiver while ground stations transmit — directly addressing the uplink challenge described above. Canada's interest is partly geographic: the country's vast distances and sparse population make satellite delivery of quantum keys more practical than building extensive fiber quantum networks across the entire country.

On the commercial side, SealSQ (part of the WISeKey Group) has announced plans for a quantum-secure satellite constellation targeting IoT and critical infrastructure applications. Boeing's Q4S (Quantum for Space) program represents U.S. defense-industrial interest in quantum satellite communications, though technical details remain limited in open literature given its national security applications.

---

THE NSA'S SKEPTICISM: A NECESSARY COUNTERPOINT

It would be incomplete to discuss quantum communications without addressing the most prominent institutional critique. The U.S. National Security Agency has issued public guidance stating that NSA does not recommend QKD for protecting national security systems. The objections are substantive, not reflexive.

First, QKD solves only the key distribution problem — it says nothing about authenticating who is on the other end of the link. Without classical public-key authentication, a man-in-the-middle attacker can insert themselves between Alice and Bob at session setup. QKD therefore still requires classical cryptographic infrastructure to bootstrap authentication, which somewhat undermines the "physics-only" security argument.

Second, QKD hardware is susceptible to implementation attacks. Real detector and laser components have subtle physical imperfections that sophisticated adversaries can exploit to extract key information without triggering the quantum alarm. Third, QKD systems create a denial-of-service vulnerability: an adversary who cannot break the encryption can simply flood the optical channel with photons, forcing key generation to halt.

Finally, post-quantum cryptography — standardized by NIST in 2024 (CRYSTALS-Kyber for key encapsulation, CRYSTALS-Dilithium for digital signatures, and others) — achieves quantum-resistant security using software upgrades to existing networks at a fraction of the cost. The NSA preference is clear: upgrade to post-quantum cryptography now rather than investing in QKD hardware infrastructure.

Where QKD defenders push back is in contexts requiring information-theoretic security — scenarios where even a computationally unbounded adversary cannot break the key — and for future quantum networks where entanglement distribution serves purposes beyond just key exchange, including distributed quantum computing.

---

REAL-WORLD APPLICATIONS

QUANTUM-SECURED FINANCIAL INFRASTRUCTURE: China has deployed a 2,000 km fiber QKD backbone between Beijing and Shanghai, supplemented by satellite relay for intercity segments, connecting multiple financial institutions and government offices. While bandwidth is limited by QKD key generation rates, this network demonstrates that quantum-secured financial data transmission at national scale is architecturally feasible today, not just in the laboratory.

GOVERNMENT AND DIPLOMATIC COMMUNICATIONS: The Beijing-Vienna call in 2017 was essentially a proof-of-concept for quantum-secured heads-of-state communications. Multiple governments are investing in satellite QKD to protect diplomatic channels against long-term adversarial collection — the threat model where an adversary records encrypted traffic today planning to decrypt it once quantum computers mature (called "harvest now, decrypt later"). Satellite QKD provides a channel where this strategy fails regardless of future computing advances.

SPACE SEGMENT TELEMETRY PROTECTION: Satellites themselves are increasingly viewed as targets for cyberattack via their command-and-control uplinks, as demonstrated by the Viasat KA-SAT attack in February 2022. A quantum-secured uplink, where commands are encrypted with QKD-derived keys, would make it physically impossible for an attacker to inject false commands without being detected. Several commercial satellite operators are evaluating this application, particularly for satellites in critical infrastructure roles.

---

PAPER SUMMARY

Liao, S.-K. et al. "Satellite-to-ground quantum key distribution." Nature, 549, 43-47 (2017). DOI: 10.1038/nature23655. arXiv: 1707.00542.

This paper addresses the central question of whether quantum key distribution can be performed from a moving satellite to a ground receiver over atmospheric channels at distances exceeding what any ground-based or fiber-based QKD system could achieve. The team used the Micius satellite to transmit polarization-encoded single photons to ground stations in China's Qinghai province during nighttime passes, with the satellite precisely pointing its 30 cm aperture telescope using a coarse-fine two-stage tracking system accurate to less than one arcsecond. They demonstrated a secure key rate of 1.1 kbps at 645 km slant range, and showed that the satellite-to-ground quantum channel outperforms optical fiber at these distances by many orders of magnitude because the vacuum portion of the path introduces no photon absorption loss. This work established the experimental and engineering foundation for all subsequent satellite quantum communication demonstrations and made the case for a global quantum communication network built on satellite relays.

Full citation: Liao, S.-K., Cai, W.-Y., Liu, W.-Y. et al. Satellite-to-ground quantum key distribution. Nature 549, 43-47 (2017). https://doi.org/10.1038/nature23655

---

FURTHER READING

1. Liao et al. 2017 — free author preprint on arXiv:
   https://arxiv.org/abs/1707.00542
   The primary source for today's lesson. The supplementary materials section contains the full photon loss budget and pointing/tracking system details — essential reading if you want to understand the engineering under the hood.

2. NSA Quantum Key Distribution FAQ:
   https://www.nsa.gov/Cybersecurity/Quantum-Key-Distribution-and-Quantum-Cryptography/
   The NSA's official position paper. Essential for understanding why not everyone embraces QKD and what the practical objections are. Read alongside the QKD literature to form a balanced view.

3. Yin, J. et al. "Entanglement-based secure quantum cryptography over 1,120 kilometres." Nature 582, 501-505 (2020). DOI: 10.1038/s41586-020-2401-y
   The follow-on Micius paper demonstrating entanglement-based QKD over 1,120 km. A significant step beyond the 2017 downlink experiment and directly relevant to device-independent security proofs.

4. NIST Post-Quantum Cryptography Standards (2024):
   https://csrc.nist.gov/projects/post-quantum-cryptography
   The NSA-recommended alternative to QKD. Understanding both the quantum and post-quantum approaches lets you reason clearly about which solution fits which threat model.

---

KEY TAKEAWAY

Quantum communications in space turns the laws of physics into a security guarantee — eavesdropping is not just detectable, it is unavoidable — but the real-world path from laboratory demonstration to global infrastructure still runs squarely through the hard problems of authentication, implementation security, cost, and practical competition from post-quantum cryptography software upgrades.

---

Next up — Day 26: AI for Space Debris Tracking and Collision Avoidance.

## Callback Question
> On Day 18, we covered optical and laser inter-satellite links (ISLs) as high-bandwidth, low-latency communication channels. Quantum key distribution via satellite relies on transmitting single photons or entangled photon pairs — a fundamentally different use of optical links than classical laser comms. How do the pointing accuracy requirements, atmospheric effects, and hardware constraints for QKD differ from those for conventional optical ISLs, and what does this suggest about whether QKD payloads could realistically share hardware with classical laser comm systems on the same satellite?

## Feynman Prompt
> In 2-3 sentences, explain quantum key distribution (QKD) as if describing it to a colleague who has never heard of it.
