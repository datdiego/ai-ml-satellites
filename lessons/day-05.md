# Day 5: Machine Learning Fundamentals for Satellite Data
**Phase:** FUNDAMENTALS  
**Date:** March 05, 2026

---

## Prediction Prompt
> Satellite images contain many more "color channels" than a standard RGB photo — sensors like Sentinel-2 capture 13 spectral bands including infrared and shortwave infrared. Before deep learning became dominant, analysts hand-crafted indices like NDVI (which combines red and near-infrared bands) to detect vegetation. Do you think modern CNNs trained end-to-end on raw multispectral data outperform these hand-crafted feature approaches for land use classification, or do the engineered features still hold an edge — and why?

## Lesson

Here is the Day 5 lesson in full:

---

**Subject: Day 5 — Machine Learning Fundamentals for Satellite Data**

---

MakeMeAnExpert: AI/ML in Satellites and Space Systems
Day 5 of 30 — Machine Learning Fundamentals for Satellite Data

---

**OPENING**

When Google Maps updated its satellite layer to show sharper, more current imagery of your neighborhood, or when a news outlet published a story about North Korean military movements detected from space, the invisible engine behind those capabilities was machine learning applied to satellite data. We are now at a point where the rate of data collection from orbit exceeds what any team of human analysts could ever process. ML is not a nice-to-have — it is the only scalable solution.

Today we build the technical foundation: what makes satellite imagery a unique ML challenge, which learning paradigms apply, which tasks matter most, and which datasets and techniques have moved the field forward.

---

**WHY SATELLITE DATA IS DIFFERENT**

Most ML practitioners start with ImageNet — clean, well-lit photographs of single objects centered in frame, captured by consumer cameras with three RGB bands. Satellite data breaks nearly every one of those assumptions.

Scale: A single Sentinel-2 tile is a 100 x 100 km patch at 10-meter resolution, which yields a 10,980 x 10,980 pixel image. A full daily acquisition from a constellation like Planet (which captures the entire Earth every day) produces petabytes of data annually. The sheer volume forces you to think about efficient tiling, streaming pipelines, and distributed processing from day one.

Multispectral bands: Where a smartphone photo has three channels (R, G, B), Sentinel-2 has 13 spectral bands ranging from visible light through shortwave infrared. Landsat 8 has 11. Hyperspectral sensors like NASA's AVIRIS can have over 200 contiguous bands. Each band captures a different physical signature — water absorbs near-infrared strongly, healthy vegetation reflects it intensely, bare soil and concrete look very different in shortwave infrared than in visible light. This multi-band structure is a feature, not a complication, but your model architecture must be designed to exploit it.

Temporal sequences: The same geographic tile may be captured every 5 days (Sentinel-2), every 16 days (Landsat), or even daily (Planet). This temporal dimension enables change detection, crop growth monitoring, and disaster response — but it also means your input is often a time series of images rather than a single snapshot.

Noise characteristics: Cloud cover is the enemy. Globally, around 67% of Earth's surface is covered by clouds at any given time. Atmospheric haze, sensor calibration drift, bidirectional reflectance effects (the same surface looks different depending on the sun and sensor angles), and seasonal lighting variation all introduce noise that behaves very differently from the Gaussian noise ML practitioners typically encounter. Preprocessing pipelines — atmospheric correction, cloud masking, sun-angle normalization — are not optional steps you can skip.

---

**SUPERVISED, UNSUPERVISED, AND SELF-SUPERVISED LEARNING**

Supervised learning is the most mature approach in satellite ML. You provide labeled examples — this pixel is forest, that polygon is a building — and train a model to generalize. The challenge is that high-quality labels are expensive. Pixel-level annotation of satellite imagery requires expert knowledge and is painfully slow. This creates a fundamental bottleneck: you can collect data faster than you can label it.

Unsupervised learning sidesteps the labeling problem by finding structure in unlabeled data. K-means clustering applied to spectral bands can separate broad land cover classes without any labels. Principal Component Analysis (PCA) can compress 13 Sentinel-2 bands into 2-3 components that capture most variance. These methods are useful for data exploration, anomaly detection, and generating initial region proposals, but they tend to produce coarse results rather than fine-grained task-specific outputs.

Self-supervised learning has emerged in the last five years as a compelling middle ground, and it is arguably the most important paradigm shift currently happening in the field. The core idea: design a pretext task that generates its own labels from the data structure itself, then use the learned representations for downstream tasks. For satellite imagery, effective pretext tasks include predicting the relationship between two temporally adjacent tiles of the same location, predicting masked patches within an image (masked autoencoders, inspired by BERT in NLP), or learning that spatially overlapping tiles from different sensors should have similar embeddings. IBM and NASA released the Prithvi foundation model in 2023, pretrained on 1TB of Harmonized Landsat Sentinel-2 data using masked autoencoding — a direct application of self-supervised learning that produces representations fine-tunable with very few labeled examples.

