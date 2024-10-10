# Snakemake executor plugin: pcluster-slurm v_0.0.6_

# Snakemake Executor Plugins (generally)
[Snakemake plugin catalog docs](https://snakemake.github.io/snakemake-plugin-catalog/plugins/executor).

## `pcluster-slurm` plugin
### AWS Parallel Cluster, `pcluster` `slurm`
[AWS Parallel Cluster](https://aws.amazon.com/hpc/parallelcluster/) is a framework to deploy and manage dynamically scalable HPC clusters on AWS, running SLURM as the batch system, and `pcluster` manages all of the creating, configuring, and deleting of the cluster compute nodes. Nodes may be spot or dedicated.  **note**, the `AWS Parallel Cluster` port of slurm has a few small, but critical differences from the standard slurm distribution.  This plugin enables using slurm from pcluster head and compute nodes via snakemake `>=v8.*`.


# Pre-requisites
## Snakemake >=8.*
### Conda
```bash
conda create -n snakemake -c conda-forge -c bioconda snakemake==8.20.6
conda activate snakemake
```

# Installation (pip)
_from an environment with snakemake and pip installed_
```bash
pip install snakemake-executor-plugin-pcluster-slurm
```

# Example Usage [daylily cluster headnode](https://github.com/Daylily-Informatics/daylily)
```bash
mkdir -p /fsx/resources/environments/containers/ubuntu/cache/
export SNAKEMAKE_OUTPUT_CACHE=/fsx/resources/environments/containers/ubuntu/cache/
snakemake --use-conda --use-singularity -j 10  --singularity-prefix /fsx/resources/environments/containers/ubuntu/ip-10-0-0-240/ --singularity-args "  -B /tmp:/tmp -B /fsx:/fsx  -B /home/$USER:/home/$USER -B $PWD/:$PWD" --conda-prefix /fsx/resources/environments/containers/ubuntu/ip-10-0-0-240/ --executor pcluster-slurm --default-resources slurm_partition='i64,i128,i192' --cache  --verbose
```

# More Documentation Pending For:
## How slurm uses `--comment` to tag resources created by the autoscaling cluster to tracke and mnage budgets.