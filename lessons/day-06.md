# Day 6: Satellite Image Classification with Deep Learning
**Phase:** FUNDAMENTALS  
**Date:** March 06, 2026

---

## Prediction Prompt
> When you train a CNN on ImageNet (photos of dogs, cats, cars, etc.) and then fine-tune it on satellite imagery, do you think the low-level features learned from everyday photos will transfer well to overhead multispectral images — and if not, at what layer do you expect the transfer to break down?

## Lesson

Here is the Day 6 lesson:

---

DAY 6: SATELLITE IMAGE CLASSIFICATION WITH DEEP LEARNING

When Planet Labs announces it can detect individual cars in a parking lot from 500 kilometers up, or when the European Space Agency claims its algorithms can map global deforestation faster than any human team, they are all betting on one core capability: teaching a computer to understand what it sees from orbit. Today we break open the machinery behind that claim.


CNNS FOR MULTISPECTRAL IMAGERY: NOT JUST RGB

Standard convolutional neural networks were designed with three-channel RGB images in mind. A satellite sensor like Sentinel-2 gives you 13 spectral bands — visible red, green, and blue, plus near-infrared, red edge, short-wave infrared, and several others. Landsat 8 provides 9 bands. Hyperspectral sensors can produce hundreds.

This creates an immediate design decision: how do you feed this data into a CNN?

The naive approach is to just treat extra bands as extra input channels. A standard ResNet50 expects a 3-channel input; you can modify the first convolutional layer to accept 13 channels instead. The tradeoff is that you lose the benefit of pretrained weights on that first layer, since ImageNet weights were trained on RGB. A common fix is to initialize the extra channel weights by averaging or copying the weights from existing RGB channels, then fine-tuning from there. It is imperfect but it works in practice.

A more principled approach is to hand-select which bands to use based on your task. Detecting vegetation? Use near-infrared and red (that is how NDVI is computed). Detecting water bodies? Short-wave infrared bands make water appear very dark and easy to separate from land. Detecting clouds? Blue and short-wave infrared together are a strong signal. You are not obligated to use all the bands just because they exist.

Spatial resolution also matters enormously. Sentinel-2 delivers 10-meter pixels in the visible and near-infrared bands. Planet's commercial satellites reach 3-meter resolution. Very high resolution (VHR) sensors like WorldView-3 can resolve objects at 30 centimeters. The spatial scale of features you care about — a road, a field, a building — determines whether your CNN will even have enough pixels to learn from.


TRANSFER LEARNING: BENEFITS, PITFALLS, AND WHAT ACTUALLY WORKS

Transfer learning is the practice of starting with weights pretrained on a large dataset (usually ImageNet, which has 1.2 million labeled photographs) and then fine-tuning them on your target task. The intuition is that low-level features — edges, textures, corners — are universal enough that what the network learned on cats and cars will help it recognize patterns in aerial imagery.

The benefits are real. Training a ResNet50 from scratch on a satellite image dataset might take days on expensive hardware and require tens of thousands of labeled examples. Starting from ImageNet weights often converges faster and to a better solution, particularly when your labeled satellite dataset is small. Numerous studies have confirmed that even though the image statistics are quite different between natural photos and aerial views, pretrained weights still give a meaningful head start.

The pitfalls are equally real. ImageNet images are taken at human eye level. Satellite images are orthographic — taken from directly above. Objects like cars and buildings look fundamentally different from above than from the side. Textures that a network learns from natural scenes (fur, grass up close, fabric) are semantically different from what those same textures mean at 10-meter resolution. High-altitude imagery also has spectral characteristics (the Rayleigh scattering haze in blue bands, the brightness of snow in shortwave infrared) that never appear in any natural photograph.

The practical recommendation: use ImageNet pretraining as your starting point, but plan for substantial fine-tuning on domain-specific data. Do not freeze the backbone entirely. Also consider pretraining on large satellite-specific datasets like BigEarthNet (590,000 Sentinel-2 image patches) or EuroSAT (27,000 labeled scenes) before fine-tuning on your specific application. This two-stage transfer (ImageNet → general satellite data → specific task) consistently outperforms single-stage transfer.


ARCHITECTURE SHOWDOWN: RESNET, EFFICIENTNET, AND VISION TRANSFORMERS

