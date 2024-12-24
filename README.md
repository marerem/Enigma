# Eingma

## Overview

The **Eingma** project focuses on analyzing visual latent factors and their influence on perception and reaction. By dissecting scenes into smaller segments, we aim to better understand how contextual elements, such as prior experiences, visual stimuli, and auditory components, shape reactions. This is particularly relevant in scenarios where subjects (e.g., primates) encounter familiar or unfamiliar patterns.


---

### File Structure

The repository is organized as follows:

```
├── Enigma_CV_Scientist_Engineer.ipynb
├── SAM_automatic_mask_generator_example.ipynb
```

#### **Files and Descriptions**

1. **Enigma_CV_Scientist_Engineer.ipynb**  
   - Contains implementations and descriptions for:
     - **Depth Estimation**: Using `MiDaS` with `DPT_Large` for scene depth analysis.  
     - **Segmentation**: Includes `DeepLabV3` for object/person segmentation.  
     - **Object Detection**: Implements `Faster R-CNN` for bounding box detection and object classification.  
     - **Object Description**: Integrates **BLIP** (Bootstrapped Language-Image Pre-training) to generate natural language descriptions for detected objects, enhancing semantic understanding.

2. **SAM_automatic_mask_generator_example.ipynb**  
   - Demonstrates semantic segmentation using the **Segment Anything Model (SAM)**.  
   - Focuses on automatic mask generation for diverse objects in images.  
   - Useful for detailed scene parsing and ROI extraction.

---

### Note:
- The **BLIP** model in `Enigma_CV_Scientist_Engineer.ipynb` generates textual descriptions for:
  - Cropped Regions of Interest (ROIs) from object detection.
  - Entire images, providing a contextual summary of the scene.

---

## Solution Proposal

### 1. Scene Analysis

To conduct an effective experiment, the original video content is segmented into distinct parts for easier interpretation and analysis. Key factors influencing reactions include:

- **Prior Experiences**: A subject's familiarity with patterns (e.g., human faces) may yield different responses compared to novel objects (e.g., robots in motion).
- **Auditory Influence**: Sounds can significantly alter perceptions. For example:
  - A scene with robots and humans might be interpreted as aggressive if accompanied by loud, frightening sounds.
  - The same scene may feel neutral or conversational with soft dialog-based audio.

Taking these considerations into account, the scenes have been divided as follows:

#### Segmented Scenes
1. **Scene 1**: A boy converses with a girl who has a robot arm.  
   - **Frames #**: 73  
2. **Scene 2**: The same dialogue and voices, represented as drawings.  
   - **Frames #**: 312  
3. **Scene 3**: A conversation among three people, focusing on facial expressions.  
   - **Frame #s**: 264  

