## **1\. Project Objective**

The primary objective of this project is to develop a robust, deep learning-based framework to detect and quantify forest cover change in the North East (NE) India region between 2019 and 2024\.

Specifically, the project aims to:

* **Overcome Cloud Constraints:** Utilize a fusion of Sentinel-1 (Synthetic Aperture Radar) and Sentinel-2 (Optical) imagery to ensure consistent monitoring in a region characterized by persistent cloud cover.  
* **Improve Spatial Precision:** Enhance the detection of small-scale deforestation (e.g., shifting cultivation or "Jhum") by utilizing 10m resolution data, surpassing the 30m standard of current global products.  
* **Automated Change Mapping:** Implement a Siamese U-Net architecture to automatically identify "change features" by comparing temporal image pairs rather than relying on traditional post-classification comparison.

---

## **2\. Research and Review of Existing Solutions**

Current forest monitoring relies on three major pillars, each acting as a baseline for this project:

* **Hansen Global Forest Change (UMD):** A 30m resolution product derived from Landsat. It is the global standard for annual forest loss but suffers from coarse resolution in fragmented landscapes and lacks the ability to "see" through clouds during the monsoon.  
* **Dynamic World (Google/WRI):** A near-real-time 10m LULC (Land Use/Land Cover) product. While highly precise, it is a "single-snapshot" classifier. Detecting change requires manual subtraction of two maps, which often compounds classification errors.  
* **India State of Forest Report (ISFR/FSI):** The biennial report by the Forest Survey of India. It provides excellent administrative statistics but lacks the real-time, high-frequency spatial data needed for rapid intervention.  
* **Technical Benchmarks:** In the deep learning domain, the **OSCD (Ouchucha Semantic Change Detection)** and **Hi-UCD** datasets serve as benchmarks for evaluating Siamese architectures in satellite imagery.

---

## **3\. Detailed Findings and Data Sources**

Research indicates that North East India is a critical biodiversity hotspot currently facing significant forest loss.

* **Regional Statistics:** According to the *ISFR 2021*, the North East region showed a decrease of 1,020 sq. km of forest cover. States like Arunachal Pradesh and Nagaland are among the most affected.  
* **Primary Data Sources:**  
  * **Sentinel-2 (L2A):** Provides 10m multi-spectral bands (R, G, B, NIR) for vegetation health.  
  * **Sentinel-1 (GRD):** Provides C-band Radar (VV, VH) to detect structural changes in forest canopy regardless of weather.  
  * **Hansen GFC (v1.11):** Used for supervised training labels (extracted via the lossyear band).  
* **Technical Constraints:** Cloud cover in NE India is present over 70% of the year during the monsoon, necessitating the use of "Dry Season" (Nov–Mar) composites.

---

## **4\. Comparison: Existing Solutions vs. Proposed Approach**

| Feature | Existing Solutions (e.g., Hansen) | Proposed Approach |
| :---- | :---- | :---- |
| **Resolution** | 30m (Landsat-based) | 10m (Sentinel-based) |
| **Data Type** | Optical Only | **Multi-Modal Fusion** (Radar \+ Optical) |
| **Change Method** | Post-Classification Comparison | **Siamese U-Net** (Feature-level change) |
| **Cloud Robustness** | Low (Optical-only) | **High** (Radar integration) |
| **Detection Goal** | Large scale clear-cutting | Small-scale Jhum and fragmented loss |

**Key Difference:** While existing solutions look at a pixel in isolation, our proposed **Siamese U-Net** looks at **spatial context (texture and patterns)**, allowing it to distinguish between permanent deforestation and seasonal agricultural changes.

---

## **5\. Baseline Metrics and Improvement Plan**

The current state-of-the-art for forest change detection in this region (using 30m products) yields an estimated **F1-Score of 0.70–0.75**.

**Current Baselines:**

* **Hansen GFC Accuracy:** \~75% in tropical/sub-tropical fragmented forests.  
* **Dynamic World mIoU:** \~0.72 for tree cover classes.

**Our Goal for Improvement:**

* **Target Metric:** Achieve an **F1-Score \> 0.80** and **IoU (Intersection over Union) \> 0.70**.  
* **Strategy for Improvement:**  
  1. **Radar Fusion:** By adding Sentinel-1, we eliminate "no-data" areas caused by clouds.  
  2. **Siamese Learning:** By training the model to look at *differences* directly, we reduce the "noise" found in comparing two separate classifications.  
  3. **Spatial Refinement:** Using 10m imagery will allow the model to capture the narrow logging roads and small Jhum patches that are "blurred" in 30m baseline products.  
* 

---

## **6\. Evaluation and Comparison Strategy**

To validate that our solution is superior to market-available options, we will use a three-tier evaluation:

1. **Quantitative Evaluation:** Compute Precision, Recall, and F1-Score against a "Hold-out" test set derived from the Hansen Dataset.  
2. **Cross-Product Comparison:** Directly compare our 10m change map with the Dynamic World "Mode" subtraction map to see if our model reduces "false positives" caused by shadows or seasonal spectral shifts.  
3. **Visual "Ground Truth" Verification:** Randomly sample 50 points where our model detects change but baselines do not. We will use **Historical Imagery from Google Earth Pro** (high-resolution) to manually verify which product was correct.  
     
   

---

## **7\. Gaps, Limitations, and Opportunities**

### **Identified Gaps:**

* **The "Cloud Gap":** Most existing maps of the North East have "holes" during the monsoon months.  
* **The Resolution Gap:** Small-scale shifting cultivation (Jhum), which is common in NE India, is often too small to be accurately mapped at 30m.  
* **The Temporal Gap:** Many products have a 1–2 year lag in reporting.

### **Limitations:**

* **Compute Constraints:** Deep learning on satellite data requires high GPU RAM.  
* **Label Noise:** 30m labels (Hansen) applied to 10m pixels can create "fuzzy" boundaries during training.

### **Opportunities for Improvement:**

* **Leveraging Radar:** The availability of Sentinel-1 is a massive opportunity for NE India that remains underutilized in mainstream LULC products.  
* **Transfer Learning:** Pre-training the Siamese model on global change datasets (like OSCD) and fine-tuning it on NE India data provides a path to high accuracy with limited local labels.  
* **Impact:** A localized, high-resolution forest monitor can empower local communities and forest departments in Nagaland, Manipur, and beyond to respond to illegal logging in real-time.

