# Day 19: Object Detection and Change Detection from Space
**Phase:** INTERMEDIATE TO ADVANCED  
**Date:** March 19, 2026

---

## Prediction Prompt
> When detecting ships or aircraft in satellite imagery, a standard bounding box (aligned with the image edges) is often used in everyday object detection — but why might this be a poor fit for overhead imagery specifically, and what alternative representation do you think researchers use instead?

## Lesson

I don't have write access to the lessons directories, so here's the complete Day 19 lesson for you to save:

---

MAKEMEanexpert: AI/ML IN SATELLITES AND SPACE SYSTEMS
Day 19 of 30 — Object Detection and Change Detection from Space


OPENING

When Hurricane Ian made landfall in Florida in September 2022, emergency managers needed to know — within hours — which neighborhoods were flooded, which roads were blocked, and where buildings had collapsed. The answer came partly from orbit: commercial satellite operators tasked their birds to image the impact zone, and AI models began flagging damaged structures before a single damage assessment team had boots on the ground. What made that possible was the technology you are learning about today: object detection and change detection from space.


WHAT WE'RE TALKING ABOUT TODAY

Detecting objects and measuring change from satellite imagery sounds straightforward — it's just computer vision, right? In a sense, yes. But the geometry of overhead imagery introduces a set of challenges that make space-based detection genuinely different from detecting cars in a street-level photo or tumors in a medical scan. The architectures, the training data, and even the bounding box format all need rethinking. Let's walk through each layer of the problem.


OBJECT DETECTION ARCHITECTURES FOR SATELLITE IMAGERY

Three families of detectors dominate the satellite imagery literature today, each representing a different trade-off between accuracy, speed, and architectural complexity.

Faster R-CNN, introduced by Ren et al. in 2015, is a two-stage detector. In the first stage, a Region Proposal Network (RPN) scans a feature map and proposes candidate bounding boxes — thousands of them — that are likely to contain objects. In the second stage, each proposal is classified and its box is refined. Two-stage detectors like Faster R-CNN tend to be highly accurate, especially on small and densely packed objects, making them a natural fit for satellite scenes. The cost is speed: inference can take several hundred milliseconds per image, which matters when you're processing thousands of square kilometers of imagery in a disaster response pipeline.

YOLO (You Only Look Once) and its successors — YOLOv5, YOLOv7, YOLOv8 — take the opposite approach. A single neural network pass produces both class probabilities and bounding box coordinates directly, without a separate proposal stage. This makes YOLO-family detectors significantly faster, often capable of real-time inference on embedded hardware. The tradeoff is that the original axis-aligned YOLO formulation struggles with the rotation and scale issues endemic to satellite data, though modified variants and post-processing tricks have closed much of that gap.

DETR — the Detection Transformer, introduced by Carion et al. at Facebook AI Research in 2020 — takes a fundamentally different approach. It frames object detection as a direct set prediction problem, eliminating both anchor boxes and non-maximum suppression entirely. An encoder-decoder transformer processes image features and produces a fixed set of predictions in a single pass. DETR's global attention mechanism is particularly appealing for satellite imagery because it can model long-range spatial relationships — useful when context matters for classification. Its main weakness historically has been slow convergence during training, though follow-on architectures like Deformable DETR and DINO have largely addressed this.

In practice, state-of-the-art satellite object detection systems often borrow from all three families: a YOLO backbone for speed, attention mechanisms from the transformer world, and two-stage refinement for high-value detection tasks.


CHALLENGES UNIQUE TO OVERHEAD IMAGERY

If you've built computer vision systems for ground-level cameras, overhead imagery will feel like a different discipline. Four challenges stand out:

Small objects. A commercial fishing vessel imaged at one-meter resolution might occupy only 10x30 pixels. A car in a parking lot at 50-centimeter resolution might be 8x4 pixels. At this scale, standard ImageNet-pretrained feature extractors lose most of the discriminative detail before it ever reaches the detection head. Solutions include feature pyramid networks (FPNs) that preserve multi-scale information, and training on super-resolved imagery.

Rotation variance. A dog on the street is almost always oriented horizontally. A ship in a harbor can point in any direction. Standard detection models trained on horizontal objects fail badly when confronted with objects at arbitrary angles, and worse, produce bounding boxes that enclose large amounts of background when a rotated ship is squeezed into an axis-aligned rectangle. We'll address the dedicated solution to this in a moment.

