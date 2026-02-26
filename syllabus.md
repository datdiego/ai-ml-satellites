# MakeMeAnExpert: AI/ML in Satellites and Space Systems

**30-Day Daily Email Syllabus**
**v2 — 2026-02-26 (all 30 papers verified with DOIs/URLs)**
**Status:** APPROVED — ready for content production

---

## Overview

This syllabus covers the intersection of artificial intelligence, machine learning, and space systems across 30 days. It progresses from satellite communication fundamentals through advanced ML applications to cutting-edge research topics. Each day includes a scientific paper to summarize, giving the reader exposure to primary literature alongside conceptual explanations.

**Audience:** Technical professionals and enthusiasts with basic programming knowledge. No prior satellite or aerospace background assumed.

**Cadence:** One email per day, estimated 10-15 minute read time.

---

## PHASE 1: FUNDAMENTALS (Days 1-10)

### Day 1: How Satellites Communicate

- How satellite communication works: uplink, downlink, transponders, and ground stations
- Packet types and data protocols in satellite communications (TT&C, CCSDS standards)
- Frequency bands overview: L, S, C, X, Ku, Ka, V — what each is used for and why
- Scientific vs. defense vs. commercial satellites: different communication strategies, encryption levels, and data priorities
- Introduction to link budgets: why distance, power, and frequency all matter

