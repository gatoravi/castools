# castools

Processing tools and analysis code for **SARGENT** (Single-cell Analysis of
Reporter Gene Expression Noise and Transcriptome), a method that measures
expression noise of reporter genes integrated across the genome together with
the full transcriptome of every cell.

> Hong CKY, Ramu A, Zhao S, Cohen BA. *Effect of genomic and cellular
> environments on gene expression noise.* **Genome Biology** 25, 137 (2024).
> [doi:10.1186/s13059-024-03277-9](https://doi.org/10.1186/s13059-024-03277-9)
> · [bioRxiv preprint](https://www.biorxiv.org/content/10.1101/2022.08.31.506082v1)
> · data and code archive [doi:10.5281/zenodo.10616403](https://doi.org/10.5281/zenodo.10616403)

![SARGENT](./figures/SARGENT.png)

## The question

Genetically identical cells in the same environment still express genes at
different levels. How much of that noise comes from where a gene sits in the
genome (its chromatin environment), and how much comes from the state of the
cell it is in? SARGENT answers this by reading out thousands of barcoded
reporter integrations and the transcriptome of the same single cells.

## Key findings

- Different histone modifications are associated with a reporter's mean
  expression and with its noise, so mean and noise are controlled
  independently.
- In cells carrying several reporter integrations, intrinsic noise accounts
  for about 54% of total variability. The rest is extrinsic, driven mainly by
  cell-cycle phase and cellular substates.
- Reporter integrations can change the expression of nearby endogenous genes,
  which matters when choosing genomic "safe-harbor" sites for transgenes.

## What's in this repository

| Folder | Contents |
| --- | --- |
| [`castools/`](castools) | Python command-line tools that turn raw SARGENT sequencing reads into per-cell reporter measurements: parsing cell barcodes, UMIs and integration barcodes from FASTQ against the 10x v3 whitelist, barcode error correction by Hamming distance, merging and counting barcode trios, replicate comparison, and clone finding from shared integration barcodes |
| [`analysis-code/`](analysis-code) | Jupyter and R Markdown notebooks for the analyses in the paper: histone-mark and chromatin analysis, cell-cycle and cellular-state effects, clonal inference, and the leave-one-out classifiers that predict high- vs low-noise and high- vs low-mean genomic locations |
| [`figures/`](figures) | R scripts that regenerate the paper figures, one script per figure panel (see [`figures/README.md`](figures/README.md)) |
| [`dat/`](dat) | Processed cell barcode / UMI / integration barcode / read-count tables for the four pools, plus the 10x barcode whitelist and translate table |

## Install

```bash
git clone https://github.com/gatoravi/castools
cd castools
pip install -e .
```

The figure and notebook code uses R packages from CRAN and Bioconductor, and
Jupyter for the Python notebooks.

## Example

Extract white-listed cell barcodes, UMIs and integration barcodes from a
paired-end run:

```bash
python castools/parse_reads.py sample_R1.fastq.gz sample_R2.fastq.gz > trios.txt
```

`parse_reads.py` reads the 10x whitelist from a hard-coded cluster path, so
point it at `dat/10xv3_whitelist.txt.gz` before running it elsewhere. Scripts
that use `argparse` print their options with `--help`.

## Authors

Clarice K. Y. Hong, Avinash Ramu and Siqi Zhao, in
[Barak Cohen's lab](https://bclab.wustl.edu/) at Washington University in
St. Louis.

## Citation

If you use this code, please cite the Genome Biology paper above. Citation
metadata is also in [`CITATION.cff`](CITATION.cff).

## License

MIT. See [LICENSE](LICENSE).