Scale variation. A single satellite scene can contain objects that vary by two or three orders of magnitude in apparent size — a stadium and a manhole cover might both be "infrastructure objects" but differ by a factor of a thousand in pixel footprint. Feature pyramid networks and multi-scale inference help, but this remains an open research problem.

Dense packing. Port terminals, parking lots, and refugee camps all exhibit extreme object density. When objects touch or overlap, standard non-maximum suppression can suppress valid detections. Architectures like DETR that avoid NMS have an advantage here, as do specialized training augmentations that simulate crowded scenes.


ORIENTED BOUNDING BOXES: WHY AXIS-ALIGNED BOXES FAIL

A standard bounding box is defined by four values: x, y, width, and height. It is always axis-aligned — its sides run parallel to the image edges. For ground-level photography, this is almost always fine. For overhead imagery, it is often catastrophically wrong.

Consider a cargo ship at a 45-degree heading. An axis-aligned box that tightly encloses it will be roughly square and will include substantial sea area at each corner — area that contains no ship whatsoever. That background noise degrades classification confidence and makes it impossible to reliably separate adjacent ships from each other.

The solution is the oriented bounding box (OBB), which adds a rotation angle as a fifth parameter: x, y, width, height, theta. An OBB can rotate to align with the long axis of the ship, yielding a tight, low-background enclosure regardless of heading.

The DOTA (Dataset for Object deTection in Aerial images) benchmark, published by Xia et al. in 2018 and expanded in DOTA-v2.0, has become the standard evaluation dataset for oriented detection, containing 188,282 instances across 18 categories annotated with oriented bounding boxes. Architectures like Oriented R-CNN, RoI Transformer, and S2ANet were specifically designed for this challenge and now form the backbone of commercial aerial and satellite detection pipelines.


CHANGE DETECTION: MEASURING WHAT THE EARTH DOES OVER TIME

Object detection finds things. Change detection finds what is different between two or more images of the same location taken at different times. The applications are enormous: tracking deforestation month by month, watching urban sprawl spread across a desert, identifying new military construction, or quantifying damage in the hours after an earthquake.

The core challenge is distinguishing real change — a new building, a cleared forest — from false change caused by seasonal vegetation differences, lighting angle shifts between image dates, sensor noise, or slight geolocation errors. This is harder than it sounds. A wheat field photographed in April versus October looks completely different even if nothing has "changed" in any meaningful sense.

Bi-temporal approaches compare two images: a before and an after. The simplest methods compute pixel-wise or feature-wise differences and threshold them. Deep learning bi-temporal methods feed both images through shared or separate encoders, then fuse the resulting feature maps and classify each pixel as changed or unchanged. This pixel-level output is called a change map.

Multi-temporal approaches extend this to sequences — three, ten, or hundreds of images — allowing models to separate long-term gradual trends (urbanization, ecosystem degradation) from acute events (flood, fire). These methods often incorporate recurrent networks or temporal attention to process the time dimension.

Siamese networks are the dominant architecture for bi-temporal change detection. A Siamese network has two branches with identical (and usually shared) weights. Each branch processes one of the two temporal images independently, producing a feature embedding. The difference or concatenation of those embeddings is then passed through a decoder to produce the change map. Sharing weights ensures that the same "vocabulary" is used to describe both images, which reduces false positives caused by domain shift between acquisition dates.

Attention mechanisms have become increasingly important in change detection. Spatial attention highlights which spatial regions are most relevant for the change decision; channel attention emphasizes which feature dimensions carry the most discriminative information about change. The paper we'll discuss shortly uses both to significant effect.


REAL-WORLD APPLICATIONS

Global Forest Watch, operated by the World Resources Institute, combines Landsat and Sentinel-2 imagery with ML-based change detection to publish near-real-time alerts when deforestation events are detected anywhere on Earth. Their GLAD (Global Land Analysis and Discovery) alert system has flagged hundreds of millions of hectares of tree cover loss. The system processes planetary-scale imagery weekly, making it the most operationally impactful change detection deployment in existence.

Maxar Technologies used object detection and change analysis following the 2023 earthquake in Turkey and Syria to produce rapid damage assessment maps. Within 48 hours of the event, their models had classified building damage at the structure level across the impact zone, producing data that FEMA, the UN, and local emergency managers used to prioritize search and rescue. Detecting collapsed buildings requires comparing post-event imagery against pre-event baselines and flagging structures where the spectral and geometric signature has shifted from "intact" to "rubble."

