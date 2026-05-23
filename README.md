# Tamil Nadu 2021 Assembly Elections: Booth-Wise Dataset

[![License: CC0-1.0](https://img.shields.io/badge/License-CC0%201.0-lightgrey.svg)](http://creativecommons.org/publicdomain/zero/1.0/)
[![Python](https://img.shields.io/badge/python-3.8%20%7C%203.9%20%7C%203.10-blue)](https://www.python.org/)
[![Data Size](https://img.shields.io/badge/dataset-308%20MB-success)](https://github.com/heisenricher/tamil-nadu-2021-elections-booth-wise)

A complete, granular dataset containing **booth-wise (polling station-level) Form 20 vote counts** and **polling station location/boundary directories** for all **234 Assembly Constituencies** in the **2021 Tamil Nadu Legislative Assembly Election** (held on 6 April 2021).

---

## 📁 Repository Structure

The repository is organized constituency-by-constituency. Inside each folder (from `001-Gummidipoondi` to `234-Killiyoor`), you will find the following files:

```text
boothwise_dataset/
├── 001-Gummidipoondi/
│   ├── 001-Gummidipoondi_Form_20.pdf                # Official signed ECI booth votes
│   ├── 001-Gummidipoondi_Form_20.csv                # Tabular, cleaned booth votes (216 constituencies)
│   ├── 001-Gummidipoondi_Poll_Station_Details.pdf   # Official polling station directory
│   └── 001-Gummidipoondi_Poll_Station_Details.csv   # Tabular polling station buildings/wards
├── 002-Ponneri (SC)/
│   └── ...
```

### File Details:
1. **`*_Form_20.pdf`**: The official final result sheet showing votes recorded at each polling station, signed by the returning officer.
2. **`*_Form_20.csv`**: A clean, parsed tabular version of the Form 20 PDF, mapping individual candidate votes by polling station/booth.
3. **`*_Poll_Station_Details.pdf`**: The official ECI directory outlining the physical building details and geographic voter boundaries assigned to each booth.
4. **`*_Poll_Station_Details.csv`**: Parsed directory listing physical booth buildings and voter zones (streets, villages, wards).

---

## ⚠️ Special Note on 18 Constituencies (Image-Scanned Records)

For **18 constituencies**, the official ECI Form 20 records were only published as **image-scanned PDFs** (scans of paper printouts). Due to paper tilts, handwritten markings, and physical scanning distortions, structured candidate-level tabular `.csv` outputs are not included for these folders. 

These 18 folders contain the `Form_20.pdf`, `Poll_Station_Details.pdf`, and `Poll_Station_Details.csv`, but do not have a parsed `Form_20.csv` file.

The 18 constituencies with image-scanned Form 20 records are:
1. **`027-Shozhinganallur`**
2. **`030-Pallavaram`**
3. **`031-Tambaram`**
4. **`032-Chengalpattu`**
5. **`033-Thiruporur`**
6. **`034-Cheyyur (SC)`**
7. **`035-Madurantakam (SC)`**
8. **`040-Katpadi`**
9. **`043-Vellore`**
10. **`044-Anaikattu`**
11. **`046-Gudiyatham (SC)`**
12. **`047-Vaniyambadi`**
13. **`049-Jolarpet`**
14. **`050-Tiruppattur`**
15. **`108-Udhagamandalam`**
16. **`109-Gudalur (SC)`**
17. **`147-Perambalur (SC)`**
18. **`148-Kunnam`**

---

## ⚙️ Data Cleaning & Processing Methodology

The raw data was programmatically extracted and cleaned from official ECI and CEO Tamil Nadu PDF publications:
* **Mirrored Text Correction**: The original ECI PDF exports suffered from severe encoding issues where strings in candidate headers were mirrored (e.g. "lareneG" instead of "General", "limaT" instead of "Tamil", "udaN" instead of "Nadu"). An automated word-reversal algorithm was applied to reconstruct correct headers and candidate names.
* **Audit & Validation**: The extracted votes for every single booth were summed and verified against the official ECI candidate total votes, guaranteeing 100% numerical accuracy.
* **Polling Station Parsing**: Polling building descriptions and voter assignment boundaries (streets, villages, wards) were parsed from PDF directories and structured into clean, tabular CSV files.

---

## 🐍 Python Usage Example

Here is a quick snippet to load a constituency's booth-wise votes and plot the top 5 candidates using Python:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load a sample constituency
const_name = "001-Gummidipoondi"
df = pd.read_csv(f"{const_name}/{const_name}_Form_20.csv")

# Identify candidate columns (exclude metadata/totals)
exclude_cols = ['Sl. No', 'Polling Station No', 'Total Valid Votes', 'Rejected Votes', 'Total Votes']
candidate_cols = [col for col in df.columns if col not in exclude_cols]

# Aggregate votes
total_votes = df[candidate_cols].sum().sort_values(ascending=False)

# Plot top 5 candidates
sns.set_theme(style="whitegrid")
total_votes.head(5).plot(kind='bar', color='skyblue')
plt.title(f"Top 5 Candidates in {const_name} (2021 Elections)")
plt.ylabel("Total Votes Secured")
plt.xticks(rotation=45, ha='right')
plt.tight_layout()
plt.show()
```

---

## 📜 Sources & License

* **Data Sources**: Chief Electoral Officer of Tamil Nadu (CEO TN) and the Election Commission of India (ECI).
* **License**: Released under the [Creative Commons Zero v1.0 Universal](LICENSE) (CC0-1.0) Public Domain Dedication. You are free to copy, modify, and distribute this data for commercial or non-commercial purposes without asking permission.
