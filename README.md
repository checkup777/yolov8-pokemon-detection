# Pokémon Detection with YOLOv8

A custom object detector built with YOLOv8 (`yolov8n`) that finds and classifies multiple, sometimes overlapping, Pokémon in an image or video frame. Course project for a Computer Vision class.

*Pokémon and related names are trademarks of their respective owners. This is an educational project and is not affiliated with or endorsed by them.*

## Problem

Detect and classify several overlapping objects per image across 7 classes (bulbasaur, charmander, eevee, jigglypuff, pikachu, squirtle, and `other`), using a small custom dataset.

## Data

- 197 images collected from the internet and annotated in Label Studio (YOLO format).
- The images are third-party artwork, so **the dataset is not included** in this repo. See "Rebuilding the dataset" below.
- 197 images is a small dataset.

## Method

- Pretrained `yolov8n.pt` (downloaded automatically by Ultralytics), fine-tuned for 50 epochs at image size 640, batch size 16.
- Random 80/20 train/validation split (157 / 40 images), done in the notebook. The split is not seeded.
- Precision/recall curves and the confusion matrix were used to look at class-level errors and background misclassification.

## Results

On the **validation split** (40 images):

| Precision | Recall | mAP@0.5 | mAP@0.5:0.95 | F1 |
|---|---|---|---|---|
| 0.93 | 0.86 | 93.8% | 0.76 | 0.89 |

Precision, recall and mAP are from the final epoch (50) in `train results/results.csv`. F1 is the peak of the F1–confidence curve (0.89 at confidence 0.693).

There is **no separate test set**. The validation set was also used to select the best checkpoint, so these numbers are likely optimistic.

Training outputs are in [`train results/`](train%20results/):

![Normalized confusion matrix](train%20results/confusion_matrix_normalized.png)
![F1 curve](train%20results/F1_curve.png)
![Precision-recall curve](train%20results/PR_curve.png)

Also included: raw confusion matrix, precision and recall curves, label distribution plots, and `results.csv` with per-epoch metrics.

## Rebuilding the dataset

1. Collect and annotate your own images (for example in Label Studio) and export them in YOLO format as `dataset/images/`, `dataset/labels/` and `dataset/classes.txt`.
2. Run the notebook's split and `data.yaml` cells to create `dataset/pokemon dataset/` and `dataset/data.yaml`.

## Running it

Developed with Python 3.12. The Ultralytics version was not recorded.

1. Install dependencies: `ultralytics`, `opencv-python`, `matplotlib`, `pyyaml`, `pillow`, `jupyter`.
2. Run `pokedetection.ipynb` from the top. Training writes its output to `runs/detect/train` by default. The later cells expect it in a folder named `train results/`, so copy or rename it there.
3. The inference cells need your own test media (`testvid.mp4`, `test.mp4`, `testimg.jpg`) next to the notebook. The last cell expects the saved prediction to be copied to `testimgresult.jpg`.

Not included: the dataset, trained weights, test videos and images, and prediction outputs.

## Limitations

- Small dataset (197 images, 7 classes); results are indicative only.
- No held-out test set, and the validation set also selected the checkpoint.
- Only 40 validation images, so the per-class curves are noisy.
- The split is random and unseeded, so it can't be reproduced exactly.
- Video and single-image testing in the notebook was qualitative only, with no quantitative evaluation beyond the validation metrics.

## Author

Arqia M, Universitas Gadjah Mada