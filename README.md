# Eingma

## Overview

The **Eingma** project focuses on analyzing visual latent factors and their influence on perception and reaction. By dissecting scenes into smaller segments, we aim to better understand how contextual elements, such as prior experiences, visual stimuli, and auditory components, shape reactions. This is particularly relevant in scenarios where subjects (e.g., primates) encounter familiar or unfamiliar patterns.

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

### 3.1 Geometric modalities (Depth Estimation)

Resorse : https://github.com/isl-org/MiDaS
Model : DPT_Large

Result : ![combined_depth_frames-2](https://github.com/user-attachments/assets/d5177772-0d9e-407c-a802-f34b9b6ce071)

MiDaS was trained on diverse datasets containing everyday scenes, landscapes, and objects. Robots might possess unique visual characteristics that are underrepresented or absent in MiDaS's training data. The model may struggle to generalize depth cues from robot-specific features, leading to inaccurate depth map. robots were not part of MiDaS's training datasets, the model lacks learned priors specific to robotic structures. Without prior knowledge of robot geometries, materials, and typical environments, MiDaS may misinterpret depth cues.Robots often feature intricate designs, moving parts, and reflective materials (like metal or glossy plastics). Reflective surfaces can distort perceived depth by causing inconsistent reflections, while complex geometries can confuse the model's ability to infer accurate depth relationships.Industrial environments where robots operate may have harsh lighting, shadows, or uneven illumination.Poor or inconsistent lighting can obscure depth cues such as texture gradients and shadows, which MiDaS relies on for depth estimation.Robots might have uniform textures or lack the rich texture variations present in natural scenes.Texture gradients are essential for monocular depth estimation. Uniform or artificial textures can deprive MiDaS of necessary cues to infer depth accurately.MiDaS performs depth estimation based solely on single images without temporal context.

Solution : Fine-Tuning MiDaS on Robot-Specific Data, Integrating Multi-View or Temporal Information. Can use Multi-Frame (Video-Based) Depth Estimation Models like DeepV2D or DROID-SLAM to captute temproral domian.

### 3.1* Geometric modalities (Surface Estimation)

Based on depth we can estimate surface normal :https://github.com/baegwangbin/DSINE/blob/main/notes/depth_to_normal.ipynb
But again quality will depedns on depth.
Here’s a well-structured and polished version for your README.md:

---

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
