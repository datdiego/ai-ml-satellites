# Day 8: Signal Processing and Anomaly Detection in Satellite Systems
**Phase:** FUNDAMENTALS  
**Date:** March 08, 2026

---

## Prediction Prompt
> Satellites generate thousands of sensor readings per second — temperature, voltage, orientation, signal strength. Before reading today's lesson, take a guess: if you were designing a system to automatically detect when something is about to go wrong on a satellite, would you rely on fixed threshold alerts (e.g., "temperature > 80°C = alarm") or a machine learning model that learns normal patterns over time, and what's the biggest weakness of whichever approach you'd choose?

## Lesson

Here is the Day 8 lesson:

---

DAY 8: SIGNAL PROCESSING AND ANOMALY DETECTION IN SATELLITE SYSTEMS

When a battery cell on a spacecraft starts quietly degrading six months before it fails, there is no mechanic to pop the hood. The only window into a satellite's health is the telemetry stream — thousands of sensor readings per second, flowing down from orbit. Getting anomaly detection right is the difference between a planned contingency and a multi-hundred-million-dollar loss.

Today we go deep on how satellite operators monitor their assets, how machine learning is transforming that process, and how the physics of radio signals in orbit creates unique engineering challenges.


SATELLITE TELEMETRY: THE VITAL SIGNS OF A SPACECRAFT

A modern satellite generates an enormous amount of self-monitoring data. Telemetry — shorthand for "remote measurement" — is the continuous stream of sensor readings that ground operators use to assess spacecraft health. It is structured under standards like CCSDS (Consultative Committee for Space Data Systems), which defines how bytes are framed, timestamped, and transmitted in the downlink.

The telemetry breaks down into four main categories:

Power subsystem telemetry tracks solar array output voltage and current, battery state of charge, battery cell temperatures, and power distribution unit switching states. Solar arrays degrade roughly 1-3% per year from radiation, so operators trend these values over years to project remaining mission life. A sudden drop in solar array current on one wing could indicate shadowing, a stuck deploy mechanism, or cell damage from a debris strike.

Thermal telemetry covers dozens to hundreds of temperature sensors distributed across the spacecraft structure, propulsion system, electronics boxes, and payload instruments. Satellites experience extreme thermal cycling — on the sunlit side, surfaces can reach +120°C; in eclipse, they drop to -150°C. Thermal anomalies often precede hardware failures: a heater that stops cycling properly, or a component running 15°C hotter than historical averages, frequently signals impending trouble.

