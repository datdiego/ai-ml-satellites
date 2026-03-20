# Day 20: Multi-Temporal and Time-Series Satellite Analysis
**Phase:** INTERMEDIATE TO ADVANCED  
**Date:** March 20, 2026

---

## Prediction Prompt
> Optical satellites can't see through clouds, which creates gaps in any time series of images — if you were designing a system to classify crop types using a full year of satellite imagery, how would you handle months where cloud cover blocks observations, and why might radar (SAR) satellites be particularly valuable here?

## Lesson

Here is the Day 20 lesson:

---

**Subject: Day 20 — Why One Snapshot Is Never Enough: Mastering Satellite Time Series**

---

OPENING

A single photograph tells you what something looks like. A time-lapse tells you what it is doing. That distinction is the entire motivation behind multi-temporal satellite analysis — and it is why some of the most powerful Earth observation AI systems treat time not as a nuisance but as their primary signal.

---

WHY SINGLE IMAGES ARE NOT ENOUGH

Imagine trying to identify a wheat field from a single aerial photo taken in February. The field is bare, brown, and indistinguishable from a fallow plot, a construction site, or a dirt road. Now imagine watching that same patch of ground every five days from March through September. You see it sprout, green up, reach peak canopy, yellow, and be harvested — a distinctive temporal signature that no single image could convey.

This is the fundamental insight behind satellite image time series (SITS) analysis: many of Earth's most important processes are defined by their temporal dynamics, not their instantaneous appearance.

Consider three canonical examples. Crop growth follows phenological cycles — planting, vegetative growth, flowering, senescence, harvest — that vary predictably by crop type and geography. Algorithms that classify fields crop-by-crop using a single mid-season image achieve modest accuracy; those that use a full growing-season time series routinely exceed 90% accuracy on major crop types.

Urban expansion is another case. A single image shows you the current boundary of a city. A decade of annual images reveals which neighborhoods grew first, at what rate, and in which direction — information that directly feeds infrastructure planning and climate adaptation policy. The European Urban Atlas, for instance, uses multi-temporal Sentinel-2 data to track built-up area changes across hundreds of European cities simultaneously.

Ice and glacier dynamics are perhaps the most consequential. The Arctic sea ice minimum is measured every September, but understanding the trend requires thirty or forty years of satellite data. Researchers at the National Snow and Ice Data Center (NSIDC) use passive microwave time series dating to 1979 to track the long-term decline — a 13% per decade reduction in September sea ice extent. No single image makes that story visible.

---

TIME-SERIES APPROACHES: FROM RNNS TO TRANSFORMERS

The ML community initially attacked SITS classification by borrowing from natural language processing — both problems involve sequences of variable length where order matters.

Recurrent Neural Networks (RNNs) and their variants Long Short-Term Memory (LSTM) and Gated Recurrent Units (GRU) were early favorites. An LSTM processes satellite observations sequentially, maintaining a hidden state that captures what it has "seen" so far. For crop classification over a 30-image Sentinel-2 time series, LSTMs demonstrated clear improvements over single-image CNNs, particularly for crops with similar peak-season appearances but different growth timing (e.g., winter wheat versus spring barley).

Temporal Convolutional Networks (TCNs) offered an alternative: apply 1D convolutions along the time axis, allowing parallel processing rather than the sequential bottleneck of RNNs. TCNs proved faster to train and easier to parallelize while matching or beating LSTM accuracy on standard benchmarks.

The most significant recent advance has been Transformers applied to time series. The self-attention mechanism is a natural fit: it can directly compare any two time steps without the vanishing-gradient problem that plagues long RNN chains. Temporal Attention Encoders (TAE), introduced by Garnot et al. and discussed further in the paper summary below, process all time steps simultaneously and learn which dates matter most for each spatial location — a form of learned, spatially-adaptive temporal attention.

A key practical consideration: satellite time series are irregularly sampled. A Sentinel-2 tile nominally revisits every five days, but clouds mask observations unpredictably. Real-world time series have gaps, and any architecture must handle variable-length sequences gracefully. Transformer-based approaches handle this naturally by encoding observation dates as positional embeddings rather than assuming uniform spacing.

---

SITS DATASETS: PASTIS, TIMESENSCROP, AND BREIZHCROPS

Benchmarking requires standardized datasets, and the SITS community has produced three widely used ones.