Planet Labs operates a constellation of over 200 small satellites that images every point on Earth's land surface daily. Their analytics platform uses ML-based change detection to sell "monitoring" products to customers ranging from agricultural firms tracking crop health to financial analysts monitoring parking lots at retailer distribution centers — counting cars as a proxy for commercial activity. The daily cadence turns change detection into an almost continuous signal rather than a periodic snapshot.


PAPER SUMMARY

Chen, H. & Shi, Z. (2020). A Spatial-Temporal Attention-Based Method and a New Dataset for Remote Sensing Image Change Detection. Remote Sensing, 12(10), 1662. DOI: 10.3390/rs12101662. Code: https://github.com/justchenhao/STANet

The paper addresses a fundamental limitation of standard Siamese change detection networks: they compare feature maps locally but ignore the broader spatial and temporal context that helps distinguish real structural change from transient differences like shadows or phenological cycles. The authors propose STANet, a Spatial-Temporal Attention Network with two key components: a Pyramid Sampling Module (PSM) that captures multi-scale spatial context by pooling features at different receptive field sizes, and a Basic Attention Module (BAM) that computes pairwise spatial relations across both temporal images simultaneously. The network is evaluated on the WHU Building dataset and the authors' own newly introduced LEVIR-CD dataset (637 image pairs at 0.5 meter GSD, focused on building change over 5-14 years in Texas). STANet achieved an F1 score of 87.3% on LEVIR-CD, outperforming prior Siamese baselines by several percentage points, and the LEVIR-CD dataset has since become one of the most widely used benchmarks in the change detection literature.


FURTHER READING

1. DOTA: A Large-Scale Dataset for Object Detection in Aerial Images (Xia et al., 2018). The foundational benchmark for oriented object detection in overhead imagery. Paper and dataset at: https://captain-whu.github.io/DOTA/

2. LEVIR-CD Dataset and leaderboard for building change detection, directly associated with the STANet paper. Accessible at: https://justchenhao.github.io/LEVIR/

3. Oriented R-CNN for Object Detection (Xie et al., 2021, ICCV). A clean, high-performance architecture for oriented bounding box detection that significantly reduces inference time compared to earlier OBB methods. Available via: https://arxiv.org/abs/2108.05699

4. torchgeo — a PyTorch library for geospatial data that includes pre-built dataloaders for DOTA, LEVIR-CD, and dozens of other remote sensing datasets, plus pretrained models for detection and segmentation. A practical starting point for anyone implementing these methods. Repository: https://github.com/microsoft/torchgeo


KEY TAKEAWAY

The satellite image is just a photograph until you teach a model to notice what's missing, what's new, and what's pointing the wrong way — and that gap between raw pixels and operational intelligence is exactly where machine learning earns its place in space systems.

---

The lesson covers all five syllabus bullet points: the three detector families (YOLO/Faster R-CNN/DETR), the four unique overhead challenges, oriented bounding boxes with the DOTA dataset, bi-temporal/multi-temporal/Siamese/attention-based change detection, and three real-world applications (Global Forest Watch, Maxar disaster response, Planet Labs monitoring). The STANet paper summary and four further reading resources with real URLs are included. Approximately 1,800 words.

## Callback Question
> On Day 13, we covered Edge AI and onboard satellite processing, where strict compute budgets force tradeoffs in model complexity. Given those constraints, how would you decide between deploying a lightweight YOLO variant versus a more accurate but heavier architecture like DETR for real-time object detection onboard a satellite — and what role might change detection latency requirements play in that decision?

## Feynman Prompt
> In 2-3 sentences, explain oriented bounding boxes as if describing it to a colleague who has never heard of them.

## Visual References

![Object detection](https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Detected-with-YOLO--Schreibtisch-mit-Objekten.jpg/960px-Detected-with-YOLO--Schreibtisch-mit-Objekten.jpg)
*Object detection* — Source: [Wikipedia ((MTheiler), CC BY-SA 4.0)](https://en.wikipedia.org/wiki/Object%20detection)

![Bottomley world map projection](https://upload.wikimedia.org/wikipedia/commons/4/4b/Bottomley_projection.png)
*Bottomley world map projection* — Source: [Wikipedia (Henry Bottomley, Public domain)](https://en.wikipedia.org/wiki/Change%20detection%20%28GIS%29)
