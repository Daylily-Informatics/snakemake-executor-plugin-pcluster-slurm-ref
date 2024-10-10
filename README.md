# Snakemake executor plugin: pcluster-slurm v_0.0.4_

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

# More Documentation Pending For:
## How slurm uses `--comment` to tag resources created by the autoscaling cluster to tracke and mnage budgets.