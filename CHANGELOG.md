# Changelog

All notable changes to the ChinaComx Lianhuanhua Publication Dataset will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
*(This section is a placeholder to track upcoming fixes, data additions, or script updates that have been committed to GitHub but not yet published as a formal Zenodo release.)*

---

## [1.1.1] - 2026-11-13

### Changed
- **Updated Final CSV Versions:** Using an updated bidirectional substring matching (`BIDIRECTIONAL_PUBLISHER = True`) for `Publisher_1` across `Ref002` and `Ref004` we updated the dataset totals to **1,557** matched `Ref002` entries (**1,239** unmatched) and **36,605** unique *lianhuanhua* publications (`LHH000001`–`LHH036605`). We then replaced the processed dataset files in `/data/processed/` with `2026_11_13_CombinedData.csv` (38,162 rows) and `2026_11_13_UniqueData.csv` (36,605 rows).

---

## [1.1.0] - 2026-11-12

### Added
- **Cross-Catalogue Deduplication & Dual CSV Release:** Implemented automated record linkage between `Ref002` and `Ref004`, identifying 1,556 overlapping entries across the two source catalogues. `/data/processed/` now provides two master files:
  - `2026_11_12_CombinedData.csv` (all 38,162 catalogue entries linked by `Unique_LHH_ID`).
  - `2026_11_12_UniqueData.csv` (36,606 deduplicated unique *lianhuanhua* entries).
- **Two-Tiered Identifier Schema (Columns 47 & 48):** Added `Unique_Ref_ID` (`R002_XXXXX` / `R004_XXXXX`) for sequential source-catalogue tracking and `Unique_LHH_ID` (`LHHXXXXXX`) for top-level work grouping across catalogues.
- **Deduplication Script:** Added `/scripts/Duplicates.ipynb` documenting the four-key bibliographic matching heuristic (`Title_Full`, `Year`, `Publication_Month`, `Format`) and substring-based publisher matching rule.

### Changed
- **Column Header Normalization:** Standardized the retail price column header from `Price[元]` to `Price_Yuan` to prevent syntax conflicts in statistical and visualization software.
- **Documentation (`README.md` & `CITATION.cff`):** Expanded the data dictionary to 48 columns, clarified the distinction between OCR sequence parsing and downstream data transformation, documented all `OCR_Process` tools (`ABBYY_FineReader_OCR`, `PaddlePaddle`, and `Gemini 3 Flash`), updated the citation policy, and finalized Zenodo DOI metadata.

### Fixed
- **Category Label Typo (`Ref002`):** Corrected the category label `'古曲题材'` to `'古典题材'` in `Category/Chapter` for rows processed with `Gemini 3 Flash`.
- **Provenance & Comment Alignment (`Ref004`):** Corrected 4 entries (`R004_17236`, `R004_19281`, `R004_25308`, and `R004_29395`) where editorial annotations had shifted into the `Source` column, restoring their exact `Ref004` page citations (`Ref004:443`, `Ref004:501`, `Ref004:669`, and `Ref004:782`) and consolidating the notes under `Editorial_Comments`.
- **Repository File Naming:** Corrected the license filename typo from `LICENCESE.md` to `LICENSE.md`.

---

## [1.0.0] - 2026-09-01

### Added
- **Initial Dataset Release:** Consolidated *lianhuanhua* publication database comprising over 38,000 entries.
- **Raw Data:** Included original, unparsed OCR extractions from catalogues Ref002 and Ref004 in `/data/raw/`.
- **Processed Data:** Cleaned, structured datasets with disambiguated titles, parsed authors, and standardized publisher locations in `/data/processed/`.
- **Processing Scripts:** Python/Jupyter notebooks for layout analysis, OCR extraction, title splitting, and Transformer-based Named Entity Recognition (NER) author parsing in `/scripts/`.
- **Repository Infrastructure:** Standardized `README.md`, `CITATION.cff`, `LICENSE.md` (CC BY-NC-SA 4.0), and customized GitHub Issue templates.