PASTIS (Panoptic Agricultural Time-Series) is perhaps the most ambitious. Released by Garnot and Landrieu alongside their ICCV 2021 paper, PASTIS covers approximately 2,433 km² of France with Sentinel-2 time series (33 to 61 observations per tile across a growing season) and provides parcel-level panoptic annotations — both semantic labels (which crop type) and instance labels (which specific field). This dual annotation enables evaluation of both classification and field boundary delineation simultaneously.

TimeSen2Crop is a large-scale dataset covering Austria, containing over 1 million labeled pixels from 16 crop types, derived from Sentinel-2 time series with 71 observations across a full year. Its strength is scale and class diversity; its limitation is that it provides pixel-level (not parcel-level) labels, making it less suitable for field boundary tasks.

BreizhCrops focuses on Brittany, France (Breizh is the Breton name for Brittany) and covers approximately 600,000 labeled field parcels from 2017 and 2018 Sentinel-2 time series. It was explicitly designed to study temporal classification models and has been used to benchmark LSTMs, TCNs, Transformers, and random forests on an apples-to-apples basis. The associated 2019 paper by Rußwurm and Körner remains one of the cleaner SITS benchmark studies available.

---

CLOUD-GAP FILLING AND TEMPORAL INTERPOLATION

Cloud cover is the nemesis of optical remote sensing. In tropical regions, cloud cover can exceed 80% of observations during the wet season, leaving time series with massive gaps. Even in temperate Europe, a typical Sentinel-2 time series over a growing season might have 20-40% of observations partially or fully clouded.

Several strategies address this. The simplest is temporal compositing: for each fixed time window (say, 16 days), take the cloud-free observation closest to the window center. This produces regularly-spaced synthetic time series at the cost of spatial mixing between dates.

More sophisticated approaches use interpolation. Linear interpolation fills gaps by assuming linear change between the nearest cloud-free observations on either side of the gap — computationally cheap but physically naive. Smoothing functions like Savitzky-Golay filters or harmonic analysis (fitting sinusoidal functions to the annual vegetation cycle) can reconstruct smooth, gap-filled NDVI time series that closely match the underlying phenology.

Deep learning approaches treat gap-filling as a self-supervised reconstruction task. A model trained to reconstruct intentionally masked observations learns the temporal structure of vegetation indices well enough to fill real cloud gaps. The STGAN (Spatial-Temporal Generative Adversarial Network) approach, for instance, uses surrounding spatial context plus temporal context to hallucinate plausible missing observations.

A practical point: many SITS classification models have been explicitly designed to be robust to missing observations, sidestepping gap-filling entirely. The Transformer-based TAE processes whatever observations are available, weighted by the attention mechanism, rather than requiring a complete time series.

---

COMBINING SAR AND OPTICAL TIME SERIES

The cleanest solution to cloud gaps is not to fill them but to avoid needing them: synthetic aperture radar (SAR) signals penetrate clouds entirely, providing observations regardless of weather. This complementarity between SAR and optical time series has driven significant interest in multi-modal fusion.

Sentinel-1 (C-band SAR) and Sentinel-2 (multispectral optical) together offer near-daily, all-weather coverage of most of Earth's land surface. The two sensors are physically co-registered, operated by ESA under the same Copernicus program, and can be directly combined.

SAR backscatter responds to surface roughness and moisture rather than spectral reflectance, so the two modalities carry genuinely complementary information. During the vegetative growth phase, Sentinel-2 NDVI and Sentinel-1 VV/VH backscatter both change, but in different ways that together better discriminate crop types than either alone. For rice detection — where flooding creates a distinctive SAR signature — SAR is often the primary modality even when optical data is available.

Fusion strategies range from simple feature concatenation (stack SAR and optical features then train one model) to attention-based cross-modal fusion where the model learns when to trust SAR versus optical based on cloud context. The latter approach, explored in several recent papers, effectively teaches the network to be skeptical of cloud-contaminated optical observations and lean on SAR as a fallback.

---

REAL-WORLD APPLICATIONS

Application 1: European Crop Monitoring. The EU's Integrated Administration and Control System (IACS) processes agricultural subsidy claims from millions of farmers. Automated crop type verification using SITS-based AI now runs at national scale in several EU member states, cross-checking declared crops against Sentinel-2 time series classifications. This has reduced manual field inspection rates while maintaining compliance accuracy. Countries including Denmark, Belgium, and the Netherlands have piloted this approach through ESA's Sen4CAP (Sentinels for Common Agricultural Policy) project.