---

**COMMON ML TASKS IN SATELLITE IMAGERY**

Image classification: Assign a single label to an entire image patch. What land use type is this 64x64 pixel tile? Forest, urban, agriculture, water? This is the simplest formulation and works well for coarse-scale mapping. The EuroSAT benchmark (the paper we cover today) addresses exactly this task.

Object detection: Locate and classify individual objects within an image with bounding boxes. Ships in a harbor, aircraft at airports, vehicles on a highway. The xView dataset, released by the Defense Innovation Unit in 2018, contains over 1 million annotated objects across 60 categories in overhead imagery — a key benchmark for this task.

Semantic segmentation: Assign a class label to every individual pixel in the image. This gives you a dense, pixel-precise map — which pixels are road, which are building, which are vegetation. SpaceNet, a public-private collaboration releasing annotated satellite imagery datasets, has pushed building footprint and road segmentation heavily, with crowdsourced challenge competitions driving the state-of-the-art forward.

Change detection: Given two images of the same location at different times, identify what changed. This is one of the highest-value applications — detecting deforestation, monitoring construction, assessing flood extent after a storm, tracking troop movements. It is also more technically complex: you must disentangle true surface changes from seasonal vegetation variation, atmospheric differences between acquisition dates, and slight geometric misalignment between images.

---

**FEATURE ENGINEERING VS. DEEP LEARNING**

Before deep learning became practical, satellite image analysis relied on hand-engineered spectral indices and texture features. These are not obsolete — they are fast, interpretable, and often good enough.

The most famous is NDVI, the Normalized Difference Vegetation Index: (NIR - Red) / (NIR + Red). Healthy vegetation has high NIR reflectance and low red reflectance, so NDVI ranges from about 0.6 to 0.9 for dense forest and drops near zero for bare soil or urban surfaces. A single-band derived index replaces a complex spectral analysis with one formula. NDWI (water), NDBI (built-up), and NBR (burn ratio) follow the same pattern. These indices can be computed instantly on any multispectral image and fed into a simple random forest or gradient-boosted tree.

Texture features — computed from the Grey Level Co-occurrence Matrix (GLCM) — capture spatial patterns: how regularly spaced are pixel values? Forests have fine-grained, irregular texture; agricultural fields are smoother and more uniform. These can distinguish spectrally similar surfaces that NDVI alone cannot separate.

Deep learning, specifically Convolutional Neural Networks (CNNs), learns these features automatically from data rather than requiring manual design. A CNN trained on labeled satellite patches will learn band ratios resembling NDVI in its early layers and progressively more abstract spatial patterns in deeper layers — without being told to. The trade-off is that CNNs require large labeled datasets to generalize well and are far less interpretable than an index you can write on a whiteboard.

In practice, hybrid approaches often win: use NDVI and other indices as additional input channels to a CNN, giving the network a head start on known physical relationships while letting it learn everything else from the data.

---

**KEY DATASETS**

EuroSAT: 27,000 labeled image patches (64x64 pixels) from Sentinel-2, covering 10 land use and land cover classes: Annual Crop, Forest, Herbaceous Vegetation, Highway, Industrial, Pasture, Permanent Crop, Residential, River, and Sea/Lake. Covers 34 European countries. A ResNet-50 trained on EuroSAT achieves 98.57% overall accuracy. We cover the paper behind this dataset below.

BigEarthNet: 590,326 Sentinel-2 image patches with multi-label annotations based on the CORINE Land Cover map. Unlike EuroSAT's single label per patch, BigEarthNet supports multi-label classification — a forest patch might also be labeled as "coniferous" and "transitional woodland." This makes it significantly more realistic and more challenging.

SpaceNet: A series of open challenge datasets focused on building footprint extraction, road network detection, and off-nadir building detection. SpaceNet data is hosted on AWS S3 and spans cities across multiple continents. The building footprint challenge alone covers Las Vegas, Paris, Shanghai, and Khartoum with over 685,000 labeled buildings.

xView: Released by the Defense Innovation Unit (DIU) in 2018. Contains 1,127 km² of WorldView-3 imagery at 0.3-meter resolution with over 1 million labeled objects across 60 categories — cars, aircraft, ships, storage tanks, construction vehicles. This is a primary benchmark for fine-grained overhead object detection and has driven significant research on small-object detection in satellite imagery.

---

**PAPER SUMMARY**

