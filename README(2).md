# Data

The dataset is not stored in this repository. Download it from Kaggle:

**MHEALTH (Mobile Health)**: https://www.kaggle.com/datasets/gaurav2022/mobile-health/data

## Setup

1. Sign in to Kaggle and download the dataset.
2. Unzip it and place `mhealth_raw_data.csv` in this folder, so the path is:

```
data/mhealth_raw_data.csv
```

Files in this folder ending in `.csv` are ignored by git (see `.gitignore`), so the data will not be committed by accident.

## Columns

- Sensor readings: acceleration and gyroscope values (x, y, z axes) from body-worn sensors
- `Activity`: integer label 0-12 (see the activity table in the main README)
- `subject`: ID of the volunteer (`subject1` to `subject10`)