**Paper:** Kodheli, O. et al. — "Satellite Communications in the New Space Era: A Survey and Future Challenges" (IEEE Communications Surveys & Tutorials, 2021). DOI: [10.1109/COMST.2020.3028247](https://doi.org/10.1109/COMST.2020.3028247)

---

### Day 2: Orbital Mechanics for the AI Engineer

- LEO, MEO, GEO, and HEO: altitudes, orbital periods, and use cases for each
- Why orbit choice determines latency, coverage, revisit time, and power requirements
- Sun-synchronous orbits and their importance for Earth observation
- Constellation geometries: Walker constellations, polar orbits, and inclined planes
- Two-Line Element (TLE) sets: how satellite positions are tracked and predicted

**Paper:** Vallado, D.A. — *Fundamentals of Astrodynamics and Applications*, 4th ed. (Microcosm Press, 2013). ISBN: 978-1881883180. Selected chapter on orbital elements and propagation models used in modern tracking systems.

---

### Day 3: Satellite Hardware and Onboard Computing

- Anatomy of a satellite: bus, payload, power, thermal, attitude control subsystems
- Onboard computers: radiation-hardened processors vs. COTS (commercial off-the-shelf) hardware
- Processing constraints in space: power budgets, thermal limits, radiation effects (SEUs, latch-up)
- Evolution from simple command execution to onboard autonomy
- Overview of space-qualified AI accelerators: Intel Movidius Myriad 2, Xilinx FPGAs, Ubotica CogniSAT

**Paper:** Furano, G., Meoni, G., Dunber, A. et al. — "Towards the Use of Artificial Intelligence on the Edge in Space Systems: Challenges and Opportunities" (IEEE Aerospace and Electronic Systems Magazine, 2020). DOI: [10.1109/MAES.2020.3008468](https://doi.org/10.1109/MAES.2020.3008468)

---

### Day 4: Introduction to Earth Observation and Remote Sensing

- What remote sensing is: passive (optical, multispectral, hyperspectral) vs. active (radar, lidar) sensors
- Electromagnetic spectrum in remote sensing: visible, infrared, microwave bands
- Spatial, spectral, temporal, and radiometric resolution — the four pillars of image quality
- Key missions: Landsat, Sentinel, MODIS, GOES — what they measure and why it matters
- How raw satellite data becomes usable imagery: preprocessing, atmospheric correction, orthorectification

**Paper:** Zhu, X.X. et al. — "Deep Learning in Remote Sensing: A Comprehensive Review and List of Resources" (IEEE GRSM, 2017). DOI: [10.1109/MGRS.2017.2762307](https://doi.org/10.1109/MGRS.2017.2762307) | arXiv: [1710.03959](https://arxiv.org/abs/1710.03959)

---

### Day 5: Machine Learning Fundamentals for Satellite Data

- Why satellite data is different: massive scale, multispectral bands, temporal sequences, noise characteristics
- Supervised vs. unsupervised vs. self-supervised learning in the context of satellite imagery
- Common ML tasks: classification, object detection, semantic segmentation, change detection
- Feature engineering vs. deep learning: traditional approaches (NDVI, texture features) vs. CNNs
- Key datasets: EuroSAT (27,000 images, 10 LULC classes), BigEarthNet, SpaceNet, xView

**Paper:** Helber, P. et al. — "EuroSAT: A Novel Dataset and Deep Learning Benchmark for Land Use and Land Cover Classification" (IEEE JSTARS, 2019). DOI: [10.1109/JSTARS.2019.2918242](https://doi.org/10.1109/JSTARS.2019.2918242) | arXiv: [1709.00029](https://arxiv.org/abs/1709.00029)

---

### Day 6: Satellite Image Classification with Deep Learning

- CNNs applied to satellite imagery: architecture considerations for multispectral inputs
- Transfer learning from ImageNet to satellite domains: benefits and pitfalls
- ResNet, EfficientNet, and Vision Transformers (ViTs) for satellite scene classification
- Handling class imbalance in land cover classification (oversampling, focal loss, weighted cross-entropy)
- Evaluation metrics: overall accuracy, per-class F1, Cohen's kappa, confusion matrices

**Paper:** Neumann, M., Pinto, A.S., Zhai, X. & Houlsby, N. — "In-Domain Representation Learning for Remote Sensing" (arXiv, 2019; ICLR 2020 Workshop). arXiv: [1911.06721](https://arxiv.org/abs/1911.06721)

---

### Day 7: Introduction to Synthetic Aperture Radar (SAR)

- How SAR works: side-looking geometry, pulse compression, synthetic aperture formation
- Why SAR matters: all-weather, day-and-night imaging capability
- SAR image characteristics: speckle noise, geometric distortions (foreshortening, layover, shadow)
- Polarimetric SAR (PolSAR): single-pol, dual-pol, quad-pol — what information each captures
- Key SAR missions: Sentinel-1, NISAR, TerraSAR-X, Capella Space, ICEYE

**Paper:** Moreira, A. et al. — "A Tutorial on Synthetic Aperture Radar" (IEEE GRSM, 2013). DOI: [10.1109/MGRS.2013.2248301](https://doi.org/10.1109/MGRS.2013.2248301)

---

### Day 8: Signal Processing and Anomaly Detection in Satellite Systems

- Satellite telemetry: what gets monitored (power, thermal, attitude, payload health)
- Time-series anomaly detection: statistical methods vs. ML approaches (isolation forests, autoencoders)
- RF signal processing: spectrum monitoring, interference detection, jamming identification
- Doppler shift handling in LEO communications
- Real-world case: using ML to detect satellite anomalies before component failure

**Paper:** Hundman, K. et al. — "Detecting Spacecraft Anomalies Using LSTMs and Nonparametric Dynamic Thresholding" (KDD 2018). DOI: [10.1145/3219819.3219845](https://doi.org/10.1145/3219819.3219845) | arXiv: [1802.04431](https://arxiv.org/abs/1802.04431) | Code: [github.com/khundman/telemanom](https://github.com/khundman/telemanom)

---

### Day 9: Satellite Constellation Basics and Network Topology

- What is a satellite constellation: definitions, configuration types (Walker-Delta, Walker-Star)
- Starlink, OneWeb, Kuiper: architecture comparison and design trade-offs
- Inter-satellite links (ISLs): radio vs. optical, intra-plane vs. inter-plane connectivity
- Routing in satellite networks: challenges of dynamic topology and intermittent links
- Ground segment architecture: gateway stations, user terminals, network operations centers

**Paper:** del Portillo, I., Cameron, B.G. & Crawley, E.F. — "A Technical Comparison of Three Low Earth Orbit Satellite Constellation Systems to Provide Global Broadband" (Acta Astronautica, 2019). DOI: [10.1016/j.actaastro.2019.03.040](https://doi.org/10.1016/j.actaastro.2019.03.040)

---

### Day 10: Defense vs. Commercial Satellite Systems

- Military satellite categories: SIGINT, IMINT, COMSAT, early warning, navigation (GPS/GLONASS/Galileo)
- Classification levels and data handling: how defense systems protect information in transit
- Commercial Earth observation: Planet Labs, Maxar, BlackSky — business models and data products
- Dual-use dilemma: commercial imagery in conflict zones, regulatory frameworks (ITAR, EAR)
- How AI is changing the defense-commercial boundary: NRO's commercial imagery purchases, Project Maven

**Paper:** Paikowsky, D. — *The Power of the Space Club* (Cambridge University Press, 2017). DOI: [10.1017/9781108159883](https://doi.org/10.1017/9781108159883). Analysis of how space capabilities shape geopolitical power, with focus on intelligence satellite programs.

---

## PHASE 2: INTERMEDIATE TO ADVANCED (Days 11-20)

### Day 11: Semantic Segmentation of Satellite Imagery

- From classification to pixel-level understanding: why segmentation matters for geospatial analysis
- Architecture deep dive: U-Net, DeepLabV3+, Feature Pyramid Networks (FPN), SegFormer
- Encoder-backbone comparisons: ResNet, DenseNet-121, MobileNetV2 for resource-constrained deployment
- Multi-class land cover segmentation: handling 10-50+ classes with hierarchical labeling
- Practical challenge: training with noisy, weakly-annotated, or partially-labeled satellite data

**Paper:** Diakogiannis, F.I. et al. — "ResUNet-a: A Deep Learning Framework for Semantic Segmentation of Remotely Sensed Data" (ISPRS J. Photogramm. Remote Sens., 2020). DOI: [10.1016/j.isprsjprs.2020.01.013](https://doi.org/10.1016/j.isprsjprs.2020.01.013) | arXiv: [1904.00592](https://arxiv.org/abs/1904.00592)

---

### Day 12: Deep Learning for SAR Image Analysis

- SAR despeckling with deep neural networks: from traditional filters to learned approaches
- SAR target detection and classification: ship detection, vehicle classification, infrastructure monitoring
- SAR change detection: coherent vs. incoherent methods, deep learning approaches
- SAR super-resolution: enhancing spatial detail with CNNs (Capella satellite SR models)
- Fusion of SAR and optical data: complementary strengths and multi-modal architectures

**Paper:** Wang, P., Zhang, H. & Patel, V.M. — "SAR Image Despeckling Using a Convolutional Neural Network" (IEEE Signal Processing Letters, 2017). DOI: [10.1109/LSP.2017.2758203](https://doi.org/10.1109/LSP.2017.2758203) | arXiv: [1706.00552](https://arxiv.org/abs/1706.00552)

---

### Day 13: Edge AI and Onboard Satellite Processing

- Why process data in orbit: bandwidth bottleneck (satellites generate TB/day, downlink is limited)
- Onboard AI architectures: neural network inference on FPGAs, GPUs, and neuromorphic chips
- ESA's PhiSat missions: PhiSat-1 (cloud detection) and PhiSat-2 (multi-application AI platform)
- Satellogic's AI-first approach: designing satellites around ML inference requirements
- AI-eXpress constellation: hybrid edge/cloud architecture with blockchain and modular AI deployment

**Paper:** Giuffrida, G. et al. — "CloudScout: A Deep Neural Network for On-Board Cloud Detection on Hyperspectral Images" (Remote Sensing, 2020). DOI: [10.3390/rs12142205](https://doi.org/10.3390/rs12142205) — the PhiSat-1 cloud detection model that flew in orbit.

---

### Day 14: Space Debris Tracking and Collision Avoidance

- The Kessler syndrome: cascading collisions and the growing debris problem (1.2M+ objects > 1cm)
- Space situational awareness (SSA): radar tracking, optical observation, catalog maintenance
- ML for conjunction assessment: predicting collision probability from TLE data and covariance matrices
- ESA's CREAM project: automating collision risk estimation with ML (22% lower false positives)
- Autonomous maneuver planning: reinforcement learning for multi-debris avoidance strategies

**Paper:** Uriot, T., Izzo, D. et al. — "Spacecraft Collision Avoidance Challenge: Design and Results of a Machine Learning Competition" (Astrodynamics, 2022). DOI: [10.1007/s42064-021-0101-5](https://doi.org/10.1007/s42064-021-0101-5) | arXiv: [2008.03069](https://arxiv.org/abs/2008.03069)

---

### Day 15: Autonomous Satellite Operations

- Levels of autonomy in space systems: from scripted commands to fully autonomous decision-making
- Task scheduling and resource allocation: constraint satisfaction and optimization under uncertainty
- Onboard fault detection, diagnosis, and recovery (FDIR): rule-based vs. ML approaches
- Autonomous orbit maintenance: station-keeping, formation flying, rendezvous and proximity operations
- Case study: Starlink's autonomous collision avoidance — neural network-based "self-driving" satellites

**Paper:** Chien, S. et al. — "Using Autonomy Flight Software to Improve Science Return on Earth Observing One" (AIAA JACIC, 2005). DOI: [10.2514/1.12923](https://doi.org/10.2514/1.12923) | PDF: [ai.jpl.nasa.gov](https://ai.jpl.nasa.gov/public/documents/papers/chien-JACIC2005-UsingAutonomy.pdf)

---

### Day 16: AI for Satellite Constellation Management

- Fleet-level optimization: coordinating thousands of satellites for coverage, capacity, and efficiency
- Dynamic spectrum management: ML-driven frequency allocation and interference mitigation
- Predictive maintenance at scale: using telemetry patterns to anticipate component degradation
- Traffic engineering with neural networks: congestion prediction and adaptive routing (Starlink's approach)
- ESA's ConstellAI project: exploring AI for mega-constellation operations and decision support

**Paper:** Stock, G.F., Fraire, J.A. et al. — "On the Role of AI in Managing Satellite Constellations: Insights from the ConstellAI Project" (SpaceOps 2025). arXiv: [2507.15574](https://arxiv.org/abs/2507.15574)

---

### Day 17: Space Weather Prediction with Machine Learning

- Solar activity fundamentals: solar cycle, coronal mass ejections, solar flares, solar wind
- Impact on satellites: single-event upsets, surface charging, drag increase, communication blackouts
- ML models for solar flare prediction: CNN classifiers on magnetogram imagery
- NASA/IBM's Surya model: open-source ML trained on a decade of solar data for flare forecasting
- Geomagnetic storm prediction: LSTM networks for Dst index and Kp index forecasting

**Paper:** Camporeale, E. — "The Challenge of Machine Learning in Space Weather: Nowcasting and Forecasting" (Space Weather, 2019). DOI: [10.1029/2018SW002061](https://doi.org/10.1029/2018SW002061) | arXiv: [1903.05192](https://arxiv.org/abs/1903.05192)

---

### Day 18: Optical and Laser Inter-Satellite Links

- Free-space optical communication: principles, advantages over RF (bandwidth, security, SWaP)
- Pointing, acquisition, and tracking (PAT): the precision challenge of laser links between moving satellites
- Starlink's laser ISL network: architecture, throughput, and latency reduction
- China's 400 Gbps laser ISL record: Three-Body Computing Constellation
- AI-driven beam steering and signal acquisition: automating PAT with ML

**Paper:** Kaushal, H. & Kaddoum, G. — "Optical Communication in Space: Challenges and Mitigation Techniques" (IEEE Communications Surveys & Tutorials, 2017). DOI: [10.1109/COMST.2016.2603518](https://doi.org/10.1109/COMST.2016.2603518)

---

### Day 19: Object Detection and Change Detection from Space

- Object detection architectures for satellite imagery: YOLO variants, Faster R-CNN, DETR
- Challenges unique to overhead imagery: small objects, rotation variance, scale variation, dense packing
- Oriented bounding boxes: why axis-aligned boxes fail for overhead vehicle and ship detection
- Change detection: bi-temporal and multi-temporal approaches, Siamese networks, attention mechanisms
- Applications: urban growth monitoring, deforestation tracking, disaster damage assessment

**Paper:** Chen, H. & Shi, Z. — "A Spatial-Temporal Attention-Based Method and a New Dataset for Remote Sensing Image Change Detection" (Remote Sensing, 2020). DOI: [10.3390/rs12101662](https://doi.org/10.3390/rs12101662) | Code: [github.com/justchenhao/STANet](https://github.com/justchenhao/STANet)

---

### Day 20: Multi-Temporal and Time-Series Satellite Analysis

- Why single images are not enough: temporal patterns reveal processes (crop growth, urban expansion, ice melt)
- Time-series approaches: SITS classification with temporal CNNs, RNNs, and Transformers
- Satellite Image Time Series (SITS) datasets: PASTIS, TimeSen2Crop, BreizhCrops
- Cloud-gap filling and temporal interpolation: handling missing data in optical time series
- Combining SAR and optical time series: leveraging SAR's cloud-penetrating capability

**Paper:** Garnot, V.S.F. & Landrieu, L. — "Panoptic Segmentation of Satellite Image Time Series with Convolutional Temporal Attention Networks" (ICCV, 2021). arXiv: [2107.07933](https://arxiv.org/abs/2107.07933) | Code: [github.com/VSainteuf/utae-paps](https://github.com/VSainteuf/utae-paps)

---

## PHASE 3: EXPERT-LEVEL AND CUTTING-EDGE RESEARCH (Days 21-30)

### Day 21: Foundation Models for Earth Observation

- What are foundation models and why they matter for satellite data (self-supervised pretraining at scale)
- IBM-NASA Prithvi: geospatial foundation model trained on HLS (Harmonized Landsat-Sentinel) data
- ESA's PhilEO Bench: benchmarking foundation models for downstream EO tasks
- SatMAE, Scale-MAE, and other masked autoencoder approaches for satellite imagery
- Fine-tuning strategies: few-shot adaptation of foundation models to new geographies and tasks

**Paper:** Jakubik, J. et al. — "Foundation Models for Generalist Geospatial Artificial Intelligence" (2023). arXiv: [2310.18660](https://arxiv.org/abs/2310.18660) — the Prithvi foundation model paper from NASA and IBM.

---

### Day 22: Reinforcement Learning for Space Systems

- RL formulations for satellite problems: orbit transfer, rendezvous, debris removal, scheduling
- Autonomous orbit maneuvering: deep RL agents learning fuel-optimal trajectories
- Multi-agent RL for constellation coordination: cooperative observation scheduling
- Sim-to-real transfer: training in orbital simulators and deploying on flight hardware
- Challenges: sparse rewards, long horizons, safety constraints in RL for space

**Paper:** Hovell, K. & Ulrich, S. — "Deep Reinforcement Learning for Spacecraft Proximity Operations Guidance" (J. Spacecraft and Rockets, 2021). DOI: [10.2514/1.A34838](https://doi.org/10.2514/1.A34838)

---

### Day 23: Federated Learning and Distributed AI in Satellite Networks

- Why federated learning fits satellite constellations: privacy, bandwidth, and heterogeneity
- Architecture: satellites as clients, ground station or lead satellite as aggregator
- Communication-efficient FL: gradient compression, quantization, and sparse updates over ISLs
- Asynchronous FL for LEO: handling variable link availability and orbital dynamics
- Applications: collaborative anomaly detection, distributed Earth observation model training

**Paper:** Razmi, N. et al. — "On-board Federated Learning for Satellite Clusters with Inter-Satellite Links" (IEEE Trans. Communications, 2024). DOI: [10.1109/TCOMM.2024.3356429](https://doi.org/10.1109/TCOMM.2024.3356429) | arXiv: [2307.08346](https://arxiv.org/abs/2307.08346)

---

### Day 24: Generative AI and Synthetic Data for Space Applications

- Data scarcity in space ML: few labeled examples, rare events, restricted access to defense imagery
- GANs for satellite image synthesis: generating realistic optical and SAR training data
- Domain adaptation with synthetic data: training on simulated imagery, deploying on real sensors
- Diffusion models for satellite image super-resolution and cloud removal
- Simulation environments: STK, GMAT, and custom orbital simulators for RL training data

**Paper:** Bittner, K., d'Angelo, P., Korner, M. & Reinartz, P. — "Automatic Large-Scale 3D Building Shape Refinement Using Conditional Generative Adversarial Networks" (ISPRS Archives / CVPR 2018 Workshop). DOI: [10.5194/isprs-archives-XLII-2-103-2018](https://doi.org/10.5194/isprs-archives-XLII-2-103-2018)

---

### Day 25: Quantum Communications in Space

- Quantum key distribution (QKD) from orbit: BB84 protocol adapted for satellite-ground links
- China's Micius (QUESS) satellite: first satellite-based QKD, entanglement distribution over 1,200 km
- Upcoming missions: Eagle-1 (ESA, 2025-2026), QEYSSat (Canada), SealSQ constellation, Boeing Q4S
- Ground-to-satellite uplink QKD: 2025 breakthrough proving bidirectional quantum links are feasible
- Limitations and debate: NSA position on QKD impracticality for national security systems

**Paper:** Liao, S.-K. et al. — "Satellite-to-ground quantum key distribution" (Nature, 2017). DOI: [10.1038/nature23655](https://doi.org/10.1038/nature23655) | arXiv: [1707.00542](https://arxiv.org/abs/1707.00542) — landmark Micius satellite QKD demonstration.

---

### Day 26: AI-Powered Mega-Constellations and Space-Based Computing

- SpaceX's satellite data center filing: million-satellite orbital AI compute constellation concept
- Project Suncatcher (Google): solar-powered satellite constellations with TPUs for ML inference in space
- ASCEND (ESA): orbital data center demonstration mission planned for 2026
- China's Three-Body Computing Constellation: 2,800 satellites for distributed space computing
- Economics and physics: power generation, heat dissipation, and latency trade-offs for space compute

**Paper:** Aguera y Arcas, B. et al. — "Towards a future space-based, highly scalable AI infrastructure system design" (Google Research, 2025). arXiv: [2511.19468](https://arxiv.org/abs/2511.19468) — the Project Suncatcher technical exploration.

---

### Day 27: Neuromorphic Computing and Novel Architectures for Space

- Why neuromorphic computing suits space: ultra-low power, radiation tolerance, event-driven processing
- Intel Loihi and IBM TrueNorth: neuromorphic chips evaluated for space applications
- Spiking neural networks (SNNs) for satellite sensor fusion and anomaly detection
- Optical computing for space: photonic neural networks and their radiation hardness
- Tensor Processing Units in orbit: Google's TPU deployment roadmap for space-based inference

**Paper:** Schuman, C.D., Potok, T.E., Patton, R.M. et al. — "A Survey of Neuromorphic Computing and Neural Networks in Hardware" (arXiv, 2017). arXiv: [1705.06963](https://arxiv.org/abs/1705.06963) — comprehensive review of neuromorphic architectures with space applicability.

---

### Day 28: AI for Active Debris Removal and In-Orbit Servicing

- Active debris removal (ADR): capturing and deorbiting space junk — nets, harpoons, robotic arms, lasers
- Computer vision for non-cooperative rendezvous: pose estimation of tumbling debris
- ML for relative navigation: monocular depth estimation and 6-DOF pose tracking in space
- In-orbit servicing: refueling, repair, and life extension of existing satellites
- DARPA RSGS, Astroscale ELSA-d, ClearSpace-1: current and planned missions

**Paper:** Sharma, S. & D'Amico, S. — "Neural Network-Based Pose Estimation for Noncooperative Spacecraft Rendezvous" (IEEE Trans. Aerospace and Electronic Systems, 2020). DOI: [10.1109/TAES.2020.2999148](https://doi.org/10.1109/TAES.2020.2999148) | arXiv: [1906.09868](https://arxiv.org/abs/1906.09868)

---

### Day 29: Responsible AI and Governance in Space Systems

- Dual-use AI: the tension between civilian and military applications of satellite AI
- Bias in satellite ML: geographic and temporal biases in training data (Global North overrepresentation)
- Orbital sustainability: AI's role in space traffic management and long-term debris mitigation
- International frameworks: UN COPUOS guidelines, Artemis Accords, ITU spectrum governance
- Autonomous weapons in space: ethical considerations of AI-driven satellite defense systems

**Paper:** Adilov, N., Alexander, P.J., Braun, V. & Cunningham, B.M. — "An economic indicator of the orbital debris environment" (J. Space Safety Engineering, 2024). DOI: [10.1016/j.jsse.2024.06.001](https://www.sciencedirect.com/science/article/abs/pii/S2468896724000600)

---

### Day 30: The Future — 2030 and Beyond

- Zero-touch satellite operations: AI managing 80%+ of routine constellation tasks by 2030
- Cognitive constellations: satellites that self-organize, self-heal, and self-optimize
- 6G and non-terrestrial networks (NTN): satellite-terrestrial integration with AI orchestration
- Deep space AI: autonomous navigation, science prioritization, and communication for Mars and beyond
- AI-native satellite design: building spacecraft from the ground up around ML workloads

**Paper:** Wang, Z. — "Space AI: Leveraging Artificial Intelligence for Space to Improve Life on Earth" (arXiv, 2025). arXiv: [2512.22399](https://arxiv.org/abs/2512.22399) | Code: [github.com/ziyangwang007/AI4Space](https://github.com/ziyangwang007/AI4Space)

---

## Appendix A: Recommended Paper Sources

For finding the papers referenced in each day's lesson, the following repositories and databases are recommended:

| Source | URL | Notes |
|--------|-----|-------|
| arXiv | arxiv.org | Preprints, free access |
| IEEE Xplore | ieeexplore.ieee.org | Top venue for remote sensing and signal processing |
| NASA Technical Reports | ntrs.nasa.gov | Free, space-specific research |
| ESA Publications | esa.int/publications | European Space Agency research |
| ACM Digital Library | dl.acm.org | Computing and networking papers |
| ScienceDirect | sciencedirect.com | Acta Astronautica, Advances in Space Research |
| MDPI Remote Sensing | mdpi.com/journal/remotesensing | Open-access remote sensing journal |

## Appendix B: Key Datasets for Hands-On Exploration

| Dataset | Domain | Size | Access |
|---------|--------|------|--------|
| EuroSAT | Land cover classification | 27,000 images, 10 classes | Free (Sentinel-2) |
| BigEarthNet | Multi-label classification | 590,326 patches | Free (Sentinel-2) |
| SpaceNet | Building/road detection | Multiple AOIs | Free (Maxar) |
| xView | Object detection | 1M+ objects, 60 classes | Free (DIUx) |
| PASTIS | Time-series segmentation | 2,433 parcels | Free (Sentinel-2) |
| SAR Ship Detection | Ship detection | 43,819 chips | Free (Gaofen-3) |
| DOTA | Oriented object detection | 188,282 instances | Free |

## Appendix C: Revision Notes

| Version | Date | Changes |
|---------|------|---------|
| v1 | 2026-02-26 | Initial draft for director review |
| v2 | 2026-02-26 | All 30 papers verified — 5 hallucinated citations replaced with real papers, DOIs/arXiv IDs added to all entries, author corrections applied |
