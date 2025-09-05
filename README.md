# How to run USC MGC pipelines and scripts

## Quick Links
- [Pipeline tutorials](/pipeline_tutorials/README.md)
- [Other tutorials](#other-tutorials)

## Description

This guide provides step-by-step instructions for running the Molecular Genomics Core (MGC) pipelines and scripts on USC's HPC system. All of those files can be found here:

```
/project2/weisenbe_1344/scripts
```

These pipelines are designed to process a variety of next-generation sequencing datasets, including single-cell RNA-seq (GEX), immune repertoire sequencing (VDJ), chromatin accessibility (ATAC-seq), multiome experiments, spatial transcriptomics (Visium / HD), and bulk RNA-seq.

The tutorials are intended for both new and experienced users, serving as a practical step-by-step guide for running the pipelines on the HPC system, as well as a useful refresher for those familiar with the workflows.


## Getting Started

### Requirements
1. Verify that you have access to USC's high-performance computing (HPC) system 
2. Check if you have access to the MGC project2 folder: `/project2/weisenbe_1344`

### Accessing the HPC
The tutorials are a step-by-step on how to run these scripts via the terminal. There are two primary ways to access the HPC from your machine via the terminal.

1. **CARC On Demand** (Web-based)  
   
   Log-in to CARC OnDemand https://ondemand.carc.usc.edu/
   
   Navigate to `Home Directory > Open in Terminal`. 
   
   This opens a browser-based terminal that starts in your HPC home directory.
   
   For more info, see CARC OnDemand overview: https://www.carc.usc.edu/user-guides/carc-ondemand/ondemand-overview

2. **SSH** (Local terminal)  
   
   Recommended if you’re comfortable with the command line and prefer working outside of the browser.

   Connect via: `ssh [USERNAME]@discovery.usc.edu`


## Pipeline Tutorials

Choose the correct pipeline based on the type of data you are analysing.

Here are all the pipelines currently being run at USC Molecular Genomics Core:

| Pipeline | Data Type | Recommended Use Case | Tutorial Link |
| ------------ | ------------ | ------------ | ------------ |
| **cellranger** | GEX only     | Single-cell RNA-seq (gene expression) | [cellranger](./pipeline_tutorials/cellranger.md) |
| **cellranger-vdj** | VDJ only | Single-cell immune repertoire analysis | [cellranger-vdj] |
| **cellranger-atac** | ATAC only | Single-cell chromatin accessibility (ATAC-seq) | [cellranger-atac] |
| **cellranger-multi** | GEX + VDJ | Multiome single-cell RNA + VDJ analysis | [cellranger-multi] |
| **cellranger-arc** | GEX + ATAC | Multiome single-cell RNA + ATAC analysis | [cellranger-arc]|
| **spaceranger** | Visium / HD | Spatial transcriptomics | [spaceranger] |
| **rna-seq** | Bulk RNA-seq | Bulk RNA sequencing analysis | [rna-seq] |

## Other Tutorials
These cover miscellaneous commands or standalone scripts used in conjunction with the pipelines. Organized by function.

### Transferring files
| Script | Program/Location? | Tutorial Link |
| ------------ | ------------ | ------------ |
| **basespace** | Illumina Basespace| [basespace] |
| **aws-cli** | Amazon Web Services (AWS) | [aws-script] |
| **scp-transfer** | Local machine (scp)| [scp-transfer](/other_tutorials/file_transfers.md) |
| **10x-cli** | 10X Cloud Analysis | [10x-cli]() |

### Miscellaneous Notes
| Script | Contents | Link |
| ------------ | ------------ | ------------ |
| **mgc-locations** | Commonly used locations | [mgc-locations](/other_tutorials/mgc-locations.md) |
| **hpc-notes** | Commonly used commands | [hpc-notes](/other_tutorials/hpc-linux-notes.md) |



## HPC Scripts Directory
The path to the main scripts directory:
```
/project2/weisenbe_1344/scripts
```

## Authors

Daphne Wong (daphnew@usc.edu)


## Acknowledgments

Special thanks to:
- Bigy, for writing and testing most of these pipelines for the core
- Stephanie, for supporting me and overseing my work at the core
- Matt, for encouraging me to write and upload these tutorials to Github