# Changelog

All notable changes to the ChinaComx Lianhuanhua Publication Dataset will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
*(This section is a placeholder to track upcoming fixes, data additions, or script updates that have been committed to GitHub but not yet published as a formal Zenodo release.)*

---

## [1.0.0] - 2026-09-01

### Added
* **Initial Dataset Release:** Consolidated *lianhuanhua* publication database comprising over 38,000 entries.
* **Raw Data:** Included original, unparsed OCR extractions from catalogues Ref002 and Ref004 in `/data/raw/`.
* **Processed Data:** Cleaned, structured datasets with disambiguated titles, parsed authors, and standardized publisher locations in `/data/processed/`.
* **Processing Scripts:** Python/Jupyter notebooks for layout analysis, OCR extraction, title splitting, and Transformer-based Named Entity Recognition (NER) author parsing in `/scripts/`.
* **Repository Infrastructure:** Standardized `README.md`, `CITATION.cff`, `LICENSE.md` (CC BY-NC-SA 4.0), and customized GitHub Issue templates.