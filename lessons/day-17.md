# Day 17: Space Weather Prediction with Machine Learning
**Phase:** INTERMEDIATE TO ADVANCED  
**Date:** March 17, 2026

---

## Prediction Prompt
> Solar flares and coronal mass ejections can damage satellites — but before reading today's lesson, take a guess: what type of satellite data or sensor input do you think a machine learning model would use to predict a solar flare before it happens, and why would accurate prediction even matter if light and radiation travel at nearly the same speed?

## Lesson

Here is the Day 17 lesson:

---

MAKEMEANEXPERT: AI/ML IN SATELLITES AND SPACE SYSTEMS
Day 17 of 30 — Space Weather Prediction with Machine Learning

---

OPENING

In February 2022, SpaceX lost 38 of 49 newly launched Starlink satellites — not to a collision, not to a software bug, but to the Sun. A moderate geomagnetic storm heated Earth's upper atmosphere, increasing drag on the low-orbit spacecraft so significantly that they could not raise their orbits in time and reentered. The estimated loss: around $50 million. Space weather is not an abstraction. It is an operational risk that ML researchers are now working hard to forecast, and the problem turns out to be exactly the kind of messy, high-stakes prediction task that machine learning is built for.

---

SOLAR ACTIVITY FUNDAMENTALS

The Sun is not a steady lamp. It breathes on an approximately 11-year cycle — the solar cycle — measured by tracking the number of sunspots visible on the solar surface. Sunspots are regions of intense magnetic activity that appear darker because strong magnetic fields inhibit convective energy transport, making those areas cooler than their surroundings. At solar minimum, the Sun is relatively quiet. At solar maximum, it is a prolific source of energetic events.

The two most operationally relevant events are solar flares and coronal mass ejections (CMEs), and they are related but distinct phenomena.

Solar flares are sudden, intense bursts of electromagnetic radiation — primarily X-rays and extreme ultraviolet — released when magnetic energy stored in the Sun's atmosphere is abruptly converted to radiation. They travel at the speed of light, reaching Earth in about eight minutes. Their impact is almost instantaneous: a strong flare can cause radio blackouts on the sunlit side of Earth within minutes.

Coronal mass ejections are slower but more physically massive. A CME is a large cloud of magnetized plasma ejected from the Sun's corona, sometimes carrying billions of tons of material. CMEs travel at speeds ranging from a few hundred to over 2,000 kilometers per second. The fastest ones can reach Earth in 15 to 18 hours; slower ones take two to four days. When a CME's magnetic field is oriented opposite to Earth's magnetic field — what scientists call a southward Bz — it reconnects with Earth's magnetosphere and drives a geomagnetic storm.

The solar wind, meanwhile, is the continuous background flow of charged particles streaming from the Sun at roughly 400 to 800 km/s. It is always present. It fills interplanetary space and constantly exerts pressure on Earth's magnetosphere, compressing it on the sunward side and stretching it into a long tail on the opposite side.

---

IMPACT ON SATELLITES

Space weather affects spacecraft in four primary ways, each a distinct engineering problem.

Single-event upsets (SEUs) occur when a high-energy particle — typically a cosmic ray or a proton from a solar energetic particle event — strikes a memory cell or logic circuit and flips its state. A single bit flip can corrupt data or cause a software crash. For most applications this is recoverable, but in critical avionics or guidance systems the consequences can be severe. Satellite designers use radiation-hardened components and error-correcting memory to mitigate this, but shielding adds mass and cost.

Surface charging builds up when energetic electrons from the radiation belts embed in dielectric surfaces — blankets, solar panels, circuit boards. If the voltage difference between charged and uncharged regions becomes large enough, electrostatic discharge occurs. This can damage electronics or cause erroneous signals. Deep dielectric charging, where electrons penetrate into the interior of a spacecraft, is particularly insidious because it is harder to detect until discharge occurs.

Drag increase is the mechanism that destroyed the Starlink satellites mentioned above. During geomagnetic storms, heating of the upper atmosphere causes it to expand upward. At LEO altitudes (below about 600 km), this expansion increases atmospheric density, which increases aerodynamic drag. Satellites must either fire thrusters to compensate or accept orbital decay. For large constellations operating at these altitudes, the operational burden during storm periods is enormous.

Communication blackouts are caused by two separate mechanisms: radio blackouts from solar flares (which occur on the sunlit hemisphere within minutes of a large flare) and satellite navigation errors from ionospheric scintillation during geomagnetic storms. GPS, Galileo, and other GNSS systems rely on the predictability of radio wave travel through the ionosphere. Storms disturb electron density irregularly, causing position errors that can exceed tens of meters — a serious problem for aviation, surveying, and precision agriculture.

---

ML MODELS FOR SOLAR FLARE PREDICTION: CNN CLASSIFIERS ON MAGNETOGRAMS

A magnetogram is a map of the magnetic field on the Sun's surface, produced by instruments like the Helioseismic and Magnetic Imager (HMI) aboard NASA's Solar Dynamics Observatory (SDO). Strong, complex magnetic configurations are precursors to flares. The question is: given a magnetogram image, can a model predict whether a large flare will occur in the next 24 hours?

