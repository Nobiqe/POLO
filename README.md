# YOLOv8n vs YOLOv11n: ID Card Detection for Lightweight Devices

## Overview
This repository documents my journey of fine-tuning and comparing two versions of the YOLO (You Only Look Once) object detection model—**POLOv8** and **POLOv11**—for detecting ID cards in a custom dataset. The project began as a practical need to develop a lightweight detection system, such as for electronic eyes in ATM machines, where resource efficiency and accuracy are critical. This led me to train both models, not only for practical use but also to conduct a comparative analysis, which forms the basis of this work. 

## Project Background
I embarked on this project with the goal of creating an efficient ID card detection system for resource-constrained environments, such as embedded devices in ATMs. The need to balance accuracy, speed, and memory usage on a 4GB GPU (NVIDIA GeForce GTX 950M) drove the development of two model series. This repository captures the step-by-step process, from initial training to final comparisons, with the intent to share insights and potentially inspire similar lightweight AI applications.

## Dataset
The dataset used for training and validation was a custom combination of two publicly available datasets, both focused on identity documents:
- **MIDV-500 Dataset**: A dataset containing 500 images of identity documents (e.g., ID cards, passports) captured under various real-world conditions, such as different lighting, angles, and backgrounds. This dataset provided a diverse set of examples for robust training. [(Dataset Link)](https://paperswithcode.com/dataset/midv-500)
- **MIDV-2019 Dataset**: An earlier dataset with images of identity documents, similar in nature to MIDV-500 but with its own set of variations in document types and capture conditions. [(Dataset Link)](https://paperswithcode.com/dataset/midv-2019)
- **Combination Process**: I merged the MIDV-500 and MIDV-2019 datasets by standardizing their annotations into a YOLO-compatible bounding box format. After combining, I split the data into training and validation sets, resulting in 16,552 training images and 4,046 validation images. The dataset contains a single class (`id_card`) with a confidence score of 1.0. The images were organized into separate training and validation sets to ensure proper evaluation.

## fine-tuning Process
### Hardware and Setup
- **GPU**: NVIDIA GeForce GTX 950M (4GB).
- **Storage**: SSD for faster data loading.
- **Software**: Python 3.10.0, PyTorch 1.12.1+cu113, Ultralytics 8.3.81.

### POLOv8n fine-tuning
- **Model**: Started with the pre-trained YOLOv8n model.
- **Parameters**:
  - Epochs: 40
  - Batch Size: 64
  - Image Size: 416x416
  - Learning Rate: 0.004
  - Optimizer: AdamW
  - Augmentation: Enabled
  - Cosine LR Scheduling: Enabled
- **Duration**: Approximately 20 hours.
- **Final Results**:
  - Precision: 0.999
  - Recall: 0.997
  - mAP50: 0.995
  - mAP50-95: 0.994
  - Inference Time: 20.0ms per image

### POLOv11n fine-tuning
- **Model**: Started with the pre-trained YOLOv11n model.
- **Parameters**: Same as YOLOv8n for fair comparison.
- **Duration**: Approximately 20 hours.
- **Final Results**:
  - Precision: 0.999
  - Recall: 0.998
  - mAP50: 0.995
  - mAP50-95: 0.994
  - Inference Time: 15.9ms per image
  - Parameters: 2.58M, 6.3 GFLOPs (lighter than YOLOv8n's 3.0M, 8.1 GFLOPs)

### Key Differences Between YOLOv8n and YOLOv11n
- **Improved Feature Pyramid Network (FPN)**: Better feature fusion for detecting small objects.
- **Optimized Activation Functions**: Uses **SiLU** instead of ReLU for better gradient flow.
- **Lighter Backbone**: More efficient architecture with reduced computational cost.
- **Refined Post-Processing**: Enhanced Non-Maximum Suppression (NMS) reduces false positives.

## Real-World Applications
- **ATMs**: Secure authentication in banking kiosks.
- **Border Control**: Smart kiosks for document verification.
- **Access Control**: Security gate authorization.
- **Retail & E-commerce**: Identity verification for age-restricted purchases.
- **Mobile Apps**: Real-time document scanning for banking.

## Comparison
| Metric          | polov8.pt (YOLOv8n) |  polov11.pt (YOLOv11n)| Difference         |
|-----------------|-------------------|-----------------------|--------------------|
| Precision       | 0.999             | 0.999                | No change          |
| Recall          | 0.997             | 0.998                | +0.001 (better)    |
| mAP50           | 0.995             | 0.995                | No change          |
| mAP50-95        | 0.994             | 0.994                | No change          |
| Inference Time  | 20.0ms            | 15.9ms               | -4.1ms (20% faster)|
| Parameters      | 3.0M              | 2.58M                | -0.42M (lighter)   |
| GFLOPs          | 8.1               | 6.3                  | -1.8 (optimized)   |

## Usage
- **Training Code**: Located in `train_yolo.py`. Adjust paths and parameters as needed.
- **Inference**: Run:
  ```sh
  yolo detect predict model=<path_to_polo_or_polov11.pt> source=<path_to_images>
  ```



## Inference Results
Visual examples of ID card detection results from fine-tuning:

| POLOv8          | POLOv11          |
|-----------------|------------------|
| ![POLOv8 Results](https://github.com/user-attachments/assets/4eaa2e17-7c5a-4b80-96db-041f4ec24a46) | ![POLOv11 Results](https://github.com/user-attachments/assets/d16d9e7f-9326-4526-b699-05376487591a) |
