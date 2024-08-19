YOLOv8 Object Detection Project
This project demonstrates how to build an object detection system using YOLOv8. It covers the entire pipeline from environment setup, dataset labeling with X-AnyLabeling, converting the labels to YOLO format, and finally training the model.
Table of Contents


Prerequisites
Before starting, make sure you have the following installed:

Python 3.8+
pip
Git
PyTorch
OpenCV
Ultralytics YOLOv8
X-AnyLabeling

Data Preparation
1. Label the Data using X-AnyLabeling
X-AnyLabeling is a versatile labeling tool. Follow these steps to label your images:

Install X-AnyLabeling:

2. Convert Annotations to YOLO Format
Once you have labeled the data, you need to convert the COCO JSON annotations to the YOLO TXT format.

Model Training
Once your dataset is ready, you can start training your YOLOv8 model.

Create a data.yaml

Evaluation
After training, you can evaluate your model's performance:
yolo task=detect mode=val model=path/to/best.pt data=data.yaml


Below is a complete README.md template that covers the process from setting up the project to labeling the data with X-AnyLabeling, converting annotations to YOLO format, and training the model.

YOLOv8 Object Detection Project
This project demonstrates how to build an object detection system using YOLOv8. It covers the entire pipeline from environment setup, dataset labeling with X-AnyLabeling, converting the labels to YOLO format, and finally training the model.

Table of Contents
Prerequisites
Project Setup
Data Preparation
1. Label the Data using X-AnyLabeling
2. Convert Annotations to YOLO Format
Model Training
Evaluation
Inference
References
Prerequisites
Before starting, make sure you have the following installed:

Python 3.8+
pip
Git
PyTorch
OpenCV
Ultralytics YOLOv8
X-AnyLabeling
You can install the required Python packages using:

bash
Copy code
pip install ultralytics opencv-python x-anylabeling
Project Setup
Clone this repository:
bash
Copy code
git clone <your-repository-url>
cd <your-repository-folder>
Create and activate a Python virtual environment:
bash
Copy code
python -m venv venv
source venv/bin/activate  # For Windows: venv\Scripts\activate
Install the dependencies:
bash
Copy code
pip install -r requirements.txt
Data Preparation
1. Label the Data using X-AnyLabeling
X-AnyLabeling is a versatile labeling tool. Follow these steps to label your images:

Install X-AnyLabeling:
bash
Copy code
pip install x-anylabeling
Launch the labeling tool:
bash
Copy code
xanylabeling
In the tool, create a new project and start labeling your images. Ensure that you use the class names defined in your YOLOv8 project.

After labeling, export the annotations in COCO JSON format.

2. Convert Annotations to YOLO Format
Once you have labeled the data, you need to convert the COCO JSON annotations to the YOLO TXT format.

Use the following script to perform the conversion:

python
Copy code
import json
import os

def convert_coco_to_yolo(json_file, output_dir):
    with open(json_file, 'r') as f:
        data = json.load(f)

    categories = {cat['id']: cat['name'] for cat in data['categories']}

    for annotation in data['annotations']:
        image_id = annotation['image_id']
        image_file = f"{image_id}.jpg"  # Adjust based on your file extension
        image_path = os.path.join(output_dir, image_file)

        category_id = annotation['category_id']
        category_name = categories[category_id]
        bbox = annotation['bbox']  # [x, y, width, height]

        # Convert COCO bbox to YOLO format
        x_center = bbox[0] + bbox[2] / 2
        y_center = bbox[1] + bbox[3] / 2
        width = bbox[2]
        height = bbox[3]

        # Normalize values (YOLO format expects values between 0 and 1)
        img_width, img_height = 640, 480  # Replace with your actual image dimensions
        x_center /= img_width
        y_center /= img_height
        width /= img_width
        height /= img_height

        # Write YOLO format to file
        with open(os.path.join(output_dir, f"{image_id}.txt"), 'a') as txt_file:
            txt_file.write(f"{category_id} {x_center} {y_center} {width} {height}\n")

convert_coco_to_yolo('path_to_your_coco_annotations.json', 'output_directory')
Run the script to generate YOLO-compatible .txt annotation files.

Model Training
Once your dataset is ready, you can start training your YOLOv8 model.

Create a data.yaml file with the following structure:
yaml
Copy code
train: /path/to/train/images
val: /path/to/validation/images

nc: 10  # Number of classes
names: [ 'class1', 'class2', ..., 'classN' ]
Start training:
bash
Copy code
yolo task=detect mode=train model=yolov8n.pt data=data.yaml epochs=50 imgsz=640
Replace yolov8n.pt with your preferred YOLOv8 model variant (e.g., yolov8s.pt, yolov8m.pt).

Evaluation
After training, you can evaluate your model's performance:

bash
Copy code
yolo task=detect mode=val model=path/to/best.pt data=data.yaml
Inference
Use the trained model to run inference on new images:

bash
Copy code
yolo task=detect mode=predict model=path/to/best.pt source=/path/to/your/images

References
Ultralytics YOLOv8 Documentation
X-AnyLabeling Documentation
