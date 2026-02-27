# **AI-Based Forest Cover Change Detection and Monitoring System**

## **\[ Milestone-1 \]**

# **Objective**

The Aravalli Range and the North Eastern Region (NER) of India represent two of the most ecologically significant yet vulnerable landscapes in South Asia. The Aravallis, spanning approximately 692 kilometers, function as a vital natural barrier against the Thar Desert and a critical groundwater recharge zone for the National Capital Region.1 Conversely, the North Eastern states constitute a global biodiversity hotspot, characterized by unique physiography and dense tropical forests that provide essential carbon sequestration and climate regulation. However, both regions face severe anthropogenic threats. While the Aravallis are jeopardized by unregulated mining and rapid urbanization, the North Eastern forests face degradation from urban sprawl and traditional shifting cultivation (Jhum).

The primary objective of this project is to develop a specialized AI-based monitoring and change detection system tailored to ecologically sensitive forest regions in India. Using multi-temporal Sentinel satellite imagery at a 10-meter spatial resolution, the system will employ deep learning–based semantic segmentation to identify, quantify, and analyze land cover transitions over a five-year period. The proposed approach integrates pretrained segmentation backbones with region-specific fine-tuning to assess forest health, quantify vegetation loss, and detect fragmentation across diverse forest ecosystems, including the semi-arid Aravalli hills and the humid tropical forests of North-East India. To address key limitations of existing solutions, the system will incorporate Sentinel-1 (Synthetic Aperture Radar) and Sentinel-2 (optical) data fusion to ensure reliable monitoring in cloud-prone regions, improve spatial precision for detecting small-scale deforestation activities such as shifting cultivation (Jhum) using high-resolution imagery, and implement an automated change mapping framework based on a Siamese U-Net architecture. This design enables direct learning of change features from temporal image pairs, reducing reliance on error-prone post-classification comparison and enhancing the robustness and accuracy of forest cover change detection.

# **Related Works**

The literature review for this project is organized into historical approaches, deep learning architectures, hybrid techniques, and broader systematic reviews.

### **A. Historical Approaches & Traditional ML**

Historical forest mapping relied on manual interpretation and later transitioned to automated pixel-based classification.3 Traditional machine learning (ML) algorithms have served as the benchmarks for land cover classification in both study regions.  

#### **Random Forest, Support Vector Machines, and k-NN Land Cover Methods**

The **Random Forest (RF)** algorithm remains a primary benchmark due to its robustness against noise. In the North Eastern states, RF has shown superior performance in identifying high-risk deforestation zones by analyzing forest density and topographical characteristics. In the Aravallis, RF effectively distinguishes forest from non-forest areas using Sentinel-2’s red-edge and SWIR bands.  

**Support Vector Machines (SVM)** and **k-Nearest Neighbor (k-NN)** have also been widely applied. SVM often achieves high overall accuracies (\~95.8%) in forest identification but is computationally expensive for large-scale regional monitoring. Studies in the Mediterranean and Indian watersheds indicate that while SVM can produce the highest accuracy, all traditional classifiers suffer from the "salt-and-pepper" effect—misclassifying isolated pixels because they ignore spatial context.4

### **B. Deep Learning for Semantic Segmentation**

Deep Learning (DL) has revolutionized remote sensing by automating feature extraction and capturing spatial hierarchies.5

#### **U-Net and Its Variants for Change Detection**

The **U-Net** architecture, featuring an encoder-decoder structure with skip connections, has become the standard for segmenting fine-grained details like forest boundaries.6

1. **Attention U-Net**: Incorporates attention gates to highlight salient features while suppressing noise like cloud shadows, which is critical for the steep ridges of the Aravallis and the high cloud-cover regions of the North East.6  
2. **Residual U-Net (ResUNet)**: Uses residual connections to enable deeper network training, showing superior performance in identifying complex forest polygons compared to standard architectures.7

**U-Net++**: Employs nested skip connections to reduce the semantic gap between encoder and decoder, improving segmentation in areas with ill-defined class boundaries.8

### **D. Reviews & Surveys**

Systematic reviews from 2011 to 2025 highlight a shift from local Convolutional Neural Networks (CNNs) to global **Vision Transformers (ViTs)**, which improve species classification by \~8.4%.9 However, "domain adaptation" remains a significant challenge; models trained on global datasets often experience a 23-45% drop in performance when applied to specific Indian biomes like the semi-arid Aravallis or North Eastern tropical forests.9 This underscores the necessity for region-specific tools that account for localized phenology and drivers of change, such as Jhum cultivation.

