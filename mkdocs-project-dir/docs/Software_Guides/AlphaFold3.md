---
title: AlphaFold3
layout: docs
---

The AlphaFold 3 Inference pipeline was developed by Google Deepmind to predict protein structures.

It is installed on Myriad as an Apptainer package meaning it is not in environment modules.

## Setup

!!! important
    You will need to apply to Google for access to the model parameters which you can do [here](https://forms.gle/svvpY4u2jsHEwWYS6).

Once you have downloaded them, put them in a filesystem you have access to on Myriad.

The databases are already downloaded and put in `/shared/ucl/apps/AlphaFold3_db`.

**Note on V100s**

Most of the GPUs on Myriad are V100s, which need to have additional parameters passed to the container, namely setting an environment variable in the contaner (`XLA_FLAGS='--xla_disable_hlo_passes=custom-kernel-fusion-rewriter`) and passing a command line option to AlphaFold3 itself (`--flash_attention_implementation=xla`) in order to produce good structures. You can find more details on this on [this Github issue](https://github.com/google-deepmind/alphafold3/issues/59).

Having added the model weights, you will also need to create input and output directories.

You should now have three locations which are unique to you. For ease of reference  in the job script I'm going to set environment variables to them so that the command-line is consistent and so you can change them without messing with the command line options.

```
export AF3_INPUT=/home/${USER}/Scratch/af3_input # Replace with your input folder
export AF3_INPUT_FILE=fold_input.json # Replace with a file in your input folder
export AF3_OUTPUT=/home/${USER}/Scratch/af3_output # Replace with your output folder
export AF3_WEIGHTS=/home/${USER}/Scratch/weights # Replace with the folder you put the weights in
```

## Running AlphaFold3

Write a job script that requests GPU nodes:

```
#!/bin/bash -l

# Batch script to run AlphaFold 3 on a V100 job under SGE.

# Request one V100
#$ -l gpu=1
#$ -ac allow=EF

# Request 18 cores (half a node)
#$ -pe smp 18

# Request one hour of wallclock time (format hours:minutes:seconds).
#$ -l h_rt=1:0:0

# Request 5 gigabyte of RAM per core.
#$ -l mem=5G

# Set the name of the job.
#$ -N AlphaFold3

# Set the working directory to the current working directory.
#$ -cwd

export AF3_INPUT=/home/${USER}/Scratch/af3_input # Replace with your input folder
export AF3_INPUT_FILE=fold_input.json # Replace with a file in your input folder
export AF3_OUTPUT=/home/${USER}/Scratch/af3_output # Replace with your output folder
export AF3_WEIGHTS=/home/${USER}/Scratch/weights # Replace with the folder you put the weights in

apptainer exec --nv --bind ${AF3_INPUT}:/root/af_input --bind ${AF3_OUTPUT}:/root/af_output --bind ${AF3_WEIGHTS}:/root/models --bind /shared/ucl/apps/AlphaFold3_db:/root/public_databases --no-home --no-mount bind-paths  /shared/ucl/apps/AlphaFold3/alphafold3.sif sh -c "XLA_FLAGS='--xla_disable_hlo_passes=custom-kernel-fusion-rewriter' python3 /app/alphafold/run_alphafold.py --json_path=/root/af_input/${AF3_INPUT_FILE} --model_dir=/root/models --db_dir=/root/public_databases --output_dir=/root/af_output --flash_attention_implementation=xla"
```

The flags which are set to work on V100s almost certainly modestly harm performance on A100 so we set this job to request a V100 (the line that says `#$ -ac allow=EF`). If you delete that line the job will run on any GPU in the cluster.

If you wish to queue for an A100, this job should work:

```
#!/bin/bash -l

# Batch script to run AlphaFold 3 on a V100 job under SGE.

# Request one A100
#$ -l gpu=1
#$ -ac allow=L

# Request 18 cores (half a node)
#$ -pe smp 18

# Request ten minutes of wallclock time (format hours:minutes:seconds).
#$ -l h_rt=1:0:0

# Request 5 gigabyte of RAM per core.
#$ -l mem=5G

# Set the name of the job.
#$ -N AlphaFold3

# Set the working directory to the current working directory.
#$ -cwd

export AF3_INPUT=/home/${USER}/Scratch/af3_input # Replace with your input folder
export AF3_INPUT_FILE=fold_input.json # Replace with a file in your input folder
export AF3_OUTPUT=/home/${USER}/Scratch/af3_output # Replace with your output folder
export AF3_WEIGHTS=/home/${USER}/Scratch/weights # Replace with the folder you put the weights in

apptainer exec --nv --bind ${AF3_INPUT}:/root/af_input --bind ${AF3_OUTPUT}:/root/af_output --bind ${AF3_WEIGHTS}:/root/models --bind /shared/ucl/apps/AlphaFold3_db:/root/public_databases --no-home --no-mount bind-paths  /shared/ucl/apps/AlphaFold3/alphafold3.sif sh -c "python3 /app/alphafold/run_alphafold.py --json_path=/root/af_input/${AF3_INPUT_FILE} --model_dir=/root/models --db_dir=/root/public_databases --output_dir=/root/af_output"
```

For other options you can pass to AlphaFold 3, please consult [DeepMind's documentation](https://github.com/google-deepmind/alphafold3/tree/main/docs).
