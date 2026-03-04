# Day 4: Introduction to Earth Observation and Remote Sensing
**Phase:** FUNDAMENTALS  
**Date:** March 04, 2026

---

## Prediction Prompt
> Satellites like Landsat and Sentinel carry multiple sensors that capture the same scene across different wavelength bands — not just visible light, but also infrared and microwave. Before reading today's lesson, why do you think scientists need these extra, invisible bands? What might they reveal that a regular color photograph cannot?

## Lesson

Here is the Day 4 lesson:

---

DAY 4: INTRODUCTION TO EARTH OBSERVATION AND REMOTE SENSING

Welcome to MakeMeAnExpert: AI/ML in Satellites and Space Systems
Day 4 of 30


HOOK

When the Copernicus Emergency Management Service activated its satellite assets to map the 2023 Turkey-Syria earthquake damage within hours of the event, they did not send anyone into the rubble. They had already seen everything they needed from 700 kilometers up. That is the promise of Earth observation: turning raw photons and radar echoes into actionable intelligence about our planet, in near real time.


WHAT REMOTE SENSING ACTUALLY IS

Remote sensing is the science of gathering information about an object or surface without direct physical contact. Satellites do this by detecting energy that interacts with the Earth's surface and atmosphere, then recording that interaction as digital data.

The fundamental split is between passive and active sensors.

Passive sensors measure energy that originates somewhere else — most commonly reflected sunlight. Your eye is a passive sensor. So is the camera on a Landsat satellite. Passive optical sensors work great in daylight and clear conditions, but they are helpless when clouds are in the way or when the sun is not shining on the area of interest. The subcategories matter:

- Optical/panchromatic sensors record a single broad band of visible light, producing grayscale imagery. Think of a black-and-white aerial photograph.
- Multispectral sensors record a handful of discrete bands — typically 4 to 12 — including visible, near-infrared, and sometimes shortwave infrared. Landsat 8 has 11 bands. This is the workhorse of most operational Earth observation.
- Hyperspectral sensors record hundreds of contiguous, narrow spectral bands. Where a multispectral sensor sees "red," a hyperspectral sensor sees dozens of slices of red. This lets you fingerprint materials — identifying mineral composition, plant species, or water contaminants — at the cost of much larger data volumes.

Active sensors generate their own energy, transmit it toward the target, and record what bounces back. Two types dominate:

- Radar (Radio Detection And Ranging) uses microwave pulses. Because microwaves penetrate clouds and work day or night, radar is invaluable for persistent monitoring. Synthetic Aperture Radar (SAR) is the dominant radar mode in Earth observation; it synthesizes a large virtual antenna from a moving platform to achieve fine spatial resolution despite using wavelengths measured in centimeters. Sentinel-1 and the Canadian RADARSAT missions use SAR.
- Lidar (Light Detection And Ranging) uses laser pulses, typically in the near-infrared, to measure precise distances. ICESat-2 uses photon-counting lidar to measure ice sheet elevation changes to centimeter accuracy from 500 km altitude. Lidar is also the backbone of airborne forest canopy mapping.


THE ELECTROMAGNETIC SPECTRUM IN REMOTE SENSING

Not all wavelengths behave the same way in Earth's atmosphere, and different surface materials reflect or emit differently across wavelengths. Understanding this is non-negotiable for working with satellite data.

Visible light (roughly 0.4 to 0.7 micrometers) is what humans see. Healthy vegetation is green in visible light but extremely reflective in the near-infrared — a contrast the human eye cannot see, but one that satellites exploit constantly. The Normalized Difference Vegetation Index (NDVI) is simply (NIR - Red) / (NIR + Red), and it has become one of the most widely used geospatial data products in history.

Near-infrared (0.7 to 1.3 micrometers) is absorbed by water and reflected by vegetation. Shortwave infrared (1.3 to 3.0 micrometers) distinguishes rock types and soil moisture. Thermal infrared (8 to 14 micrometers) detects heat emitted by the surface — used for detecting urban heat islands, volcanic activity, and wildfires. Microwave (1 mm to 1 m) penetrates clouds and vegetation canopies, enabling SAR and passive microwave radiometry used in weather forecasting and soil moisture mapping.

The atmosphere is not transparent at all wavelengths. There are "atmospheric windows" — ranges where the atmosphere is relatively transparent — and absorption bands where water vapor, CO2, or ozone soak up specific wavelengths. Sensor design is constrained by these windows: you can only observe through wavelengths the atmosphere lets through (unless you are above the atmosphere entirely, as is increasingly the case with science missions).


THE FOUR PILLARS OF IMAGE QUALITY

