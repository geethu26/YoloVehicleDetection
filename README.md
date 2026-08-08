# Autonomous Vehicle Detection

End-to-end YOLOv8 object detection for autonomous-vehicle scenes.

This repository walks through the full pipeline: exploring annotations, converting labels into YOLO format, training a detector, evaluating checkpoints, and running inference on unseen images.

## At a Glance

| Item          | Details                                                      |
| ------------- | ------------------------------------------------------------ |
| Model         | YOLOv8n                                                      |
| Classes       | 11 vehicle and road-user categories                          |
| Main workflow | Data exploration, YOLO prep, training, evaluation, inference |
| Key artifacts | `models/best.pt`, `models/last.pt`, `results/` plots         |

## Project Overview

The notebooks in this repo follow a full end-to-end pipeline:

1. Explore the source annotations and image availability.
2. Filter and convert the labels into YOLO-ready training data.
3. Train a YOLOv8n detector.
4. Evaluate the trained model using validation metrics and plots.
5. Run single-image and batch inference with the best checkpoint.

The annotated dataset used by the notebooks contains 11 classes:

- articulated_truck
- bicycle
- bus
- car
- motorcycle
- motorized_vehicle
- non-motorized_vehicle
- pedestrian
- pickup_truck
- single_unit_truck
- work_van

## Repository Structure

```text
Data/
	class_mapping.json
	data.yaml
	labels.csv
	labels_filtered.csv
	yolo_dataset.zip
models/
	best.pt
	last.pt
notebooks/
	01_Data_Exploration.ipynb
	02_YoloData_prep.ipynb
	03_Model_Training.ipynb
	04_Model_Evaluation.ipynb
	05_Model_inference.ipynb
results/
	BoxF1_curve.png
	BoxP_curve.png
	BoxPR_curve.png
	BoxR_curve.png
	confusion_matrix.png
	confusion_matrix_normalized.png
	predictions/
	results.png
```

## Data Notes

- `Data/labels.csv` contains the original annotation table.
- `Data/labels_filtered.csv` contains the cleaned subset used for YOLO conversion.
- `Data/class_mapping.json` stores the class-to-index mapping.
- `Data/data.yaml` is the YOLO dataset configuration.
- `Data/yolo_dataset.zip` is the packaged YOLO dataset used for training.

From the notebook outputs, the prepared dataset split is:

- Train: 4,500 images
- Validation: 563 images
- Test: 563 images

The notebooks also report 17,967 annotation rows across 5,626 unique images before YOLO conversion.

## Setup

Create a virtual environment and install the project dependencies:

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

If you are using a GPU, install a CUDA-enabled build of PyTorch that matches your system. The notebooks were originally run in Colab with GPU support, so update paths if you are running locally.

## Recommended Workflow

Run the notebooks in order:

1. `notebooks/01_Data_Exploration.ipynb` - inspect the raw labels and image set.
2. `notebooks/02_YoloData_prep.ipynb` - convert the annotations into YOLO format and create the class mapping.
3. `notebooks/03_Model_Training.ipynb` - train YOLOv8n and save the checkpoints.
4. `notebooks/04_Model_Evaluation.ipynb` - review metrics, confusion matrices, and precision-recall plots.
5. `notebooks/05_Model_inference.ipynb` - run single-image and batch inference using `models/best.pt`.

## Training

The training notebook uses Ultralytics YOLOv8n. During training it verifies the dataset layout, updates the `data.yaml` path, and saves the model weights to `models/best.pt` and `models/last.pt`.

## Evaluation

The evaluation notebook focuses on:

- overall model metrics
- training history
- precision-recall curves
- confusion matrices
- validation predictions
- model strengths and limitations

Evaluation outputs are stored in `results/`.

## Inference

The inference notebook loads `models/best.pt` and runs predictions on:

- a single test image
- a batch of test images

Predicted boxes and labels can be visualized directly in the notebook and saved to disk.

## Dependencies

The project uses Python, PyTorch, Ultralytics YOLO, and the standard data-science stack listed in `requirements.txt`.

## Notes

- The notebooks contain absolute Colab paths in a few places. If you run locally, update those paths to match your workspace.
- Large generated artifacts such as YOLO export folders, notebook run outputs, and derived prediction results are excluded by `.gitignore`.