## **Identified Gaps and Project Motivation**

Current forest monitoring relies on three major pillars, each acting as a baseline for this project:

* **Hansen Global Forest Change (UMD):** A 30m resolution product derived from Landsat. It is the global standard for annual forest loss but suffers from coarse resolution in fragmented landscapes and lacks the ability to "see" through clouds during the monsoon.12  
* **Dynamic World (Google/WRI):** A near-real-time 10m LULC (Land Use/Land Cover) product. While highly precise, it is a "single-snapshot" classifier. Detecting change requires manual subtraction of two maps, which often compounds classification errors.13  
* **India State of Forest Report (ISFR/FSI):** The biennial report by the Forest Survey of India. It provides excellent administrative statistics but lacks the real-time, high-frequency spatial data needed for rapid intervention.11  
* **Technical Benchmarks:** In the deep learning domain, the **OSCD (Ouchucha Semantic Change Detection)** and **Hi-UCD** datasets serve as benchmarks for evaluating Siamese architectures in satellite imagery.14

**Identified Gaps:**

* **The "Cloud Gap":** Most existing maps of the North East have "holes" during the monsoon months.  
* **The Resolution Gap:** Small-scale shifting cultivation (Jhum), which is common in NE India, is often too small to be accurately mapped at 30m.  
* **The Temporal Gap:** Many products have a 1–2 year lag in reporting.

### **Limitations:**

* **Compute Constraints:** Deep learning on satellite data requires high GPU RAM.  
* **Label Noise:** 30m labels (Hansen) applied to 10m pixels can create "fuzzy" boundaries during training.

## **Project Alignment and Addressing Gaps**

### **Dataset :** 

**Primary Data Sources:**

* **Sentinel-2 (L2A):** Provides 10m multi-spectral bands (R, G, B, NIR) for vegetation health.  
* **Sentinel-1 (GRD):** Provides C-band Radar (VV, VH) to detect structural changes in forest canopy regardless of weather.  
* **Hansen GFC (v1.11):** Used for supervised training labels (extracted via the lossyear band).

### **Comparison: Existing Solutions vs. Proposed Approach**

## 

| Feature | Existing Solutions (e.g., Hansen) | Proposed Approach |
| :---- | :---- | :---- |
| **Resolution** | 30m (Landsat-based) | 10m (Sentinel-based) |
| **Data Type** | Optical Only | **Multi-Modal Fusion** (Radar \+ Optical) |
| **Change Method** | Post-Classification Comparison | **Siamese U-Net** (Feature-level change) |
| **Cloud Robustness** | Low (Optical-only) | **High** (Radar integration) |
| **Detection Goal** | Large scale clear-cutting | Small-scale Jhum and fragmented loss |

**Key Difference:** While existing solutions look at a pixel in isolation, our proposed **Siamese U-Net** looks at **spatial context (texture and patterns)**, allowing it to distinguish between permanent deforestation and seasonal agricultural changes.

### **Baseline Metrics and Improvement Plan**

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

###  **Evaluation and Comparison Strategy**

To validate that our solution is superior to market-available options, we will use a three-tier evaluation:

1. **Quantitative Evaluation:** Compute Precision, Recall, and F1-Score against a "Hold-out" test set derived from the Hansen Dataset.  
2. **Cross-Product Comparison:** Directly compare our 10m change map with the Dynamic World "Mode" subtraction map to see if our model reduces "false positives" caused by shadows or seasonal spectral shifts.  
3. **Visual "Ground Truth" Verification:** Randomly sample 50 points where our model detects change but baselines do not. We will use **Historical Imagery from Google Earth Pro** (high-resolution) to manually verify which product was correct.

# 

# References