The workhorse architectures for satellite scene classification have evolved over the past decade.

ResNet (Residual Networks) introduced skip connections that allow gradients to flow through very deep networks without vanishing. ResNet-50 and ResNet-101 remain extremely popular baselines for satellite classification. They are well-understood, computationally predictable, and have strong pretrained weights available in every major deep learning framework.

EfficientNet, introduced by Google in 2019, changed the calculus by systematically scaling network width, depth, and input resolution together using a compound coefficient. On satellite benchmarks, EfficientNet-B4 and B5 frequently outperform ResNet models at lower parameter counts. If you are trying to optimize for accuracy-per-FLOP — which matters when you have thousands of satellite tiles to classify — EfficientNet variants are worth serious attention.

Vision Transformers (ViTs) apply the self-attention mechanism from natural language processing to image patches. Instead of sliding convolutional filters, a ViT divides an image into fixed-size patches (typically 16x16 pixels) and treats each patch as a token in a sequence. This allows the model to capture long-range spatial relationships that convolutions struggle with. In satellite imagery, this is useful: the relationship between a river and downstream agricultural patterns can span hundreds of pixels. Recent results show ViTs matching or exceeding CNNs on standard satellite benchmarks when sufficient training data is available, but they need more data than CNNs to train from scratch. Hybrid architectures — convolutional stems feeding into transformer layers — often give the best of both worlds.


HANDLING CLASS IMBALANCE: THE PROBLEM NOBODY TALKS ABOUT ENOUGH

Land cover maps of the real Earth are not uniformly distributed. Dense urban areas cover roughly 3% of Earth's land surface. Forests cover about 31%. Open ocean covers 71% of the planet total. When you train a model on a globally representative dataset, it will see vastly more ocean and forest tiles than urban, wetland, or cropland tiles.

A naive model will learn to predict the majority class almost exclusively because that is how it minimizes cross-entropy loss. You will see 95% overall accuracy on a dataset where 95% of the samples are water — the model learned nothing useful.

There are three main fixes:

Oversampling minority classes: duplicate samples from underrepresented classes during training so the model sees them more often. This is simple and effective. A variant, SMOTE (Synthetic Minority Over-sampling Technique), generates synthetic examples by interpolating between existing minority class samples, though this works better for tabular data than images.

Weighted cross-entropy: assign a higher loss penalty to misclassifying rare classes. If urban is 3% of your data, its weight should be roughly 1/0.03 = 33x higher than a class at 100% frequency. This is easy to implement in PyTorch and TensorFlow and should be your default starting point for any imbalanced classification task.

Focal loss: introduced by Lin et al. in the RetinaNet paper (2017), focal loss dynamically scales the loss contribution of each sample based on how confident the model already is. Easy samples that the model classifies correctly with high confidence contribute very little to the loss. Hard, ambiguous samples (often minority class examples) contribute disproportionately more. It is particularly effective when combined with class weighting. Focal loss is now nearly universal in satellite semantic segmentation pipelines.


EVALUATION METRICS: WHAT OVERALL ACCURACY IS HIDING

Never report only overall accuracy on an imbalanced dataset. If you do, reviewers will know you are hiding something.

Overall accuracy (OA) is simply the fraction of pixels or tiles classified correctly. On an imbalanced dataset, it will flatter majority classes and obscure failures on rare but important classes.

Per-class F1 score is the harmonic mean of precision and recall for each class individually. It forces you to report how well the model actually performs on wetlands, urban areas, or burned land — not just on the dominant forest class. Report per-class F1 for every class, and macro-average F1 (simple average across classes) as your headline metric.

Cohen's kappa adjusts for the probability that correct classifications happen by chance. A model that randomly guesses in proportion to class frequency will achieve nonzero overall accuracy; kappa corrects for this and gives a truer picture of learned ability. Kappa of 0.8 or above is generally considered strong agreement. Kappa below 0.6 is cause for concern even if overall accuracy looks high.

The confusion matrix is your debugging tool. A well-organized confusion matrix will immediately reveal systematic confusions — for instance, a model that consistently misclassifies scrubland as grassland, or shallow water as wetland. These confusions often reveal spectral or spatial ambiguities that point toward needed improvements in your input features or training data.


REAL-WORLD APPLICATIONS