Convolutional neural networks were a natural fit for this task because magnetograms are images, and CNNs excel at extracting spatial features without manual feature engineering. Researchers have trained CNNs to classify active regions into flare-producing and non-flare-producing categories using labeled historical SDO data going back to 2010. The basic pipeline involves extracting image patches around each active region, augmenting the dataset to handle severe class imbalance (large flares are rare), and training binary or multi-class classifiers.

The class imbalance problem is worth emphasizing. Major X-class flares — the largest category — occur perhaps a few dozen times per solar cycle. Datasets covering years of observations might have thousands of negative examples (quiet active regions) for every positive one. Standard accuracy metrics become misleading: a model that always predicts "no flare" achieves very high accuracy but provides zero operational value. Practitioners in this domain rely on skill scores like the True Skill Statistic (TSS) and the Heidke Skill Score (HSS), which measure improvement over a baseline forecast that accounts for class frequency.

Recent CNN architectures have incorporated multi-channel inputs — combining line-of-sight magnetograms with continuum intensity images and UV emission maps — to give the model richer information about the three-dimensional structure of active regions.

---

NASA/IBM'S SURYA MODEL

The Surya model represents a significant step forward in applying foundation model architectures to heliophysics data. Developed through a collaboration between NASA and IBM Research, Surya is an open-source ML model trained on approximately a decade of solar imagery and data from SDO and related instruments. Rather than being trained for one narrow prediction task, foundation models like Surya are pre-trained on large volumes of domain data and can be fine-tuned for multiple downstream applications — including solar flare classification, active region characterization, and solar wind speed prediction.

The foundation model approach is valuable in space weather for a reason that will be familiar from earlier lessons on onboard AI: labeled data is scarce. Generating training labels requires expert annotation or retrospective event catalogs, and both are limited. A model pre-trained to understand the structure of solar imagery can be fine-tuned for flare prediction with far fewer labeled examples than training from scratch. This is the same transfer learning principle behind ImageNet pre-training in computer vision, now applied to solar physics.

---

GEOMAGNETIC STORM PREDICTION: LSTM NETWORKS FOR DST AND KP

Once a CME or solar wind disturbance reaches Earth, the question shifts from "will something happen?" to "how bad will the storm get?" Two widely used indices summarize geomagnetic storm intensity.

The Dst (Disturbance Storm Time) index measures the strength of the ring current — a belt of energetic particles circling Earth in the inner magnetosphere. During a geomagnetic storm, the ring current intensifies and its magnetic field partially cancels Earth's own field, causing ground-level magnetometers to measure a decrease. Dst values are given in nanoteslas (nT). Mild storms reach about -30 to -50 nT; moderate storms reach -50 to -100 nT; severe storms reach below -100 nT. The legendary Carrington Event of 1859 is estimated to have produced a Dst of around -1,750 nT. The Halloween Storms of October-November 2003 produced values below -400 nT, disrupted power grids in Sweden, and caused GPS outages across large portions of Earth.

The Kp index is a global measure of geomagnetic activity derived from a network of magnetometers worldwide, reported on a scale from 0 (very quiet) to 9 (extreme storm). Operational thresholds for satellite operators are typically set at Kp 5 (minor storm) and Kp 7 (severe storm).

LSTM networks have become the dominant tool for real-time Dst and Kp forecasting because these indices evolve over hours, and forecasting them is inherently a time-series problem with long-range dependencies. A typical architecture ingests a sequence of solar wind measurements from the Deep Space Climate Observatory (DSCOVR) — located at the L1 Lagrange point about 1.5 million km upstream from Earth — including solar wind speed, density, and the critical Bz component of the interplanetary magnetic field. The LSTM learns the temporal patterns that precede large decreases in Dst.

The L1 vantage point gives forecasters roughly 15 to 60 minutes of warning before a solar wind disturbance reaches Earth, depending on solar wind speed. That is a very narrow window for operational response. Improving forecast lead time — ideally to several hours — requires predicting conditions in the solar wind before it reaches L1, using observations of the Sun itself.

---

REAL-WORLD APPLICATIONS

Three cases illustrate these concepts at operational scale.

The NOAA Space Weather Prediction Center (SWPC) provides the official US government forecast for space weather events, including real-time Kp and Dst predictions, 24- and 48-hour solar flare probability forecasts, and geomagnetic storm watches and warnings. SWPC has been integrating ML-based tools alongside its traditional physics-based models, running them in experimental mode while validating against historical events.

The Community Coordinated Modeling Center (CCMC) at NASA Goddard Space Flight Center serves as a validation and benchmarking hub for space weather models. The CCMC runs real-time simulations of solar wind-magnetosphere interaction using models like ENLIL and OpenGGCM, and it maintains the Space Weather Database of Notifications, Knowledge, Information (DONKI) — a machine-readable catalog of CMEs, solar flares, and geomagnetic storms that researchers use to train and evaluate ML models.

