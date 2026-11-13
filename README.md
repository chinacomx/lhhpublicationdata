# The ChinaComx Lianhuanhua Publication Dataset
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.22661555.svg)](https://doi.org/10.5281/zenodo.22661555)

This repository contains a digitized and consolidated dataset of twentieth-century *lianhuanhua* (连环画, lit. “linked serial pictures”) publications, generated via OCR, computational parsing, and manual proofreading from two printed bibliographical catalogues:

- Wang Zhongming 王忠明, Bian Qiangsheng 卞强生, and Zhang Zhenhua 张振华, eds. 2001. 中国连环画收藏 [*Collecting Chinese Lianhuanhua*]. Jilin renmin chubanshe. (referenced in the dataset as `Ref002`)
- Zhang Qiming 张奇明 and Wang Yuxing 王玉兴, eds. 2003. 中国连环画目录汇编 1949–1994 [*Collected Catalogue of Chinese Lianhuanhua: 1949–1994*]. Shanghai huabao chubanshe. (referenced in the dataset as `Ref004`)

Provided in comma-separated values (CSV) format, the dataset digitizes over 1,200 printed pages of densely packed tables to document detailed publication information for 38,162 catalogue entries—representing **36,605 unique *lianhuanhua* publications** after cross-catalogue deduplication—published between 1949 and 1994 (as well as a smaller sample of titles from 1931–1949).

As such, the dataset not only systematizes publication data as preserved in these two collector-compiled reference works, but also reveals novel, multilayered connections across tens of thousands of titles, deepening our understanding of what was one of the world’s most widely consumed reading materials throughout the twentieth century.

## Repository Structure
- `/data/raw/`: The raw OCR-processed data extracted from `Ref002` and `Ref004`.
- `/data/processed/`: The cleaned and edited individual datasets for `Ref002` and `Ref004`, alongside the two consolidated master files introduced in v1.1:
  - [`2026_11_13_CombinedData.csv`](data/processed/2026_11_13_CombinedData.csv): The complete combined database retaining all **38,162 catalogue entries** (`35,366` from `Ref004` and `2,796` from `Ref002`), linked across overlapping entries via `Unique_LHH_ID`.
  - [`2026_11_13_UniqueData.csv`](data/processed/2026_11_13_UniqueData.csv): The deduplicated dataset containing the **36,605 unique *lianhuanhua* entries** (`35,366` from `Ref004` and `1,239` unique entries from `Ref002`). For the 1,557 overlapping entries recorded in both catalogues, the `Ref004` entry is retained as the primary record; users wishing to cross-reference `Ref002`-specific collector valuations, print runs, or footnotes across matched pairs can query the shared `Unique_LHH_ID` in `2026_11_13_CombinedData.csv`.
- `/scripts/`: The Python/Jupyter notebooks used for OCR extraction, data refinement and transformation, and cross-catalogue deduplication.

## Data Dictionary
Both [`2026_11_13_CombinedData.csv`](data/processed/2026_11_13_CombinedData.csv) and [`2026_11_13_UniqueData.csv`](data/processed/2026_11_13_UniqueData.csv) contain **48 columns**, adapted from the original columns in `Ref002` and `Ref004` and enriched with standardized metadata and linkage identifiers. Key columns include:

- `Title_Full`: Full title of the publication as extracted.
- `Title_Core` & `Title_Extra`: Standardized core title and separated subtitle or series/volume designation.
- `Author_Full`: The raw, unsplit string containing all author and role information.
- `Author_1` ... `Author_7`: Sequentially split author strings.
- `Author_Core_1` ... `Author_Core_7`: Standardized personal names of the authors.
- `Author_Role_1` ... `Author_Role_7`: The specific bibliographic role of each author (e.g., adaptation 改编, illustration 绘画).
- `Publisher_Full`: Standardized full institutional name of the publishing house(s).
- `Publisher_1` & `Publisher_2`: Individual publisher designations as recorded in the catalogues.
- `Publisher_Location`: Standardized geographic location (city/province) of the publishing house.
- `Publication_Month` & `Year`: Temporal metadata for the publication.
- `First_Edition_First_Print` & `Other_Editions`: Edition and printing history metadata.
- `Format`: The physical dimensions/format of the book (开本, e.g., `60`, `64`).
- `Print_Run`: Total number of copies printed (印数), standardized into full integer values.
- `Price_Yuan`: Historical retail price (in RMB) at the time of publication.
- `Market_Exchange_Price_9品_in_Yuan_in_Year_2001` & `Market_Exchange_Price_10品_in_Yuan_in_Year_2001`: Secondary market valuation (in RMB) for 9-grade and 10-grade condition copies in the year 2001 (sourced from `Ref002`).
- `Page_Count`, `Additional_Info`, `Editorial_Comments`, `Category/Chapter`, & `Footnotes`: Supplementary physical descriptions, catalogue thematic classifications, footnotes, and editorial annotations.
- `Source`: Indicates which printed bibliographical catalogue and page number the entry originated from (e.g., `Ref002:7` or `Ref004:443`).
- `OCR_Data`: The raw, unparsed text string extracted directly by the OCR software, preserved for data provenance verification against the print catalogues.
- `OCR_Process`: Software or model used for text digitization (`ABBYY_FineReader_OCR`, `PaddlePaddle`, or `Gemini 3 Flash`).
- `Unique_Ref_ID`: Entry-level identifier following the original sequence of titles within each source catalogue (`R002_00001`–`R002_02796` and `R004_00001`–`R004_35366`).
- `Unique_LHH_ID`: Top-level work identifier (`LHH000001`–`LHH036606`) grouping duplicate entries of the same *lianhuanhua* publication across catalogues under a single ID.

Please note that not all *lianhuanhua* listed in the dataset contain information across every column. Because the dataset reflects what the two retrospective reference catalogues compiled in 2001 and 2003 documented—and those printed catalogues themselves contain omissions and abbreviations—we adhered strictly to the information printed in `Ref002` and `Ref004` while preserving the original extracted strings in `OCR_Data`.

## Data Processing, Transformation & Scripts
The `/scripts/` directory contains the Python notebooks used to extract, parse, transform, clean, and deduplicate the bibliographical data across a multi-stage workflow:

- **OCR Extraction, Layout Analysis & Initial Parsing:** Combines outputs from `ABBYY FineReader`, GPU-accelerated `PaddleOCR` (using bounding-box regions of interest to parse complex multi-column grid layouts and isolate page footnotes in `Ref002`), and targeted extraction via `Gemini 3 Flash`, paired with LLM-assisted sequence parsing (`Gemini 2.5` / `Gemini 3 Flash`) to separate grouped catalogue strings into initial tabular fields.
- **Title Disambiguation:** Conditionally splits raw title strings into `Title_Core` and `Title_Extra` (capturing volume numbers and subtitles) based on parsed delimiters (e.g., full-width brackets `（` or em-dashes `—`). Unsplit edge cases with complex punctuation are programmatically isolated for manual review.
- **Transformer-Based Author & Role Identification (`Ref004`):** Utilizes GPU-enabled `spaCy` with its Chinese transformer model (`zh_core_web_trf`) to perform Named Entity Recognition (NER). This extracts personal names (`PERSON` entities) from the author strings and segregates residual non-name text to identify specific bibliographic roles (e.g., adaptation 改编, illustration 绘画), which were sorted by frequency and manually reviewed to build a standardized role dictionary.
- **Author Array Splitting:** Cleans bracketed extra information from author columns and transforms multi-author strings (using slashes, spaces, and Chinese enumeration commas `、`) into sequential standardized columns (`Author_1`–`Author_7`, `Author_Core_1`–`Author_Core_7`, and `Author_Role_1`–`Author_Role_7`).
- **Publisher & Location Normalization:** Maps and replaces abbreviated publishing house designations with their full institutional names (`Publisher_Full`) and geographic locations (`Publisher_Location`) using a custom lookup dictionary digitized from the publisher abbreviation table in `Ref004` (Zhang and Wang 2003).
- **Data Refinement & Quality Control:** Combines programmatic anomaly detection—scanning for abnormally short author names, missing roles, or conflicting role assignments—with manual post-processing and verification in LibreOffice Calc.
- **Cross-Catalogue Record Linkage & Deduplication (`Duplicates.ipynb`):** Resolves cross-catalogue overlap between `Ref002` and `Ref004` using a rule-based Python matching engine. Because identical titles published in the same year by the same publisher sometimes exist in distinct physical formats (`Format`, e.g., 60 vs. 64 *kaiben*), entries across the two catalogues are paired only when they share identical normalized values across four strict bibliographic keys (`Title_Full`, `Year`, `Publication_Month`, and `Format`) combined with a bidirectional substring-based publisher rule (`Publisher_1`) to accommodate unexpanded press abbreviations. The script assigns sequential catalogue entry IDs (`Unique_Ref_ID`) and top-level work IDs (`Unique_LHH_ID`), identifying 1,557 overlapping records and generating both the linked combined dataset (38,162 rows) and the deduplicated dataset (36,605 rows).

## Authors
The ChinaComx Lianhuanhua Publication Dataset has been created within the [ERC-ChinaComx Project](https://chinacomx.github.io/):

- Principal Investigator: **Lena Henningsen**
- Concept and Supervision: **Damian Mandzunowski**
- OCR and Implementation: **Tilen Zupan**
- Initial OCR: **Bettina Jin**
- External Technical Advisor: **Matthias Arnold**
  
## Citation & Attribution Policy
This dataset is the product of extensive digitization, computational parsing, and manual scholarly curation. In accordance with academic standards and the terms of our [CC BY-NC-SA 4.0 License](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en), **proper attribution is required whenever this dataset contributes to your research or project.**

Please cite the dataset (and, once published, the accompanying paper) not only when performing quantitative or computational analyses, but also when using it as a **research, discovery, or reference tool**—including to:
- identify, locate, or discover specific *lianhuanhua* titles, editions, or series;
- verify or cross-reference bibliographic metadata (such as authors, roles, print runs, dates, or publishers);
- trace creator networks, publishing output, or thematic trends across the corpus;
- build derivative datasets, visualizations, exhibition catalogues, or digital tools.

When referencing individual records in your publications, we encourage citing their stable work identifier (`Unique_LHH_ID`, e.g., `LHH016846`) alongside the dataset citation:

> Mandzunowski, Damian, Tilen Zupan, Bettina Jin, and Lena Henningsen. 2026. *The ChinaComx Lianhuanhua Publication Dataset* (version 1.1.1). Zenodo. [https://doi.org/10.5281/zenodo.22661555](https://doi.org/10.5281/zenodo.22661555).

## Related Publications
A first methodological overview of the work behind the dataset was presented at the [European Association for Digital Humanities (EADH) Annual Conference 2026](https://eadh2026.confer.uj.edu.pl/) (Jagiellonian University, Kraków) and published in the conference proceedings as:

> Mandzunowski, Damian, and Tilen Zupan. 2026. “How to (Machine) Read 38,000 Chinese Comics? Distilling Publication Metadata from Lianhuanhua Catalogues.” In *European Association for Digital Humanities (EADH) Annual Conference 2026: Book of Abstracts*, edited by Aleksandra Rykowska and Jagiellonian Centre for Digital Humanities, 405–410. Zenodo. [https://doi.org/10.5281/zenodo.22078032](https://doi.org/10.5281/zenodo.22078032).

In addition, a comprehensive data paper is currently (November 2026) under review at the *Journal of Open Humanities Data*.

## Known Issues and Future Updates
The dataset is a work-in-progress; we have spent substantial effort on data cleaning and verification, but are aware of remaining edge cases documented in our repository's Issues tab. Contributions and corrections are welcome!

All future updates and expanded releases of the dataset will be documented in `CHANGELOG.md`. 

## Acknowledgements
This dataset is an academic output of the ERC-ChinaComx project, led by Principal Investigator Lena Henningsen. Damian Mandzunowski conceptualized and supervised its development. Tilen Zupan executed the main OCR extraction, data transformation, and curation, building upon initial digitization groundwork by Bettina Jin. We thank Matthias Arnold and Aijia Zhang for their valuable feedback and brainstorming throughout the work on the dataset.

Primary source materials such as the lianhuanhua publication catalogues were sourced from the [Centre for Asian and Transcultural Studies (CATS) Library, Heidelberg University](https://www.cats.uni-heidelberg.de/library/) as well as from the ERC-ChinaComx collection at the CATS Library; the digitized lianhuanhua collection is part of the [digiCATS Special Collections](https://digiportal.cats.uni-heidelberg.de/), part of the CATS Library.

This work is supported by [ERC grant ChinaComx, Grant agreement ID: 101088049](https://doi.org/10.3030/101088049). Funded by the European Union. Views and opinions expressed are however those of the authors only and do not necessarily reflect those of the European Union or the European Research Council Executive Agency. Neither the European Union nor the granting authority can be held responsible for them.

<table width="100%" style="border: none;">
  <tr>
    <td align="center" width="20%">
      <a href="https://chinacomx.github.io/">
        <img src="https://chinacomx.github.io/assets/images/chinacomx-logo.png" alt="The ChinaComx Project." title="The ChinaComx Project." width="140">
      </a>
    </td>
    <td align="center" width="30%">
      <a href="https://cordis.europa.eu/project/id/101088049">
        <img src="https://chinacomx.github.io/assets/images/erc-logo.png" alt="Funded by the European Union (ERC, Grant ID: 101088049). Views and opinions expressed are however those of the author(s) only and do not necessarily reflect those of the European Union or the European Research Council Executive Agency." title="Funded by the European Union (ERC, Grant ID: 101088049). Views and opinions expressed are however those of the author(s) only and do not necessarily reflect those of the European Union or the European Research Council Executive Agency." width="140">
      </a>
    </td>
    <td align="center" width="30%">
      <a href="https://www.cats.uni-heidelberg.de/">
        <img src="https://chinacomx.github.io/assets/images/cats-logo.png" alt="Located at the Centre for Asian and Transcultural Studies, Institute of Chinese Studies." title="Located at the Centre for Asian and Transcultural Studies, Institute of Chinese Studies." width="140">
      </a>
    </td>
    <td align="center" width="20%">
      <a href="https://www.uni-heidelberg.de/">
        <img src="https://chinacomx.github.io/assets/images/hd-logo.png" alt="Affiliated with Heidelberg University." title="Affiliated with Heidelberg University." width="140">
      </a>
    </td>
  </tr>
</table>

## License
The ChinaComx Lianhuanhua Publication Dataset is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en).

See `LICENSE.md` for details.
