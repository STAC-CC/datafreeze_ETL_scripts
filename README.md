# Data Freeze ETL Scripts

Transformation scripts for standardizing data freeze submission files into the format required by the submission guide.

---

## What it does

Takes raw data dictionary CSVs downloaded from the data freeze portal and:

1. Strips each file down to the columns required for submission
2. Renames columns to match the submission schema
3. Adds a blank `units` column in the correct position
4. Exports clean, consistently named files ready for submission

---

## Folder structure

```
datafreeze_ETL_scripts/
├── dd_downloads/       ← place downloaded CSV files here
├── output/             ← transformed files are saved here (auto-created)
└── python/
    └── data_dictionary.ipynb
```

> See `dd_downloads/sample.csv` for the expected input format and `output/sample.csv` for the expected output format.

---

## Usage

Open `python/data_dictionary.ipynb` in Jupyter or VS Code and run all cells top to bottom.

Two run modes are available — set `MODE` in the Configuration cell:

| Mode | Description |
|---|---|
| `"folder"` | Process every CSV in `dd_downloads/` at once |
| `"standalone"` | Process a single file — update `SINGLE_FILE` with the filename |

---

## Requirements

```
pandas
```
