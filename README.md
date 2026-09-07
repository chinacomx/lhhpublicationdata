# The ChinaComx Lianhuanhua Publication Dataset
[![DOI](https://zenodo.org/badge/DOI/YOUR-PENDING-DOI.svg)](https://doi.org/YOUR-PENDING-DOI)

This repository contains a digitized and consolidated dataset of twentieth-century *lianhuanhua* (连环画) publications, generated via OCR and manual proofreading from two historical catalogues:

* **Ref002:** Wang, Zhongming, Qiangsheng Bian, and Zhenhua Zhang, eds. 2001. *Zhongguo lianhuanhua shoucang* [Collecting Chinese Lianhuanhua]. Jilin renmin chubanshe.
* **Ref004:** Zhang, Qiming, and Yuxing Wang, eds. 2003. *Zhongguo lianhuanhua mulu huibian: 1949–1994* [Collected Catalogue of Chinese Lianhuanhua: 1949–1994]. Shanghai huabao chubanshe.

Provided in a comma-separated values (CSV) format, the dataset digitizes over 1,200 printed pages of densely packed tables to document detailed publication information for over 38,000 unique *lianhuanhua* published between 1949 and 1992.

As such, the dataset not only systematizes publication data as preserved in these two decades-old catalogues, but also reveals novel, multilayered connections across tens of thousands of titles, deepening our understanding of what was one of the world’s most widely consumed reading materials throughout the twentieth century.

## Repository Structure
* `/data/raw/`: The raw OCR-processed data from Ref002 and Ref004.
* `/data/processed/`: The cleaned and edited individual datasets for Ref002 and Ref004, as well as the consolidated combined database (38,000+ entries).
* `/scripts/`: The scripts used for data processing and cleaning.

## Data Dictionary
The combined [`ChinaComx Lianhuanhua Publication Dataset`](data/processed/2026_06_22_CombinedData.csv) contains 46 columns, adapted from the original columns taken from both Ref002 and Ref004. Key columns include:

* `Title_Full`: Full title of the publication as extracted.
* `Title_Core`: Standardized core title (removing extraneous subtitle or series data).
* `Author_Full`: The raw, unparsed string containing all author information.
* `Author_1` ... `Author_7`: Sequentially parsed author names.
* `Author_Core_1` ... `Author_Core_7`: Standardized versions of the author names.
* `Author_Role_1` ... `Author_Role_7`: The specific role of each author (e.g., adaptation 改编, illustration 绘画).
* `Publisher_Full`: Full name of the publishing house.
* `Publication_Month` & `Year`: Temporal metadata for the publication.
* `Format`: The physical dimensions/format of the book (e.g., 开本).
* `Print_Run`: Total number of copies printed (印数).
* `Price[元]`: Historical pricing information at the time of publication.
* `Market_Exchange_Price_9品` & `10品`: Secondary market valuation (in RMB) for 9-grade and 10-grade condition copies in the year 2001 (sourced from Ref002).
* `Source`: Indicates which historical catalogue the entry originated from (Ref002 or Ref004).
* `OCR_Data`: The raw, unparsed text string extracted directly by the OCR software.
* `OCR_Process`: Software used for digitization (e.g., ABBYY_FineReader_OCR, PaddlePaddle).

## Data Processing & Scripts
The `/scripts/` directory contains the Python notebooks created by Tilen Zupan and used to digitize, merge, and clean the historical catalogues. The data pipeline utilizes a mix of deep learning and rule-based parsing to standardize the metadata:

* **Layout Analysis & OCR Extraction (Ref002):** Implements GPU-accelerated OCR via `PaddleOCR`. The scripts define dynamic bounding-box regions of interest (ROIs) to parse complex multi-column grid layouts, group text vertically, and automatically isolate page footnotes.
* **Title Disambiguation:** Conditionally splits raw title strings into `Title_Core` and `Title_Extra` (capturing volume numbers and subtitles) based on parsed delimiters (e.g., full-width brackets `（` and em-dashes `—`). Unsplit edge cases with complex punctuation are programmatically isolated for manual review.
* **Transformer-Based Author Parsing (Ref004):** Utilizes GPU-enabled `spaCy` with a Chinese BERT model (`zh_core_web_trf`) to perform Named Entity Recognition (NER). This extracts valid personal names (`PERSON` entities) from the raw author strings and segregates residual non-name data to identify specific bibliographic roles (e.g., adaptation 改编, illustration 绘画).
* **Author Array Splitting:** Cleans bracketed extra information from author columns and splits multi-author strings (using slashes, spaces, and Chinese enumeration commas `、`) into sequential standardized columns (`Author_1`, `Author_2`, etc.).
* **Publisher & Location Normalization:** Maps and replaces highly abbreviated publishing house designations with their complete institutional names using a custom dictionary lookup, and subsequently merges geographic publication data (`所在地`) into the database. 
* **Data Quality & Conflict Flagging:** Algorithmically scans for structural anomalies—such as abnormally short author names paired with missing roles, or identical authors with conflicting role assignments across entries—flagging these rows (e.g., `Y=1,2`) for targeted manual correction.

## Authors
The ChinaComx Lianhuanhua Publication Dataset has been created by the [ERC-ChinaComx Project](https://chinacomx.github.io/).

**Principal Investigator:** Lena Henningsen  

**Compilers:** Damian Mandzunowski and Tilen Zupan  

**Assistance:** Bettina Jin

**Technical Advisor:** Matthias Arnold

## Citation Format
If you use this dataset, please cite it as:

> ERC-ChinaComx (2026). *The ChinaComx Lianhuanhua Publication Dataset* (1.0.0) [Data set]. Zenodo. https://doi.org/YOUR-PENDING-DOI

## Related Publications
A first overview of the work behind the dataset was presented by the authors at the [European Association for Digital Humanities (EADH) Annual Conference 2026](https://eadh2026.confer.uj.edu.pl/) as:

> Damian Mandzunowski and Tilen Zupan, “How to (Machine) Read 38,000 Chinese Comics? Distilling Publication Metadata from Lianhuanhua Catalogues,” Jagiellonian University Cracow, 16.09.2026. (The short paper is available in the conference *Book of Abstracts*, pp. 405–410, via [Zenodo](https://zenodo.org/records/22078032).)

In addition, a detailed data paper as well as a longer journal article are currently (September 2026) also in the works.

## Known Issues
The dataset is a work-in-progress; we are aware of a range of data gaps, (potential) transcription errors, and a few other elements that still need to be dealt with. For details, please check the [Issues](https://github.com/chinacomx/lhhpublicationdata/issues) tab. Contributions and corrections are welcome!

## Acknowledgements
This work is supported by [ERC grant ChinaComx, Grant agreement ID: 101088049](https://doi.org/10.3030/101088049). Funded by the European Union. Views and opinions expressed are however those of the authors only and do not necessarily reflect those of the European Union or the European Research Council Executive Agency. Neither the European Union nor the granting authority can be held responsible for them. 

Primary source materials such as the lianhuanhua publication catalogues were sourced from the [Centre for Asian and Transcultural Studies (CATS) Library](https://www.cats.uni-heidelberg.de/library/), Heidelberg University, as well as from the ERC-ChinaComx collection at the CATS Library; the digitized lianhuanhua collection is part of the digiCATS Special Collections ([https://digiportal.cats.uni-heidelberg.de/](https://digiportal.cats.uni-heidelberg.de/)), part of the CATS Library.

We thank Bettina Jin for her initial work on the OCR of the data, and Matthias Arnold and Aijia Zhang for their helpful comments on previous versions of the dataset.

## License
The ChinaComx Lianhuanhua Publications Dataset is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en).

See `LICENSE.md` for details.