All video frames can be accessed via the following link:  
[Download Frames](https://drive.google.com/drive/folders/19g_h4KV-4YgqZCSMhJWflbaMyozMm6wU?usp=share_link)

---

### 2. Audio Analysis (Optional)

If audio data is available, Fourier-domain representations of the audio for each scene would be highly beneficial. This allows us to analyze correlations between:

- **Brain Activity**: Patterns of neural response.
- **Frequency Response**: Auditory stimuli linked to specific scenes.

---

### 3. Task: Compute Annotations/Modalities Using Different Techniques

To efficiently analyze the data within the constraints of time and resources, a subset of frames will be used for computation:

- **Selected Frames**: 1–5 frames from each video clip.  

This subset will be analyzed across various techniques to annotate and evaluate different modalities effectively.


### 3.1 Geometric Modalities: Depth Estimation

#### **Resource**  
[MiDaS](https://github.com/isl-org/MiDaS)

#### **Model**  
`DPT_Large`

#### **Result**  
![Depth Estimation Output](https://github.com/user-attachments/assets/d5177772-0d9e-407c-a802-f34b9b6ce071)

---

#### **Analysis**

MiDaS, a cutting-edge depth estimation model, excels in diverse scenarios involving everyday scenes, landscapes, and objects. However, applying it to robotic environments reveals several challenges:

1. **Limited Training Data for Robots**  
   - MiDaS was not trained on robot-specific data. This absence of learned priors for robotic structures—such as their unique geometries and materials—results in suboptimal depth predictions.  
   - Without prior knowledge, the model struggles to generalize depth cues for robotic environments effectively.

2. **Reflective Surfaces and Intricate Designs**  
   - Robots often feature materials like metal and glossy plastics, which cause inconsistent reflections, confusing the model's depth inference.  
   - Complex geometries and moving parts further complicate the depth estimation process.

3. **Challenges in Industrial Environments**  
   - Harsh lighting, shadows, and uneven illumination typical of industrial settings obscure critical depth cues like texture gradients and shadows.  
   - Uniform or artificial textures prevalent in robotic scenes lack the rich variations essential for monocular depth estimation.

4. **Absence of Temporal Context**  
   - MiDaS operates on single images, making it unable to leverage temporal cues that could improve depth estimation accuracy in dynamic scenes.

---

#### **Proposed Solutions**

To address these limitations, we suggest the following enhancements:

- **Fine-Tune MiDaS on Robot-Specific Data**  
  Incorporate robot-centric datasets into the model training pipeline to adapt MiDaS for robotic environments.  

- **Integrate Multi-View or Temporal Information**  
  Leverage advanced video-based depth estimation models like:
  - [DeepV2D](https://github.com/princeton-vl/DeepV2D)  
  - [DROID-SLAM](https://github.com/princeton-vl/DROID-SLAM)  

These models utilize temporal and multi-frame data to capture depth cues more effectively in dynamic scenarios.

---

### 3.1* Geometric Modalities: Surface Estimation

Surface normals can be derived from depth data to enhance geometric understanding. For implementation, refer to this resource:  
[Depth-to-Normal Estimation](https://github.com/baegwangbin/DSINE/blob/main/notes/depth_to_normal.ipynb)

**Note**: The quality of surface estimation is directly dependent on the accuracy of the depth map. Enhancing depth predictions is critical for reliable surface normal estimation.

Here’s a polished and structured version of the **Segmentation** section for your README.md:

---

### 4. Segmentation

#### **Model**  
`DeepLabV3`

#### **Result**  
![Segmentation Output](https://github.com/user-attachments/assets/5b2baf7c-8e18-4cd5-907b-3632786cc101)

---

#### **Analysis**

The `DeepLabV3` model demonstrates strong performance in detecting common classes, such as people. However, when applied to robot-specific tasks, it faces significant limitations:

1. **Lack of Robot-Specific Classes**  
   - The pretrained model lacks classes specifically for robots or robotic components, resulting in poor segmentation for robotic objects.  
   - While person detection performs well, the model struggles with robotic designs, especially for objects not covered in its training data.

2. **Challenges with Drawings and Non-Standard Representations**  
   - For scenes involving drawings or abstract representations of robots, the pretrained model often fails to produce meaningful segmentations.  
   - The results are either mismatched to existing classes or fail to classify at all.

---

#### **Proposed Solution: Using SAM**  

The [Segment Anything Model (SAM)](https://github.com/facebookresearch/segment-anything) offers a promising alternative. SAM provides robust semantic segmentation capabilities and demonstrates significant advantages:

1. **Human Segmentation**  
   - SAM performs exceptionally well in segmenting human-related features, producing clean and accurate masks.

2. **Object Detection in Drawings**  
   - While SAM may not classify robotic objects in drawings directly, it effectively identifies major objects within a scene.  
   - These objects can be categorized into approximate classes, improving segmentation of abstract or non-standard visuals.

---

#### **Examples**

##### 1. **Human Segmentation Results**
![Result 1](https://github.com/user-attachments/assets/9d73cc84-8f4e-4a2e-af1f-6488a1984c5b)  
![Result 2](https://github.com/user-attachments/assets/d6036b45-6434-45a7-8514-709f56c62d86)  

##### 2. **Segmentation of Drawings**
![Result 3](https://github.com/user-attachments/assets/4f5796e5-aecb-42d3-9799-be8506447eb5)  
![Result 4](https://github.com/user-attachments/assets/073d247d-fedf-4504-9b37-22ae190838d5)  

---

### **Next Steps**  
1. Fine-tune SAM or other segmentation models on datasets containing robot-specific and drawing-based representations.  
2. Integrate SAM with post-processing to map segments into meaningful classes for robotic and abstract objects.  
3. Explore hybrid models combining pretrained semantic segmentation with custom classes for improved performance.  

Here’s a well-structured version of the **Edge Detection** section for your README.md:

---

### 5. Edge Detection

#### **Overview**

Edge detection is a critical preprocessing step in many computer vision tasks, particularly for feature extraction and object analysis. Various approaches can be applied, ranging from traditional methods to advanced deep learning-based models.

---

#### **Methods Explored**

1. **Traditional Approaches**  
   - **Gradient-based techniques**: Use image gradients to detect intensity changes at edges.  
   - **Top-Hat filtering**: Enhances edges by subtracting a smoothed version of the image.  
   - **Canny Edge Detection**: A widely used algorithm for precise edge localization.

2. **Advanced Approaches**  
   - **Holistically-Nested Edge Detection (HED)**:  
     A deep learning-based method designed for high-quality edge detection.  
     [GitHub Repository: HED](https://github.com/s9xie/hed)  

---

#### **Observations**

While traditional methods work well for general edge detection, their utility is limited when applied to complex object-specific tasks. Similarly, advanced methods like HED require a deeper understanding and careful tuning to produce meaningful results.

1. **Challenges**  
   - Applying edge detection as a standalone process doesn’t directly contribute to meaningful object analysis, particularly for specific tasks involving robots or abstract objects.  
   - Integrating edge detection with other modalities (e.g., depth estimation) could enhance its applicability.  

2. **Proposed Use Case**  
   - **Feature Extraction for Specific Objects**:  
     Combine edge detection with depth-based object segmentation to detect and analyze specific objects, such as apples or balls, within a scene.  
   - **Application in Experiments**:  
     For example, edge detection can be used as part of a pipeline to analyze how a subject (e.g., a monkey) responds to visually distinct objects.  

---

#### **Next Steps**

1. Focus on **integrating edge detection with depth and segmentation pipelines** for object-specific feature extraction.  
2. Evaluate the effectiveness of combining edge detection results with depth maps to prioritize key objects in a scene.  
3. Commit HED or similar advanced models only if edge-based features are shown to improve task-specific analysis.

Here’s a polished and well-structured version of the **Object Detection** section for your README.md:

---

### 6. Object Detection

#### **Example Output**  
<img width="911" alt="Screenshot 2024-12-24 at 20 15 12" src="https://github.com/user-attachments/assets/aca72d03-8a4e-4ffd-a193-f7f45a575b2a" />

---

#### **Model and Process**

1. **Model Loading**  
   - A pre-trained Faster R-CNN model is used for object detection.  
   - The model identifies objects in the image, providing:
     - **Bounding Boxes**: Coordinates of detected objects.
     - **Labels**: Predicted object names.
     - **Confidence Scores**: Probabilities for each detection.

---

#### **Pipeline Steps**

1. **Crop and Extract Detected Regions**  
   - Utilize bounding box coordinates `(x1, y1, x2, y2)` to crop **Regions of Interest (ROIs)** from the original image.  
   - ROIs allow further analysis on detected objects.

2. **Classify Objects (Optional)**  
   - If additional classification is required, pass cropped ROIs through a classification model such as:
     - **ResNet**  
     - **MobileNet**  
   - This step categorizes objects into specific classes if the detection model doesn’t provide sufficient detail.

3. **Generate Descriptions**  
   - Use a pre-trained image captioning model to create natural language descriptions for each ROI. Example models include:
     - **BLIP**  
     - **CLIP**  
     - **Show-and-Tell**  
   - Descriptions can provide semantic context for detected objects.

4. **Combine Results**  
   - Annotate the original image with:
     - Bounding boxes and object labels.  
     - Textual descriptions for each detected object or ROI.  
   - Optionally, generate a holistic description of the entire image using image captioning models like **BLIP**.

---

#### **Example Description using BLIP**

**Scene Context**:  
"This image depicts what looks like a sci-fi or futuristic scene featuring holographic figures. The figures are stylized in glowing neon outlines—one orange and the other blue—likely representing digital avatars, robots, or virtual constructs. These holographic structures are blocky and humanoid in shape, composed of wireframe cubes that outline their forms.

The background suggests a high-tech or industrial environment, with complex machinery, wires, and components in a dimly lit, metallic setting. There are red laser-like points scattered throughout, adding a sense of high-tech ambiance."

---

#### **Future Enhancements**

1. Integrate scene understanding using image captioning to describe the entire context of an image dynamically.  
2. Fine-tune object detection models to include specific categories relevant to the domain (e.g., robots, machinery, futuristic elements).  
3. Explore multimodal approaches combining object detection, captioning, and depth estimation for richer scene annotations.  
