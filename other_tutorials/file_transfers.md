# File Transfer Commands

## Quick Links
[SCP](#scp-overview)  
[Basespace](#basespace-overview)  
[AWS]()

--------------------------------------------------------------------------------

## SCP Overview
Transferring files to and from your local machine requires use of the terminal. 

Use scp (secure copy) to move files between your machine and the HPC.

General command structure:
```
scp /path/to/file /path/to/destination
```

### Upload from Local to Remote

#### 1. Get local source path (where files you want to upload are stored on local machine)
   - Mac
     - Finder: Right click on file/folder > alt/option key > "Copy as Pathname"
     - Terminal: use `pwd` command
   - Windows
       - File Explorer: Copy from address bar


#### 2. Get remote destination path (on HPC)
   - Terminal: use `pwd` command
   - Browser: 'Copy Path' button


#### 3. Upload file/folder
   - Add the `-r` (recursive) flag if you are uploading an entire folder
   - Using hpc-transfer1.usc.edu allows you to transfer large files very quickly
     - Requires DUO verification every time
```
# Upload file to HPC
scp /path/to/local/file [USERNAME]@hpc-transfer1.usc.edu:/path/to/remote/destination


# Upload folder to HPC
scp -r /path/to/local/folder [USERNAME]@hpc-transfer1.usc.edu:/path/to/remote/destination
```

### Download from HPC to Local
- The command structure is same as upload, but in reverse, as the path to your files is on the HPC rather than local

#### 1. Get remote source path from HPC (where files you want to download are stored)
   - Terminal: use `pwd` command
   - Browser: 'Copy Path' button

#### 2. Get local destination path (on local machine)
   - Mac
     - Finder: Right click on file/folder > alt/option key > "Copy as Pathname"
     - Terminal: use `pwd` command
   - Windows
       - File Explorer: Copy from address bar

#### 3. Download file/folder
   - Add the `-r` (recursive) flag if you are downloading an entire folder
   - Using hpc-transfer1.usc.edu allows you to transfer large files very quickly
     - Requires DUO verification every time

```
# Download file from HPC
scp [USERNAME]@hpc-transfer1.usc.edu:/path/to/remote/file /path/to/local/destination


# Download folder to HPC
scp -r [USERNAME]@hpc-transfer1.usc.edu:/path/to/remote/folder /path/to/local/destination
```

### Common Issues
- Missing a space between the two paths
- Typo in `[USERNAME]@hpc-transfer1.usc.edu:`
  - Don't forget the **colon** `:` before the remote path

--------------------------------------------------------------------------------

## Basespace Overview

Some sequencing runs will auto-upload FASTQ and BCL files into Illumina Basespace. The Basespace Command-Line Interface (CLI) allows users to download and use those files.

### Requirements
(1) Basespace requires a conda environment 
- If this is your first time running Basespace, run the following commands in the terminal on the HPC
```
mamba create -n basespace-cli
mamba activate basespace-cli
mamba install hcc::basespace-cli
```
(2) Account authentication
- Required to link an associated account to the CLI
- Use `-force` flag if you are switching accounts

```
# If you have never used basespace on your account before
/project/weisenbe_1344/Complete_David_870_Project_Directory_Transfer/Packages/bs auth


# If you have used basespace before, and are switching to a new account
/project/weisenbe_1344/Complete_David_870_Project_Directory_Transfer/Packages/bs auth --force
```

### Step 1. Copy the script
The script can be found in the MGC scripts folder. 
- Change the output path to the correct destination.
```
cp /project2/weisenbe_1344/scripts/basespace/basespace-script.slurm /path/to/output
```

### Step 2. Find the project/run ID 
Running this command in the terminal will list all projects in the associated Basespace account.
```
# For FASTQ files
bs list projects

# FOR BCL files
bs list runs
```

### Step 3. Update the script 
- Replace `${PROJECT_ID}` or `${RUN_ID}` with the correct ID and change the output path
- By default, the script is written to download fastq files, so the BCL commands will be commented out. 
- If downloading BCL files, make sure to check if the script reflects that.
```
# Downloading FASTQ files
bs download project -i ${PROJECT_ID} -o /path/to/output/dir --extension=fastq.gz

# Downloading BCL files
bs download run -i ${RUN_ID} -o /path/to/output/dir
```

### Step 4. Run the script
```
sbatch basespace-script.slurm
```

### Step 5. Extract files from directories
Once downloaded, the FASTQ files will be found in several directories. 
- The following command will extract the files from each directory, and move them to the correct output directory.
- Change the paths to the downloaded directories and the output directory

```
find /path/to/download/dirs -type f -name "*.fastq.gz" -exec mv {} /path/to/output/dir \;
```

### Step 6. Remove empty directories
This command removes ALL empty subdirectories within the provided path;
- Change path to where the now empty downloaded directories are
```
find /path/to/empty/dirs -type d -empty -delete
```