Application 2: Tropical Deforestation Alerts. Global Forest Watch's GLAD (Global Land Analysis and Discovery) alert system, developed at the University of Maryland, uses Landsat time series to detect deforestation events typically within 8-16 days of clearing. By continuously updating a baseline against which new observations are compared, it identifies statistically anomalous changes at 30-meter resolution across all tropical forests globally. Over 100,000 alerts are generated per week during active deforestation periods.

Application 3: Urban Heat Island Monitoring. Time series of Landsat Land Surface Temperature (LST) imagery, spanning 1984 to present, have been used to track the intensification of urban heat islands over decades. Studies of cities including Phoenix, Houston, and Beijing show LST in dense urban cores increasing 1-3°C relative to surrounding rural areas over 40-year periods — dynamics only visible through multi-decadal time series analysis.

---

PAPER SUMMARY

Garnot, V.S.F. & Landrieu, L. (2021). "Panoptic Segmentation of Satellite Image Time Series with Convolutional Temporal Attention Networks." Proceedings of the IEEE/CVF International Conference on Computer Vision (ICCV), 2021. arXiv:2107.07933. Code: github.com/VSainteuf/utae-paps

This paper tackles the challenging task of simultaneously identifying every agricultural parcel in a satellite image time series AND classifying its crop type — a task called panoptic segmentation, combining semantic and instance segmentation. The authors propose U-TAE (U-Net with Temporal Attention Encoder), a spatially-aware temporal attention mechanism that processes all observations in a time series simultaneously and learns location-dependent attention weights — the model learns that the most discriminative dates differ between, say, a corn field versus a sunflower field. They also introduce the PASTIS benchmark dataset covering 2,433 km² of France with dense panoptic annotations. U-TAE achieves state-of-the-art panoptic quality scores significantly outperforming single-date baselines and prior recurrent approaches, demonstrating that temporally-aware architectures can jointly solve segmentation and classification in a single forward pass.

---

FURTHER READING

1. Rußwurm, M. & Körner, M. (2020). "Self-Attention for Raw Optical Satellite Time Series Classification." ISPRS Journal of Photogrammetry and Remote Sensing. arXiv:1910.10536. The paper that introduced Transformer-based classification for SITS, with the BreizhCrops benchmark evaluation.

2. BreizhCrops dataset and benchmark: github.com/dl4sits/BreizhCrops — includes code, dataset download, and pre-trained baselines for LSTM, TCN, and Transformer models.

3. Sentinel Hub EO Browser (apps.sentinel-hub.com/eo-browser) — free browser tool for exploring multi-temporal Sentinel-1 and Sentinel-2 imagery interactively; excellent for building intuition about what temporal signatures look like before diving into code.

4. Pelletier, C., Webb, G.I. & Petitjean, F. (2019). "Temporal Convolutional Neural Network for the Classification of Satellite Image Time Series." Remote Sensing, 11(5), 523. DOI: 10.3390/rs11050523. Clean comparison of TCN versus LSTM for SITS classification, available open access.

---

KEY TAKEAWAY

A satellite watching the same field every week for a year sees not just what is there, but what it is becoming — and that temporal fingerprint is often the most powerful feature of all.

---

*MakeMeAnExpert: AI/ML in Satellites and Space Systems — Day 20 of 30*

## Callback Question
> On Day 7, we covered SAR's ability to penetrate clouds and acquire imagery regardless of weather or lighting conditions. Given today's lesson on cloud-gap filling as a major challenge for optical time series, explain how fusing SAR and optical data in a multi-temporal analysis pipeline could reduce — but not entirely eliminate — the need for temporal interpolation techniques. What residual challenges might remain even with SAR data available?

## Feynman Prompt
> In 2-3 sentences, explain temporal attention in satellite image time series classification as if describing it to a colleague who has never heard of it.

## Visual References

![Time series](https://upload.wikimedia.org/wikipedia/commons/7/77/Random-data-plus-trend-r2.png)
*Time series* — Source: [Wikipedia (Wikipedia, CC BY-SA 3.0)](https://en.wikipedia.org/wiki/Time%20series)