ESA's Climate Change Initiative Land Cover project has used deep learning to produce annual global land cover maps at 300-meter resolution since 2015, updating them with Sentinel data using transfer learning pipelines that adapt classification from one year's ground truth labels to subsequent years.

Planet Labs applies CNNs across their daily global mosaic to automatically tag scenes with land cover attributes — differentiating between active cropland, fallow fields, and forests — enabling their customers to detect field-level changes in agricultural monitoring applications.

The NASA Harvest program uses EfficientNet-based classifiers on Sentinel-2 and Landsat data to produce in-season crop area estimates for food security forecasting in East Africa and South Asia. Their models must handle severe class imbalance (small fields versus vast grassland expanses) using weighted loss functions and careful per-class evaluation.


PAPER SUMMARY

Neumann, M., Pinto, A.S., Zhai, X., & Houlsby, N. "In-Domain Representation Learning for Remote Sensing." arXiv preprint arXiv:1911.06721 (2019); presented at ICLR 2020 Workshop.

The paper asks a fundamental question: does pretraining on ImageNet actually provide good representations for satellite image classification, or would pretraining directly on large unlabeled remote sensing datasets produce better features? The authors use contrastive and self-supervised learning methods to pretrain models on large collections of satellite imagery without labels, then benchmark against ImageNet-pretrained counterparts on multiple remote sensing classification tasks. Their key finding is that in-domain pretraining substantially outperforms ImageNet pretraining on satellite tasks, with the performance gap widening when downstream labeled data is scarce — exactly the regime most real-world satellite applications operate in. This work laid important groundwork for the now-widespread use of self-supervised pretraining (SimCLR, MoCo, and later masked autoencoders) on satellite-specific datasets.

Full citation: https://arxiv.org/abs/1911.06721


FURTHER READING

BigEarthNet dataset (590,000 Sentinel-2 patches with 19-class land cover labels — the standard benchmark for transfer learning research in remote sensing):
https://bigearth.net

EuroSAT: A Novel Dataset and Deep Learning Benchmark for Land Use and Land Cover Classification (Helber et al., 2019 — the paper that introduced EuroSAT, widely used for pretraining experiments):
https://arxiv.org/abs/1709.00029

Focal Loss for Dense Object Detection (Lin et al., 2017 — the RetinaNet paper that introduced focal loss, essential reading for anyone doing classification on imbalanced geospatial data):
https://arxiv.org/abs/1708.02002

TorchGeo: Deep Learning With Geospatial Data (Stewart et al., 2022 — a PyTorch library that standardizes dataloaders, transforms, and pretrained models specifically for satellite imagery):
https://arxiv.org/abs/2111.08872


KEY TAKEAWAY

A model that reports 95% overall accuracy on a satellite classification task may have learned almost nothing useful — always evaluate with per-class F1 and Cohen's kappa, and always check your confusion matrix before trusting any headline number.

## Callback Question
> On Day 3, we covered the constraints of onboard computing hardware in satellites, including limited processing power, memory, and energy budgets. Given these constraints, how would you decide between deploying a lightweight model like MobileNet versus a more accurate but heavier model like EfficientNet-B7 or a Vision Transformer for onboard satellite image classification — and what tradeoffs would you accept in terms of the evaluation metrics covered today (accuracy, F1, Cohen's kappa)?

## Feynman Prompt
> In 2-3 sentences, explain transfer learning as if describing it to a colleague who has never heard of it.

## Visual References

![A 3 layer 1D CNN feed-forward diagram with kernel size of 3 and stride of 1.](https://upload.wikimedia.org/wikipedia/commons/3/31/1D_Convolutional_Neural_Network_feed_forward_example.png)
*A 3 layer 1D CNN feed-forward diagram with kernel size of 3 and stride of 1.* — Source: [Wikipedia (Vicente Oyanedel M., CC BY-SA 4.0)](https://en.wikipedia.org/wiki/Convolutional%20neural%20network)

![Land cover](https://upload.wikimedia.org/wikipedia/commons/thumb/2/25/Land_cover_IGBP.png/960px-Land_cover_IGBP.png)
*Land cover* — Source: [Wikipedia (T. Hengl, CC BY-SA 3.0)](https://en.wikipedia.org/wiki/Land%20cover)