1. The Aravalli crisis: Why Ecology needs to be protected & strategies to ensure sustainable development \- IRM India,[https://www.theirmindia.org/blog/aravallis-at-risk-how-tropical-losses-threaten-global-systems-the-strategies-to-mitigate-their-impacts/](https://www.theirmindia.org/blog/aravallis-at-risk-how-tropical-losses-threaten-global-systems-the-strategies-to-mitigate-their-impacts/)  
2. he Application of Sentinel-2 Data for Automatic Forest Cover Changes Assessment – Białowieża Primeval Forest Case Study \- Civil and Environmental Engineering Reports,[https://www.ceer.com.pl/The-Application-of-Sentinel-2-Data-for-Automatic-Forest-Cover-Changes-Assessment,167604,0,2.html](https://www.ceer.com.pl/The-Application-of-Sentinel-2-Data-for-Automatic-Forest-Cover-Changes-Assessment,167604,0,2.html)  
3. Forest Land Resource Information Acquisition with Sentinel-2 Image Utilizing Support Vector Machine, K-Nearest Neighbor, Random Forest, Decision Trees and Multi-Layer Perceptron \- MDPI,[https://www.mdpi.com/1999-4907/14/2/254](https://www.mdpi.com/1999-4907/14/2/254)  
4. Remote sensing based forest cover classification using machine learning \- PMC, [https://pmc.ncbi.nlm.nih.gov/articles/PMC10762169/](https://pmc.ncbi.nlm.nih.gov/articles/PMC10762169/)  
5. Deep learning for deforestation detection. An insight into the state of the art., [https://mobile.fhstp.ac.at/allgemein/deep-learning-for-deforestation-detection-an-insight-into-the-state-of-the-art/](https://mobile.fhstp.ac.at/allgemein/deep-learning-for-deforestation-detection-an-insight-into-the-state-of-the-art/)  
6. (PDF) FCD-AttResU-Net: An improved forest change detection in, [https://www.researchgate.net/publication/373113713\_FCD-AttResU-Net\_An\_improved\_forest\_change\_detection\_in\_Sentinel-2\_satellite\_images\_using\_attention\_residual\_U-Net](https://www.researchgate.net/publication/373113713_FCD-AttResU-Net_An_improved_forest_change_detection_in_Sentinel-2_satellite_images_using_attention_residual_U-Net)  
7. ForCM: Forest Cover Mapping from Multispectral Sentinel-2 Image by Integrating Deep Learning with Object-Based Image Analysis \- ResearchGate,[https://www.researchgate.net/publication/399176538\_ForCM\_Forest\_Cover\_Mapping\_from\_Multispectral\_Sentinel-2\_Image\_by\_Integrating\_Deep\_Learning\_with\_Object-Based\_Image\_Analysis](https://www.researchgate.net/publication/399176538_ForCM_Forest_Cover_Mapping_from_Multispectral_Sentinel-2_Image_by_Integrating_Deep_Learning_with_Object-Based_Image_Analysis)  
8. Comparative Analysis of Deep Learning and OBIA on Satellite Images for Forest Cover Monitoring ResearchGate,[https://www.researchgate.net/publication/392564028\_Comparative\_Analysis\_of\_Deep\_Learning\_and\_OBIA\_on\_Satellite\_Images\_for\_Forest\_Cover\_Monitoring](https://www.researchgate.net/publication/392564028_Comparative_Analysis_of_Deep_Learning_and_OBIA_on_Satellite_Images_for_Forest_Cover_Monitoring)  
9. (PDF) Deep Learning Architectures for Forest Monitoring and Tree Inventory Management: A Review of Computer Vision Applications and Challenges \- ResearchGate,[https://www.researchgate.net/publication/400936414\_Deep\_Learning\_Architectures\_for\_Forest\_Monitoring\_and\_Tree\_Inventory\_Management\_A\_Review\_of\_Computer\_Vision\_Applications\_and\_Challenges](https://www.researchgate.net/publication/400936414_Deep_Learning_Architectures_for_Forest_Monitoring_and_Tree_Inventory_Management_A_Review_of_Computer_Vision_Applications_and_Challenges)  
10. Deep Learning-Based Detection of Urban Forest Cover Change ..., [https://pdfs.semanticscholar.org/8bb2/28fe652ea79f24326342fbfb144f7ad85c4a.pdf](https://pdfs.semanticscholar.org/8bb2/28fe652ea79f24326342fbfb144f7ad85c4a.pdf)  
11. **India State of Forest Report ,**[https://fsi.nic.in/forest-report-2023](https://fsi.nic.in/forest-report-2023)  
12. **Hansen Global Forest Change (UMD) ,** [https://developers.google.com/earth-engine/datasets/catalog/UMD\_hansen\_global\_forest\_change\_2024\_v1\_12](https://developers.google.com/earth-engine/datasets/catalog/UMD_hansen_global_forest_change_2024_v1_12)  
13. **Dynamic World (Google/WRI) ,** [https://dynamicworld.app/about/](https://dynamicworld.app/about/)  
14. Ouchucha Semantic Change Detection [https://www.researchgate.net/figure/Change-detection-results-of-several-methods-on-the-OSCD-dataset\_tbl2\_328418297](https://www.researchgate.net/figure/Change-detection-results-of-several-methods-on-the-OSCD-dataset_tbl2_328418297)