Describing the quality of satellite imagery requires four distinct, partially competing dimensions. Engineers and scientists trade them off against each other constantly.

Spatial resolution is the ground area represented by a single pixel. Worldview-3 delivers 31 cm pixels commercially. Sentinel-2 offers 10 meters in its visible and near-infrared bands. MODIS, used for global climate monitoring, operates at 250 meters to 1 kilometer. Higher spatial resolution generally means smaller swath width and slower revisit time — you can zoom in or zoom out, but not both simultaneously.

Spectral resolution is how finely the sensor slices the electromagnetic spectrum. A panchromatic sensor has one band. A typical multispectral sensor has 4 to 12 bands. Hyperspectral sensors like NASA's AVIRIS have over 200 contiguous bands. More spectral bands enable more precise material identification but multiply the data volume accordingly.

Temporal resolution is how often the satellite revisits the same location. A geostationary weather satellite like GOES-16 images the full Western Hemisphere every 5 to 15 minutes. Landsat 8 has a 16-day repeat cycle. Planet Labs' Dove constellation achieves daily to near-daily revisit globally by deploying hundreds of small satellites. Temporal resolution is critical for detecting change — agricultural yield estimation, disaster response, and deforestation monitoring all depend on dense time series.

Radiometric resolution is the number of distinct intensity values the sensor can record — essentially its bit depth. An 8-bit sensor records 256 levels of brightness. A 12-bit sensor records 4,096 levels. Higher radiometric resolution allows detection of subtle differences in surface brightness or temperature, which matters when you are doing quantitative analysis rather than just making pretty images.


KEY MISSIONS: LANDSAT, SENTINEL, MODIS, AND GOES

Landsat is the longest continuously running Earth observation program in history. Landsat 1 launched in 1972; Landsat 9 launched in 2021 and operates alongside Landsat 8. Together they provide the definitive archive for long-term land change analysis. Landsat data is freely available and has been downloaded over 100 million times from USGS servers. The 30-meter spatial resolution and 16-day revisit are well-matched to agricultural, forest, and land cover applications.

Sentinel is the European Space Agency's Copernicus fleet. Sentinel-1 is a SAR mission for all-weather monitoring. Sentinel-2 is a multispectral optical mission with 13 bands and 10-meter resolution in key bands, providing 5-day global revisit using two satellites in tandem. Sentinel-3 measures ocean color, sea surface temperature, and vegetation. Sentinel-5P tracks atmospheric chemistry, including NO2, methane, and ozone. All Sentinel data is free and open.

MODIS (Moderate Resolution Imaging Spectroradiometer) flies on NASA's Terra and Aqua satellites, launched in 1999 and 2002. With 36 spectral bands at 250 meters to 1 kilometer resolution and daily global coverage, MODIS has generated foundational datasets for climate science, wildfire detection, and aerosol monitoring. The MODIS active fire product detects thermal anomalies globally every day and feeds directly into operational fire management systems.

GOES (Geostationary Operational Environmental Satellite) is NOAA's geostationary weather satellite system. GOES-16 (East) and GOES-18 (West) together cover the Americas continuously, updating every 5 minutes in full disk mode and as frequently as 30 seconds for mesoscale regions. GOES data drives the weather forecasts most Americans rely on daily.


FROM RAW DATA TO USABLE IMAGERY: THE PREPROCESSING PIPELINE

A satellite sensor delivers raw digital numbers — counts of photons or radar returns recorded by individual detector elements. Turning these into scientifically usable imagery requires a pipeline that is often taken for granted but is crucial to understand.

Radiometric calibration converts the raw digital numbers to physical units — watts per steradian per square meter per micrometer for optical sensors, or backscatter coefficient for radar. Satellite operators publish calibration coefficients regularly as instruments age.

Atmospheric correction removes the effect of the atmosphere on measured radiance. Molecules and aerosols scatter and absorb light between the surface and the sensor, changing the apparent brightness and color of the ground. Without atmospheric correction, NDVI values from different dates or locations are not comparable. Methods range from simple dark-object subtraction to full radiative transfer modeling (tools like 6S and MODTRAN).

Geometric correction ensures pixel locations correspond accurately to their true geographic positions. Raw imagery has distortions from satellite attitude, Earth curvature, terrain relief, and sensor geometry. Orthorectification corrects terrain-induced distortions using a digital elevation model, producing an "orthophoto" where every pixel is planimetrically accurate. Without orthorectification, a mountain appears shifted from its true position.

Mosaicking and cloud masking assemble multiple scene acquisitions into seamless, cloud-free composites — combining imagery from different dates to fill gaps where clouds obscured the surface.


