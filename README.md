# Project 4 — Image or Text Recognition (Basic)
**DecodeLabs Artificial Intelligence Training Kit — Batch 2026**

This project implements **both** optional execution paths described in the
brief, each satisfying the four Gatekeeper Rule validations.

# DecodeLabs_P3_Image_or_Text_Recognition / Object Detection using MobileNet SSD

## 📌 Project Overview
This project implements an object detection system using the **MobileNet SSD (Single Shot Detector)** deep learning model with **OpenCV**. The application detects multiple objects in an image and draws bounding boxes with class labels and confidence scores.

The project is designed to run in **Google Colab** and allows users to upload their own images for object detection.

---

## 🚀 Features

- Detects multiple objects in a single image
- Uses the pre-trained MobileNet SSD model
- Displays object labels with confidence scores
- Supports custom image uploads
- Easy to run in Google Colab
- Lightweight and fast inference

---

## 🛠 Technologies Used

- Python
- OpenCV
- NumPy
- Google Colab
- MobileNet SSD (Caffe Model)

---

## 📂 Project Structure

```
project4/
│
├── object_detection_path/
│   ├── object_detection.py
│   ├── MobileNetSSD_deploy.prototxt
│   └── MobileNetSSD_deploy.caffemodel
│
├── sample_images/
│
└── DecodeLabs_P3_Image_or_Text_Recognition.ipynb
```

---

## 📥 Installation

Clone the repository:

```bash
git clone https://github.com/ubaid32007/DecodeLabs_P4_Image-or-Text-Recognition
```

Install dependencies:

```bash
pip install opencv-python numpy
```

---

## ▶️ How to Run

1. Open the notebook in Google Colab.
2. Upload your image.
3. Run all notebook cells.
4. The model will:
   - Load the MobileNet SSD network.
   - Detect objects in the uploaded image.
   - Draw bounding boxes and labels.
   - Display the final output image.

---

## 📊 Model

The project uses the pre-trained **MobileNet SSD** model trained on the **PASCAL VOC** dataset.

Some detectable classes include:

- Person
- Car
- Bus
- Bicycle
- Dog
- Cat
- Chair
- Bottle
- Bird
- Horse
- Motorbike
- TV Monitor
- Sofa
- Dining Table
- Train
- Aeroplane
- Boat

---

## 📸 Example Output

The output image contains:

- Bounding boxes around detected objects
- Object class labels
- Confidence percentages

Example:

```
Person: 98%
Car: 95%
Dog: 91%
```

---

## 📈 Applications

- Smart surveillance
- Traffic monitoring
- Autonomous vehicles
- Retail analytics
- Robotics
- Security systems

---

## 📚 Future Improvements

- Real-time webcam detection
- YOLOv8 integration
- Video object detection
- Custom dataset training
- Object tracking
- GPU optimization

---

## 👨‍💻 Author

**Ubaid Akbar**

Computer Science Undergraduate

University of Engineering & Technology (UET) Mardan

---

## 📄 License
DecodeLabs intern project 4
This project is intended for educational and academic purposes.