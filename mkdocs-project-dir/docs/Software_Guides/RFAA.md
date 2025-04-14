---
title: RoseTTAFold All-Atom
layout: docs
---

The RoseTTAFold All-Atom (RFAA) developed by Baker Lab is an inference pipeline for protein structure prediction.

The version of it that is current on the 4th of March 2025 is installed on Myriad, available in environment modules as `rfaa/20250304`.

!!! important
    RFAA depends on a piece of software called [SignalP 6.0h](https://services.healthtech.dtu.dk/services/SignalP-6.0/) which is licensed under an academic licence that explicitly forbids commercial use. **You therefore may not under any circumstances use the RFAA pipeline as it is presently installed for commercial workloads.** Where any ambiguity exists about the non-commercial nature of your work you must cease using RFAA immediately and contact [rc-support](mailto:rc-support@ucl.ac.uk) to discuss paths forward. Violation of software licenses is a violation of the UCL Computer terms of use which may result in removal of access to all IT systems.

## Using RFAA

!!! important
    RFAA contains an embedded miniforge3 install and so conflicts with other versions of Python.

When run, RFAA relies on some very large databases (about 2.5TiB) as well as the model weights from the original paper. These, and other files are expected to be in a predictable structure in the input directory which you run the code in. To facilitate this, we have placed those reference sets in a central location and provided a shell script `prepare_rfaa_input_directory` with which will either make appropriate file-system links in the current working directory, or, if given an argument will do so there so:

```
$ module load rfaa/20250304

Please note that the license of SignalP 6.0h which is used
in RFAA ONLY allows its use for non-commercial work.
This means you may only use RFAA for non-commercial work.

$ cd ~/Scratch/
$ mkdir rfaa_input
$ prepare_rfaa_input_directory rfaa_input/
Preparing rfaa_input/ as an RFAA input directory.

Linking databases to rfaa_input/ directory...
/shared/ucl/apps/RoseTTAFold-All-Atom_db/bfd <- rfaa_input//bfd
/shared/ucl/apps/RoseTTAFold-All-Atom_db/pdb100_2021Mar03 <- rfaa_input//pdb100_2021Mar03
/shared/ucl/apps/RoseTTAFold-All-Atom_db/UniRef30_2020_06 <- rfaa_input//UniRef30_2020_06

Linking weights to rfaa_input/ directory...
/shared/ucl/apps/RoseTTAFold-All-Atom_db/RFAA_paper_weights.pt <- rfaa_input//RFAA_paper_weights.pt

Done.

Assuming you have the environment module correctly loaded, you can now run RFAA from inside this directory.

$ cd rfaa_input/
$ ls -lah
total 20K
drwx------  2 uccaoke uccapc3 4.0K Mar 13 16:15 .
drwxr-xr-x 16 uccaoke uccapc3  12K Mar 13 16:15 ..
lrwxrwxrwx  1 uccaoke uccapc3   44 Mar 13 16:15 bfd -> /shared/ucl/apps/RoseTTAFold-All-Atom_db/bfd
lrwxrwxrwx  1 uccaoke uccapc3   57 Mar 13 16:15 pdb100_2021Mar03 -> /shared/ucl/apps/RoseTTAFold-All-Atom_db/pdb100_2021Mar03
lrwxrwxrwx  1 uccaoke uccapc3   62 Mar 13 16:15 RFAA_paper_weights.pt -> /shared/ucl/apps/RoseTTAFold-All-Atom_db/RFAA_paper_weights.pt
lrwxrwxrwx  1 uccaoke uccapc3   57 Mar 13 16:15 UniRef30_2020_06 -> /shared/ucl/apps/RoseTTAFold-All-Atom_db/UniRef30_2020_06
$ 
```

You then need to prepare your input files which are YAML files. We will take the `protein` example.

```
cp /shared/ucl/apps/rfaa/20250304/RoseTTAFold-All-Atom/rf2aa/config/inference/protein.yaml .
cp /shared/ucl/apps/rfaa/20250304/RoseTTAFold-All-Atom/rf2aa/config/inference/base.yaml .
```

If we look at `protein.yaml`, it depends on `base.yaml`:

```yaml
defaults:
  - base

job_name: "7u7w_protein"
protein_inputs: 
  A:
    fasta_file: examples/protein/7u7w_A.fasta
```

We also need the `fasta` file, so we can copy this to our current directory and modify `protein.yaml` so it can find it.

```
$ cp /shared/ucl/apps/rfaa/20250304/RoseTTAFold-All-Atom/examples/protein/7u7w_A.fasta .
```

```yaml
defaults:
  - base

job_name: "7u7w_protein"
protein_inputs: 
  A:
    fasta_file: 7u7w_A.fasta
```

If we are on a compute node (preferrably with a GPU), booked interactively with `qrsh` we can then directly run the pipeline:

```
python3 -m rf2aa.run_inference --config-path=$(pwd) --config-name protein
```

Note that we specify where to find the config files with the `--config-path` option and give it a configuration name to run with `--config-name`.

```
$ python3 -m rf2aa.run_inference --config-path=$(pwd) --config-name protein
/shared/ucl/apps/rfaa/20250304/miniforge3/envs/RFAA/lib/python3.10/site-packages/hydra/_internal/defaults_list.py:251: UserWarning: In 'protein': Defaults list is missing `_self_`. See https://hydra.cc/docs/1.2/upgrades/1.0_to_1.1/default_composition_order for more information
  warnings.warn(msg, UserWarning)
Using the cif atom ordering for TRP.
make_msa.sh 7u7w_A.fasta 7u7w_protein/A 4 64  pdb100_2021Mar03/pdb100_2021Mar03
Predicting: 100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 1/1 [00:04<00:00,  4.52s/sequences]
Running HHblits against UniRef30 with E-value cutoff 1e-10
- 16:46:14.500 INFO: Input file = 7u7w_protein/A/hhblits/t000_.1e-10.a3m

- 16:46:14.500 INFO: Output file = 7u7w_protein/A/hhblits/t000_.1e-10.id90cov75.a3m

- 16:46:14.727 WARNING: Maximum number 100000 of sequences exceeded in file 7u7w_protein/A/hhblits/t000_.1e-10.a3m

- 16:46:48.226 INFO: Input file = 7u7w_protein/A/hhblits/t000_.1e-10.a3m

- 16:46:48.226 INFO: Output file = 7u7w_protein/A/hhblits/t000_.1e-10.id90cov50.a3m

- 16:46:48.451 WARNING: Maximum number 100000 of sequences exceeded in file 7u7w_protein/A/hhblits/t000_.1e-10.a3m

Running PSIPRED
Running hhsearch
$ ls
u7w_A.fasta  7u7w_protein  7u7w_protein_aux.pt  7u7w_protein.pdb  base.yaml  bfd  outputs  pdb100_2021Mar03  protein.yaml  RFAA_paper_weights.pt  UniRef30_2020_06
```

When the process has run, we should find in our current working directory both a PDB file (`7u7w_protein.pdb`)  with the structure and a PyTorch file (`7u7w_protein_aux.pt`) with some statistical information about the run, as per the documentation.

## Writing a job script

Assuming we have our input YAML and FASTA files in a directory inside our home directory called `rfaa_input`, and the configuration is `input.yaml`, a job script for RFAA looks like this:

```
#!/bin/bash -l

# Batch script to run RoseTTAFold All-Atom on Myriad.

# Request one GPU
#$ -l gpu=1

# Request 18 cores (half a node)
#$ -pe smp 18

# Request two hours of wallclock time (format hours:minutes:seconds).
#$ -l h_rt=2:0:0

# Request 5 gigabyte of RAM per core.
#$ -l mem=5G

# Set the name of the job.
#$ -N RFAA

# Set the working directory to the current working directory.
#$ -cwd # You may wish to change this to a specific directory

module load rfaa/20250304

prepare_rfaa_input_directory # sets up current directory.

# Copy input files into our directory.
# Assume we have our input.yaml and fasta files in ~/rfaa_input - amend.
cp ${HOME}/rfaa_input/*.yaml ${HOME}/rfaa_input/*.fasta .

python3 -m rf2aa.run_inference --config-path=$(pwd) --config-name input

```