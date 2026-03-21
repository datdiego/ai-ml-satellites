# Day 21: Foundation Models for Earth Observation
**Phase:** EXPERT-LEVEL AND CUTTING-EDGE RESEARCH  
**Date:** March 21, 2026

---

## Prediction Prompt
> Large language models like GPT are pretrained on billions of text documents before being fine-tuned for specific tasks — if you were to apply this same "pretrain on massive data, then fine-tune" strategy to satellite imagery, what kind of pretraining task would you design, and what data would you use to train a geospatial foundation model from scratch?

## Lesson

The lesson content is written above. Here's a summary of what was produced:

---

**Day 21 lesson covers all 5 syllabus bullet points:**

1. **Foundation Models & self-supervised pretraining** — explained the MAE approach and why the "pretrain once, fine-tune many" paradigm solves the label-scarcity problem in EO.

2. **IBM-NASA Prithvi** — described the HLS pretraining data (Landsat 8/9 + Sentinel-2, 30m, 6 bands, 1M chips over CONUS), the temporal MAE architecture, and the open Hugging Face release.

3. **ESA PhilEO Bench** — covered the Sentinel-2-based benchmark tasks (building density, road segmentation, forest change), its low-data-regime evaluation methodology, and the geographic generalization findings.

4. **SatMAE & Scale-MAE** — SatMAE (NeurIPS 2022): band-specific encodings + temporal encoding on fMoW-Sentinel. Scale-MAE (ICCV 2023): scale-aware positional encodings for multi-GSD imagery.

5. **Fine-tuning strategies** — linear probing, full fine-tuning with layer-wise LR decay, LoRA/adapter methods, and the geographic adaptation challenge.

**Three real-world applications:** Pakistan 2022 flood mapping, wildfire burn scar detection, crop type mapping for food security.

**Paper summarized:** Jakubik et al. 2023, arXiv:2310.18660 (Prithvi).

**Further reading:** SatMAE, Scale-MAE, PhilEO Bench papers + Hugging Face model hub.

I also prepared updated content for `answers/day-21.md` (prediction prompt, callback question referencing Day 20's temporal analysis, Feynman prompt) and `answers/quiz-week-3.md` (5 questions spanning Days 15-21) — grant write permission to save those files.

## Callback Question
> On Day 13, we covered Edge AI and onboard satellite processing, where models must operate under tight constraints on compute, memory, and power. Foundation models like Prithvi are massive, pretrained on vast HLS datasets — so how would you approach deploying a fine-tuned geospatial foundation model to a resource-constrained satellite, and what trade-offs would you need to make compared to running inference on the ground?

## Feynman Prompt
> In 2-3 sentences, explain self-supervised pretraining at scale as if describing it to a colleague who has never heard of it.

## Quiz (week-3)

1. What is FDIR, and what is the key difference between rule-based and ML-based approaches to fault detection and recovery in autonomous satellite systems?
2. Explain how LSTM networks are used for geomagnetic storm prediction. What indices do they forecast, and why is accurate forecasting important for satellite operators?
3. Why do overhead satellite imagery object detectors use oriented bounding boxes instead of standard axis-aligned bounding boxes? Give a concrete example of a use case where this matters.
4. What problem does combining SAR and optical satellite time series solve that neither data source can address well on its own?
5. What is self-supervised pretraining, and why is it particularly valuable for geospatial foundation models like IBM-NASA Prithvi given the nature of satellite imagery datasets?