Attitude control telemetry includes gyroscope angular rate readings, star tracker quaternions (the spacecraft's calculated pointing direction), reaction wheel speeds, and magnetometer field measurements. The Hubble Space Telescope has had six gyroscope failures since launch in 1990, with each one detectable in the telemetry before the gyro fully failed.

Payload health telemetry depends on the instrument type — a synthetic aperture radar will report pulse repetition frequency, duty cycle, and data volume; a communications transponder will report signal-to-noise ratios, gain states, and channel error rates. Each payload has its own fingerprint of normal behavior.

A single large satellite might have 500 to several thousand telemetry channels. A constellation of hundreds of small satellites multiplies this by orders of magnitude. Manual monitoring simply does not scale.


TIME-SERIES ANOMALY DETECTION: FROM STATISTICS TO MACHINE LEARNING

The classical approach to satellite anomaly detection is limit checking: engineers define a red limit (definitely wrong) and a yellow limit (investigate) for every channel. If battery voltage drops below 24.5V, an alarm triggers. This works for gross failures but misses subtle degradation patterns and context-dependent anomalies — a reaction wheel spinning at 3,000 RPM might be normal in one orbital geometry and anomalous in another.

Statistical methods improved on simple limit checking. CUSUM (Cumulative Sum Control Chart) detects persistent drift by accumulating small deviations over time, catching slow trends that instantaneous thresholds miss entirely. Moving average and exponential smoothing filters reduce noise and make trend detection more reliable. These approaches are interpretable and computationally cheap, but they require manual parameter tuning and struggle with the non-stationarity of satellite data (orbital effects create periodic patterns that look like anomalies to a naive detector).

Machine learning approaches have shifted the paradigm in two important ways.

Isolation Forests, introduced by Liu et al. in 2008, work by randomly partitioning data using decision trees. Anomalous points are "isolated" more quickly than normal points because they occupy sparse regions of feature space. For satellite telemetry, you train an isolation forest on months of nominal operations data, then score new readings by how quickly they get isolated. A score close to 1.0 means anomalous; near 0.5 means normal. The method handles high-dimensional data well and requires no labeled anomaly examples — critical in a domain where true anomalies are rare.

Autoencoders take a different approach. You train a neural network to compress telemetry readings into a low-dimensional latent representation and then reconstruct them. On normal data, reconstruction error is low. When something unusual happens, the autoencoder — having never seen that pattern — produces high reconstruction error, flagging the anomaly. The trick is training exclusively on nominal data so the model learns what "normal" looks like. One operator reported using an autoencoder on Intelsat propulsion telemetry to detect thruster valve anomalies six weeks before they would have been caught by limit checking.

LSTM-based methods go further by modeling time: they predict what the next telemetry value should be, given the recent history. The residual between prediction and actual observation is the anomaly signal. This temporal modeling captures dependencies that snapshot-based methods miss — the fact that, say, a reaction wheel speed should follow a specific ramp profile during a momentum dump maneuver.


RF SIGNAL PROCESSING: WATCHING THE SPECTRUM

Satellite communications happen across a shared and increasingly crowded radio spectrum. Signal processing for spectrum management is a distinct discipline from telemetry anomaly detection, though both involve detecting when something unexpected is happening.

Spectrum monitoring involves continuously measuring signal power across frequency bands to verify that satellites are transmitting where and when they should, that transponder gain states are correct, and that no unauthorized signals are present. Software-Defined Radio (SDR) receivers — which perform signal processing in software rather than dedicated hardware — have democratized this capability. A ground station with an SDR and appropriate antenna can generate a waterfall plot (frequency vs. time vs. power) that makes interference immediately visible.

Interference detection is a persistent operational headache. Adjacent-channel interference occurs when a nearby transmitter bleeds power into an adjacent frequency band. Co-channel interference happens when two signals share the same frequency, often due to reuse planning errors or satellite orbital repositioning. Intermodulation interference is a non-linear mixing product — when two strong signals at frequencies f1 and f2 combine in a non-linear amplifier, they produce spurious tones at frequencies like 2f1 - f2, which can fall right in the middle of another satellite's band.

Jamming identification requires distinguishing intentional interference from accidental. Narrowband (spot) jamming targets a specific frequency with a strong continuous wave or modulated signal. Barrage (wideband) jamming floods an entire frequency band with noise. Sweep jamming rapidly sweeps a narrowband jammer across a wide range. Each has a distinct spectral and temporal signature. Cyclostationary feature detection — which exploits the periodic statistical properties of modulated signals — is a standard technique for identifying signal types even in low signal-to-noise environments. Neural networks trained on signal spectrograms have more recently shown strong performance in automatic modulation classification (AMC).


DOPPLER SHIFT: THE LEO COMMUNICATION CHALLENGE

A satellite in low Earth orbit travels at approximately 7.5 km/s. From the perspective of a ground station, the satellite is approaching at high speed, then receding — and this creates a Doppler frequency shift that is impossible to ignore.

The Doppler shift is: Δf = f₀ × (v_r / c), where v_r is the radial velocity component (the velocity directly toward or away from the observer) and c is the speed of light.

At L-band (1.5 GHz), maximum Doppler shifts reach roughly 37-40 kHz across a pass. At Ka-band (26.5 GHz), the same orbital velocity produces shifts of up to 660 kHz. A typical LEO pass lasts about 8-12 minutes, during which the Doppler shifts from positive maximum (approaching), through zero (at closest approach), to negative maximum (receding).

Ground station modems handle this in two ways. Pre-Doppler correction uses the satellite's predicted trajectory (from TLE sets or precision ephemerides) to tune the transmit and receive frequencies ahead of time, keeping the actual residual Doppler small. Automatic Frequency Control (AFC) loops then track and correct whatever small residual error remains in real time. For link-layer protocols, the rapid Doppler change also affects carrier phase, requiring careful modem design to avoid cycle slips.

For LEO constellations like Starlink, which operate at altitudes of 340-560 km, the problem is compounded because each user terminal must hand off between satellites every few minutes, re-establishing Doppler tracking with each new satellite in view.


REAL-WORLD APPLICATIONS

Application 1: NASA's SMAP and Curiosity Rover Telemetry (JPL Anomaly Detection)

NASA's Jet Propulsion Laboratory operates some of the most complex spacecraft ever built. The SMAP (Soil Moisture Active Passive) satellite and the Mars Science Laboratory (Curiosity rover) both generate continuous telemetry that JPL engineers must monitor. With hundreds of channels and years of data, automated anomaly detection is essential. This is the subject of today's featured paper.

Application 2: ESA's SMART-OLAF System

The European Space Agency's mission control center in Darmstadt, Germany operates a tool called SMART-OLAF (Spacecraft Monitoring and Anomaly Response Toolkit — On-Line Anomaly Finder). It uses a combination of statistical models and neural networks to flag unusual behavior in ESA's fleet, including the Sentinel Earth observation satellites and deep space missions like Mars Express. ESA has published on using one-class classification and isolation forests for this purpose.

Application 3: Commercial Satellite Fleet Management

Commercial operators managing GEO communication satellites have used ML-based predictive maintenance to extend satellite life. Battery health prediction is one key application: GEO satellites spend roughly 45 days per year in Earth's shadow (eclipse), forcing their batteries to discharge and recharge. By fitting ML models to years of battery telemetry, operators have been able to predict battery end-of-life 12-18 months in advance, enabling revenue protection through early orbit adjustments or transponder capacity planning.


PAPER SUMMARY

Hundman, K., LaDDaga, V., Baker, L., Imholt, T., and Gormasz, M. — "Detecting Spacecraft Anomalies Using LSTMs and Nonparametric Dynamic Thresholding." Proceedings of the 24th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining (KDD 2018). DOI: 10.1145/3219819.3219845 | arXiv: 1802.04431.

This paper from NASA's Jet Propulsion Laboratory addresses a core operational challenge: how to automatically detect anomalies in spacecraft telemetry without relying on fixed, manually-set thresholds. The authors trained LSTM networks on nominal (non-anomalous) telemetry to predict expected channel values, then used a nonparametric dynamic thresholding approach — one that adapts to the local distribution of prediction errors — to flag values that deviate more than expected. They evaluated the method on real telemetry data from the SMAP satellite and the MSL Curiosity rover, covering 245 labeled anomalies across 82 telemetry channels. The approach outperformed static threshold methods and other anomaly detection algorithms, and the authors released both the code (github.com/khundman/telemanom) and a sanitized version of the dataset publicly, making this one of the few real-world spacecraft anomaly benchmarks available to the research community.


FURTHER READING

1. The telemanom codebase and the SMAP/MSL dataset. Kyle Hundman's GitHub repository includes the full implementation and a link to download the labeled telemetry data. It is one of the only publicly available real spacecraft anomaly datasets and is worth working through hands-on.
GitHub: https://github.com/khundman/telemanom

2. Malhotra, P. et al. — "LSTM-based Encoder-Decoder for Multi-sensor Anomaly Detection." ICML 2016 Anomaly Detection Workshop. arXiv: 1607.00148. This paper laid important groundwork for LSTM-based anomaly detection that Hundman et al. built upon. It is a clear and readable introduction to the sequence-to-sequence approach.
arXiv: https://arxiv.org/abs/1607.00148

3. ESA Advanced Concepts Team — papers on machine learning for space operations. The ACT has published work on using Gaussian processes and isolation forests for spacecraft telemetry. Their publications page is a good entry point.
https://www.esa.int/gsp/ACT/projects/

4. Scikit-learn documentation on Isolation Forest and One-Class SVM. If you want to implement anomaly detection on a telemetry-like dataset today, these two estimators are the fastest path. The scikit-learn user guide covers both with worked examples.
https://scikit-learn.org/stable/modules/outlier_detection.html


KEY TAKEAWAY

A satellite cannot call for help — it can only send data, which means the quality of your anomaly detection is precisely the quality of your early warning system, and that early warning is often the difference between a recoverable anomaly and a total spacecraft loss.

---

## Callback Question
> On Day 3, we covered the onboard computing constraints of satellite hardware, including limited processing power and memory. Given those constraints, how would you decide whether to run anomaly detection (such as an LSTM-based model) onboard the satellite in real time versus downlinking raw telemetry for ground-based processing — and what tradeoffs does each approach involve?

## Feynman Prompt
> In 2-3 sentences, explain nonparametric dynamic thresholding as if describing it to a colleague who has never heard of it.

## Visual References

![color flow ultrasonography (Doppler) of a carotid artery - scanner and screen](https://upload.wikimedia.org/wikipedia/commons/thumb/6/64/CarotidDoppler1.jpg/960px-CarotidDoppler1.jpg)
*color flow ultrasonography (Doppler) of a carotid artery - scanner and screen* — Source: [Wikipedia (Etan J. Tal, CC BY 3.0)](https://en.wikipedia.org/wiki/Doppler%20effect)
