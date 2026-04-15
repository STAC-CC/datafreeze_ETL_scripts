# Data Freeze ETL Scripts

Transformation scripts for standardizing data freeze submission files into the format required by the submission guide. All scripts are built in Python using Jupyter notebooks.

---

## Overview

When a new data freeze is downloaded, the raw files need to be cleaned and reformatted before they can be submitted. These two notebooks handle that process — one for the data dictionary files, one for the UDS visit data. Both are designed to work with however many files are in the folder without needing to know exact filenames, since downloaded files always come with a date stamp appended to the name.

---

## Notebooks

### `data_dictionary.ipynb`

Prepares the data dictionary CSV files for submission.

Downloaded data dictionary files contain more columns than the submission guide asks for, and the column names don't match what the guide expects. This notebook strips each file down to the required columns, renames them, and adds a blank `units` column in the correct position.

**What it does step by step:**
1. Loads all CSVs from `input/dd_downloads/`
2. Drops all columns except `packet`, `var_name`, `question`, `conformity`, `response_labels`, and `data_type`
3. Handles a known inconsistency where a small number of forms use `form_name` instead of `packet` — those are normalized automatically
4. Renames columns to match the submission schema:

   | Source column | Submission column |
   |---|---|
   | `var_name` | `variable_name` |
   | `question` | `description` |
   | `conformity` | `key_description` |

5. Inserts a blank `units` column between `description` and `key_description`
6. Exports clean files to `output/data_dictionary/` with the date stamp stripped from the filename

---

### `data_freeze_filter.ipynb`

Filters UDS visit data files down to a specified date range.

Downloaded UDS data files contain all visits on record. This notebook keeps only the rows that fall within the date window you set, dropping anything outside that range. Both the start and end dates are inclusive.

**What it does step by step:**
1. Loads all CSVs from `input/fw_downloads/`
2. Parses the `visitdate` column in each file
3. Drops any row where the visit date falls before `START_DATE` or after `END_DATE`
4. Flags any rows with unreadable or missing dates so nothing disappears silently
5. Exports the filtered files to `output/uds_data/` with the date stamp stripped from the filename

---

## Folder structure

```
datafreeze_ETL_scripts/
├── data_freeze_SOP/            <- SOP documentation
├── input/
│   ├── dd_downloads/           <- place data dictionary CSV files here
│   └── fw_downloads/           <- place UDS data CSV files here
├── output/
│   ├── data_dictionary/        <- output from data_dictionary.ipynb
│   └── uds_data/               <- output from data_freeze_filter.ipynb
├── notebooks/
│   ├── data_dictionary.ipynb
│   └── data_freeze_filter.ipynb
├── .gitignore
└── README.md
```

> See each folder's `sample.csv` for the expected input and output column structure.

---

## Usage

Open either notebook in Jupyter or VS Code and run all cells top to bottom. The only cell you need to edit before running is the **Configuration** cell near the top.

Both notebooks support two run modes:

| Mode | Description |
|---|---|
| `"folder"` | Process every CSV in the input folder at once — the most common use case |
| `"standalone"` | Process a single file — useful for a quick check or one-off submission |

For `data_freeze_filter.ipynb`, also set `START_DATE` and `END_DATE` in the Configuration cell before running.

---

## Requirements

```
pandas
```