REAL-WORLD APPLICATIONS

Agricultural monitoring. NASA Harvest and related programs use Landsat and Sentinel-2 NDVI time series to forecast crop production at national and global scales weeks before harvest. The USDA Foreign Agricultural Service uses these products to issue early warnings of food security risk in countries like Ukraine and Ethiopia.

Disaster response. When Typhoon Hainan struck the Philippines in 2013, the Copernicus EMS produced damage maps within 24 hours using optical and SAR imagery. SAR was essential because cloud cover persisted for days after the storm — traditional optical satellites would have seen nothing but cloud tops.

Sea ice monitoring. The National Snow and Ice Data Center uses passive microwave data from SSMI/S sensors to produce daily sea ice extent maps that have documented the dramatic decline of Arctic summer sea ice over four decades. These records, extending back to 1979, are among the most cited datasets in climate science.


PAPER SUMMARY

Zhu, X.X., Tuia, D., Mou, L., Xia, G.S., Zhang, L., Xu, F., & Fraundorfer, F. (2017). Deep Learning in Remote Sensing: A Comprehensive Review and List of Resources. IEEE Geoscience and Remote Sensing Magazine, 5(4), 8-36. DOI: 10.1109/MGRS.2017.2762307 | arXiv: https://arxiv.org/abs/1710.03959

This paper addressed the question of whether and how deep learning — which had transformed computer vision and speech recognition — could be systematically applied to the unique challenges of remote sensing data. The authors surveyed the full landscape of deep learning architectures (CNNs, RNNs, autoencoders, generative adversarial networks) and mapped them to remote sensing tasks including scene classification, object detection, semantic segmentation, and change detection. A key contribution was identifying what makes remote sensing distinct from natural image analysis: enormous image sizes, limited labeled training data, complex and variable imaging geometries, and the need to fuse information across multiple sensors and temporal snapshots. The paper became one of the most-cited works in the field and remains an essential entry point for anyone approaching remote sensing from an AI/ML background.


FURTHER READING

1. NASA Earthdata Learning Resources — tutorials, notebooks, and datasets covering the full preprocessing pipeline for Landsat, MODIS, and more.
   https://www.earthdata.nasa.gov/learn/tutorials

2. ESA Sentinel Online — official documentation, user guides, and data access for the full Sentinel fleet.
   https://sentinel.esa.int/web/sentinel/home

3. USGS Landsat Missions — data access, science products, and decades of imagery free to download.
   https://www.usgs.gov/landsat-missions

4. Google Earth Engine — cloud-based platform for planetary-scale geospatial analysis with petabytes of ready-to-use satellite data. The Code Editor lets you run analyses in a browser without downloading a single file.
   https://earthengine.google.com/


KEY TAKEAWAY

Every satellite image is a measurement, not a photograph — and understanding what was actually measured, at what resolution, through what atmosphere, and corrected by what assumptions, is the foundation on which all reliable AI/ML in Earth observation is built.


---

See you tomorrow for Day 5: Machine Learning Fundamentals for Space Applications.

## Callback Question
> On Day 3, we covered the significant constraints on onboard computing power in satellites — limited processing speed, memory, and energy budgets. Given that hyperspectral sensors can generate hundreds of spectral bands per image, how do those hardware constraints influence which preprocessing steps (such as atmospheric correction or band selection) can realistically be performed onboard versus downlinked raw for processing on the ground?

## Feynman Prompt
> In 2-3 sentences, explain the four resolutions of satellite imagery (spatial, spectral, temporal, and radiometric) as if describing it to a colleague who has never heard of them.

## Visual References

![ARIA Damage Proxy Map of Lombok, Indonesia Earthquakes](https://images-assets.nasa.gov/image/PIA22495/PIA22495~thumb.jpg)
*ARIA Damage Proxy Map of Lombok, Indonesia Earthquakes* — Source: [NASA](https://images.nasa.gov/details/PIA22495)

![Multispectral imaging](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/AS09-26-3741_CDB_to_RGB.jpg/960px-AS09-26-3741_CDB_to_RGB.jpg)
*Multispectral imaging* — Source: [Wikipedia (James Stuby based on NASA images, Public domain)](https://en.wikipedia.org/wiki/Multispectral%20imaging)

![Aegopodium podagraria](https://upload.wikimedia.org/wikipedia/commons/b/bf/Aegopodium_podagraria1_ies.jpg)
*Aegopodium podagraria* — Source: [Wikipedia (Frank Vincentz, CC BY-SA 3.0)](https://en.wikipedia.org/wiki/Landsat%20program)
