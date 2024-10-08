# Multi-target Multi-camera Person Tracking

This project introduces an innovative framework designed to improve surveillance, monitoring, and anomaly detection systems through consistent tracking and re-identification of people across both overlapping and non-overlapping cameras. To continuously track and re-identify individuals, a fusion of people’s motion and appearance features is leveraged. Addressing the difficulties posed by fluctuating lighting conditions, quality of frames, and occlusions, this approach refines and connects the tracking results from individual cameras to produce reliable and continuous tracklets. Lastly, hierarchical clustering organizes single-camera tracklets into distinct groups of unique identities.

### Problem Definition
- **Goal**: Consistently track and re-identify people across multi-view cameras.
- **Scope**:
  - Generalize to both overlapping and non-overlapping cameras indoors and outdoors.
  - Address challenges in viewpoint variance and frame quality through refinement techniques.

<img src="images/box_exchanging_img.png" alt="box_exchanging_img" width="400">

### Importance of the Problem
- **Public safety and security**
- **Traffic monitoring and management**
- **Healthcare and elderly care**

### Proposed Tracking Framework

<img src="images/proposed_pipeline.png" alt="proposed_pipeline" width="600">

### Step 0: Dataset Preparation
- **Dataset**: [MEVA](https://mevadata.org/) (Multimedia Event Detection and Activity Dataset)
  - A challenging large-scale video dataset designed for activity recognition in multi-camera environments.
  - Contains over 9,300 hours of untrimmed videos with:
    - Diverse backgrounds, camera poses, illuminations, and indoor/outdoor scenes.
    - Videos taken at different times of the day, month, and year, split into 5-minute segments.
  - Involves 158 unique people wearing 598 outfits across 33 camera views.
  
<img src="images/meva_dataset_samples.png" alt="meva_dataset_samples" width="600">

- **Focus**:
  - We focus on 5 connected cameras:
    - 14 videos.
    - Each video contains 9,000 frames (~5 minutes long).
    - 20 unique people across cameras.

<img src="images/meva_dataset_site_map_focus.png" alt="meva_dataset_site_map_focus" width="400">

- **Annotations**:
  - By default, the dataset doesn’t provide consistent IDs for people.
  - We annotated part of the dataset using a custom annotation tool due to the dataset’s large scale.

<img src="images/annotaion_results.png" alt="annotaion_results" width="600">

### Step 1: Single-view Multi-Object Tracking
- **[BoT-SORT](https://github.com/NirAharon/BoT-SORT/tree/main): Robust Associations Multi-Pedestrian Tracking**:
  - Fuses motion and appearance features.
  - Uses Kalman filter for motion-based future position estimation.
  - Employs BoT (SBS) appearance-based feature extractor to reduce tracking errors.

### Step 2: Tracklet Refinement
- **Objective**: Split tracklets containing different identities.
  - Uses intra-variance of tracklets to detect tracklets that should be split.
  - **Key insight**:
    - Higher intra-variance indicates higher appearance variation, suggesting that the tracklet contains different identities.
  - Applies K-Means clustering to split tracklets.
  - Reduces errors caused by single-camera trackers.

<img src="images/tracklet_refinement.png" alt="tracklet_refinement" width="600">

### Step 3: Intra-camera Tracklet Association
- **Approach**: 
  - Uses agglomerative clustering to group tracklets with the same identity based on appearance features.
  - **Process**:
    - Calculate the aggregated distance matrix between each pair of tracklets.
    - Cluster tracklets using the aggregated distance matrix.

<img src="images/tracklet_clustering.png" alt="tracklet_clustering" width="600">

### Step 4: Intra-camera Clustering Refinement
- Follows the same approach described in **Tracklet Refinement**.

### Step 5: Inter-camera Tracklet Association
- Follows the same approach described in **Intra-camera Tracklet Association**.

### Quantitative Results
| Method  | IDF1 | IDP | IDR | IDS |
| --- | --- | --- | --- | --- | 
| Pipeline w/o refinement | 27.3% | 22.2% | 34.7% | 375 | 
| Pipeline + pre-clustering refinement | 32.2% | 26.9% | 37.9% | 386 |
| Pipeline + pre/post-clustering refinement | 34.8% | 28.8% | 44.3% | 353 |

### Qualitative Results
**Single-camera Different Time Person Tracking**

<img src="images/multi-times-results.png" alt="multi-times-results" width="600">

**Multi-view Person Tracking**

<img src="images/multi-camera-results.png" alt="multi-camera-results" width="600">

For the technical details of the project and experiments, please refer to [my presentation slides](files/Kimia_Afshari_MS_Project_Defense_Presentation_Slides.pdf).
