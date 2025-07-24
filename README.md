# 🧠 Image Processing for Retinal and Neuronal Images

## 📘 Overview

This project applies classical image processing techniques to biomedical images, particularly:
- **Retinal blood vessel segmentation**
- **Neuronal cell extraction**
- **X-ray CT imaging analysis**

We employ a combination of **OpenCV**, **morphological operations**, **histogram analysis**, and **filtering techniques** to improve segmentation accuracy and clarity.

---

## 🔍 Project Structure

### 🔴 Part 1: Retina Image Masking

📌 **Goal**: Extract the circular retinal region from `retina.png`, removing the black background.

**Techniques Used**:
- Grayscale conversion
- Binary thresholding (`cv2.threshold`)
- Morphological operations:
  - Erosion
  - Dilation
  - Opening (`erosion ➝ dilation`)
  - Closing (`dilation ➝ erosion`)

**Kernel Used**:
```python
kernel = np.ones((3,3), np.uint8)
```

📊 **Mathematical Expressions**:

- **Erosion**:
  ```
  f ⊖ h = { j ∈ ℤ | h: ⊆ f }
  ```

- **Dilation**:
  ```
  f ⊕ h = { j ∈ ℤ | (h: ∩ f ≠ ∅) }
  ```

- **Opening**:
  ```
  f ∘ h = (f ⊖ h) ⊕ h
  ```

- **Closing**:
  ```
  f • h = (f ⊕ h) ⊖ h
  ```

🖼️ **Figures**:
- Thresholded image
- Cleaned retinal mask
- Final masked retinal image

---

### 🌈 Part 2: Retinal Background Normalisation

📌 **Goal**: Reduce uneven illumination across the retina to enhance vascular contrast.

**Filtering Techniques Compared**:
- Gaussian Filter
- Mean Filter
- Median Filter

**Kernel Size**: `99x99` (6.6% width, 8.6% height of image)

📐 **Mathematical Formula**:
```math
I_normalised(x,y) = I_original(x,y) / I_background(x,y)
```

🖼️ **Figures**:
- Background illumination estimate
- Evenly illuminated image (Gaussian best)

---

### 🩸 Part 3: Retinal Vessel Segmentation

📌 **Goal**: Segment blood vessels using edge detection methods.

**Pipeline**:
1. Green channel extraction
2. CLAHE
3. Non-local means denoising
4. Morphological masking

**Edge Detection Methods**:
- **Canny**:
  - Thresholds: 25 & 10
  - Good for sharp edges
- **Sobel + Otsu**:
  - Auto-thresholding using inter-class variance minimization
- **Laplacian of Gaussian (LoG)**:
  - 2nd derivative + smoothing

📐 **Canny Gradient Formula**:
```math
G = √(G_x² + G_y²)
```

📐 **Otsu Thresholding**:
```math
σ²_B(t) = w₀(t)σ²₀(t) + w₁(t)σ²₁(t)
```

🖼️ **Figures**:
- Canny edge detection
- Sobel and LoG segmentation
- Morphologically refined vessel maps

---

### ⚙️ Part 4: XCT Physics & Segmentation

📌 **Goal**: Analyse quality factors for blood vessel segmentation from XCT scans.

**Key Concepts**:
- **Beer-Lambert Law**:
  ```math
  I = I₀ * e^{-μx}
  ```
- **Hounsfield Unit (HU)**:
  ```math
  HU = 1000 * (μ - μ_water) / μ_water
  ```

🧪 **Artifacts & Issues**:
- Beam hardening
- Compton scattering
- Motion artefacts
- Partial volume effect

📈 **Solution Strategy**:
- Adequate photon count
- Use of iodine contrast
- Denoising and high-res projections

---

## 🧬 Part 5: Neuronal Image Processing

📌 **Goal**: Segment and analyse mature neurons based on intensity and morphology.

### a) Image Preprocessing
- Grayscale conversion
- Global thresholding (160–255)
- Morphological processing:
  - Erosion `(2x2, iter=2)`
  - Dilation `(3x3, iter=1)`
  - Closing `(3x3)`
  - Opening `(2x2)`

🖼️ **Figures**:
- Grayscale neuron image
- Threshold mask
- Final mature neuron segmentation

---

### b) Brightness Histogram of Neurons

📊 **Steps**:
- Contour detection
- Compute average intensity in each contour
- Plot histogram (10 bins)

📐 **Code Snippet**:
```python
cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
cv2.mean(image, contour_mask)
```

📈 **Insight**:
- Mature neurons: intensity ~190–200

---

### c) Astrocyte Edge Detection

📌 **Goal**: Segment astrocytes using edge detection.

**Methods Compared**:
- **Sobel**
- **Canny**

📐 **Morphological Postprocessing**:
- Small kernel `(2x2)`
- Erosion and dilation with adjusted iterations

🧪 **Challenges**:
- Thin astrocyte structure
- Noise removal limitations
- Kernel size trade-offs

🖼️ **Figures**:
- Edge detection output
- Processed astrocyte segmentation

---

## 🧰 Tools & Libraries

- **Python 3.9+**
- **OpenCV**
- **NumPy**
- **Matplotlib**

---

## 📊 Results Summary

| Task                         | Best Method             | Notes |
|-----------------------------|--------------------------|-------|
| Retina Masking              | Threshold + Morphology  | Clean circle extraction |
| Illumination Normalisation  | Gaussian Filter         | Best balance of detail |
| Vessel Segmentation         | LoG + CLAHE + Otsu      | Preserves fine vessels |
| Neuron Detection            | Global Threshold + Morph | Accurate segmentation |
| Astrocyte Segmentation      | Canny + Morphology      | Limited by resolution |

---

## 📌 Future Work

- Try Deep Learning models (e.g., U-Net, Mask R-CNN)
- Improve astrocyte detection with adaptive filtering
- Apply active contour models for vessel refinement

---

## 🖼️ Sample Outputs

![Retinal Mask](images/retinal_mask.png)
![Neuron Segmentation](images/final_neuron.png)

