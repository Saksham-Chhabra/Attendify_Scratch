# EDA Preprocessing Report

## Scope

This report summarizes exploratory data analysis (EDA) for the face-recognition preprocessing stage using:

- source metadata: `2_Train_FaceRecognition_with_ML/data/Dataset.csv`
- cropped images: `2_Train_FaceRecognition_with_ML/data/Faces/Faces`
- failure log: `2_Train_FaceRecognition_with_ML/data/crop_failures.csv`
- generated outputs: `2_Train_FaceRecognition_with_ML/data/eda_outputs`

## Dataset Integrity

- Total records in dataset: 2562
- Unique labels (classes): 31
- Duplicate `id` rows: 0
- Duplicate (`id`, `label`) rows: 0
- Missing values in required columns (`id`, `label`): 0
- Source images present: 2562/2562
- Cropped images present: 2475/2562
- Missing cropped images: 87

## Crop Consistency and Geometry

### Cropped images

- Count: 2475
- Width: mean 160.0, std 0.0, min 160, max 160
- Height: mean 160.0, std 0.0, min 160, max 160
- Aspect ratio: mean 1.0, std 0.0
- Pixel count: mean 25600, std 0

Result: the preprocessing output is perfectly fixed-size at 160x160.

### Original images

- Count: 2562
- Width: mean 1099.38, median 967, range 216 to 5760
- Height: mean 1125.44, median 891, range 200 to 5568
- Aspect ratio: mean 1.11, median 0.94, range 0.46 to 2.66
- Pixel count: mean 1,655,322, median 770,175

Result: original images are highly variable in size and shape, validating the need for normalization through cropping/resizing.

## Class Distribution

- Total classes: 31
- Min samples/class: 30
- Q1: 70.5
- Median: 80.0
- Q3: 101.5
- Max samples/class: 120

Top classes by count:

1. Brad Pitt: 120
2. Vijay Deverakonda: 115
3. Robert Downey Jr: 113
4. Hugh Jackman: 112
5. Jessica Alba: 108

Lowest classes by count:

1. Kashyap: 30
2. Marmik: 32
3. Virat Kohli: 49
4. Akshay Kumar: 50

Result: class imbalance exists but is moderate; a few classes are notably underrepresented.

## Failure Analysis

- Total failed crops: 87
- Overall failure rate: 3.40%
- Failure reasons:
  - no_face_detected: 87

Highest failure rates by class:

1. Virat Kohli: 18.37% (9/49)
2. Marmik: 15.62% (5/32)
3. Billie Eilish: 13.27% (13/98)
4. Roger Federer: 12.99% (10/77)
5. Hrithik Roshan: 10.89% (11/101)

Result: all failures are face-detection misses, likely due to pose, occlusion, low contrast, or profile views not handled by current detector settings.

## Generated Artifacts

CSV exports:

- `2_Train_FaceRecognition_with_ML/data/eda_outputs/size_stats.csv`
- `2_Train_FaceRecognition_with_ML/data/eda_outputs/label_distribution.csv`
- `2_Train_FaceRecognition_with_ML/data/eda_outputs/failure_summary.csv`

Plot exports:

- `2_Train_FaceRecognition_with_ML/data/eda_outputs/width_distribution.png`
- `2_Train_FaceRecognition_with_ML/data/eda_outputs/top_label_distribution.png`
- `2_Train_FaceRecognition_with_ML/data/eda_outputs/failure_reasons.png`

## Recommendations

1. Keep 160x160 output size for model training consistency.
2. Improve recall on failed images by testing stronger detectors (for example, DNN/MTCNN/RetinaFace) or detector parameter sweeps.
3. Address low-sample classes with targeted data collection or balanced sampling/augmentation.
4. Track per-class failure rate during future preprocessing runs to detect regressions early.
5. Preserve failure logs as a hard-negative pool for future detector improvement experiments.

## Conclusion

Preprocessing successfully standardized image geometry for training and produced usable outputs for 2475 images. The main limitation is a 3.40% face-detection miss rate (all `no_face_detected`) concentrated in specific classes, which is the highest-impact improvement area for the next iteration.
