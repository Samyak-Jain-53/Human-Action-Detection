# Human Action Detection

Human activity recognition from wearable sensor data. This project trains and compares classical machine learning models (Logistic Regression, K-Nearest Neighbors, Decision Tree) to classify what a person is doing, such as walking, jogging, cycling, or lying down, using accelerometer and gyroscope readings.

## Dataset

**MHEALTH (Mobile Health)** dataset from Kaggle: [gaurav2022/mobile-health](https://www.kaggle.com/datasets/gaurav2022/mobile-health/data)

- 10 volunteers performed 12 physical activities while wearing body-worn sensors
- Signals include acceleration, gyroscope, and magnetometer readings from sensors placed on the body
- The file used here is `mhealth_raw_data.csv`, which has one row per sensor reading, an `Activity` label, and a `subject` ID

**Activities (labels 0-12)**

| Label | Activity | Label | Activity |
|-------|----------|-------|----------|
| 0 | None (no activity) | 7 | Frontal elevation of arms (20x) |
| 1 | Standing still (1 min) | 8 | Knees bending / crouching (20x) |
| 2 | Sitting and relaxing (1 min) | 9 | Cycling (1 min) |
| 3 | Lying down (1 min) | 10 | Jogging (1 min) |
| 4 | Walking (1 min) | 11 | Running (1 min) |
| 5 | Climbing stairs (1 min) | 12 | Jump front & back (20x) |
| 6 | Waist bends forward (20x) | | |

The dataset is not included in this repository. See [`data/README.md`](data/README.md) for download instructions.

## Approach

1. **Exploration**: checked data types, missing values, duplicates, and class distribution. Plotted sensor signals and histograms per activity for one subject.
2. **Class balancing**: activity `0` (the "None" class) heavily dominates the data, so it was downsampled to 40,000 rows.
3. **Preprocessing**: label-encoded the target, dropped the `subject` column, and split the data 75% train / 25% test. Features were scaled with `RobustScaler`, which is less sensitive to outliers than standard scaling.
4. **Modeling**: trained Logistic Regression, K-Nearest Neighbors (tuning k from 1 to 10), and a Decision Tree (`max_depth=14`).
5. **Evaluation**: accuracy plus macro-averaged precision, recall, and F1, along with a confusion matrix.

## Results

Metrics on the 25% held-out test set (precision, recall, and F1 are macro-averaged):

| Model | Accuracy | Precision | Recall | F1 |
|-------|----------|-----------|--------|-----|
| Logistic Regression (scaled) | 87.29% | 86.89% | 87.27% | 86.86% |
| Decision Tree (max_depth=14) | 88.65% | 88.14% | 88.55% | 87.92% |
| KNN, k=5 (scaled) | 93.77% | 93.56% | 93.58% | 93.22% |
| **KNN, k=3 (scaled)** | **94.08%** | **93.84%** | **93.94%** | **93.60%** |

KNN on scaled features performed best, with k=3 giving the top accuracy in the k=1 to 10 sweep.

## Limitations

- The train/test split is random at the row level. Neighboring sensor readings from the same recording are very similar, so this likely makes results look better than they would on completely unseen people. A subject-wise split (train on some volunteers, test on others) would be a more realistic evaluation.
- No `random_state` is set, so exact numbers will vary slightly between runs.
- Only classical models were tested. Windowed features or deep learning models (CNN/LSTM) are natural next steps.

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Samyak-Jain-53/human-action-detection.git
cd human-action-detection
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Get the data

Download `mhealth_raw_data.csv` from [Kaggle](https://www.kaggle.com/datasets/gaurav2022/mobile-health/data) and place it in the `data/` folder.

### 4. Run the notebook

```bash
jupyter notebook human_action_detection.ipynb
```

The notebook was originally written in Google Colab and uploads the CSV with `files.upload()`. If you run it locally, remove that cell and read the file from `data/mhealth_raw_data.csv` instead.

## Project Structure

```
human-action-detection/
├── human_action_detection.ipynb   # Full analysis and model training
├── requirements.txt               # Python dependencies
├── data/
│   └── README.md                  # Where to get and place the dataset
├── .gitignore
├── LICENSE
└── README.md
```

## Tech Stack

Python, pandas, NumPy, scikit-learn, Matplotlib, Seaborn

## License

Released under the [MIT License](LICENSE).

## Author

Samyak Jain - [@Samyak-Jain-53](https://github.com/Samyak-Jain-53)