For satellite operators specifically, commercial space weather services such as Spaceweather.com and dedicated aerospace providers integrate forecast products directly into mission control software, flagging elevated single-event upset risk, charging risk periods, and drag enhancement predictions for LEO operators. During the Halloween Storms in 2003, NOAA operators lost contact with two NOAA satellites temporarily and the Japanese ADEOS-2 satellite lost power permanently — largely due to surface charging. Post-event analysis drove significant investment in predictive charging models.

---

PAPER SUMMARY

Camporeale, E. (2019). "The Challenge of Machine Learning in Space Weather: Nowcasting and Forecasting." Space Weather, 17(8), 1166-1207. DOI: 10.1029/2018SW002061 | arXiv: 1903.05192

This review paper by Enrico Camporeale at CIRES/University of Colorado Boulder takes a systematic look at why machine learning is both attractive and difficult to apply in space weather contexts. Camporeale surveys the range of ML approaches that have been applied across the field — from support vector machines for flare classification to neural networks for solar wind prediction — and then carefully examines the pitfalls that make space weather a non-standard ML problem: severe class imbalance, small labeled datasets, non-stationarity due to the solar cycle, the need for calibrated uncertainty quantification, and the difficulty of comparing models when the literature uses inconsistent evaluation metrics. The key contribution is less a single technical result than a methodological warning: many published ML models in space weather report impressive accuracy numbers that do not survive rigorous comparison against persistence or climatological baselines, and the field needs more standardized benchmarks and evaluation protocols to make real progress. This paper is essential reading before implementing or evaluating any space weather ML model.

---

FURTHER READING

NOAA Space Weather Prediction Center — real-time forecasts, historical data, and educational resources on all aspects of space weather monitoring and forecasting: https://www.swpc.noaa.gov/

NASA Solar Dynamics Observatory Data — full-disk solar imagery and magnetograms from 2010 to present, freely available for ML research through Helioviewer and the Joint Science Operations Center: https://sdo.gsfc.nasa.gov/data/

NASA Community Coordinated Modeling Center — model validation runs, DONKI event catalog, and space weather model comparisons useful for building ML training datasets: https://ccmc.gsfc.nasa.gov/

Angryk, R.A. et al. (2020). "Multivariate Time Series Dataset for Space Weather Data Analytics." Scientific Data, 7(1), 227. DOI: 10.1038/s41597-020-0548-x — A curated, benchmark-ready dataset of SDO magnetogram-derived features and GOES flare event labels specifically intended for ML research, addressing the reproducibility gap Camporeale describes.

---

KEY TAKEAWAY

Space weather forecasting is a race against a 15-minute warning window, and machine learning's greatest contribution may not be accuracy per se, but the ability to extract predictive signal from a decade of solar observations faster than any human analyst can — which is why rigorous evaluation against proper baselines matters as much as the model architecture itself.

---

*Day 17 of 30 — MakeMeAnExpert: AI/ML in Satellites and Space Systems*

## Callback Question
> On Day 13, we covered Edge AI and onboard satellite processing, where compute and power constraints limit what ML models can run in orbit. Space weather events like geomagnetic storms and solar flares can cause single-event upsets (SEUs) that corrupt memory or flip bits in onboard processors — yet these same events are exactly when reliable autonomous operation matters most. How should an onboard Edge AI system be designed to remain functional and trustworthy during the very space weather conditions it might need to respond to, and what does this imply for model architecture choices or hardware redundancy?

## Feynman Prompt
> In 2-3 sentences, explain geomagnetic storm prediction using LSTM networks as if describing it to a colleague who has never heard of it.

## Visual References

![Cascading Post-coronal Loops](https://images-assets.nasa.gov/image/PIA21598/PIA21598~thumb.jpg)
*Cascading Post-coronal Loops* — Source: [NASA](https://images.nasa.gov/details/PIA21598)

![Drawing of sunspots by Richard Carrington (1826-1875), the English astronomer](https://upload.wikimedia.org/wikipedia/commons/thumb/9/92/Carrington_Richard_drawing_of_1859_sunspots.jpeg/960px-Carrington_Richard_drawing_of_1859_sunspots.jpeg)
*Drawing of sunspots by Richard Carrington (1826-1875), the English astronomer* — Source: [Wikipedia (Richard Christopher Carrington, Public domain)](https://en.wikipedia.org/wiki/Solar%20flare)

![Aurora Borealis viewed during a geomagnetic storm, viewed east of Churchill, Manitoba near the shore of Hudson Bay](https://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Aurora_borealis2%2C_Churchill%2C_MB.JPG/960px-Aurora_borealis2%2C_Churchill%2C_MB.JPG)
*Aurora Borealis viewed during a geomagnetic storm, viewed east of Churchill, Manitoba near the shore of Hudson Bay* — Source: [Wikipedia (anonymous donor, Public domain)](https://en.wikipedia.org/wiki/Geomagnetic%20storm)
