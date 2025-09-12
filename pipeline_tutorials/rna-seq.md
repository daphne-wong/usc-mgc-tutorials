# RNA-seq (Nextflow `nf-core/rnaseq`) pipeline

## Overview

>This tutorial is written specifically for the pipeline setup used by USC MGC. These steps may not work if running on a local machine.

The Nextflow `nf-core/rnaseq` pipeline is for bulk RNA-seq . 

To see tutorials for other pipelines, see here: [choosing the correct pipeline](../README.md#pipeline-tutorials).


---

## Step 1. Set up project directory

**1. Set up the project directory within the `RNA-Seq` directory.**

- General naming convention: [PI_last_name]\_[analysis type]_[date]

```
cd /project2/weisenbe_1344/MGC/RNA-Seq

mkdir Wong_20250908
```

**2. Within the project directory, create the following folders:**

```
cd /project2/weisenbe_1344/MGC/RNA-Seq/Wong_20250908

mkdir fastqs run1
```

## Step 2. Download FASTQ files from source

From [Illumina Basespace](../other_tutorials/file_transfers.md#basespace-overview)

From [AWS]()

From [local machine]()


## Step 3. Prepare required files

Each RNA-seq run has two required files: `nextflow_script_run1.slurm` and `samplesheet.csv`.

**1. Copy the files from the scripts folder.**

```
cd /project2/weisenbe_1344/MGC/RNA-Seq/Wong_20250910/run1

# Copy over the scripts
cp /project2/weisenbe_1344/scripts/experiments/RNA-Seq/* .
```

**2. Re-write the samplesheet (pulls sample names from fastq file names).**
- The command separates the file name by underscores and separates them into fields. 
- Change `-f1-4` based on how many fields you want to keep. 
- EXAMPLE:
   - For `222_20101169_S9_L001_R1_002.fastq.gz`, the file name will be separated into:
     - 222
     - 20101169
     - S9
     - L0001
     - R1
     - 002
    - Since the sample name is 222_20101169, the command will be modified to `-f1-2`.
```
# Replace -f1-4 for fastq_name
# Replace fastq path

# Paste this directly in the terminal within the run1 directory

(
  echo "sample,fastq_1,fastq_2,strandedness"
  for fastq_file in /project2/weisenbe_1344/MGC/RNA-Seq/Wong_20250910/fastqs/*.fastq.gz; do
      dir_name_cut=$(dirname "$fastq_file" | cut -d '_' -f1-9)
      fastq_name=$(basename "$fastq_file" | cut -d '_' -f1-4)
      echo "${fastq_name},${dir_name_cut}/${fastq_name}_L001_R1_001.fastq.gz,${dir_name_cut}/${fastq_name}_L001_R2_001.fastq.gz,auto"
  done | awk 'NR%2==1'
) > samplesheet.csv
```

**3. Edit slurm script**
- Add path to samplesheet for `--input`
- Add path to out_dir (usually run1) for `--output`
- Change gtf and fasta references if needed
- _Note_: Verify the slurm script is billed to the correct account (`#SBATCH --account=weisenbe_1344`).

```
nextflow run nf-core/rnaseq \
    --input /project2/weisenbe_1344/MGC/RNA-Seq/Wong_20250910/run1/samplesheet.csv \
    --outdir /project2/weisenbe_1344/MGC/RNA-Seq/Wong_20250910/run1 \
    --gtf /project/weisenbe_1344/Complete_David_870_Project_Directory_Transfer/Resources/cellranger/refdata-gex-GRCh38-2024-A/genes/genes.gtf \
    --fasta /project/weisenbe_1344/Complete_David_870_Project_Directory_Transfer/Resources/cellranger/refdata-gex-GRCh38-2024-A/fasta/genome.fa \
    --igenomes_ignore \
    --genome null \
    --trimmer fastp \
    -profile slurm,apptainer \
    --save_align_intermeds \
    -resume
```

## Step 4. Run the script
**1. Submit the job to SLURM**
- Nextflow will handle the samples for you, no need to use a SLURM array

```
sbatch nextflow_script_run1.slurm
```
**2. Check on the status of your run**
- Run `myqueue` to check on all of your current SLURM jobs (there will be a lot once the run starts, but this is normal -- Nextflow will handle them for you)
  - It may take some time for the job to start (PENDING > STARTED) depending on how busy the queue is and how many resources you are requesting.
- If you want to cancel any of the SLURM jobs, use `scancel ${JOB_ID}`

```
# Cancel all array jobs
scancel 2113160
```

## Nextflow RNA-seq Outputs
- Each QC program will have a dedicated directory within your output folder
- Double check that the QC metrics from multiqc `/multiqc/star_salmon/multiqc_report.html` looks okay.
  - Open the HTML file in the browser, not terminal

- Please make sure the directory permissions are set to 775 so others can access or move the directory if needed.
```
chmod 775 /project2/weisenbe_1344/MGC/RNA-Seq/Wong_20250910
```