Helber, P., Bischke, B., Dengel, A., & Borth, D. (2019). EuroSAT: A Novel Dataset and Deep Learning Benchmark for Land Use and Land Cover Classification. IEEE Journal of Selected Topics in Applied Earth Observations and Remote Sensing (JSTARS), 12(7), 2217-2226.
DOI: https://doi.org/10.1109/JSTARS.2019.2918242 | arXiv: https://arxiv.org/abs/1709.00029

The paper addresses the lack of a large, publicly available labeled dataset for satellite-based land use classification in Europe — a gap that had forced researchers to use small, institution-specific datasets that could not be fairly compared. The authors created EuroSAT by downloading 27,000 Sentinel-2 patches across 34 European countries and manually assigning each to one of 10 LULC classes, then benchmarked CNN architectures including VGG-16, ResNet-50, GoogleNet, and InceptionV4 in both RGB-only and all-13-band configurations. Using all multispectral bands consistently improved accuracy over RGB-only inputs across every architecture tested. The key finding: a ResNet-50 trained on all 13 bands reaches 98.57% accuracy on the EuroSAT test set, far exceeding traditional hand-crafted approaches and establishing a reproducible deep learning baseline that the community could build upon.

---

**REAL-WORLD APPLICATIONS**

Crop yield forecasting: USDA and research groups use ML on Landsat and MODIS satellite time series to forecast crop yields months before harvest. A time series of NDVI readings over a growing season, combined with precipitation data, feeds regression models that estimate yield per hectare at county or even field level. Google research published results in 2017 on soybean yield prediction in the US Corn Belt, achieving errors within 5% of USDA ground-truth estimates using only satellite time series — no ground surveys needed.

Urban expansion monitoring: The European Environment Agency uses Sentinel-2 classification pipelines to update the CORINE Land Cover map across all EU member states, tracking how agricultural land is converted to residential or industrial use. What once took years of aerial survey and manual digitization now runs as an automated ML pipeline updated on a 6-year cycle.

Wildfire burn severity mapping: After every major wildfire in the western United States, the USGS Burned Area Emergency Response (BAER) team uses the Normalized Burn Ratio index — derived from Landsat and Sentinel-2 NIR and SWIR bands — combined with CNN-based segmentation to generate burn severity maps within days of fire containment. These maps directly inform where erosion barriers and reseeding are needed, and they feed into emergency relief allocation decisions.

---

**FURTHER READING**

EuroSAT Dataset and Pretrained Models (GitHub):
https://github.com/phelber/EuroSAT
Includes download links, pretrained ResNet weights, and training code to reproduce the paper's results.

SpaceNet Datasets and Baselines:
https://spacenet.ai/datasets/
Direct access to all SpaceNet challenge datasets hosted on AWS S3, with baseline code for building footprint and road detection.

Awesome Satellite Imagery Datasets (curated community list):
https://github.com/chrieke/awesome-satellite-imagery-datasets
A community-maintained index of nearly every publicly available labeled satellite dataset, organized by task type.

IBM/NASA Prithvi Foundation Model (HuggingFace):
https://huggingface.co/ibm-nasa-geospatial/Prithvi-100M
The 100M-parameter geospatial foundation model pretrained on Harmonized Landsat Sentinel-2 data via masked autoencoding, with fine-tuning examples for flood detection, crop segmentation, and burn scar mapping.

---

**KEY TAKEAWAY**

Satellite data is not just bigger ImageNet — its unique combination of spectral depth, temporal continuity, and planetary scale demands purpose-built ML approaches, and the engineers who understand both the physics of remote sensing and the mechanics of deep learning will define what this field becomes.

---

## Callback Question
> On Day 3, we covered onboard computing constraints — limited CPU, memory, and power budgets on satellite hardware. Given that satellite imagery involves massive scale, multispectral bands, and temporal sequences, explain how these data characteristics create tension with onboard processing constraints, and what trade-offs an engineer might face when deciding whether to run ML inference on the satellite versus downlinking raw data for ground-based processing.

## Feynman Prompt
> In 2-3 sentences, explain semantic segmentation as if describing it to a colleague who has never heard of it.

## Visual References

![Tropical Storm Yagi off Japan](https://images-assets.nasa.gov/image/GSFC_20171208_Archive_e001437/GSFC_20171208_Archive_e001437~thumb.jpg)
*Tropical Storm Yagi off Japan* — Source: [NASA](https://images.nasa.gov/details/GSFC_20171208_Archive_e001437)

![Illegal slash and burn practise in the region west of Manantenina.](https://upload.wikimedia.org/wikipedia/commons/thumb/1/17/Manantenina_bushfire.jpg/960px-Manantenina_bushfire.jpg)
*Illegal slash and burn practise in the region west of Manantenina.* — Source: [Wikipedia (Diorit, CC BY-SA 3.0)](https://en.wikipedia.org/wiki/Land%20use%2C%20land-use%20change%2C%20and%20forestry)
