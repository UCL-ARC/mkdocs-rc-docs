---
title: Shared Datasets
layout: docs
---

# Shared Datasets

There are several shared datasets or software spaces maintained by UCL researchers and available for
others to use.

## Myriad

On Myriad, these spaces are available:

### Alphafold2

Location: `/myriadfs/projects/alphafold2`

Contains Alphafold2 datasets.

This one is maintained centrally by rc-support@ucl.ac.uk

### BlastDB

Location: `/myriadfs/projects/blastdb`

Contains weekly updated [NCBI BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) repositories for BLAST 
nucleotide (nt), protein (nr) , core nucl (core_nt), and Diamond. Plus Taxonomizr and fcs (foreign 
contamination scan) downloaded when needed.

There is a README at `/myriadfs/projects/blastdb/README.md`.

If you use these databases, you should record the date you used, as this will correspond to the database 
you have used and when they were downloaded (For reproducibility). 

### Cancer

Location: `/myriadfs/projects/cancer`

Software modules: `module load blic-modules`

This space provides modules for NextFlow and other tools that can be made visible by first loading the
`blic-modules` module and then typing `module avail`. 

Contact ci.bioinfohub@ucl.ac.uk if you have any questions about the modules.


### DTAdb

Location: `/myriadfs/projects/DTAdb`

Contains datasets such as GWAS summary statistics and some core datasets like 1000 genomes and VEP.
The GWAS data were normalised using this pipeline: https://cfinan.gitlab.io/gwas-norm/index.html

