# Advanced AI Proctoring & Attention Monitoring System

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![OpenCV](https://img.shields.io/badge/OpenCV-4.x-green.svg)
![YOLOv11](https://img.shields.io/badge/YOLO-v11-orange.svg)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Latest-blueviolet.svg)

## Overview
This repository contains a multi-modal Computer Vision and Behavioral Telemetry pipeline designed for high-stakes exam proctoring and e-learning attention monitoring. 

Unlike standard proctoring scripts that rely on static object detection, this system utilizes **temporal heuristics, Euclidean geometry, and Model Ensembling** to mathematically verify human behavior and eliminate environmental false positives. The pipeline ingests video, processes it through three concurrent AI models, and generates a structured, time-series dataset for Exploratory Data Analysis (EDA).

## Core Architecture
* **MediaPipe Face Mesh & Pose:** Real-time 468-point 3D sub-pixel facial geometry and skeletal tracking.
* **Dual-YOLOv11 Ensemble:** Concurrent execution of a highly specific custom YOLO model and a generalized COCO YOLO model.
* **OS Sandboxing (`PyGetWindow`):** Live Operating System hooks to monitor desktop focus and background applications.
* **NumPy Matrix Processing:** C-level mathematical operations for instant environmental array analysis.

---

## Key Features & Logic

### 1. Biometric & Behavioral Tracking
* **Speech Detection (Derivative Oscillation):** Tracks a 10-frame rolling history of the Mouth Aspect Ratio (MAR). Differentiates rhythmic human speech from static expressions (smiling/shock) by calculating jaw direction reversals.
* **Microsleep Detection (Temporal Buffers):** Monitors Eye Aspect Ratio (EAR). Ignores standard human blinks (~300ms) by requiring the EAR to remain below the threshold for > 2.0 continuous seconds before triggering a critical violation.
* **Dynamic Spatial Bounding (Hand-on-Face):** Dynamically calculates exact face dimensions using ear and chin landmarks to draw a customized 3D bounding box (with a 30% neck buffer). Flags obscuration if wrist/finger joints enter this zone.
* **Gaze Tracking:** Calculates head rotation via nose-to-eye alignment, applying a strict 6.0-second time buffer to differentiate natural thinking from active distraction.

### 2. Model Ensembling (Contraband Detection)
* **IoU Consensus Logic:** To eliminate "flashlight attacks" or screen glare triggering false phone detections, the system runs two YOLO models simultaneously. A detection is only validated if both models flag the object and their bounding boxes achieve an Intersection over Union (IoU) of > 40%.

### 3. Environmental Auditing
* **Lighting & Glare Checks:** Converts frames to grayscale matrices to calculate mean pixel intensity (flagging dark rooms) and boolean masking to count blown-out pure white pixels (flagging camera blinding attacks).

### 4. Digital OS Sandboxing
* **Background Polling:** Continuously polls the OS to detect banned applications (e.g., Chrome, Safari, Edge), even if they are minimized in the background.
* **Active Focus Tracking:** Validates that the active window matches the authorized exam software or IDE.

### 5. Time-Series Synchronization
* **Hardware-Agnostic Telemetry:** Decouples system logic from the internal CPU clock. Calculates true "video time" based on absolute frame counts and extracted FPS, ensuring perfect data synchronization even under heavy processing loads.

---

## Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/yourusername/ai-proctoring-system.git](https://github.com/yourusername/ai-proctoring-system.git)
   cd ai-proctoring-system
